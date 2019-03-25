---
title: 在 ASP.NET Core Web API 中格式化回應資料
author: ardalis
description: 了解如何在 ASP.NET Core Web API 中格式化回應資料。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
uid: web-api/advanced/formatting
ms.openlocfilehash: b0fce0632fd2d885cb8e9a056923ec365d2f327d
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209982"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="692da-103">在 ASP.NET Core Web API 中格式化回應資料</span><span class="sxs-lookup"><span data-stu-id="692da-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="692da-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="692da-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="692da-105">ASP.NET Core MVC 具有使用固定格式或回應用戶端規格的內建回應資料格式化支援。</span><span class="sxs-lookup"><span data-stu-id="692da-105">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="692da-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="692da-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="692da-107">格式特定動作結果</span><span class="sxs-lookup"><span data-stu-id="692da-107">Format-Specific Action Results</span></span>

<span data-ttu-id="692da-108">某些動作結果類型是特定格式所特有的，例如 `JsonResult` 和 `ContentResult`。</span><span class="sxs-lookup"><span data-stu-id="692da-108">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="692da-109">動作可以傳回一律以特定方式格式化的特定結果。</span><span class="sxs-lookup"><span data-stu-id="692da-109">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="692da-110">例如，不論用戶端喜好設定為何，傳回 `JsonResult` 都會傳回 JSON 格式化資料。</span><span class="sxs-lookup"><span data-stu-id="692da-110">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="692da-111">同樣地，傳回 `ContentResult` 將會傳回純文字格式化字串資料 (就像只傳回字串一樣)。</span><span class="sxs-lookup"><span data-stu-id="692da-111">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="692da-112">動作不要求任何特定的型別，MVC 支援任何物件的傳回值。</span><span class="sxs-lookup"><span data-stu-id="692da-112">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="692da-113">如果動作回傳 `IActionResult` 的實作並且「控制器」 繼承自 `Controller`，開發人員會有許多對應至多種選項的輔助方法。</span><span class="sxs-lookup"><span data-stu-id="692da-113">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="692da-114">回傳物件的動作結果不是 `IActionResult` 型別會使用適當的序列化 `IOutputFormatter` 實作。</span><span class="sxs-lookup"><span data-stu-id="692da-114">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="692da-115">若要從繼承自 `Controller` 基底類別的控制器傳回特定格式的資料，請使用內建協助程式方法 `Json` 傳回 JSON 和 `Content` (針對純文字)。</span><span class="sxs-lookup"><span data-stu-id="692da-115">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="692da-116">動作方法應該會傳回特定結果類型 (例如，`JsonResult`) 或 `IActionResult`。</span><span class="sxs-lookup"><span data-stu-id="692da-116">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="692da-117">傳回 JSON 格式化資料：</span><span class="sxs-lookup"><span data-stu-id="692da-117">Returning JSON-formatted data:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="692da-118">此動作的範例回應：</span><span class="sxs-lookup"><span data-stu-id="692da-118">Sample response from this action:</span></span>

![Microsoft Edge 中 [開發人員工具] 的 [網路] 索引標籤，顯示回應的內容類型為 application/json](formatting/_static/json-response.png)

<span data-ttu-id="692da-120">請注意，在網路要求清單及 [回應標頭] 區段中，回應的內容類型都是 `application/json`。</span><span class="sxs-lookup"><span data-stu-id="692da-120">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="692da-121">另請注意，在 [要求標頭] 區段的 Accept 標頭中瀏覽器 (在此情況下為 Microsoft Edge) 所呈現的選項清單。</span><span class="sxs-lookup"><span data-stu-id="692da-121">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="692da-122">目前技術將會忽略此標頭。下面會討論其遵守方式。</span><span class="sxs-lookup"><span data-stu-id="692da-122">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="692da-123">若要傳回純文字格式化資料，請使用 `ContentResult` 和 `Content` 協助程式：</span><span class="sxs-lookup"><span data-stu-id="692da-123">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="692da-124">此動作的回應：</span><span class="sxs-lookup"><span data-stu-id="692da-124">A response from this action:</span></span>

![Microsoft Edge 中 [開發人員工具] 的 [網路] 索引標籤，顯示回應的內容類型為 text/plain](formatting/_static/text-response.png)

