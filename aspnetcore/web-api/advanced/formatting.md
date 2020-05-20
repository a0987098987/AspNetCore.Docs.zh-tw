---
<span data-ttu-id="60006-101">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-101">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-102">'Blazor'</span></span>
- <span data-ttu-id="60006-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-103">'Identity'</span></span>
- <span data-ttu-id="60006-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-105">'Razor'</span></span>
- <span data-ttu-id="60006-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-106">'SignalR' uid:</span></span> 

---
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="60006-107">在 ASP.NET Core Web API 中格式化回應資料</span><span class="sxs-lookup"><span data-stu-id="60006-107">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="60006-108">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫</span><span class="sxs-lookup"><span data-stu-id="60006-108">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="60006-109">ASP.NET Core MVC 支援格式化回應資料。</span><span class="sxs-lookup"><span data-stu-id="60006-109">ASP.NET Core MVC has support for formatting response data.</span></span> <span data-ttu-id="60006-110">您可以使用特定格式或回應用戶端要求的格式來格式化回應資料。</span><span class="sxs-lookup"><span data-stu-id="60006-110">Response data can be formatted using specific formats or in response to client requested format.</span></span>

<span data-ttu-id="60006-111">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="60006-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="60006-112">特定格式的動作結果</span><span class="sxs-lookup"><span data-stu-id="60006-112">Format-specific Action Results</span></span>

<span data-ttu-id="60006-113">某些動作結果類型是特定格式所特有的，例如 <xref:Microsoft.AspNetCore.Mvc.JsonResult> 和 <xref:Microsoft.AspNetCore.Mvc.ContentResult>。</span><span class="sxs-lookup"><span data-stu-id="60006-113">Some action result types are specific to a particular format, such as <xref:Microsoft.AspNetCore.Mvc.JsonResult> and <xref:Microsoft.AspNetCore.Mvc.ContentResult>.</span></span> <span data-ttu-id="60006-114">無論用戶端喜好設定為何，動作都可以傳回以特定格式格式化的結果。</span><span class="sxs-lookup"><span data-stu-id="60006-114">Actions can return results that are formatted in a particular format, regardless of client preferences.</span></span> <span data-ttu-id="60006-115">例如，傳回會傳回 `JsonResult` JSON 格式的資料。</span><span class="sxs-lookup"><span data-stu-id="60006-115">For example, returning `JsonResult` returns JSON-formatted data.</span></span> <span data-ttu-id="60006-116">傳回 `ContentResult` 或字串會傳回純文字格式的字串資料。</span><span class="sxs-lookup"><span data-stu-id="60006-116">Returning `ContentResult` or a string returns plain-text-formatted string data.</span></span>

<span data-ttu-id="60006-117">動作不需要傳回任何特定的類型。</span><span class="sxs-lookup"><span data-stu-id="60006-117">An action isn't required to return any specific type.</span></span> <span data-ttu-id="60006-118">ASP.NET Core 支援任何物件傳回值。</span><span class="sxs-lookup"><span data-stu-id="60006-118">ASP.NET Core supports any object return value.</span></span>  <span data-ttu-id="60006-119">傳回不是型別物件之動作的結果， <xref:Microsoft.AspNetCore.Mvc.IActionResult> 會使用適當的 <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> 實作為序列化。</span><span class="sxs-lookup"><span data-stu-id="60006-119">Results from actions that return objects that are not <xref:Microsoft.AspNetCore.Mvc.IActionResult> types are serialized using the appropriate <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> implementation.</span></span> <span data-ttu-id="60006-120">如需詳細資訊，請參閱<xref:web-api/action-return-types>。</span><span class="sxs-lookup"><span data-stu-id="60006-120">For more information, see <xref:web-api/action-return-types>.</span></span>

<span data-ttu-id="60006-121">內建 helper 方法會傳回 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> JSON 格式的資料：[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span><span class="sxs-lookup"><span data-stu-id="60006-121">The built-in helper method <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> returns JSON-formatted data: [!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span></span>

<span data-ttu-id="60006-122">下載範例會傳回作者清單。</span><span class="sxs-lookup"><span data-stu-id="60006-122">The sample download returns the list of authors.</span></span> <span data-ttu-id="60006-123">使用 F12 瀏覽器開發人員工具或[Postman](https://www.getpostman.com/tools)搭配先前的程式碼：</span><span class="sxs-lookup"><span data-stu-id="60006-123">Using the F12 browser developer tools or [Postman](https://www.getpostman.com/tools) with the previous code:</span></span>

* <span data-ttu-id="60006-124">顯示包含**內容類型：** 的回應標頭 `application/json; charset=utf-8` 。</span><span class="sxs-lookup"><span data-stu-id="60006-124">The response header containing **content-type:** `application/json; charset=utf-8` is displayed.</span></span>
* <span data-ttu-id="60006-125">系統會顯示要求標頭。</span><span class="sxs-lookup"><span data-stu-id="60006-125">The request headers are displayed.</span></span> <span data-ttu-id="60006-126">例如， `Accept` 標頭。</span><span class="sxs-lookup"><span data-stu-id="60006-126">For example, the `Accept` header.</span></span> <span data-ttu-id="60006-127">`Accept`前面的程式碼會忽略標頭。</span><span class="sxs-lookup"><span data-stu-id="60006-127">The `Accept` header is ignored by the preceding code.</span></span>

<span data-ttu-id="60006-128">若要傳回純文字格式化資料，請使用 <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> 和 <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> 協助程式：</span><span class="sxs-lookup"><span data-stu-id="60006-128">To return plain text formatted data, use <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> and the <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_about)]

<span data-ttu-id="60006-129">在上述程式碼中， `Content-Type` 傳回的是 `text/plain` 。</span><span class="sxs-lookup"><span data-stu-id="60006-129">In the preceding code, the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="60006-130">傳回字串傳遞 `Content-Type` `text/plain` 下列內容：</span><span class="sxs-lookup"><span data-stu-id="60006-130">Returning a string delivers `Content-Type` of `text/plain`:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_string)]

