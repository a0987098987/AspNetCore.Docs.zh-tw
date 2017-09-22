---
title: "ASP.NET Core MVC 中的格式化回應資料"
author: ardalis
description: "了解如何設定 ASP.NET Core MVC 中的回應資料的格式。"
keywords: "ASP.NET Core、 回應資料，IOutputFormatter、 IActionResult"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c056df45-d013-4826-91a1-4a092bae1ea5
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 91ba2456178fe806b90f27bbd2940773da950423
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a><span data-ttu-id="ec7dd-104">ASP.NET Core MVC 中的格式化回應資料簡介</span><span class="sxs-lookup"><span data-stu-id="ec7dd-104">Introduction to formatting response data in ASP.NET Core MVC</span></span>

<span data-ttu-id="ec7dd-105">由[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ec7dd-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ec7dd-106">ASP.NET Core MVC 有內建支援來格式化回應資料，使用固定的格式，或在偵測到用戶端規格。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-106">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="ec7dd-107">[檢視或從 GitHub 下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample)。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-107">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="ec7dd-108">特定格式的動作結果</span><span class="sxs-lookup"><span data-stu-id="ec7dd-108">Format-Specific Action Results</span></span>

<span data-ttu-id="ec7dd-109">某些動作結果型別特有的特定格式，例如`JsonResult`和`ContentResult`。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-109">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="ec7dd-110">動作可傳回特定結果永遠以特定方式格式化。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-110">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="ec7dd-111">例如，傳回`JsonResult`會傳回 JSON 格式的資料，用戶端喜好設定無關。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-111">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="ec7dd-112">同樣地，傳回`ContentResult`（如將只傳回一個字串），就會傳回純文字格式的字串資料。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-112">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="ec7dd-113">不需要採取動作，來傳回任何特定的型別。MVC 支援任何物件的傳回值。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-113">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="ec7dd-114">如果動作傳回`IActionResult`實作與 「 控制器 」 繼承自`Controller`，開發人員有許多對應至數個選項的 helper 方法。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-114">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="ec7dd-115">傳回物件的動作結果不是`IActionResult`型別會使用適當的序列化`IOutputFormatter`實作。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-115">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="ec7dd-116">若要以特定格式傳回資料的控制站，繼承自`Controller`基底類別，請使用內建的協助程式方法`Json`傳回 JSON 和`Content`純文字。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-116">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="ec7dd-117">動作方法應該傳回特定結果型別 (例如， `JsonResult`) 或`IActionResult`。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-117">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="ec7dd-118">傳回 JSON 格式的資料：</span><span class="sxs-lookup"><span data-stu-id="ec7dd-118">Returning JSON-formatted data:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="ec7dd-119">從這個動作的範例回應：</span><span class="sxs-lookup"><span data-stu-id="ec7dd-119">Sample response from this action:</span></span>

![網路索引標籤的開發人員工具的 Microsoft Edge 顯示回應的內容類型為 application/json](formatting/_static/json-response.png)

<span data-ttu-id="ec7dd-121">請注意，回應的內容類型為`application/json`，清單中的網路要求和回應標頭區段中所示。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-121">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="ec7dd-122">也請注意顯示 （在此情況下，Microsoft Edge） Accept 標頭在要求標頭 區段中，瀏覽器選項的清單。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-122">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="ec7dd-123">目前的技術會忽略此標頭。以下將討論服從它。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-123">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="ec7dd-124">若要傳回格式化的純文字資料，請使用`ContentResult`和`Content`helper:</span><span class="sxs-lookup"><span data-stu-id="ec7dd-124">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="ec7dd-125">來自此動作的回應：</span><span class="sxs-lookup"><span data-stu-id="ec7dd-125">A response from this action:</span></span>

![在 Microsoft Edge 顯示回應的內容類型的開發人員工具的 [網路] 索引標籤是文字/純文字](formatting/_static/text-response.png)

<span data-ttu-id="ec7dd-127">請注意在此情況下`Content-Type`傳回`text/plain`。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-127">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="ec7dd-128">您也可以達到相同的行為使用剛才字串回應類型：</span><span class="sxs-lookup"><span data-stu-id="ec7dd-128">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="ec7dd-129">非簡單式具有多個動作的傳回類型或選項 （例如，根據所執行作業的結果不同的 HTTP 狀態碼）、 偏好`IActionResult`當做傳回型別。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-129">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="ec7dd-130">內容交涉</span><span class="sxs-lookup"><span data-stu-id="ec7dd-130">Content Negotiation</span></span>

<span data-ttu-id="ec7dd-131">內容交涉 (*conneg*簡稱) 發生於用戶端指定[Accept 標頭](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-131">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="ec7dd-132">使用 ASP.NET Core MVC 的預設格式為 JSON。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-132">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="ec7dd-133">內容交涉由實作`ObjectResult`。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-133">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="ec7dd-134">它也內建的 helper 方法的傳回特定的動作結果的狀態碼 (其中都以基礎`ObjectResult`)。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-134">It is also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="ec7dd-135">您也可以傳回的模型型別 （類別已定義為您的資料傳輸類型） 和架構會自動將其包裝在`ObjectResult`您。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-135">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="ec7dd-136">下列動作方法會使用`Ok`和`NotFound`helper 方法：</span><span class="sxs-lookup"><span data-stu-id="ec7dd-136">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="ec7dd-137">除非要求另一種格式，而且伺服器可以傳回要求的格式，將會傳回 JSON 格式的回應。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-137">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="ec7dd-138">您可以使用這類工具[Fiddler](http://www.telerik.com/fiddler)建立包含 Accept 標頭的要求，並指定另一種格式。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-138">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="ec7dd-139">在此情況下，如果伺服器有*格式器*可以產生的回應要求的格式，將會傳回結果中的用戶端慣用的格式。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-139">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![Fiddler 主控台顯示手動建立取得與應用程式/xml 的 Accept 標頭值的要求](formatting/_static/fiddler-composer.png)

<span data-ttu-id="ec7dd-141">在上面的螢幕擷取畫面，Fiddler 編輯器已經用來產生要求，指定`Accept: application/xml`。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-141">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="ec7dd-142">根據預設，ASP.NET Core MVC 僅支援 JSON，所以即使指定另一種格式，則傳回的結果時，仍 JSON 格式。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-142">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="ec7dd-143">您會看到如何在下一節中加入其他格式子。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-143">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="ec7dd-144">控制器動作可以傳回 POCOs （純舊 CLR 物件），將會自動建立 ASP.NET MVC`ObjectResult`您會包裝物件。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-144">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET MVC will automatically create an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="ec7dd-145">用戶端會取得格式化的序列化的物件 （JSON 格式是預設值; 您可以設定 XML 或其他格式）。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-145">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="ec7dd-146">如果物件傳回為`null`，則會傳回架構`204 No Content`回應。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-146">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="ec7dd-147">傳回的物件類型：</span><span class="sxs-lookup"><span data-stu-id="ec7dd-147">Returning an object type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="ec7dd-148">在範例中，有效的作者別名的要求將會接收 200 OK 回應作者的資料。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-148">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="ec7dd-149">無效的別名的要求將會收到 「 204 沒有內容的回應。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-149">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="ec7dd-150">螢幕擷取畫面顯示回應 XML 和 JSON 的格式如下所示。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-150">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="ec7dd-151">內容交涉程序</span><span class="sxs-lookup"><span data-stu-id="ec7dd-151">Content Negotiation Process</span></span>

<span data-ttu-id="ec7dd-152">內容*交涉*才只會發生`Accept`標頭會出現在要求中。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-152">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="ec7dd-153">當要求包含 accept 標頭時，架構會列舉 accept 標頭中的喜好設定順序中的媒體類型，並會嘗試尋找可能會產生其中一種 accept 標頭所指定格式的回應格式器。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-153">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="ec7dd-154">如果找到不到格式器可以滿足用戶端的要求，架構會嘗試尋找第一個可能會產生回應的格式器 (除非開發人員已設定選項上`MvcOptions`傳回 406 無法接受改為)。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-154">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="ec7dd-155">如果要求指定的 XML，但尚未設定的 XML 格式器，則會使用 JSON 格式器。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-155">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="ec7dd-156">較常見地，如果設定不到格式器，可以提供要求的格式，然後使用比可以格式化物件的第一個格式器。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-156">More generally, if no formatter is configured that can provide the requested format, then the first formatter than can format the object is used.</span></span> <span data-ttu-id="ec7dd-157">如果沒有標頭，可以處理的物件要傳回的第一個格式器將用來序列化回應中。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-157">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="ec7dd-158">在此情況下，沒有任何交涉進行-伺服器會判斷它會使用何種格式。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-158">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="ec7dd-159">如果 Accept 標頭包含`*/*`，將忽略標頭，除非`RespectBrowserAcceptHeader`設為 true `MvcOptions`。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-159">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="ec7dd-160">瀏覽器和內容交涉</span><span class="sxs-lookup"><span data-stu-id="ec7dd-160">Browsers and Content Negotiation</span></span>

<span data-ttu-id="ec7dd-161">不同於一般的 API 用戶端 web 瀏覽器通常會提供`Accept`包含廣泛的格式，包括萬用字元的標頭。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-161">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="ec7dd-162">根據預設，當架構偵測到在要求來自瀏覽器中，它將會忽略`Accept`頁首和改為傳回應用程式中的內容所設定的預設格式 (JSON 除非否則設定)。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-162">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="ec7dd-163">當您使用不同的瀏覽器中使用 Api 時，這會提供更一致的經驗。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-163">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="ec7dd-164">如果您想要使用您的應用程式實行瀏覽器 accept 標頭，您可以設定這為 MVC 的組態一部分藉由設定`RespectBrowserAcceptHeader`至`true`中`ConfigureServices`方法中的*Startup.cs*。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-164">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
}
```

## <a name="configuring-formatters"></a><span data-ttu-id="ec7dd-165">設定格式器</span><span class="sxs-lookup"><span data-stu-id="ec7dd-165">Configuring Formatters</span></span>

<span data-ttu-id="ec7dd-166">若您的應用程式需要支援 JSON 的預設值以外的其他格式，您可以新增 NuGet 封裝，並設定 MVC 支援它們。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-166">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="ec7dd-167">有個別的輸入和輸出的格式器。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-167">There are separate formatters for input and output.</span></span> <span data-ttu-id="ec7dd-168">輸入的格式器所使用的[模型繫結](model-binding.md); 輸出格式器會用來格式化回應。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-168">Input formatters are used by [Model Binding](model-binding.md); output formatters are used to format responses.</span></span> <span data-ttu-id="ec7dd-169">您也可以設定[自訂格式器](../advanced/custom-formatters.md)。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-169">You can also configure [Custom Formatters](../advanced/custom-formatters.md).</span></span>

### <a name="adding-xml-format-support"></a><span data-ttu-id="ec7dd-170">加入 XML 格式的支援</span><span class="sxs-lookup"><span data-stu-id="ec7dd-170">Adding XML Format Support</span></span>

<span data-ttu-id="ec7dd-171">若要新增為 XML 格式的支援，請安裝`Microsoft.AspNetCore.Mvc.Formatters.Xml`NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-171">To add support for XML formatting, install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

<span data-ttu-id="ec7dd-172">MVC 的設定中新增 XmlSerializerFormatters *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ec7dd-172">Add the XmlSerializerFormatters to MVC's configuration in *Startup.cs*:</span></span>

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="ec7dd-173">或者，您可以加入只輸出格式器：</span><span class="sxs-lookup"><span data-stu-id="ec7dd-173">Alternately, you can add just the output formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

<span data-ttu-id="ec7dd-174">這兩種方法將序列化結果使用`System.Xml.Serialization.XmlSerializer`。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-174">These two approaches will serialize results using `System.Xml.Serialization.XmlSerializer`.</span></span> <span data-ttu-id="ec7dd-175">如果您想要的話，您可以使用`System.Runtime.Serialization.DataContractSerializer`加上其相關聯的格式器：</span><span class="sxs-lookup"><span data-stu-id="ec7dd-175">If you prefer, you can use the `System.Runtime.Serialization.DataContractSerializer` by adding its associated formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

<span data-ttu-id="ec7dd-176">一旦您已經加入支援 XML 格式，控制器方法應該傳回適當的格式會根據要求`Accept`標頭，為這個範例示範的 Fiddler:</span><span class="sxs-lookup"><span data-stu-id="ec7dd-176">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Fiddler 主控台： 要求的原始索引標籤會顯示 Accept 標頭值為 application/xml。](formatting/_static/xml-response.png)

<span data-ttu-id="ec7dd-179">您可以在 [偵測器] 索引標籤將未經處理的 GET 要求進行時看到`Accept: application/xml`標頭集合。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-179">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="ec7dd-180">回應 窗格顯示`Content-Type: application/xml`標頭，而`Author`物件序列化為 XML。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-180">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="ec7dd-181">使用編輯器索引標籤，將要求修改成指定`application/json`中`Accept`標頭。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-181">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="ec7dd-182">執行要求，並將會格式化為 JSON 的回應：</span><span class="sxs-lookup"><span data-stu-id="ec7dd-182">Execute the request, and the response will be formatted as JSON:</span></span>

![Fiddler 主控台： 要求的原始索引標籤會顯示 Accept 標頭值為 application/json。](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="ec7dd-185">在這個螢幕擷取畫面中，您可以看到要求設定的標頭`Accept: application/json`並回應指定相同其`Content-Type`。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-185">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="ec7dd-186">`Author`物件如下所示的 JSON 格式的回應主體。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-186">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="ec7dd-187">強制執行特定的格式</span><span class="sxs-lookup"><span data-stu-id="ec7dd-187">Forcing a Particular Format</span></span>

<span data-ttu-id="ec7dd-188">如果您想要限制特定動作的回應格式可以您可以套用`[Produces]`篩選器。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-188">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="ec7dd-189">`[Produces]`篩選條件會指定特定的動作 （或控制站） 的回應格式。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-189">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="ec7dd-190">像大部分[篩選](../controllers/filters.md)，這可套用在動作、 控制器或全域範圍。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-190">Like most [Filters](../controllers/filters.md), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="ec7dd-191">`[Produces]`篩選將會強制執行內的所有動作`AuthorsController`傳回 JSON 格式的回應，如果應用程式和用戶端提供的已設定為其他格式子`Accept`要求不同的標頭可用格式。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-191">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="ec7dd-192">請參閱[篩選](../controllers/filters.md)如需詳細資訊，包括如何套用篩選器。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-192">See [Filters](../controllers/filters.md) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="ec7dd-193">特殊案例的格式器</span><span class="sxs-lookup"><span data-stu-id="ec7dd-193">Special Case Formatters</span></span>

<span data-ttu-id="ec7dd-194">會實作某些特殊情況下，使用內建的格式器。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-194">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="ec7dd-195">根據預設，`string`傳回型別將會格式化為*text/plain* (*text/html*要求透過`Accept`標頭)。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-195">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="ec7dd-196">此行為可以藉由移除移除`TextOutputFormatter`。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-196">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="ec7dd-197">移除中的格式器`Configure`方法中的*Startup.cs* （如下所示）。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-197">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="ec7dd-198">有模型物件的動作傳回型別時將會傳回 204 沒有內容的回應傳回`null`。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-198">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="ec7dd-199">此行為可以藉由移除移除`HttpNoContentOutputFormatter`。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-199">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="ec7dd-200">下列程式碼移除`TextOutputFormatter`和`HttpNoContentOutputFormatter`。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-200">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="ec7dd-201">不含`TextOutputFormatter`，`string`傳回型別傳回 406 無法接受的例如。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-201">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="ec7dd-202">請注意，如果存在 XML 格式器，它將會格式化`string`傳回型別，如果`TextOutputFormatter`已移除。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-202">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="ec7dd-203">不含`HttpNoContentOutputFormatter`，null 物件的格式設定的格式器。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-203">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="ec7dd-204">比方說，JSON 格式器只會傳回的回應主體包含`null`，而 XML 格式器將會傳回空的 XML 項目與屬性`xsi:nil="true"`設定。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-204">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="ec7dd-205">回應格式的 URL 對應</span><span class="sxs-lookup"><span data-stu-id="ec7dd-205">Response Format URL Mappings</span></span>

<span data-ttu-id="ec7dd-206">用戶端可以要求特定格式一部分使用的 URL，例如查詢字串或組件的路徑，或使用特定格式的檔案副檔名，例如.xml 或.json 中。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-206">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="ec7dd-207">從要求路徑對應應指定應用程式開發介面使用的路由。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-207">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="ec7dd-208">例如: </span><span class="sxs-lookup"><span data-stu-id="ec7dd-208">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="ec7dd-209">此路由會讓要求的格式指定為選擇性的副檔名。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-209">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="ec7dd-210">`[FormatFilter]`屬性檢查中的格式值是否存在`RouteData`和建立回應時，將對應的回應格式至適當的格式器。</span><span class="sxs-lookup"><span data-stu-id="ec7dd-210">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

| <span data-ttu-id="ec7dd-211">路由</span><span class="sxs-lookup"><span data-stu-id="ec7dd-211">Route</span></span>                      | <span data-ttu-id="ec7dd-212">格式器</span><span class="sxs-lookup"><span data-stu-id="ec7dd-212">Formatter</span></span>                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | <span data-ttu-id="ec7dd-213">預設的輸出格式器</span><span class="sxs-lookup"><span data-stu-id="ec7dd-213">The default output formatter</span></span>       |
| `/products/GetById/5.json` | <span data-ttu-id="ec7dd-214">JSON 格式子 （如果有設定）</span><span class="sxs-lookup"><span data-stu-id="ec7dd-214">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="ec7dd-215">XML 格式器 （如果有設定）</span><span class="sxs-lookup"><span data-stu-id="ec7dd-215">The XML formatter (if configured)</span></span>  |
