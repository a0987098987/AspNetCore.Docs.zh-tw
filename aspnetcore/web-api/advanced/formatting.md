---
title: 在 ASP.NET Core Web API 中格式化回應資料
author: ardalis
description: 了解如何在 ASP.NET Core Web API 中格式化回應資料。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 04/17/2020
uid: web-api/advanced/formatting
ms.openlocfilehash: 392e4905126ffb6801cc55055f1d511f5fa99dd1
ms.sourcegitcommit: 3d07e21868dafc503530ecae2cfa18a7490b58a6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2020
ms.locfileid: "81642701"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="67af3-103">在 ASP.NET Core Web API 中格式化回應資料</span><span class="sxs-lookup"><span data-stu-id="67af3-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="67af3-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫</span><span class="sxs-lookup"><span data-stu-id="67af3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="67af3-105">ASP.NET核心 MVC 支援格式化回應數據。</span><span class="sxs-lookup"><span data-stu-id="67af3-105">ASP.NET Core MVC has support for formatting response data.</span></span> <span data-ttu-id="67af3-106">回應數據可以使用特定格式或回應用戶端請求的格式進行格式化。</span><span class="sxs-lookup"><span data-stu-id="67af3-106">Response data can be formatted using specific formats or in response to client requested format.</span></span>

<span data-ttu-id="67af3-107">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="67af3-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="67af3-108">特定於格式的操作結果</span><span class="sxs-lookup"><span data-stu-id="67af3-108">Format-specific Action Results</span></span>

<span data-ttu-id="67af3-109">某些動作結果類型是特定格式所特有的，例如 <xref:Microsoft.AspNetCore.Mvc.JsonResult> 和 <xref:Microsoft.AspNetCore.Mvc.ContentResult>。</span><span class="sxs-lookup"><span data-stu-id="67af3-109">Some action result types are specific to a particular format, such as <xref:Microsoft.AspNetCore.Mvc.JsonResult> and <xref:Microsoft.AspNetCore.Mvc.ContentResult>.</span></span> <span data-ttu-id="67af3-110">操作可以返回以特定格式格式化的結果,而不考慮用戶端首選項。</span><span class="sxs-lookup"><span data-stu-id="67af3-110">Actions can return results that are formatted in a particular format, regardless of client preferences.</span></span> <span data-ttu-id="67af3-111">例如,返回`JsonResult`返回 JSON 格式的數據。</span><span class="sxs-lookup"><span data-stu-id="67af3-111">For example, returning `JsonResult` returns JSON-formatted data.</span></span> <span data-ttu-id="67af3-112">傳`ContentResult`回或字串傳回純文字格式的字串資料。</span><span class="sxs-lookup"><span data-stu-id="67af3-112">Returning `ContentResult` or a string returns plain-text-formatted string data.</span></span>

<span data-ttu-id="67af3-113">返回任何特定類型不需要操作。</span><span class="sxs-lookup"><span data-stu-id="67af3-113">An action isn't required to return any specific type.</span></span> <span data-ttu-id="67af3-114">ASP.NET核心支援任何物件返回值。</span><span class="sxs-lookup"><span data-stu-id="67af3-114">ASP.NET Core supports any object return value.</span></span>  <span data-ttu-id="67af3-115">返回不是<xref:Microsoft.AspNetCore.Mvc.IActionResult>類型的物件的操作的結果使用適當的<xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter>實現進行序列化。</span><span class="sxs-lookup"><span data-stu-id="67af3-115">Results from actions that return objects that are not <xref:Microsoft.AspNetCore.Mvc.IActionResult> types are serialized using the appropriate <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> implementation.</span></span> <span data-ttu-id="67af3-116">如需詳細資訊，請參閱 <xref:web-api/action-return-types>。</span><span class="sxs-lookup"><span data-stu-id="67af3-116">For more information, see <xref:web-api/action-return-types>.</span></span>