<span data-ttu-id="60006-131">針對具有多個傳回類型的動作，傳回 `IActionResult` 。</span><span class="sxs-lookup"><span data-stu-id="60006-131">For actions with multiple return types, return `IActionResult`.</span></span> <span data-ttu-id="60006-132">例如，根據執行的作業結果傳回不同的 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="60006-132">For example, returning different HTTP status codes based on the result of operations performed.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="60006-133">內容協調</span><span class="sxs-lookup"><span data-stu-id="60006-133">Content negotiation</span></span>

<span data-ttu-id="60006-134">當用戶端指定[Accept 標頭](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)時，就會發生內容協商。</span><span class="sxs-lookup"><span data-stu-id="60006-134">Content negotiation occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="60006-135">ASP.NET Core 使用的預設格式為[JSON](https://json.org/)。</span><span class="sxs-lookup"><span data-stu-id="60006-135">The default format used by ASP.NET Core is [JSON](https://json.org/).</span></span> <span data-ttu-id="60006-136">內容協商為：</span><span class="sxs-lookup"><span data-stu-id="60006-136">Content negotiation is:</span></span>

* <span data-ttu-id="60006-137">由執行 <xref:Microsoft.AspNetCore.Mvc.ObjectResult> 。</span><span class="sxs-lookup"><span data-stu-id="60006-137">Implemented by <xref:Microsoft.AspNetCore.Mvc.ObjectResult>.</span></span>
* <span data-ttu-id="60006-138">內建自 helper 方法所傳回的狀態碼特定動作結果。</span><span class="sxs-lookup"><span data-stu-id="60006-138">Built into the status code-specific action results returned from the helper methods.</span></span> <span data-ttu-id="60006-139">動作結果 helper 方法是以為基礎 `ObjectResult` 。</span><span class="sxs-lookup"><span data-stu-id="60006-139">The action results helper methods are based on `ObjectResult`.</span></span>

<span data-ttu-id="60006-140">傳回模型型別時，傳回型別會是 `ObjectResult` 。</span><span class="sxs-lookup"><span data-stu-id="60006-140">When a model type is returned,  the return type is `ObjectResult`.</span></span>

<span data-ttu-id="60006-141">下列動作方法使用 `Ok` 和 `NotFound` 協助程式方法：</span><span class="sxs-lookup"><span data-stu-id="60006-141">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_search)]

<span data-ttu-id="60006-142">根據預設，ASP.NET Core 支援 `application/json` 、 `text/json` 和 `text/plain` 媒體類型。</span><span class="sxs-lookup"><span data-stu-id="60006-142">By default, ASP.NET Core supports `application/json`, `text/json`, and `text/plain` media types.</span></span> <span data-ttu-id="60006-143">[Fiddler](https://www.telerik.com/fiddler)或[Postman](https://www.getpostman.com/tools)這類工具可以設定 `Accept` 要求標頭，以指定傳回格式。</span><span class="sxs-lookup"><span data-stu-id="60006-143">Tools such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/tools) can set the `Accept` request header to specify the return format.</span></span> <span data-ttu-id="60006-144">當 `Accept` 標頭包含伺服器支援的類型時，就會傳回該類型。</span><span class="sxs-lookup"><span data-stu-id="60006-144">When the `Accept` header contains a type the server supports, that type is returned.</span></span> <span data-ttu-id="60006-145">下一節將說明如何新增其他格式器。</span><span class="sxs-lookup"><span data-stu-id="60006-145">The next section shows how to add additional formatters.</span></span>

<span data-ttu-id="60006-146">控制器動作可以傳回 Poco （簡單的 CLR 物件）。</span><span class="sxs-lookup"><span data-stu-id="60006-146">Controller actions can return POCOs (Plain Old CLR Objects).</span></span> <span data-ttu-id="60006-147">當 POCO 傳回時，執行時間會自動建立 `ObjectResult` 包裝物件的。</span><span class="sxs-lookup"><span data-stu-id="60006-147">When a POCO is returned, the runtime automatically creates an `ObjectResult` that wraps the object.</span></span> <span data-ttu-id="60006-148">用戶端會取得已格式化的序列化物件。</span><span class="sxs-lookup"><span data-stu-id="60006-148">The client gets the formatted serialized object.</span></span> <span data-ttu-id="60006-149">如果傳回的物件是 `null` ，則 `204 No Content` 會傳迴響應。</span><span class="sxs-lookup"><span data-stu-id="60006-149">If the object being returned is `null`, a `204 No Content` response is returned.</span></span>

<span data-ttu-id="60006-150">傳回物件類型：</span><span class="sxs-lookup"><span data-stu-id="60006-150">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_alias)]