<span data-ttu-id="692da-126">請注意，在此情況下，所傳回的 `Content-Type` 是 `text/plain`。</span><span class="sxs-lookup"><span data-stu-id="692da-126">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="692da-127">只要使用字串回應類型，也可以達到這個相同的行為：</span><span class="sxs-lookup"><span data-stu-id="692da-127">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="692da-128">針對具有多個傳回類型或選項 (例如，根據所執行作業結果的不同 HTTP 狀態碼) 的非一般動作，偏好使用 `IActionResult` 作為傳回類型。</span><span class="sxs-lookup"><span data-stu-id="692da-128">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="692da-129">內容交涉</span><span class="sxs-lookup"><span data-stu-id="692da-129">Content Negotiation</span></span>

<span data-ttu-id="692da-130">用戶端指定 [Accept 標頭](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)時，會進行內容交涉 (簡稱 *conneg*)。</span><span class="sxs-lookup"><span data-stu-id="692da-130">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="692da-131">ASP.NET Core MVC 預設使用 JSON 格式。</span><span class="sxs-lookup"><span data-stu-id="692da-131">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="692da-132">內容交涉是由 `ObjectResult` 所實作。</span><span class="sxs-lookup"><span data-stu-id="692da-132">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="692da-133">它也會內建到從協助程式方法 (全部都是根據 `ObjectResult`) 所傳回的狀態碼特定動作結果。</span><span class="sxs-lookup"><span data-stu-id="692da-133">It's also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="692da-134">您也可以傳回模型類型 (已定義為資料傳輸類型的類別)，而且架構會自動將它包裝在 `ObjectResult` 中。</span><span class="sxs-lookup"><span data-stu-id="692da-134">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="692da-135">下列動作方法使用 `Ok` 和 `NotFound` 協助程式方法：</span><span class="sxs-lookup"><span data-stu-id="692da-135">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="692da-136">除非要求另一種格式，而且伺服器可以傳回所要求的格式，否則會傳回 JSON 格式的回應。</span><span class="sxs-lookup"><span data-stu-id="692da-136">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="692da-137">您可以使用 [Fiddler](http://www.telerik.com/fiddler) 這類工具建立包含 Accept 標頭的要求，以及指定另一種格式。</span><span class="sxs-lookup"><span data-stu-id="692da-137">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="692da-138">在此情況下，如果伺服器的「格式器」可以使用所要求的格式產生回應，則會以用戶端慣用的格式傳回結果。</span><span class="sxs-lookup"><span data-stu-id="692da-138">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![Fiddler 主控台，顯示 Accept 標頭值為 application/xml 的手動建立 GET 要求](formatting/_static/fiddler-composer.png)

<span data-ttu-id="692da-140">在上面的螢幕擷取畫面中，已使用 Fiddler Composer 來產生要求，並指定 `Accept: application/xml`。</span><span class="sxs-lookup"><span data-stu-id="692da-140">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="692da-141">ASP.NET Core MVC 預設僅支援 JSON；因此，即使指定另一種格式，所傳回的結果仍然會是 JSON 格式。</span><span class="sxs-lookup"><span data-stu-id="692da-141">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="692da-142">您將在下節中看到如何新增其他格式器。</span><span class="sxs-lookup"><span data-stu-id="692da-142">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="692da-143">控制器動作可以傳回 POCO (簡單的 CLR 物件)，在此情況下，ASP.NET Core MVC 會自動建立可包裝物件的 `ObjectResult`。</span><span class="sxs-lookup"><span data-stu-id="692da-143">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET Core MVC automatically creates an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="692da-144">用戶端會取得格式化的序列化物件 (JSON 格式是預設值；您可以設定 XML 或其他格式)。</span><span class="sxs-lookup"><span data-stu-id="692da-144">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="692da-145">如果所傳回的物件是 `null`，則架構將傳回 `204 No Content` 回應。</span><span class="sxs-lookup"><span data-stu-id="692da-145">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="692da-146">傳回物件類型：</span><span class="sxs-lookup"><span data-stu-id="692da-146">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="692da-147">在此範例中，有效作者別名的要求將會收到包含作者資料的 200 OK 回應。</span><span class="sxs-lookup"><span data-stu-id="692da-147">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="692da-148">無效別名的要求將會收到「204 沒有內容」回應。</span><span class="sxs-lookup"><span data-stu-id="692da-148">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="692da-149">下面顯示以 XML 和 JSON 格式顯示回應的螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="692da-149">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="692da-150">內容交涉程序</span><span class="sxs-lookup"><span data-stu-id="692da-150">Content Negotiation Process</span></span>