<span data-ttu-id="67af3-117">內建說明器方法<xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*>傳回 JSON 格式的資料:[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span><span class="sxs-lookup"><span data-stu-id="67af3-117">The built-in helper method <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> returns JSON-formatted data: [!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span></span>

<span data-ttu-id="67af3-118">範例下載返回作者清單。</span><span class="sxs-lookup"><span data-stu-id="67af3-118">The sample download returns the list of authors.</span></span> <span data-ttu-id="67af3-119">使用 F12 瀏覽器開發人員工具或[Postman](https://www.getpostman.com/tools)與前面的代碼:</span><span class="sxs-lookup"><span data-stu-id="67af3-119">Using the F12 browser developer tools or [Postman](https://www.getpostman.com/tools) with the previous code:</span></span>

* <span data-ttu-id="67af3-120">將顯示包含**內容類型的**`application/json; charset=utf-8`回應標頭:</span><span class="sxs-lookup"><span data-stu-id="67af3-120">The response header containing **content-type:** `application/json; charset=utf-8` is displayed.</span></span>
* <span data-ttu-id="67af3-121">將顯示請求標頭。</span><span class="sxs-lookup"><span data-stu-id="67af3-121">The request headers are displayed.</span></span> <span data-ttu-id="67af3-122">例如,標頭`Accept`。</span><span class="sxs-lookup"><span data-stu-id="67af3-122">For example, the `Accept` header.</span></span> <span data-ttu-id="67af3-123">前面的`Accept`代碼將忽略標頭。</span><span class="sxs-lookup"><span data-stu-id="67af3-123">The `Accept` header is ignored by the preceding code.</span></span>

<span data-ttu-id="67af3-124">若要傳回純文字格式化資料，請使用 <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> 和 <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> 協助程式：</span><span class="sxs-lookup"><span data-stu-id="67af3-124">To return plain text formatted data, use <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> and the <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_about)]

<span data-ttu-id="67af3-125">在前面的代碼中,`Content-Type`傳回`text/plain`的為 。</span><span class="sxs-lookup"><span data-stu-id="67af3-125">In the preceding code, the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="67af3-126">傳回字串的傳`Content-Type``text/plain`回字串的 :</span><span class="sxs-lookup"><span data-stu-id="67af3-126">Returning a string delivers `Content-Type` of `text/plain`:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_string)]

<span data-ttu-id="67af3-127">對於具有多個傳回類型的操作,`IActionResult`傳回 。</span><span class="sxs-lookup"><span data-stu-id="67af3-127">For actions with multiple return types, return `IActionResult`.</span></span> <span data-ttu-id="67af3-128">例如,根據執行的操作的結果返回不同的 HTTP 狀態代碼。</span><span class="sxs-lookup"><span data-stu-id="67af3-128">For example, returning different HTTP status codes based on the result of operations performed.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="67af3-129">內容協商</span><span class="sxs-lookup"><span data-stu-id="67af3-129">Content negotiation</span></span>

<span data-ttu-id="67af3-130">當用戶端指定[「接受」標頭](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)時,將發生內容協商。</span><span class="sxs-lookup"><span data-stu-id="67af3-130">Content negotiation occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="67af3-131">ASP.NET核心使用的預設格式是[JSON。](https://json.org/)</span><span class="sxs-lookup"><span data-stu-id="67af3-131">The default format used by ASP.NET Core is [JSON](https://json.org/).</span></span> <span data-ttu-id="67af3-132">內容要用:</span><span class="sxs-lookup"><span data-stu-id="67af3-132">Content negotiation is:</span></span>

* <span data-ttu-id="67af3-133">由<xref:Microsoft.AspNetCore.Mvc.ObjectResult>實現。</span><span class="sxs-lookup"><span data-stu-id="67af3-133">Implemented by <xref:Microsoft.AspNetCore.Mvc.ObjectResult>.</span></span>
* <span data-ttu-id="67af3-134">內置於從幫助器方法返回的狀態代碼特定操作結果中。</span><span class="sxs-lookup"><span data-stu-id="67af3-134">Built into the status code-specific action results returned from the helper methods.</span></span> <span data-ttu-id="67af3-135">操作結果說明器方法`ObjectResult`。</span><span class="sxs-lookup"><span data-stu-id="67af3-135">The action results helper methods are based on `ObjectResult`.</span></span>

<span data-ttu-id="67af3-136">傳回模型類型時,傳回`ObjectResult`類型為 。</span><span class="sxs-lookup"><span data-stu-id="67af3-136">When a model type is returned,  the return type is `ObjectResult`.</span></span>

<span data-ttu-id="67af3-137">下列動作方法使用 `Ok` 和 `NotFound` 協助程式方法：</span><span class="sxs-lookup"><span data-stu-id="67af3-137">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_search)]