<span data-ttu-id="60006-151">在上述程式碼中，有效作者別名的要求會傳回 `200 OK` 含有作者資料的回應。</span><span class="sxs-lookup"><span data-stu-id="60006-151">In the preceding code, a request for a valid author alias returns a `200 OK` response with the author's data.</span></span> <span data-ttu-id="60006-152">對無效別名的要求會傳回 `204 No Content` 回應。</span><span class="sxs-lookup"><span data-stu-id="60006-152">A request for an invalid alias returns a `204 No Content` response.</span></span>

### <a name="the-accept-header"></a><span data-ttu-id="60006-153">Accept 標頭</span><span class="sxs-lookup"><span data-stu-id="60006-153">The Accept header</span></span>

<span data-ttu-id="60006-154">當*negotiation* `Accept` 要求中出現標頭時，就會進行內容協商。</span><span class="sxs-lookup"><span data-stu-id="60006-154">Content *negotiation* takes place when an `Accept` header appears in the request.</span></span> <span data-ttu-id="60006-155">當要求包含 accept 標頭時，ASP.NET Core：</span><span class="sxs-lookup"><span data-stu-id="60006-155">When a request contains an accept header, ASP.NET Core:</span></span>

* <span data-ttu-id="60006-156">依照喜好設定順序，列舉 accept 標頭中的媒體類型。</span><span class="sxs-lookup"><span data-stu-id="60006-156">Enumerates the media types in the accept header in preference order.</span></span>
* <span data-ttu-id="60006-157">嘗試尋找可以使用其中一種指定的格式產生回應的格式器。</span><span class="sxs-lookup"><span data-stu-id="60006-157">Tries to find a formatter that can produce a response in one of the formats specified.</span></span>

<span data-ttu-id="60006-158">如果找不到可滿足用戶端要求的格式器，請 ASP.NET Core：</span><span class="sxs-lookup"><span data-stu-id="60006-158">If no formatter is found that can satisfy the client's request, ASP.NET Core:</span></span>

* <span data-ttu-id="60006-159">`406 Not Acceptable`如果 <xref:Microsoft.AspNetCore.Mvc.MvcOptions> 已設定，則會傳回，或-</span><span class="sxs-lookup"><span data-stu-id="60006-159">Returns `406 Not Acceptable` if <xref:Microsoft.AspNetCore.Mvc.MvcOptions> has been set, or -</span></span>
* <span data-ttu-id="60006-160">嘗試尋找可產生回應的第一個格式器。</span><span class="sxs-lookup"><span data-stu-id="60006-160">Tries to find the first formatter that can produce a response.</span></span>

<span data-ttu-id="60006-161">如果未針對要求的格式設定格式器，則會使用可格式化物件的第一個格式器。</span><span class="sxs-lookup"><span data-stu-id="60006-161">If no formatter is configured for the requested format, the first formatter that can format the object is used.</span></span> <span data-ttu-id="60006-162">如果 `Accept` 要求中未出現標頭：</span><span class="sxs-lookup"><span data-stu-id="60006-162">If no `Accept` header appears in the request:</span></span>

* <span data-ttu-id="60006-163">第一個可以處理物件的格式器是用來序列化回應。</span><span class="sxs-lookup"><span data-stu-id="60006-163">The first formatter that can handle the object is used to serialize the response.</span></span>
* <span data-ttu-id="60006-164">沒有任何進行中的協商。</span><span class="sxs-lookup"><span data-stu-id="60006-164">There isn't any negotiation taking place.</span></span> <span data-ttu-id="60006-165">伺服器正在決定要傳回的格式。</span><span class="sxs-lookup"><span data-stu-id="60006-165">The server is determining what format to return.</span></span>

<span data-ttu-id="60006-166">如果 Accept 標頭包含 `*/*` ，除非 `RespectBrowserAcceptHeader` 在上將設為 true，否則會忽略標頭 <xref:Microsoft.AspNetCore.Mvc.MvcOptions> 。</span><span class="sxs-lookup"><span data-stu-id="60006-166">If the Accept header contains `*/*`, the Header is ignored unless `RespectBrowserAcceptHeader` is set to true on <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="60006-167">瀏覽器和內容協調</span><span class="sxs-lookup"><span data-stu-id="60006-167">Browsers and content negotiation</span></span>

<span data-ttu-id="60006-168">與一般 API 用戶端不同的是，網頁瀏覽器會提供 `Accept` 標頭。</span><span class="sxs-lookup"><span data-stu-id="60006-168">Unlike typical API clients, web browsers supply `Accept` headers.</span></span> <span data-ttu-id="60006-169">網頁瀏覽器會指定許多格式，包括萬用字元。</span><span class="sxs-lookup"><span data-stu-id="60006-169">Web browser specify many formats, including wildcards.</span></span> <span data-ttu-id="60006-170">根據預設，當架構偵測到要求來自瀏覽器時：</span><span class="sxs-lookup"><span data-stu-id="60006-170">By default, when the framework detects that the request is coming from a browser:</span></span>