<span data-ttu-id="692da-151">只有在 `Accept` 標頭出現在要求中時，才會進行「內容交涉」。</span><span class="sxs-lookup"><span data-stu-id="692da-151">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="692da-152">要求包含 Accept 標頭時，架構會依喜好設定順序來列舉 Accept 標頭中的媒體類型，並嘗試尋找可產生回應的格式器，而回應的格式為 Accept 標頭所指定的其中一種格式。</span><span class="sxs-lookup"><span data-stu-id="692da-152">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="692da-153">如果找不到可滿足用戶端要求的格式器，架構會嘗試尋找第一個可產生回應的格式器 (除非開發人員已在 `MvcOptions` 上設定選項，改為傳回「406 無法接受」)。</span><span class="sxs-lookup"><span data-stu-id="692da-153">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="692da-154">如果要求指定 XML，但尚未設定 XML 格式器，則會使用 JSON 格式器。</span><span class="sxs-lookup"><span data-stu-id="692da-154">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="692da-155">更常見地是，如果未設定格式器來提供所要求的格式，則會使用可格式化物件的第一個格式器。</span><span class="sxs-lookup"><span data-stu-id="692da-155">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="692da-156">如果未指定標頭，則會使用可處理要傳回之物件的第一個格式器來序列化回應。</span><span class="sxs-lookup"><span data-stu-id="692da-156">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="692da-157">在此情況下，無法進行任何交涉，伺服器將會判斷所使用的格式。</span><span class="sxs-lookup"><span data-stu-id="692da-157">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="692da-158">如果 Accept 標頭包含 `*/*`，則除非 `MvcOptions` 上的 `RespectBrowserAcceptHeader` 設定為 true，否則會忽略標頭。</span><span class="sxs-lookup"><span data-stu-id="692da-158">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="692da-159">瀏覽器和內容交涉</span><span class="sxs-lookup"><span data-stu-id="692da-159">Browsers and Content Negotiation</span></span>

<span data-ttu-id="692da-160">與一般 API 用戶端不同，網頁瀏覽器會提供 `Accept` 標頭，內含廣泛的格式陣列 (包括萬用字元)。</span><span class="sxs-lookup"><span data-stu-id="692da-160">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="692da-161">根據預設，架構偵測到要求來自瀏覽器時，將會忽略 `Accept` 標頭，並改為傳回應用程式中已設定預設格式 (除非另外設定，否則為 JSON ) 的內容。</span><span class="sxs-lookup"><span data-stu-id="692da-161">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="692da-162">使用不同的瀏覽器來採用 API 時，這提供更一致的經驗。</span><span class="sxs-lookup"><span data-stu-id="692da-162">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="692da-163">如果您想要應用程式採用瀏覽器 Accept 標頭，則可以將這設定為 MVC 組態的一部分，方法是在 *Startup.cs* 的 `ConfigureServices` 方法中將 `RespectBrowserAcceptHeader` 設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="692da-163">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="692da-164">設定格式器</span><span class="sxs-lookup"><span data-stu-id="692da-164">Configuring Formatters</span></span>