<span data-ttu-id="67af3-138">預設情況下,ASP.NET核心支援`application/json`和`text/json``text/plain`和媒體類型。</span><span class="sxs-lookup"><span data-stu-id="67af3-138">By default, ASP.NET Core supports `application/json`, `text/json`, and `text/plain` media types.</span></span> <span data-ttu-id="67af3-139">[Fiddler](https://www.telerik.com/fiddler)或[Postman](https://www.getpostman.com/tools)`Accept`等工具可以 設定請求標頭以指定傳回格式。</span><span class="sxs-lookup"><span data-stu-id="67af3-139">Tools such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/tools) can set the `Accept` request header to specify the return format.</span></span> <span data-ttu-id="67af3-140">當`Accept`標頭包含伺服器支援的類型時,將返回該類型。</span><span class="sxs-lookup"><span data-stu-id="67af3-140">When the `Accept` header contains a type the server supports, that type is returned.</span></span> <span data-ttu-id="67af3-141">下一節將演示如何添加其他事務。</span><span class="sxs-lookup"><span data-stu-id="67af3-141">The next section shows how to add additional formatters.</span></span>

<span data-ttu-id="67af3-142">控制器操作可以返回 POCO(普通舊 CLR 物件)。</span><span class="sxs-lookup"><span data-stu-id="67af3-142">Controller actions can return POCOs (Plain Old CLR Objects).</span></span> <span data-ttu-id="67af3-143">傳回 POCO 時,執行時會自動建立環`ObjectResult`繞物件的 。</span><span class="sxs-lookup"><span data-stu-id="67af3-143">When a POCO is returned, the runtime automatically creates an `ObjectResult` that wraps the object.</span></span> <span data-ttu-id="67af3-144">用戶端獲取格式化的序列化物件。</span><span class="sxs-lookup"><span data-stu-id="67af3-144">The client gets the formatted serialized object.</span></span> <span data-ttu-id="67af3-145">如果返回的物件為`null`,則`204 No Content`返回 回應。</span><span class="sxs-lookup"><span data-stu-id="67af3-145">If the object being returned is `null`, a `204 No Content` response is returned.</span></span>

<span data-ttu-id="67af3-146">傳回物件類型：</span><span class="sxs-lookup"><span data-stu-id="67af3-146">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_alias)]

<span data-ttu-id="67af3-147">在前面的代碼中,請求有效的作者別名返回包含作者數據的`200 OK`回應。</span><span class="sxs-lookup"><span data-stu-id="67af3-147">In the preceding code, a request for a valid author alias returns a `200 OK` response with the author's data.</span></span> <span data-ttu-id="67af3-148">不合法的要求傳回回應`204 No Content`。</span><span class="sxs-lookup"><span data-stu-id="67af3-148">A request for an invalid alias returns a `204 No Content` response.</span></span>

### <a name="the-accept-header"></a><span data-ttu-id="67af3-149">Accept 標頭</span><span class="sxs-lookup"><span data-stu-id="67af3-149">The Accept header</span></span>

<span data-ttu-id="67af3-150">當*negotiation*`Accept`請求中出現標頭時,將進行內容協商。</span><span class="sxs-lookup"><span data-stu-id="67af3-150">Content *negotiation* takes place when an `Accept` header appears in the request.</span></span> <span data-ttu-id="67af3-151">當請求包含接受標頭時,ASP.NET核心:</span><span class="sxs-lookup"><span data-stu-id="67af3-151">When a request contains an accept header, ASP.NET Core:</span></span>

* <span data-ttu-id="67af3-152">按首選項順序枚舉接受標頭中的介質類型。</span><span class="sxs-lookup"><span data-stu-id="67af3-152">Enumerates the media types in the accept header in preference order.</span></span>
* <span data-ttu-id="67af3-153">嘗試尋找格式化,該事件可以指定的格式之一生成回應。</span><span class="sxs-lookup"><span data-stu-id="67af3-153">Tries to find a formatter that can produce a response in one of the formats specified.</span></span>