* <span data-ttu-id="60006-171">`Accept`已忽略標頭。</span><span class="sxs-lookup"><span data-stu-id="60006-171">The `Accept` header is ignored.</span></span>
* <span data-ttu-id="60006-172">除非另有設定，否則內容會以 JSON 格式傳回。</span><span class="sxs-lookup"><span data-stu-id="60006-172">The content is returned in JSON, unless otherwise configured.</span></span>

<span data-ttu-id="60006-173">使用 Api 時，這會在瀏覽器之間提供更一致的體驗。</span><span class="sxs-lookup"><span data-stu-id="60006-173">This provides a more consistent experience across browsers when consuming APIs.</span></span>

<span data-ttu-id="60006-174">若要設定應用程式以接受瀏覽器 accept 標頭，請將設 <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> 為 `true` ：</span><span class="sxs-lookup"><span data-stu-id="60006-174">To configure an app to honor browser accept headers, set <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> to `true`:</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end

### <a name="configure-formatters"></a><span data-ttu-id="60006-175">設定格式器</span><span class="sxs-lookup"><span data-stu-id="60006-175">Configure formatters</span></span>

<span data-ttu-id="60006-176">需要支援其他格式的應用程式可以新增適當的 NuGet 套件，並設定支援。</span><span class="sxs-lookup"><span data-stu-id="60006-176">Apps that need to support additional formats can add the appropriate NuGet packages and configure support.</span></span> <span data-ttu-id="60006-177">輸入和輸出有個別的格式器。</span><span class="sxs-lookup"><span data-stu-id="60006-177">There are separate formatters for input and output.</span></span> <span data-ttu-id="60006-178">[模型](xref:mvc/models/model-binding)系結會使用輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="60006-178">Input formatters are used by [Model Binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="60006-179">輸出格式器是用來格式化回應。</span><span class="sxs-lookup"><span data-stu-id="60006-179">Output formatters are used to format responses.</span></span> <span data-ttu-id="60006-180">如需建立自訂格式器的詳細資訊，請參閱[自訂格式化](xref:web-api/advanced/custom-formatters)器。</span><span class="sxs-lookup"><span data-stu-id="60006-180">For information on creating a custom formatter, see [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="add-xml-format-support"></a><span data-ttu-id="60006-181">新增 XML 格式支援</span><span class="sxs-lookup"><span data-stu-id="60006-181">Add XML format support</span></span>

<span data-ttu-id="60006-182">使用執行的 XML 格式器 <xref:System.Xml.Serialization.XmlSerializer> 是透過呼叫來設定 <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> ：</span><span class="sxs-lookup"><span data-stu-id="60006-182">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/3.0sample/Startup.cs?name=snippet)]

<span data-ttu-id="60006-183">上述程式碼會使用來序列化結果 `XmlSerializer` 。</span><span class="sxs-lookup"><span data-stu-id="60006-183">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="60006-184">使用上述程式碼時，控制器方法會根據要求的標頭傳回適當的格式 `Accept` 。</span><span class="sxs-lookup"><span data-stu-id="60006-184">When using the preceding code, controller methods return the appropriate format based on the request's `Accept` header.</span></span>

### <a name="configure-systemtextjson-based-formatters"></a><span data-ttu-id="60006-185">設定 System.Text.Json-based 格式器</span><span class="sxs-lookup"><span data-stu-id="60006-185">Configure System.Text.Json-based formatters</span></span>