<span data-ttu-id="692da-165">如果您的應用程式需要支援 JSON 預設值以外的其他格式，則可以新增 NuGet 套件，並設定 MVC 來支援它們。</span><span class="sxs-lookup"><span data-stu-id="692da-165">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="692da-166">輸入和輸出有個別的格式器。</span><span class="sxs-lookup"><span data-stu-id="692da-166">There are separate formatters for input and output.</span></span> <span data-ttu-id="692da-167">[模型繫結](xref:mvc/models/model-binding)會使用輸入格式器；輸出格式器是用來格式化回應。</span><span class="sxs-lookup"><span data-stu-id="692da-167">Input formatters are used by [Model Binding](xref:mvc/models/model-binding); output formatters are used to format responses.</span></span> <span data-ttu-id="692da-168">您也可以設定[自訂格式器](xref:web-api/advanced/custom-formatters)。</span><span class="sxs-lookup"><span data-stu-id="692da-168">You can also configure [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

### <a name="adding-xml-format-support"></a><span data-ttu-id="692da-169">新增 XML 格式支援</span><span class="sxs-lookup"><span data-stu-id="692da-169">Adding XML Format Support</span></span>

<span data-ttu-id="692da-170">若要新增 XML 格式支援，請安裝 `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="692da-170">To add support for XML formatting, install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

<span data-ttu-id="692da-171">將 XmlSerializerFormatters 新增至 *Startup.cs* 中的 MVC 組態：</span><span class="sxs-lookup"><span data-stu-id="692da-171">Add the XmlSerializerFormatters to MVC's configuration in *Startup.cs*:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="692da-172">或者，您可以只新增輸出格式器：</span><span class="sxs-lookup"><span data-stu-id="692da-172">Alternately, you can add just the output formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

<span data-ttu-id="692da-173">這兩種方法將使用 `System.Xml.Serialization.XmlSerializer` 來序列化結果。</span><span class="sxs-lookup"><span data-stu-id="692da-173">These two approaches will serialize results using `System.Xml.Serialization.XmlSerializer`.</span></span> <span data-ttu-id="692da-174">如果想要的話，可以新增其相關聯的格式器來使用 `System.Runtime.Serialization.DataContractSerializer`：</span><span class="sxs-lookup"><span data-stu-id="692da-174">If you prefer, you can use the `System.Runtime.Serialization.DataContractSerializer` by adding its associated formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

<span data-ttu-id="692da-175">新增 XML 格式支援之後，控制器方法應該會根據要求的 `Accept` 標頭來傳回適當的格式，如這個 Fiddler 範例所示範：</span><span class="sxs-lookup"><span data-stu-id="692da-175">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Fiddler 主控台：要求的 [原始] 索引標籤會顯示 Accept 標頭值為 application/xml。](formatting/_static/xml-response.png)

<span data-ttu-id="692da-178">您可以在 [偵測器] 索引標籤中看到已設定 `Accept: application/xml` 標頭來提出原始 GET 要求。</span><span class="sxs-lookup"><span data-stu-id="692da-178">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="692da-179">回應窗格會顯示 `Content-Type: application/xml` 標頭，並已將 `Author` 物件序列化為 XML。</span><span class="sxs-lookup"><span data-stu-id="692da-179">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="692da-180">使用 [編輯器] 索引標籤，修改在 `Accept` 標頭中指定 `application/json` 的要求。</span><span class="sxs-lookup"><span data-stu-id="692da-180">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="692da-181">執行要求，並將回應格式化為 JSON：</span><span class="sxs-lookup"><span data-stu-id="692da-181">Execute the request, and the response will be formatted as JSON:</span></span>

![Fiddler 主控台：要求的 [原始] 索引標籤會顯示 Accept 標頭值為 application/json。](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="692da-184">在此螢幕擷取畫面中，您可以看到要求設定 `Accept: application/json` 標頭，而回應指定與其 `Content-Type` 相同的值。</span><span class="sxs-lookup"><span data-stu-id="692da-184">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="692da-185">`Author` 物件會以 JSON 格式顯示在回應本文中。</span><span class="sxs-lookup"><span data-stu-id="692da-185">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="692da-186">強制執行特定格式</span><span class="sxs-lookup"><span data-stu-id="692da-186">Forcing a Particular Format</span></span>

<span data-ttu-id="692da-187">如果您想要限制特定動作的回應格式，則可以套用 `[Produces]` 篩選。</span><span class="sxs-lookup"><span data-stu-id="692da-187">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="692da-188">`[Produces]` 篩選可指定特定動作 (或控制器) 的回應格式。</span><span class="sxs-lookup"><span data-stu-id="692da-188">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="692da-189">與大部分[篩選](xref:mvc/controllers/filters)類似，這可以套用至動作、控制器或全域範圍。</span><span class="sxs-lookup"><span data-stu-id="692da-189">Like most [Filters](xref:mvc/controllers/filters), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="692da-190">`[Produces]` 篩選將強制執行 `AuthorsController` 內的所有動作，以傳回 JSON 格式化回應，即使已設定應用程式和用戶端的其他格式器也是一樣，但前提是 `Accept` 標頭要求不同且可用的格式。</span><span class="sxs-lookup"><span data-stu-id="692da-190">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="692da-191">若要深入了解，請參閱[篩選](xref:mvc/controllers/filters) (包括如何全域套用篩選)。</span><span class="sxs-lookup"><span data-stu-id="692da-191">See [Filters](xref:mvc/controllers/filters) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="692da-192">特殊案例格式器</span><span class="sxs-lookup"><span data-stu-id="692da-192">Special Case Formatters</span></span>

<span data-ttu-id="692da-193">有些特殊案例是使用內建格式器所實作。</span><span class="sxs-lookup"><span data-stu-id="692da-193">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="692da-194">根據預設，`string` 傳回類型將會格式化為 *text/plain* (如果透過 `Accept` 標頭要求，則為 *text/html*)。</span><span class="sxs-lookup"><span data-stu-id="692da-194">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="692da-195">移除 `TextOutputFormatter`，即可移除此行為。</span><span class="sxs-lookup"><span data-stu-id="692da-195">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="692da-196">您可以在 *Startup.cs* 的 `Configure` 方法中移除格式器 (如下所示)。</span><span class="sxs-lookup"><span data-stu-id="692da-196">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="692da-197">傳回 `null` 時，具有模型物件傳回類型的動作會傳回「204 沒有內容」回應。</span><span class="sxs-lookup"><span data-stu-id="692da-197">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="692da-198">移除 `HttpNoContentOutputFormatter`，即可移除此行為。</span><span class="sxs-lookup"><span data-stu-id="692da-198">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="692da-199">下列程式碼會移除 `TextOutputFormatter` 和 `HttpNoContentOutputFormatter`。</span><span class="sxs-lookup"><span data-stu-id="692da-199">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="692da-200">例如，如果沒有 `TextOutputFormatter`，則 `string` 傳回類型會傳回「406 無法接受」。</span><span class="sxs-lookup"><span data-stu-id="692da-200">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="692da-201">請注意，如果存在 XML 格式器，則移除 `TextOutputFormatter` 時，會格式化 `string` 傳回類型。</span><span class="sxs-lookup"><span data-stu-id="692da-201">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="692da-202">如果沒有 `HttpNoContentOutputFormatter`，則會使用已設定的格式器來格式化 Null 物件。</span><span class="sxs-lookup"><span data-stu-id="692da-202">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="692da-203">例如，JSON 格式器只會傳回本文為 `null` 的回應，而 XML 格式器將會傳回已設定屬性 `xsi:nil="true"` 的空白 XML 項目。</span><span class="sxs-lookup"><span data-stu-id="692da-203">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="692da-204">回應格式 URL 對應</span><span class="sxs-lookup"><span data-stu-id="692da-204">Response Format URL Mappings</span></span>

<span data-ttu-id="692da-205">用戶端可以要求特定格式作為 URL 的一部分 (例如在查詢字串中或作為路徑的一部分)，或是使用格式特定副檔名 (例如 .xml 或 .json)。</span><span class="sxs-lookup"><span data-stu-id="692da-205">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="692da-206">應該在 API 所使用的路由中指定要求路徑的對應。</span><span class="sxs-lookup"><span data-stu-id="692da-206">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="692da-207">例如：</span><span class="sxs-lookup"><span data-stu-id="692da-207">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="692da-208">此路由可將所要求的格式指定為選擇性副檔名。</span><span class="sxs-lookup"><span data-stu-id="692da-208">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="692da-209">`[FormatFilter]` 屬性會檢查 `RouteData` 中是否有格式值，並在建立回應時將回應格式對應至適當的格式器。</span><span class="sxs-lookup"><span data-stu-id="692da-209">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

|           <span data-ttu-id="692da-210">路由</span><span class="sxs-lookup"><span data-stu-id="692da-210">Route</span></span>            |             <span data-ttu-id="692da-211">格式器</span><span class="sxs-lookup"><span data-stu-id="692da-211">Formatter</span></span>              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    <span data-ttu-id="692da-212">預設輸出格式器</span><span class="sxs-lookup"><span data-stu-id="692da-212">The default output formatter</span></span>    |
| `/products/GetById/5.json` | <span data-ttu-id="692da-213">JSON 格式器 (如果已設定)</span><span class="sxs-lookup"><span data-stu-id="692da-213">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="692da-214">XML 格式器 (如果已設定)</span><span class="sxs-lookup"><span data-stu-id="692da-214">The XML formatter (if configured)</span></span>  |