<span data-ttu-id="67af3-154">如果找不到能夠滿足客戶請求的事項,ASP.NET核心:</span><span class="sxs-lookup"><span data-stu-id="67af3-154">If no formatter is found that can satisfy the client's request, ASP.NET Core:</span></span>

* <span data-ttu-id="67af3-155">如果`406 Not Acceptable`<xref:Microsoft.AspNetCore.Mvc.MvcOptions>已設定,則傳回 , 或</span><span class="sxs-lookup"><span data-stu-id="67af3-155">Returns `406 Not Acceptable` if <xref:Microsoft.AspNetCore.Mvc.MvcOptions> has been set, or -</span></span>
* <span data-ttu-id="67af3-156">嘗試查找可以產生回應的第一個事件。</span><span class="sxs-lookup"><span data-stu-id="67af3-156">Tries to find the first formatter that can produce a response.</span></span>

<span data-ttu-id="67af3-157">如果未為請求的格式配置任何格式化,則將使用第一個可以格式化物件的格式化的格式化。</span><span class="sxs-lookup"><span data-stu-id="67af3-157">If no formatter is configured for the requested format, the first formatter that can format the object is used.</span></span> <span data-ttu-id="67af3-158">如果要求`Accept`中未顯示標頭:</span><span class="sxs-lookup"><span data-stu-id="67af3-158">If no `Accept` header appears in the request:</span></span>

* <span data-ttu-id="67af3-159">可以處理物件的第一個要件用於序列化回應。</span><span class="sxs-lookup"><span data-stu-id="67af3-159">The first formatter that can handle the object is used to serialize the response.</span></span>
* <span data-ttu-id="67af3-160">沒有任何談判正在進行。</span><span class="sxs-lookup"><span data-stu-id="67af3-160">There isn't any negotiation taking place.</span></span> <span data-ttu-id="67af3-161">伺服器正在確定要返回的格式。</span><span class="sxs-lookup"><span data-stu-id="67af3-161">The server is determining what format to return.</span></span>

<span data-ttu-id="67af3-162">如果 Accept 標`*/*`頭包含 ,則將`RespectBrowserAcceptHeader`忽略標頭<xref:Microsoft.AspNetCore.Mvc.MvcOptions>,除非在 上 設置為 true。</span><span class="sxs-lookup"><span data-stu-id="67af3-162">If the Accept header contains `*/*`, the Header is ignored unless `RespectBrowserAcceptHeader` is set to true on <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="67af3-163">瀏覽器與內容協商</span><span class="sxs-lookup"><span data-stu-id="67af3-163">Browsers and content negotiation</span></span>

<span data-ttu-id="67af3-164">與典型的 API 用戶端不同,Web`Accept`瀏覽器提供 標頭。</span><span class="sxs-lookup"><span data-stu-id="67af3-164">Unlike typical API clients, web browsers supply `Accept` headers.</span></span> <span data-ttu-id="67af3-165">Web 瀏覽器指定多種格式,包括通配符。</span><span class="sxs-lookup"><span data-stu-id="67af3-165">Web browser specify many formats, including wildcards.</span></span> <span data-ttu-id="67af3-166">預設情況下,當框架檢測到請求來自瀏覽器時:</span><span class="sxs-lookup"><span data-stu-id="67af3-166">By default, when the framework detects that the request is coming from a browser:</span></span>

* <span data-ttu-id="67af3-167">將`Accept`忽略標頭。</span><span class="sxs-lookup"><span data-stu-id="67af3-167">The `Accept` header is ignored.</span></span>
* <span data-ttu-id="67af3-168">除非另有配置,否則內容將返回 JSON。</span><span class="sxs-lookup"><span data-stu-id="67af3-168">The content is returned in JSON, unless otherwise configured.</span></span>

<span data-ttu-id="67af3-169">這在使用 API 時跨瀏覽器提供了更一致的體驗。</span><span class="sxs-lookup"><span data-stu-id="67af3-169">This provides a more consistent experience across browsers when consuming APIs.</span></span>