<span data-ttu-id="60006-186">`System.Text.Json` 型格式器可以使用 `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions` 來設定。</span><span class="sxs-lookup"><span data-stu-id="60006-186">Features for the `System.Text.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span></span>

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.JsonSerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.JsonSerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="60006-187">以每個動作為基礎的輸出序列化選項，可以使用進行設定 `JsonResult` 。</span><span class="sxs-lookup"><span data-stu-id="60006-187">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="60006-188">例如：</span><span class="sxs-lookup"><span data-stu-id="60006-188">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        options.WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a><span data-ttu-id="60006-189">新增 Newtonsoft.Json 型 JSON 格式支援</span><span class="sxs-lookup"><span data-stu-id="60006-189">Add Newtonsoft.Json-based JSON format support</span></span>

<span data-ttu-id="60006-190">在 ASP.NET Core 3.0 之前，預設使用的 JSON 格式器會使用 `Newtonsoft.Json` 封裝來執行。</span><span class="sxs-lookup"><span data-stu-id="60006-190">Prior to ASP.NET Core 3.0, the default used JSON formatters implemented using the `Newtonsoft.Json` package.</span></span> <span data-ttu-id="60006-191">在 ASP.NET Core 3.0 或更新版本中，預設 JSON 格式器是以 `System.Text.Json` 為基礎。</span><span class="sxs-lookup"><span data-stu-id="60006-191">In ASP.NET Core 3.0 or later, the default JSON formatters are based on `System.Text.Json`.</span></span> <span data-ttu-id="60006-192">`Newtonsoft.Json` [`Microsoft.AspNetCore.Mvc.NewtonsoftJson`](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) 在中安裝 NuGet 套件並加以設定，即可取得根據格式的格式器和功能的支援 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="60006-192">Support for `Newtonsoft.Json` based formatters and features is available by installing the [`Microsoft.AspNetCore.Mvc.NewtonsoftJson`](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package and configuring it in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](./formatting/3.0sample/StartupNewtonsoftJson.cs?name=snippet)]

<span data-ttu-id="60006-193">某些功能可能不適用於以為基礎的格式器 `System.Text.Json` ，而且需要參考以為基礎的格式器 `Newtonsoft.Json` 。</span><span class="sxs-lookup"><span data-stu-id="60006-193">Some features may not work well with `System.Text.Json`-based formatters and require a reference to the `Newtonsoft.Json`-based formatters.</span></span> <span data-ttu-id="60006-194">`Newtonsoft.Json`如果應用程式有下列情況，請繼續使用以為基礎的格式器：</span><span class="sxs-lookup"><span data-stu-id="60006-194">Continue using the `Newtonsoft.Json`-based formatters if the app:</span></span>

* <span data-ttu-id="60006-195">使用 `Newtonsoft.Json` 屬性。</span><span class="sxs-lookup"><span data-stu-id="60006-195">Uses `Newtonsoft.Json` attributes.</span></span> <span data-ttu-id="60006-196">例如，`[JsonProperty]` 或 `[JsonIgnore]`。</span><span class="sxs-lookup"><span data-stu-id="60006-196">For example, `[JsonProperty]` or `[JsonIgnore]`.</span></span>
* <span data-ttu-id="60006-197">自訂序列化設定。</span><span class="sxs-lookup"><span data-stu-id="60006-197">Customizes the serialization settings.</span></span>
* <span data-ttu-id="60006-198">依賴提供的功能 `Newtonsoft.Json` 。</span><span class="sxs-lookup"><span data-stu-id="60006-198">Relies on features that `Newtonsoft.Json` provides.</span></span>
* <span data-ttu-id="60006-199">設定 `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`。</span><span class="sxs-lookup"><span data-stu-id="60006-199">Configures `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span></span> <span data-ttu-id="60006-200">在 ASP.NET Core 3.0 版之前，`JsonResult.SerializerSettings` 接受 `Newtonsoft.Json` 專屬的 `JsonSerializerSettings` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="60006-200">Prior to ASP.NET Core 3.0, `JsonResult.SerializerSettings` accepts an instance of `JsonSerializerSettings` that is specific to `Newtonsoft.Json`.</span></span>
* <span data-ttu-id="60006-201">產生 [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) 文件。</span><span class="sxs-lookup"><span data-stu-id="60006-201">Generates [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) documentation.</span></span>

