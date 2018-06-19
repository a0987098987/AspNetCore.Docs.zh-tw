---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: 從.NET 用戶端 (C#) 呼叫 Web API |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: fdb74b0eb74ce7f387f49a0b25ceebd3fc389da9
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2018
ms.locfileid: "32006153"
---
<a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="06c43-102">呼叫 Web API，從.NET 用戶端 (C#)</span><span class="sxs-lookup"><span data-stu-id="06c43-102">Call a Web API From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="06c43-103">由[Mike Wasson](https://github.com/MikeWasson)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="06c43-103">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="06c43-104">[下載完成的專案](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)。</span><span class="sxs-lookup"><span data-stu-id="06c43-104">[Download Completed Project](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span></span> <span data-ttu-id="06c43-105">[下載指示](/aspnet/core/tutorials/#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="06c43-105">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> 

<span data-ttu-id="06c43-106">本教學課程示範如何從.NET 應用程式呼叫 web API 使用[System.Net.Http.HttpClient。](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="06c43-106">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="06c43-107">在本教學課程中，用戶端應用程式會撰寫可使用下列 web 應用程式開發介面：</span><span class="sxs-lookup"><span data-stu-id="06c43-107">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="06c43-108">動作</span><span class="sxs-lookup"><span data-stu-id="06c43-108">Action</span></span> | <span data-ttu-id="06c43-109">HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="06c43-109">HTTP method</span></span> | <span data-ttu-id="06c43-110">相對 URI</span><span class="sxs-lookup"><span data-stu-id="06c43-110">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="06c43-111">取得產品識別碼</span><span class="sxs-lookup"><span data-stu-id="06c43-111">Get a product by ID</span></span> | <span data-ttu-id="06c43-112">GET</span><span class="sxs-lookup"><span data-stu-id="06c43-112">GET</span></span> | <span data-ttu-id="06c43-113">/api/產品/*識別碼*</span><span class="sxs-lookup"><span data-stu-id="06c43-113">/api/products/*id*</span></span> |
| <span data-ttu-id="06c43-114">建立新的產品</span><span class="sxs-lookup"><span data-stu-id="06c43-114">Create a new product</span></span> | <span data-ttu-id="06c43-115">POST</span><span class="sxs-lookup"><span data-stu-id="06c43-115">POST</span></span> | <span data-ttu-id="06c43-116">/ api/產品</span><span class="sxs-lookup"><span data-stu-id="06c43-116">/api/products</span></span> |
| <span data-ttu-id="06c43-117">產品更新</span><span class="sxs-lookup"><span data-stu-id="06c43-117">Update a product</span></span> | <span data-ttu-id="06c43-118">PUT</span><span class="sxs-lookup"><span data-stu-id="06c43-118">PUT</span></span> | <span data-ttu-id="06c43-119">/api/產品/*識別碼*</span><span class="sxs-lookup"><span data-stu-id="06c43-119">/api/products/*id*</span></span> |
| <span data-ttu-id="06c43-120">刪除產品</span><span class="sxs-lookup"><span data-stu-id="06c43-120">Delete a product</span></span> | <span data-ttu-id="06c43-121">DELETE</span><span class="sxs-lookup"><span data-stu-id="06c43-121">DELETE</span></span> | <span data-ttu-id="06c43-122">/api/產品/*識別碼*</span><span class="sxs-lookup"><span data-stu-id="06c43-122">/api/products/*id*</span></span> |

<span data-ttu-id="06c43-123">若要深入了解如何實作此 API 與 ASP.NET Web API，請參閱[建立 Web API 的支援 CRUD 操作](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
)。</span><span class="sxs-lookup"><span data-stu-id="06c43-123">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="06c43-124">為了簡單起見，本教學課程中的用戶端應用程式是 Windows 主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="06c43-124">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="06c43-125">**HttpClient**也支援 Windows Phone 和 Windows 市集應用程式。</span><span class="sxs-lookup"><span data-stu-id="06c43-125">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="06c43-126">如需詳細資訊，請參閱[撰寫 Web API 用戶端程式碼的多個平台使用可攜式程式庫](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="06c43-126">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="06c43-127">建立主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="06c43-127">Create the Console Application</span></span>

<span data-ttu-id="06c43-128">在 Visual Studio 中，建立名為的新 Windows 主控台應用程式**HttpClientSample**並貼上下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="06c43-128">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="06c43-129">上述程式碼是完整的用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="06c43-129">The preceding code is the complete client app.</span></span>

<span data-ttu-id="06c43-130">`RunAsync` 執行，並封鎖，直到它完成。</span><span class="sxs-lookup"><span data-stu-id="06c43-130">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="06c43-131">大部分**HttpClient**方法都是非同步，因為它們執行網路 I/O。</span><span class="sxs-lookup"><span data-stu-id="06c43-131">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="06c43-132">所有的非同步工作完成內`RunAsync`。</span><span class="sxs-lookup"><span data-stu-id="06c43-132">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="06c43-133">一般應用程式不會封鎖主執行緒，但此應用程式不允許任何互動。</span><span class="sxs-lookup"><span data-stu-id="06c43-133">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="06c43-134">安裝 Web API 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="06c43-134">Install the Web API Client Libraries</span></span>

<span data-ttu-id="06c43-135">使用 NuGet 套件管理員來安裝 Web API 的用戶端程式庫套件。</span><span class="sxs-lookup"><span data-stu-id="06c43-135">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="06c43-136">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="06c43-136">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="06c43-137">在 封裝管理員主控台 (PMC)，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="06c43-137">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="06c43-138">上述命令會將下列 NuGet 封裝加入專案：</span><span class="sxs-lookup"><span data-stu-id="06c43-138">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="06c43-139">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="06c43-139">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="06c43-140">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="06c43-140">Newtonsoft.Json</span></span>

<span data-ttu-id="06c43-141">Json.NET 是常用的高效能 JSON framework for.NET。</span><span class="sxs-lookup"><span data-stu-id="06c43-141">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="06c43-142">將模型類別</span><span class="sxs-lookup"><span data-stu-id="06c43-142">Add a Model Class</span></span>

<span data-ttu-id="06c43-143">檢查`Product`類別：</span><span class="sxs-lookup"><span data-stu-id="06c43-143">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="06c43-144">這個類別會比對 web API 所使用的資料模型。</span><span class="sxs-lookup"><span data-stu-id="06c43-144">This class matches the data model used by the web API.</span></span> <span data-ttu-id="06c43-145">應用程式可以使用**HttpClient**讀取`Product`之 HTTP 回應中的執行個體。</span><span class="sxs-lookup"><span data-stu-id="06c43-145">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="06c43-146">應用程式不需要撰寫任何的還原序列化程式碼。</span><span class="sxs-lookup"><span data-stu-id="06c43-146">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="06c43-147">建立和初始化 HttpClient</span><span class="sxs-lookup"><span data-stu-id="06c43-147">Create and Initialize HttpClient</span></span>

<span data-ttu-id="06c43-148">檢查靜態**HttpClient**屬性：</span><span class="sxs-lookup"><span data-stu-id="06c43-148">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="06c43-149">**HttpClient**是用來具現化一次，並且在應用程式的生命週期中重複使用。</span><span class="sxs-lookup"><span data-stu-id="06c43-149">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="06c43-150">以下條件可能會導致**SocketException**錯誤：</span><span class="sxs-lookup"><span data-stu-id="06c43-150">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="06c43-151">建立新**HttpClient**每個要求的執行個體。</span><span class="sxs-lookup"><span data-stu-id="06c43-151">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="06c43-152">伺服器負載過重。</span><span class="sxs-lookup"><span data-stu-id="06c43-152">Server under heavy load.</span></span>

<span data-ttu-id="06c43-153">建立新**HttpClient**每個要求的執行個體可以耗盡可用的通訊端。</span><span class="sxs-lookup"><span data-stu-id="06c43-153">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="06c43-154">下列程式碼會初始化**HttpClient**執行個體：</span><span class="sxs-lookup"><span data-stu-id="06c43-154">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="06c43-155">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="06c43-155">The preceding code:</span></span>

* <span data-ttu-id="06c43-156">設定 HTTP 要求的基底 URI。</span><span class="sxs-lookup"><span data-stu-id="06c43-156">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="06c43-157">將連接埠號碼變更為伺服器應用程式中使用的通訊埠。</span><span class="sxs-lookup"><span data-stu-id="06c43-157">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="06c43-158">應用程式將無法運作，除非連接埠為伺服器應用程式。</span><span class="sxs-lookup"><span data-stu-id="06c43-158">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="06c43-159">設定為"application/json"Accept 標頭。</span><span class="sxs-lookup"><span data-stu-id="06c43-159">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="06c43-160">設定此標頭會要求伺服器傳送 JSON 格式的資料。</span><span class="sxs-lookup"><span data-stu-id="06c43-160">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="06c43-161">傳送 GET 要求來擷取資源</span><span class="sxs-lookup"><span data-stu-id="06c43-161">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="06c43-162">下列程式碼會傳送 GET 要求的產品：</span><span class="sxs-lookup"><span data-stu-id="06c43-162">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="06c43-163">**GetAsync**方法會傳送 HTTP GET 要求。</span><span class="sxs-lookup"><span data-stu-id="06c43-163">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="06c43-164">此方法完成時，它會傳回**HttpResponseMessage** ，其中包含 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="06c43-164">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="06c43-165">在回應中的狀態碼是否成功的程式碼，回應主體會包含產品的 JSON 表示法。</span><span class="sxs-lookup"><span data-stu-id="06c43-165">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="06c43-166">呼叫**ReadAsAsync**還原序列化 JSON 裝載`Product`執行個體。</span><span class="sxs-lookup"><span data-stu-id="06c43-166">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="06c43-167">**ReadAsAsync**方法是非同步的因為回應主體可以是任意大。</span><span class="sxs-lookup"><span data-stu-id="06c43-167">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="06c43-168">**HttpClient** HTTP 回應包含錯誤程式碼時，不會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="06c43-168">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="06c43-169">相反地， **IsSuccessStatusCode**屬性是**false**如果狀態為錯誤碼。</span><span class="sxs-lookup"><span data-stu-id="06c43-169">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="06c43-170">如果您想要將 HTTP 錯誤碼都視為例外狀況，呼叫[HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx)回應物件上。</span><span class="sxs-lookup"><span data-stu-id="06c43-170">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="06c43-171">`EnsureSuccessStatusCode` 擲回例外狀況，如果狀態碼超出範圍 200&ndash;299。</span><span class="sxs-lookup"><span data-stu-id="06c43-171">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="06c43-172">請注意， **HttpClient**可以擲回例外狀況，因為其他原因而&mdash;比方說，如果要求逾時。</span><span class="sxs-lookup"><span data-stu-id="06c43-172">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="06c43-173">若要還原序列化的媒體類型格式器</span><span class="sxs-lookup"><span data-stu-id="06c43-173">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="06c43-174">當**ReadAsAsync**呼叫不含任何參數，它會使用一組預設的*媒體格式器*讀取回應主體。</span><span class="sxs-lookup"><span data-stu-id="06c43-174">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="06c43-175">預設格式器支援 JSON、 XML 和表單 url 編碼資料。</span><span class="sxs-lookup"><span data-stu-id="06c43-175">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="06c43-176">不使用預設格式器，您可以提供的格式器清單**ReadAsAsync**方法。</span><span class="sxs-lookup"><span data-stu-id="06c43-176">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="06c43-177">使用格式器清單很有用，如果您有自訂的媒體類型格式器：</span><span class="sxs-lookup"><span data-stu-id="06c43-177">Using a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="06c43-178">如需詳細資訊，請參閱[ASP.NET Web API 2 中的媒體格式器](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="06c43-178">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="06c43-179">傳送 POST 要求建立資源</span><span class="sxs-lookup"><span data-stu-id="06c43-179">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="06c43-180">下列程式碼傳送 POST 要求，其中包含`Product`JSON 格式的執行個體：</span><span class="sxs-lookup"><span data-stu-id="06c43-180">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="06c43-181">**PostAsJsonAsync**方法：</span><span class="sxs-lookup"><span data-stu-id="06c43-181">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="06c43-182">將物件序列化為 JSON。</span><span class="sxs-lookup"><span data-stu-id="06c43-182">Serializes an object to JSON.</span></span>
* <span data-ttu-id="06c43-183">將 JSON 承載傳送 POST 要求中。</span><span class="sxs-lookup"><span data-stu-id="06c43-183">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="06c43-184">如果要求成功：</span><span class="sxs-lookup"><span data-stu-id="06c43-184">If the request succeeds:</span></span>

* <span data-ttu-id="06c43-185">它應該會傳回 201 （已建立） 回應。</span><span class="sxs-lookup"><span data-stu-id="06c43-185">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="06c43-186">回應應包含建立資源的 URL 位置標頭。</span><span class="sxs-lookup"><span data-stu-id="06c43-186">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="06c43-187">傳送 PUT 的要求來更新資源</span><span class="sxs-lookup"><span data-stu-id="06c43-187">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="06c43-188">下列程式碼會傳送 PUT 要求，將產品更新：</span><span class="sxs-lookup"><span data-stu-id="06c43-188">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="06c43-189">**PutAsJsonAsync**方法的運作方式類似**PostAsJsonAsync**，只不過它會傳送 PUT 要求，而不是 POST。</span><span class="sxs-lookup"><span data-stu-id="06c43-189">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="06c43-190">刪除要求傳送到刪除資源</span><span class="sxs-lookup"><span data-stu-id="06c43-190">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="06c43-191">下列程式碼會傳送 DELETE 要求來刪除產品：</span><span class="sxs-lookup"><span data-stu-id="06c43-191">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="06c43-192">例如 GET、 DELETE 要求並沒有要求主體。</span><span class="sxs-lookup"><span data-stu-id="06c43-192">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="06c43-193">您不需要與 DELETE 一起指定 JSON 或 XML 格式。</span><span class="sxs-lookup"><span data-stu-id="06c43-193">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="06c43-194">測試範例</span><span class="sxs-lookup"><span data-stu-id="06c43-194">Test the sample</span></span>

<span data-ttu-id="06c43-195">若要測試用戶端應用程式：</span><span class="sxs-lookup"><span data-stu-id="06c43-195">To test the client app:</span></span>

1. <span data-ttu-id="06c43-196">[下載](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server)並執行伺服器應用程式。</span><span class="sxs-lookup"><span data-stu-id="06c43-196">[Download](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="06c43-197">[下載指示](/aspnet/core/tutorials/#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="06c43-197">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> <span data-ttu-id="06c43-198">請確認伺服器應用程式運作。</span><span class="sxs-lookup"><span data-stu-id="06c43-198">Verify the server app is working.</span></span> <span data-ttu-id="06c43-199">Exaxmple，如`http://localhost:64195/api/products`應該會傳回一份產品清單。</span><span class="sxs-lookup"><span data-stu-id="06c43-199">For exaxmple, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="06c43-200">設定 HTTP 要求的基底 URI。</span><span class="sxs-lookup"><span data-stu-id="06c43-200">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="06c43-201">將連接埠號碼變更為伺服器應用程式中使用的通訊埠。</span><span class="sxs-lookup"><span data-stu-id="06c43-201">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="06c43-202">執行用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="06c43-202">Run the client app.</span></span> <span data-ttu-id="06c43-203">會產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="06c43-203">The following output is produced:</span></span>

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