<span data-ttu-id="67af3-170">要設定應用以尊重瀏覽器接受標頭,請設定為<xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader>`true`:</span><span class="sxs-lookup"><span data-stu-id="67af3-170">To configure an app to honor browser accept headers, set <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> to `true`:</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end

### <a name="configure-formatters"></a><span data-ttu-id="67af3-171">設定要事項</span><span class="sxs-lookup"><span data-stu-id="67af3-171">Configure formatters</span></span>

<span data-ttu-id="67af3-172">需要支援其他格式的應用可以添加適當的 NuGet 包並配置支援。</span><span class="sxs-lookup"><span data-stu-id="67af3-172">Apps that need to support additional formats can add the appropriate NuGet packages and configure support.</span></span> <span data-ttu-id="67af3-173">輸入和輸出有個別的格式器。</span><span class="sxs-lookup"><span data-stu-id="67af3-173">There are separate formatters for input and output.</span></span> <span data-ttu-id="67af3-174">模型繫結用等事項的[輸入](xref:mvc/models/model-binding)。</span><span class="sxs-lookup"><span data-stu-id="67af3-174">Input formatters are used by [Model Binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="67af3-175">輸出事務用於格式化回應。</span><span class="sxs-lookup"><span data-stu-id="67af3-175">Output formatters are used to format responses.</span></span> <span data-ttu-id="67af3-176">有關建立自訂事件的資訊,請參閱[自訂事件](xref:web-api/advanced/custom-formatters)。</span><span class="sxs-lookup"><span data-stu-id="67af3-176">For information on creating a custom formatter, see [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="add-xml-format-support"></a><span data-ttu-id="67af3-177">新增 XML 格式支援</span><span class="sxs-lookup"><span data-stu-id="67af3-177">Add XML format support</span></span>

<span data-ttu-id="67af3-178">使用<xref:System.Xml.Serialization.XmlSerializer>XML 透過呼叫<xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>設定:</span><span class="sxs-lookup"><span data-stu-id="67af3-178">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/3.0sample/Startup.cs?name=snippet)]

<span data-ttu-id="67af3-179">前面的代碼使用`XmlSerializer`序列化結果。</span><span class="sxs-lookup"><span data-stu-id="67af3-179">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="67af3-180">使用上述代碼時,控制器方法基於請求的`Accept`標頭返回適當的格式。</span><span class="sxs-lookup"><span data-stu-id="67af3-180">When using the preceding code, controller methods return the appropriate format based on the request's `Accept` header.</span></span>

### <a name="configure-systemtextjson-based-formatters"></a><span data-ttu-id="67af3-181">設定 System.Text.Json-based 格式器</span><span class="sxs-lookup"><span data-stu-id="67af3-181">Configure System.Text.Json-based formatters</span></span>