<span data-ttu-id="60006-202">您可以使用來設定以為基礎之格式器的功能 `Newtonsoft.Json` `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings` ：</span><span class="sxs-lookup"><span data-stu-id="60006-202">Features for the `Newtonsoft.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span></span>

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefaultContractResolver();

    // Configure a custom converter
    options.SerializerSettings.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="60006-203">以每個動作為基礎的輸出序列化選項，可以使用進行設定 `JsonResult` 。</span><span class="sxs-lookup"><span data-stu-id="60006-203">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="60006-204">例如：</span><span class="sxs-lookup"><span data-stu-id="60006-204">For example:</span></span>

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

### <a name="add-xml-format-support"></a><span data-ttu-id="60006-205">新增 XML 格式支援</span><span class="sxs-lookup"><span data-stu-id="60006-205">Add XML format support</span></span>

<span data-ttu-id="60006-206">XML 格式設定需要[AspNetCore 的 xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="60006-206">XML formatting requires the [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet package.</span></span>

<span data-ttu-id="60006-207">使用執行的 XML 格式器 <xref:System.Xml.Serialization.XmlSerializer> 是透過呼叫來設定 <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> ：</span><span class="sxs-lookup"><span data-stu-id="60006-207">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet)]

<span data-ttu-id="60006-208">上述程式碼會使用來序列化結果 `XmlSerializer` 。</span><span class="sxs-lookup"><span data-stu-id="60006-208">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="60006-209">使用上述程式碼時，控制器方法應該根據要求的標頭傳回適當的格式 `Accept` 。</span><span class="sxs-lookup"><span data-stu-id="60006-209">When using the preceding code, controller methods should return the appropriate format based on the request's `Accept` header.</span></span>

::: moniker-end

### <a name="specify-a-format"></a><span data-ttu-id="60006-210">指定格式</span><span class="sxs-lookup"><span data-stu-id="60006-210">Specify a format</span></span>

<span data-ttu-id="60006-211">若要限制回應格式，請套用 [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) 篩選準則。</span><span class="sxs-lookup"><span data-stu-id="60006-211">To restrict the response formats, apply the [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter.</span></span> <span data-ttu-id="60006-212">就像大部分的[篩選器](xref:mvc/controllers/filters)一樣， `[Produces]` 可以在動作、控制器或全域範圍套用：</span><span class="sxs-lookup"><span data-stu-id="60006-212">Like most [Filters](xref:mvc/controllers/filters), `[Produces]` can be applied at the action, controller, or global scope:</span></span>

[!code-csharp[](./formatting/3.0sample/Controllers/WeatherForecastController.cs?name=snippet)]

<span data-ttu-id="60006-213">上述 [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) 篩選準則：</span><span class="sxs-lookup"><span data-stu-id="60006-213">The preceding [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter:</span></span>

* <span data-ttu-id="60006-214">強制控制器內的所有動作傳回 JSON 格式的回應。</span><span class="sxs-lookup"><span data-stu-id="60006-214">Forces all actions within the controller to return JSON-formatted responses.</span></span>
* <span data-ttu-id="60006-215">如果設定了其他格式器，而且用戶端指定了不同的格式，則會傳回 JSON。</span><span class="sxs-lookup"><span data-stu-id="60006-215">If other formatters are configured and the client specifies a different format, JSON is returned.</span></span>

<span data-ttu-id="60006-216">如需詳細資訊，請參閱[篩選](xref:mvc/controllers/filters)。</span><span class="sxs-lookup"><span data-stu-id="60006-216">For more information, see [Filters](xref:mvc/controllers/filters).</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="60006-217">特殊案例格式器</span><span class="sxs-lookup"><span data-stu-id="60006-217">Special case formatters</span></span>

<span data-ttu-id="60006-218">有些特殊案例是使用內建格式器所實作。</span><span class="sxs-lookup"><span data-stu-id="60006-218">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="60006-219">根據預設，傳回 `string` 類型的格式為*text/純文字*（如果透過標頭要求，則為*text/html* `Accept` ）。</span><span class="sxs-lookup"><span data-stu-id="60006-219">By default, `string` return types are formatted as *text/plain* (*text/html* if requested via the `Accept` header).</span></span> <span data-ttu-id="60006-220">您可以藉由移除來刪除這個行為 <xref:Microsoft.AspNetCore.Mvc.Formatters.StringOutputFormatter> 。</span><span class="sxs-lookup"><span data-stu-id="60006-220">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.StringOutputFormatter>.</span></span> <span data-ttu-id="60006-221">已移除方法中的格式器 `ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="60006-221">Formatters are removed in the `ConfigureServices` method.</span></span> <span data-ttu-id="60006-222">傳回時，具有模型物件傳回類型的動作會返回 `204 No Content` `null` 。</span><span class="sxs-lookup"><span data-stu-id="60006-222">Actions that have a model object return type return `204 No Content` when returning `null`.</span></span> <span data-ttu-id="60006-223">您可以藉由移除來刪除這個行為 <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter> 。</span><span class="sxs-lookup"><span data-stu-id="60006-223">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>.</span></span> <span data-ttu-id="60006-224">下列程式碼會移除 `StringOutputFormatter` 和 `HttpNoContentOutputFormatter`。</span><span class="sxs-lookup"><span data-stu-id="60006-224">The following code removes the `StringOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end

<span data-ttu-id="60006-225">若沒有 `StringOutputFormatter` ，內建的 JSON 格式器格式會傳回 `string` 類型。</span><span class="sxs-lookup"><span data-stu-id="60006-225">Without the `StringOutputFormatter`, the built-in JSON formatter formats `string` return types.</span></span> <span data-ttu-id="60006-226">如果已移除內建 JSON 格式器，而且有 XML 格式器，則 XML 格式器格式會傳回 `string` 類型。</span><span class="sxs-lookup"><span data-stu-id="60006-226">If the built-in JSON formatter is removed and an XML formatter is available, the XML formatter formats `string` return types.</span></span> <span data-ttu-id="60006-227">否則，傳回類型會傳回 `string` `406 Not Acceptable` 。</span><span class="sxs-lookup"><span data-stu-id="60006-227">Otherwise, `string` return types return `406 Not Acceptable`.</span></span>

<span data-ttu-id="60006-228">如果沒有 `HttpNoContentOutputFormatter`，則會使用已設定的格式器來格式化 Null 物件。</span><span class="sxs-lookup"><span data-stu-id="60006-228">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="60006-229">例如：</span><span class="sxs-lookup"><span data-stu-id="60006-229">For example:</span></span>

* <span data-ttu-id="60006-230">JSON 格式器會傳回具有主體的回應 `null` 。</span><span class="sxs-lookup"><span data-stu-id="60006-230">The JSON formatter returns a response with a body of `null`.</span></span>
* <span data-ttu-id="60006-231">XML 格式器會傳回具有屬性集的空 XML 元素 `xsi:nil="true"` 。</span><span class="sxs-lookup"><span data-stu-id="60006-231">The XML formatter returns an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="60006-232">回應格式 URL 對應</span><span class="sxs-lookup"><span data-stu-id="60006-232">Response format URL mappings</span></span>

<span data-ttu-id="60006-233">用戶端可以要求特定格式做為 URL 的一部分，例如：</span><span class="sxs-lookup"><span data-stu-id="60006-233">Clients can request a particular format as part of the URL, for example:</span></span>

* <span data-ttu-id="60006-234">在查詢字串或部分路徑中。</span><span class="sxs-lookup"><span data-stu-id="60006-234">In the query string or part of the path.</span></span>
* <span data-ttu-id="60006-235">使用格式特定的副檔名，例如 .xml 或. json。</span><span class="sxs-lookup"><span data-stu-id="60006-235">By using a format-specific file extension such as .xml or .json.</span></span>

<span data-ttu-id="60006-236">應該在 API 所使用的路由中指定要求路徑的對應。</span><span class="sxs-lookup"><span data-stu-id="60006-236">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="60006-237">例如：</span><span class="sxs-lookup"><span data-stu-id="60006-237">For example:</span></span>

[!code-csharp[](./formatting/sample/Controllers/ProductsController.cs?name=snippet)]

<span data-ttu-id="60006-238">先前的路由可讓要求的格式指定為選用的副檔名。</span><span class="sxs-lookup"><span data-stu-id="60006-238">The preceding route allows the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="60006-239">[`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute)屬性會檢查中的格式值是否存在 `RouteData` ，並在建立回應時，將回應格式對應至適當的格式器。</span><span class="sxs-lookup"><span data-stu-id="60006-239">The [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) attribute checks for the existence of the format value in the `RouteData` and maps the response format to the appropriate formatter when the response is created.</span></span>