<span data-ttu-id="67af3-182">`System.Text.Json` 型格式器可以使用 `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions` 來設定。</span><span class="sxs-lookup"><span data-stu-id="67af3-182">Features for the `System.Text.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span></span>

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.JsonSerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.JsonSerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="67af3-183">輸出序列化選項,以每個操作,可以使用設定使用`JsonResult`。</span><span class="sxs-lookup"><span data-stu-id="67af3-183">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="67af3-184">例如：</span><span class="sxs-lookup"><span data-stu-id="67af3-184">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        options.WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a><span data-ttu-id="67af3-185">新增 Newtonsoft.Json 型 JSON 格式支援</span><span class="sxs-lookup"><span data-stu-id="67af3-185">Add Newtonsoft.Json-based JSON format support</span></span>

<span data-ttu-id="67af3-186">在ASP.NET Core 3.0 之前,預設`Newtonsoft.Json`使用 JSON 處理使用包實現的事項。</span><span class="sxs-lookup"><span data-stu-id="67af3-186">Prior to ASP.NET Core 3.0, the default used JSON formatters implemented using the `Newtonsoft.Json` package.</span></span> <span data-ttu-id="67af3-187">在 ASP.NET Core 3.0 或更新版本中，預設 JSON 格式器是以 `System.Text.Json` 為基礎。</span><span class="sxs-lookup"><span data-stu-id="67af3-187">In ASP.NET Core 3.0 or later, the default JSON formatters are based on `System.Text.Json`.</span></span> <span data-ttu-id="67af3-188">通過安裝`Newtonsoft.Json` [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet 包並將`Startup.ConfigureServices`其配置在 中,可以支援基於的 for 物質和功能。</span><span class="sxs-lookup"><span data-stu-id="67af3-188">Support for `Newtonsoft.Json` based formatters and features is available by installing the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package and configuring it in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](./formatting/3.0sample/StartupNewtonsoftJson.cs?name=snippet)]

<span data-ttu-id="67af3-189">某些功能可能無法很好地處理`System.Text.Json`基於的工件,並且需要`Newtonsoft.Json`參考 基於的工件。</span><span class="sxs-lookup"><span data-stu-id="67af3-189">Some features may not work well with `System.Text.Json`-based formatters and require a reference to the `Newtonsoft.Json`-based formatters.</span></span> <span data-ttu-id="67af3-190">`Newtonsoft.Json`如果套用:</span><span class="sxs-lookup"><span data-stu-id="67af3-190">Continue using the `Newtonsoft.Json`-based formatters if the app:</span></span>

* <span data-ttu-id="67af3-191">使用`Newtonsoft.Json`屬性。</span><span class="sxs-lookup"><span data-stu-id="67af3-191">Uses `Newtonsoft.Json` attributes.</span></span> <span data-ttu-id="67af3-192">例如，`[JsonProperty]` 或 `[JsonIgnore]`。</span><span class="sxs-lookup"><span data-stu-id="67af3-192">For example, `[JsonProperty]` or `[JsonIgnore]`.</span></span>
* <span data-ttu-id="67af3-193">自定義序列化設置。</span><span class="sxs-lookup"><span data-stu-id="67af3-193">Customizes the serialization settings.</span></span>
* <span data-ttu-id="67af3-194">依賴於`Newtonsoft.Json`提供的功能。</span><span class="sxs-lookup"><span data-stu-id="67af3-194">Relies on features that `Newtonsoft.Json` provides.</span></span>
* <span data-ttu-id="67af3-195">設定 `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`。</span><span class="sxs-lookup"><span data-stu-id="67af3-195">Configures `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span></span> <span data-ttu-id="67af3-196">在 ASP.NET Core 3.0 版之前，`JsonResult.SerializerSettings` 接受 `Newtonsoft.Json` 專屬的 `JsonSerializerSettings` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="67af3-196">Prior to ASP.NET Core 3.0, `JsonResult.SerializerSettings` accepts an instance of `JsonSerializerSettings` that is specific to `Newtonsoft.Json`.</span></span>
* <span data-ttu-id="67af3-197">產生 [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) 文件。</span><span class="sxs-lookup"><span data-stu-id="67af3-197">Generates [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) documentation.</span></span>

<span data-ttu-id="67af3-198">可以使用`Newtonsoft.Json`以下設定的要事的項目`Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span><span class="sxs-lookup"><span data-stu-id="67af3-198">Features for the `Newtonsoft.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span></span>

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefaultContractResolver();

    // Configure a custom converter
    options.SerializerSettings.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="67af3-199">輸出序列化選項,以每個操作,可以使用設定使用`JsonResult`。</span><span class="sxs-lookup"><span data-stu-id="67af3-199">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="67af3-200">例如：</span><span class="sxs-lookup"><span data-stu-id="67af3-200">For example:</span></span>

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

### <a name="add-xml-format-support"></a><span data-ttu-id="67af3-201">新增 XML 格式支援</span><span class="sxs-lookup"><span data-stu-id="67af3-201">Add XML format support</span></span>

<span data-ttu-id="67af3-202">XML 格式需要[Microsoft.AspNetCore.Mvc.formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="67af3-202">XML formatting requires the [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet package.</span></span>

<span data-ttu-id="67af3-203">使用<xref:System.Xml.Serialization.XmlSerializer>XML 透過呼叫<xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>設定:</span><span class="sxs-lookup"><span data-stu-id="67af3-203">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet)]

<span data-ttu-id="67af3-204">前面的代碼使用`XmlSerializer`序列化結果。</span><span class="sxs-lookup"><span data-stu-id="67af3-204">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="67af3-205">使用上述代碼時,控制器方法應基於請求的`Accept`標頭返回適當的格式。</span><span class="sxs-lookup"><span data-stu-id="67af3-205">When using the preceding code, controller methods should return the appropriate format based on the request's `Accept` header.</span></span>