|           <span data-ttu-id="60006-240">路由</span><span class="sxs-lookup"><span data-stu-id="60006-240">Route</span></span>        |             <span data-ttu-id="60006-241">格式器</span><span class="sxs-lookup"><span data-stu-id="60006-241">Formatter</span></span>              |
|---
<span data-ttu-id="60006-242">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-242">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-243">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-243">'Blazor'</span></span>
- <span data-ttu-id="60006-244">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-244">'Identity'</span></span>
- <span data-ttu-id="60006-245">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-245">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-246">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-246">'Razor'</span></span>
- <span data-ttu-id="60006-247">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-247">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-248">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-248">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-249">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-249">'Blazor'</span></span>
- <span data-ttu-id="60006-250">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-250">'Identity'</span></span>
- <span data-ttu-id="60006-251">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-251">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-252">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-252">'Razor'</span></span>
- <span data-ttu-id="60006-253">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-253">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-254">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-254">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-255">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-255">'Blazor'</span></span>
- <span data-ttu-id="60006-256">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-256">'Identity'</span></span>
- <span data-ttu-id="60006-257">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-257">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-258">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-258">'Razor'</span></span>
- <span data-ttu-id="60006-259">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-259">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-260">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-260">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-261">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-261">'Blazor'</span></span>
- <span data-ttu-id="60006-262">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-262">'Identity'</span></span>
- <span data-ttu-id="60006-263">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-263">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-264">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-264">'Razor'</span></span>
- <span data-ttu-id="60006-265">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-265">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-266">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-266">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-267">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-267">'Blazor'</span></span>
- <span data-ttu-id="60006-268">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-268">'Identity'</span></span>
- <span data-ttu-id="60006-269">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-269">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-270">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-270">'Razor'</span></span>
- <span data-ttu-id="60006-271">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-271">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-272">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-272">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-273">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-273">'Blazor'</span></span>
- <span data-ttu-id="60006-274">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-274">'Identity'</span></span>
- <span data-ttu-id="60006-275">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-275">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-276">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-276">'Razor'</span></span>
- <span data-ttu-id="60006-277">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-277">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-278">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-278">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-279">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-279">'Blazor'</span></span>
- <span data-ttu-id="60006-280">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-280">'Identity'</span></span>
- <span data-ttu-id="60006-281">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-281">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-282">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-282">'Razor'</span></span>
- <span data-ttu-id="60006-283">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-283">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-284">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-284">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-285">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-285">'Blazor'</span></span>
- <span data-ttu-id="60006-286">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-286">'Identity'</span></span>
- <span data-ttu-id="60006-287">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-287">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-288">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-288">'Razor'</span></span>
- <span data-ttu-id="60006-289">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-289">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-290">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-290">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-291">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-291">'Blazor'</span></span>
- <span data-ttu-id="60006-292">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-292">'Identity'</span></span>
- <span data-ttu-id="60006-293">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-293">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-294">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-294">'Razor'</span></span>
- <span data-ttu-id="60006-295">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-295">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-296">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-296">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-297">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-297">'Blazor'</span></span>
- <span data-ttu-id="60006-298">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-298">'Identity'</span></span>
- <span data-ttu-id="60006-299">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-299">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-300">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-300">'Razor'</span></span>
- <span data-ttu-id="60006-301">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-301">'SignalR' uid:</span></span> 

<span data-ttu-id="60006-302">------------|---標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-302">------------|--- title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-303">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-303">'Blazor'</span></span>
- <span data-ttu-id="60006-304">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-304">'Identity'</span></span>
- <span data-ttu-id="60006-305">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-305">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-306">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-306">'Razor'</span></span>
- <span data-ttu-id="60006-307">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-307">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-308">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-308">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-309">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-309">'Blazor'</span></span>
- <span data-ttu-id="60006-310">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-310">'Identity'</span></span>
- <span data-ttu-id="60006-311">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-311">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-312">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-312">'Razor'</span></span>
- <span data-ttu-id="60006-313">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-313">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-314">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-314">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-315">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-315">'Blazor'</span></span>
- <span data-ttu-id="60006-316">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-316">'Identity'</span></span>
- <span data-ttu-id="60006-317">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-317">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-318">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-318">'Razor'</span></span>
- <span data-ttu-id="60006-319">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-319">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-320">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-320">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-321">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-321">'Blazor'</span></span>
- <span data-ttu-id="60006-322">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-322">'Identity'</span></span>
- <span data-ttu-id="60006-323">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-323">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-324">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-324">'Razor'</span></span>
- <span data-ttu-id="60006-325">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-325">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-326">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-326">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-327">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-327">'Blazor'</span></span>
- <span data-ttu-id="60006-328">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-328">'Identity'</span></span>
- <span data-ttu-id="60006-329">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-329">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-330">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-330">'Razor'</span></span>
- <span data-ttu-id="60006-331">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-331">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-332">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-332">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-333">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-333">'Blazor'</span></span>
- <span data-ttu-id="60006-334">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-334">'Identity'</span></span>
- <span data-ttu-id="60006-335">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-335">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-336">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-336">'Razor'</span></span>
- <span data-ttu-id="60006-337">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-337">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-338">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-338">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-339">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-339">'Blazor'</span></span>
- <span data-ttu-id="60006-340">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-340">'Identity'</span></span>
- <span data-ttu-id="60006-341">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-341">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-342">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-342">'Razor'</span></span>
- <span data-ttu-id="60006-343">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-343">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-344">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-344">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-345">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-345">'Blazor'</span></span>
- <span data-ttu-id="60006-346">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-346">'Identity'</span></span>
- <span data-ttu-id="60006-347">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-347">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-348">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-348">'Razor'</span></span>
- <span data-ttu-id="60006-349">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-349">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-350">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-350">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-351">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-351">'Blazor'</span></span>
- <span data-ttu-id="60006-352">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-352">'Identity'</span></span>
- <span data-ttu-id="60006-353">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-353">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-354">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-354">'Razor'</span></span>
- <span data-ttu-id="60006-355">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-355">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-356">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-356">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-357">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-357">'Blazor'</span></span>
- <span data-ttu-id="60006-358">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-358">'Identity'</span></span>
- <span data-ttu-id="60006-359">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-359">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-360">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-360">'Razor'</span></span>
- <span data-ttu-id="60006-361">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-361">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-362">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-362">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-363">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-363">'Blazor'</span></span>
- <span data-ttu-id="60006-364">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-364">'Identity'</span></span>
- <span data-ttu-id="60006-365">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-365">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-366">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-366">'Razor'</span></span>
- <span data-ttu-id="60006-367">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-367">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-368">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-368">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-369">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-369">'Blazor'</span></span>
- <span data-ttu-id="60006-370">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-370">'Identity'</span></span>
- <span data-ttu-id="60006-371">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-371">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-372">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-372">'Razor'</span></span>
- <span data-ttu-id="60006-373">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-373">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-374">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-374">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-375">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-375">'Blazor'</span></span>
- <span data-ttu-id="60006-376">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-376">'Identity'</span></span>
- <span data-ttu-id="60006-377">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-377">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-378">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-378">'Razor'</span></span>
- <span data-ttu-id="60006-379">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-379">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-380">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-380">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-381">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-381">'Blazor'</span></span>
- <span data-ttu-id="60006-382">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-382">'Identity'</span></span>
- <span data-ttu-id="60006-383">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-383">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-384">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-384">'Razor'</span></span>
- <span data-ttu-id="60006-385">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-385">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-386">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-386">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-387">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-387">'Blazor'</span></span>
- <span data-ttu-id="60006-388">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-388">'Identity'</span></span>
- <span data-ttu-id="60006-389">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-389">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-390">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-390">'Razor'</span></span>
- <span data-ttu-id="60006-391">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-391">'SignalR' uid:</span></span> 

-
<span data-ttu-id="60006-392">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="60006-392">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="60006-393">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="60006-393">'Blazor'</span></span>
- <span data-ttu-id="60006-394">'Identity'</span><span class="sxs-lookup"><span data-stu-id="60006-394">'Identity'</span></span>
- <span data-ttu-id="60006-395">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="60006-395">'Let's Encrypt'</span></span>
- <span data-ttu-id="60006-396">'Razor'</span><span class="sxs-lookup"><span data-stu-id="60006-396">'Razor'</span></span>
- <span data-ttu-id="60006-397">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="60006-397">'SignalR' uid:</span></span> 

<span data-ttu-id="60006-398">------------------| |  `/api/products/5`    |   預設輸出格式器 | |`/api/products/5.json` |JSON 格式器（如果已設定） | |`/api/products/5.xml`  |XML 格式器（如果已設定） |</span><span class="sxs-lookup"><span data-stu-id="60006-398">------------------| |   `/api/products/5`    |    The default output formatter    | | `/api/products/5.json` | The JSON formatter (if configured) | | `/api/products/5.xml`  | The XML formatter (if configured)  |</span></span>