::: moniker-end

### <a name="specify-a-format"></a><span data-ttu-id="67af3-206">指定格式</span><span class="sxs-lookup"><span data-stu-id="67af3-206">Specify a format</span></span>

<span data-ttu-id="67af3-207">要限制回應格式,請[`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute)應用篩選器。</span><span class="sxs-lookup"><span data-stu-id="67af3-207">To restrict the response formats, apply the [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter.</span></span> <span data-ttu-id="67af3-208">與大多數[篩選器](xref:mvc/controllers/filters)`[Produces]`一樣 ,可以在操作、控制器或全域作用域中應用:</span><span class="sxs-lookup"><span data-stu-id="67af3-208">Like most [Filters](xref:mvc/controllers/filters), `[Produces]` can be applied at the action, controller, or global scope:</span></span>

[!code-csharp[](./formatting/3.0sample/Controllers/WeatherForecastController.cs?name=snippet)]

<span data-ttu-id="67af3-209">前面的[`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute)篩選器:</span><span class="sxs-lookup"><span data-stu-id="67af3-209">The preceding [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter:</span></span>

* <span data-ttu-id="67af3-210">強制控制器中的所有操作返回 JSON 格式的回應。</span><span class="sxs-lookup"><span data-stu-id="67af3-210">Forces all actions within the controller to return JSON-formatted responses.</span></span>
* <span data-ttu-id="67af3-211">如果配置了其他格式化,並且客戶端指定了不同的格式,則返回 JSON。</span><span class="sxs-lookup"><span data-stu-id="67af3-211">If other formatters are configured and the client specifies a different format, JSON is returned.</span></span>

<span data-ttu-id="67af3-212">有關詳細資訊,請參閱[篩選器](xref:mvc/controllers/filters)。</span><span class="sxs-lookup"><span data-stu-id="67af3-212">For more information, see [Filters](xref:mvc/controllers/filters).</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="67af3-213">特殊案例</span><span class="sxs-lookup"><span data-stu-id="67af3-213">Special case formatters</span></span>

<span data-ttu-id="67af3-214">有些特殊案例是使用內建格式器所實作。</span><span class="sxs-lookup"><span data-stu-id="67af3-214">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="67af3-215">預設情況下,`string`傳回類型格式為*文字/純*`Accept`(如果透過標頭請求 *,則文字/html)。*</span><span class="sxs-lookup"><span data-stu-id="67af3-215">By default, `string` return types are formatted as *text/plain* (*text/html* if requested via the `Accept` header).</span></span> <span data-ttu-id="67af3-216">可以通過刪除<xref:Microsoft.AspNetCore.Mvc.Formatters.StringOutputFormatter>來 刪除此行為。</span><span class="sxs-lookup"><span data-stu-id="67af3-216">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.StringOutputFormatter>.</span></span> <span data-ttu-id="67af3-217">在`ConfigureServices`方法中刪除事點。</span><span class="sxs-lookup"><span data-stu-id="67af3-217">Formatters are removed in the `ConfigureServices` method.</span></span> <span data-ttu-id="67af3-218">具有模型物件的操作傳回類型`204 No Content`傳回`null`時傳回 。</span><span class="sxs-lookup"><span data-stu-id="67af3-218">Actions that have a model object return type return `204 No Content` when returning `null`.</span></span> <span data-ttu-id="67af3-219">可以通過刪除<xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>來 刪除此行為。</span><span class="sxs-lookup"><span data-stu-id="67af3-219">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>.</span></span> <span data-ttu-id="67af3-220">下列程式碼會移除 `StringOutputFormatter` 和 `HttpNoContentOutputFormatter`。</span><span class="sxs-lookup"><span data-stu-id="67af3-220">The following code removes the `StringOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end

<span data-ttu-id="67af3-221">如果沒有`StringOutputFormatter`,內置 JSON`string`格式化格式 返回類型。</span><span class="sxs-lookup"><span data-stu-id="67af3-221">Without the `StringOutputFormatter`, the built-in JSON formatter formats `string` return types.</span></span> <span data-ttu-id="67af3-222">如果刪除內建 JSON 格式化,並且 XML 格式化可用,則`string`XML 格式化格式 將返回類型。</span><span class="sxs-lookup"><span data-stu-id="67af3-222">If the built-in JSON formatter is removed and an XML formatter is available, the XML formatter formats `string` return types.</span></span> <span data-ttu-id="67af3-223">否則,`string`傳回`406 Not Acceptable`型態傳回 。</span><span class="sxs-lookup"><span data-stu-id="67af3-223">Otherwise, `string` return types return `406 Not Acceptable`.</span></span>

<span data-ttu-id="67af3-224">如果沒有 `HttpNoContentOutputFormatter`，則會使用已設定的格式器來格式化 Null 物件。</span><span class="sxs-lookup"><span data-stu-id="67af3-224">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="67af3-225">例如：</span><span class="sxs-lookup"><span data-stu-id="67af3-225">For example:</span></span>

* <span data-ttu-id="67af3-226">JSON 事所返回具有 正`null`文的回應。</span><span class="sxs-lookup"><span data-stu-id="67af3-226">The JSON formatter returns a response with a body of `null`.</span></span>
* <span data-ttu-id="67af3-227">XML 前物質返回具有`xsi:nil="true"`屬性 集的空 XML 元素。</span><span class="sxs-lookup"><span data-stu-id="67af3-227">The XML formatter returns an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="67af3-228">回應格式 URL 對應</span><span class="sxs-lookup"><span data-stu-id="67af3-228">Response format URL mappings</span></span>

<span data-ttu-id="67af3-229">用戶端可以要求特定格式作為網址的一部分,例如:</span><span class="sxs-lookup"><span data-stu-id="67af3-229">Clients can request a particular format as part of the URL, for example:</span></span>

* <span data-ttu-id="67af3-230">在查詢字串或路徑的一部分中。</span><span class="sxs-lookup"><span data-stu-id="67af3-230">In the query string or part of the path.</span></span>
* <span data-ttu-id="67af3-231">通過使用特定於格式的檔副檔名,如 .xml 或 .json。</span><span class="sxs-lookup"><span data-stu-id="67af3-231">By using a format-specific file extension such as .xml or .json.</span></span>

<span data-ttu-id="67af3-232">應該在 API 所使用的路由中指定要求路徑的對應。</span><span class="sxs-lookup"><span data-stu-id="67af3-232">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="67af3-233">例如：</span><span class="sxs-lookup"><span data-stu-id="67af3-233">For example:</span></span>

[!code-csharp[](./formatting/sample/Controllers/ProductsController.cs?name=snippet)]

<span data-ttu-id="67af3-234">前面的路由允許將請求的格式指定為可選檔案擴展名。</span><span class="sxs-lookup"><span data-stu-id="67af3-234">The preceding route allows the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="67af3-235">屬性[`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute)檢查 格式值`RouteData`是否存在, 並在建立回應時將回應格式映射到相應的格式化。</span><span class="sxs-lookup"><span data-stu-id="67af3-235">The [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) attribute checks for the existence of the format value in the `RouteData` and maps the response format to the appropriate formatter when the response is created.</span></span>

|           <span data-ttu-id="67af3-236">路由</span><span class="sxs-lookup"><span data-stu-id="67af3-236">Route</span></span>        |             <span data-ttu-id="67af3-237">格式器</span><span class="sxs-lookup"><span data-stu-id="67af3-237">Formatter</span></span>              |
|------------------------|------------------------------------|
|   `/api/products/5`    |    <span data-ttu-id="67af3-238">預設輸出格式器</span><span class="sxs-lookup"><span data-stu-id="67af3-238">The default output formatter</span></span>    |
| `/api/products/5.json` | <span data-ttu-id="67af3-239">JSON 格式器 (如果已設定)</span><span class="sxs-lookup"><span data-stu-id="67af3-239">The JSON formatter (if configured)</span></span> |
| `/api/products/5.xml`  | <span data-ttu-id="67af3-240">XML 格式器 (如果已設定)</span><span class="sxs-lookup"><span data-stu-id="67af3-240">The XML formatter (if configured)</span></span>  |
