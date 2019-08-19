---
title: 從 ASP.NET Core Blazor 呼叫 Web API
author: guardrex
description: 瞭解如何使用 JSON helper 從 Blazor 應用程式呼叫 Web API, 包括建立跨原始來源資源分享 (CORS) 要求。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/call-web-api
ms.openlocfilehash: 60ebd01bc07da22cd1dcd0b16297ee54c97867fc
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030388"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a><span data-ttu-id="25aa0-103">從 ASP.NET Core Blazor 呼叫 Web API</span><span class="sxs-lookup"><span data-stu-id="25aa0-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="25aa0-104">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="25aa0-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="25aa0-105">Blazor 用戶端應用程式會使用預先`HttpClient`設定的服務來呼叫 web api。</span><span class="sxs-lookup"><span data-stu-id="25aa0-105">Blazor client-side apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="25aa0-106">撰寫要求, 其中可以包含 JavaScript[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)選項、使用 Blazor JSON helper 或<xref:System.Net.Http.HttpRequestMessage>搭配。</span><span class="sxs-lookup"><span data-stu-id="25aa0-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span>

<span data-ttu-id="25aa0-107">Blazor 伺服器端應用程式會使用通常使用<xref:System.Net.Http.HttpClient> <xref:System.Net.Http.IHttpClientFactory>建立的實例來呼叫 web api。</span><span class="sxs-lookup"><span data-stu-id="25aa0-107">Blazor server-side apps call web APIs using <xref:System.Net.Http.HttpClient> instances typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="25aa0-108">如需詳細資訊，請參閱 <xref:fundamentals/http-requests>。</span><span class="sxs-lookup"><span data-stu-id="25aa0-108">For more information, see <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="25aa0-109">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="25aa0-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="25aa0-110">如需 Blazor 用戶端範例, 請參閱範例應用程式中的下列元件:</span><span class="sxs-lookup"><span data-stu-id="25aa0-110">For Blazor client-side examples, see the following components in the sample app:</span></span>

* <span data-ttu-id="25aa0-111">呼叫 Web API (*Pages/CallWebAPI. razor*)</span><span class="sxs-lookup"><span data-stu-id="25aa0-111">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="25aa0-112">HTTP 要求測試器 (*元件/HTTPRequestTester razor*)</span><span class="sxs-lookup"><span data-stu-id="25aa0-112">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="25aa0-113">HttpClient 和 JSON 協助程式</span><span class="sxs-lookup"><span data-stu-id="25aa0-113">HttpClient and JSON helpers</span></span>

<span data-ttu-id="25aa0-114">在 Blazor 用戶端應用程式中, [HttpClient](xref:fundamentals/http-requests)會當做預先設定的服務提供, 以便向源伺服器提出要求。</span><span class="sxs-lookup"><span data-stu-id="25aa0-114">In Blazor client-side apps, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span> <span data-ttu-id="25aa0-115">若要`HttpClient`使用 JSON helper, 請將套件參考`Microsoft.AspNetCore.Blazor.HttpClient`新增至。</span><span class="sxs-lookup"><span data-stu-id="25aa0-115">To use `HttpClient` JSON helpers, add a package reference to `Microsoft.AspNetCore.Blazor.HttpClient`.</span></span> <span data-ttu-id="25aa0-116">`HttpClient`和 JSON 協助程式也用來呼叫協力廠商 Web API 端點。</span><span class="sxs-lookup"><span data-stu-id="25aa0-116">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="25aa0-117">`HttpClient`會使用瀏覽器[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)來執行, 並受限於其限制, 包括強制執行相同的來源原則。</span><span class="sxs-lookup"><span data-stu-id="25aa0-117">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="25aa0-118">用戶端的基底位址會設定為源伺服器的位址。</span><span class="sxs-lookup"><span data-stu-id="25aa0-118">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="25aa0-119">使用指示詞插入`HttpClient`實例 `@inject` :</span><span class="sxs-lookup"><span data-stu-id="25aa0-119">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="25aa0-120">在下列範例中, Todo Web API 處理建立、讀取、更新和刪除 (CRUD) 作業。</span><span class="sxs-lookup"><span data-stu-id="25aa0-120">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="25aa0-121">這些範例是以儲存的`TodoItem`類別為基礎:</span><span class="sxs-lookup"><span data-stu-id="25aa0-121">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="25aa0-122">專案的`Id`識別碼`long`( &ndash; ,) 唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="25aa0-122">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="25aa0-123">專案的`Name`名稱`string`( &ndash; ,) 名稱。</span><span class="sxs-lookup"><span data-stu-id="25aa0-123">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="25aa0-124">Status (`IsComplete`, `bool`) &ndash;表示待辦事項是否已完成。</span><span class="sxs-lookup"><span data-stu-id="25aa0-124">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="25aa0-125">JSON helper 方法會將要求傳送至 URI (下列範例中的 Web API) 並處理回應:</span><span class="sxs-lookup"><span data-stu-id="25aa0-125">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="25aa0-126">`GetJsonAsync`&ndash;傳送 HTTP GET 要求, 並剖析 JSON 回應主體以建立物件。</span><span class="sxs-lookup"><span data-stu-id="25aa0-126">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="25aa0-127">在下列程式碼中, `_todoItems`元件會顯示。</span><span class="sxs-lookup"><span data-stu-id="25aa0-127">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="25aa0-128">當`GetTodoItems`元件完成呈現 ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)) 時, 就會觸發方法。</span><span class="sxs-lookup"><span data-stu-id="25aa0-128">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)).</span></span> <span data-ttu-id="25aa0-129">如需完整範例, 請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="25aa0-129">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/todo");
  }
  ```

* <span data-ttu-id="25aa0-130">`PostJsonAsync`&ndash;傳送 HTTP POST 要求, 包括 json 編碼的內容, 並剖析 json 回應主體以建立物件。</span><span class="sxs-lookup"><span data-stu-id="25aa0-130">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="25aa0-131">在下列程式碼中`_newItemName` , 是由元件的繫結項目所提供。</span><span class="sxs-lookup"><span data-stu-id="25aa0-131">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="25aa0-132">方法是藉由`<button>`選取元素來觸發。 `AddItem`</span><span class="sxs-lookup"><span data-stu-id="25aa0-132">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="25aa0-133">如需完整範例, 請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="25aa0-133">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  <input @bind="_newItemName" placeholder="New Todo Item" />
  <button @onclick="@AddItem">Add</button>

  @code {
      private string _newItemName;

      private async Task AddItem()
      {
          var addItem = new TodoItem { Name = _newItemName, IsComplete = false };
          await Http.PostJsonAsync("api/todo", addItem);
      }
  }
  ```

* <span data-ttu-id="25aa0-134">`PutJsonAsync`&ndash;傳送 HTTP PUT 要求, 包括 JSON 編碼的內容。</span><span class="sxs-lookup"><span data-stu-id="25aa0-134">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="25aa0-135">在下列程式碼中`_editItem` , `Name`和`IsCompleted`的值是由元件的繫結項目所提供。</span><span class="sxs-lookup"><span data-stu-id="25aa0-135">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="25aa0-136">當專案在`Id` UI 的另一個部分中選取並`EditItem`呼叫時, 會設定專案的。</span><span class="sxs-lookup"><span data-stu-id="25aa0-136">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="25aa0-137">方法是藉由選取 Save `<button>`元素來觸發。 `SaveItem`</span><span class="sxs-lookup"><span data-stu-id="25aa0-137">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="25aa0-138">如需完整範例, 請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="25aa0-138">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  <input type="checkbox" @bind="_editItem.IsComplete" />
  <input @bind="_editItem.Name" />
  <button @onclick="@SaveItem">Save</button>

  @code {
      private TodoItem _editItem = new TodoItem();

      private void EditItem(long id)
      {
          var editItem = _todoItems.Single(i => i.Id == id);
          _editItem = new TodoItem { Id = editItem.Id, Name = editItem.Name, 
              IsComplete = editItem.IsComplete };
      }

      private async Task SaveItem() =>
          await Http.PutJsonAsync($"api/todo/{_editItem.Id}, _editItem);
  }
  ```

<span data-ttu-id="25aa0-139"><xref:System.Net.Http>包含用來傳送 HTTP 要求和接收 HTTP 回應的其他擴充方法。</span><span class="sxs-lookup"><span data-stu-id="25aa0-139"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="25aa0-140">[HttpClient](xref:System.Net.Http.HttpClient.DeleteAsync*)是用來將 HTTP DELETE 要求傳送至 Web API。</span><span class="sxs-lookup"><span data-stu-id="25aa0-140">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="25aa0-141">在下列程式碼中, Delete `<button>`元素會`DeleteItem`呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="25aa0-141">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="25aa0-142">綁定`<input>`項會`id`提供要刪除之專案的。</span><span class="sxs-lookup"><span data-stu-id="25aa0-142">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="25aa0-143">如需完整範例, 請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="25aa0-143">See the sample app for a complete example.</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http

<input @bind="_id" />
<button @onclick="@DeleteItem">Delete</button>

@code {
    private long _id;

    private async Task DeleteItem() =>
        await Http.DeleteAsync($"api/todo/{_id}");
}
```

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="25aa0-144">跨原始來源資源分享 (CORS)</span><span class="sxs-lookup"><span data-stu-id="25aa0-144">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="25aa0-145">瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。</span><span class="sxs-lookup"><span data-stu-id="25aa0-145">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="25aa0-146">這種限制稱為「*相同來源原則*」。</span><span class="sxs-lookup"><span data-stu-id="25aa0-146">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="25aa0-147">相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="25aa0-147">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="25aa0-148">若要將來自瀏覽器的要求傳送至具有不同來源的端點,*端點*必須啟用[跨原始來源資源分享 (CORS)](https://www.w3.org/TR/cors/)。</span><span class="sxs-lookup"><span data-stu-id="25aa0-148">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="25aa0-149">範例應用程式會示範如何在呼叫 Web API 元件 (*Pages/CallWebAPI razor*) 中使用 CORS。</span><span class="sxs-lookup"><span data-stu-id="25aa0-149">The sample app demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="25aa0-150">若要允許其他網站對您的應用程式進行跨原始來源資源分享 (CORS) 要求<xref:security/cors>, 請參閱。</span><span class="sxs-lookup"><span data-stu-id="25aa0-150">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="25aa0-151">具有 Fetch API 要求選項的 HttpClient 和 HttpRequestMessage</span><span class="sxs-lookup"><span data-stu-id="25aa0-151">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="25aa0-152">在 Blazor 用戶端應用程式的 WebAssembly 上執行時, 請 [HttpClient](xref:fundamentals/http-requests) 使用 <xref:System.Net.Http.HttpRequestMessage>和來自訂要求。</span><span class="sxs-lookup"><span data-stu-id="25aa0-152">When running on WebAssembly in a Blazor client-side app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="25aa0-153">例如, 您可以指定要求 URI、HTTP 方法, 以及任何所需的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="25aa0-153">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

<span data-ttu-id="25aa0-154">使用要求上的`WebAssemblyHttpMessageHandler.FetchArgs`屬性, 將要求選項提供給基礎 JavaScript[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API) 。</span><span class="sxs-lookup"><span data-stu-id="25aa0-154">Supply request options to the underlying JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) using the `WebAssemblyHttpMessageHandler.FetchArgs` property on the request.</span></span> <span data-ttu-id="25aa0-155">如下列範例所示, `credentials`屬性會設定為下列任何值:</span><span class="sxs-lookup"><span data-stu-id="25aa0-155">As shown in the following example, the `credentials` property is set to any of the following values:</span></span>

* <span data-ttu-id="25aa0-156">`FetchCredentialsOption.Include`(「包含」)&ndash;建議瀏覽器傳送認證 (例如 cookie 或 HTTP 驗證標頭), 即使是跨原始來源要求也一樣。</span><span class="sxs-lookup"><span data-stu-id="25aa0-156">`FetchCredentialsOption.Include` ("include") &ndash; Advises the browser to send credentials (such as cookies or HTTP authentication headers) even for cross-origin requests.</span></span> <span data-ttu-id="25aa0-157">只有在 CORS 原則設定為允許認證時才允許。</span><span class="sxs-lookup"><span data-stu-id="25aa0-157">Only allowed when the CORS policy is configured to allow credentials.</span></span>
* <span data-ttu-id="25aa0-158">`FetchCredentialsOption.Omit`(「省略」)&ndash;建議瀏覽器永遠不會傳送認證 (例如 cookie 或 HTTP 驗證標頭)。</span><span class="sxs-lookup"><span data-stu-id="25aa0-158">`FetchCredentialsOption.Omit` ("omit") &ndash; Advises the browser never to send credentials (such as cookies or HTTP auth headers).</span></span>
* <span data-ttu-id="25aa0-159">`FetchCredentialsOption.SameOrigin`(「相同來源」)&ndash;只有當目標 URL 與呼叫應用程式位於相同的來源時, 才會建議瀏覽器傳送認證 (例如 cookie 或 HTTP 驗證標頭)。</span><span class="sxs-lookup"><span data-stu-id="25aa0-159">`FetchCredentialsOption.SameOrigin` ("same-origin") &ndash; Advises the browser to send credentials (such as cookies or HTTP auth headers) only if the target URL is on the same origin as the calling application.</span></span>

```cshtml
@using System.Net.Http
@using System.Net.Http.Headers
@inject HttpClient Http

@code {
    private async Task PostRequest()
    {
        Http.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", "{OAUTH TOKEN}");

        var requestMessage = new HttpRequestMessage()
        {
            Method = new HttpMethod("POST"),
            RequestUri = new Uri("https://localhost:10000/api/todo"),
            Content = 
                new StringContent(
                    @"{""name"":""A New Todo Item"",""isComplete"":false}")
        };

        requestMessage.Content.Headers.ContentType = 
            new System.Net.Http.Headers.MediaTypeHeaderValue(
                "application/json");

        requestMessage.Content.Headers.TryAddWithoutValidation(
            "x-custom-header", "value");
        
        requestMessage.Properties[WebAssemblyHttpMessageHandler.FetchArgs] = new
        { 
            credentials = FetchCredentialsOption.Include
        };

        var response = await Http.SendAsync(requestMessage);
        var responseStatusCode = response.StatusCode;
        var responseBody = await response.Content.ReadAsStringAsync();
    }
}
```

<span data-ttu-id="25aa0-160">如需有關 Fetch API 選項的詳細資訊[, 請參閱 MDN web 檔:WindowOrWorkerGlobalScope ():P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)。</span><span class="sxs-lookup"><span data-stu-id="25aa0-160">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="25aa0-161">在 cors 要求上傳送認證 (授權 cookie/標頭) 時`Authorization` , cors 原則必須允許標頭。</span><span class="sxs-lookup"><span data-stu-id="25aa0-161">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="25aa0-162">下列原則包含的設定:</span><span class="sxs-lookup"><span data-stu-id="25aa0-162">The following policy includes configuration for:</span></span>

* <span data-ttu-id="25aa0-163">要求來源 (`http://localhost:5000`、 `https://localhost:5001`)。</span><span class="sxs-lookup"><span data-stu-id="25aa0-163">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="25aa0-164">Any 方法 (動詞)。</span><span class="sxs-lookup"><span data-stu-id="25aa0-164">Any method (verb).</span></span>
* <span data-ttu-id="25aa0-165">`Content-Type`和`Authorization`標頭。</span><span class="sxs-lookup"><span data-stu-id="25aa0-165">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="25aa0-166">若要允許自訂標頭 ( `x-custom-header`例如), 請在呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>時列出標頭。</span><span class="sxs-lookup"><span data-stu-id="25aa0-166">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="25aa0-167">用戶端 JavaScript 程式碼 (`credentials`屬性設為`include`) 所設定的認證。</span><span class="sxs-lookup"><span data-stu-id="25aa0-167">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="25aa0-168">如需詳細資訊, <xref:security/cors>請參閱和範例應用程式的 HTTP 要求測試器元件 (*Components/HTTPRequestTester*)。</span><span class="sxs-lookup"><span data-stu-id="25aa0-168">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="25aa0-169">其他資源</span><span class="sxs-lookup"><span data-stu-id="25aa0-169">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* [<span data-ttu-id="25aa0-170">位於 W3C 的跨原始來源資源分享 (CORS)</span><span class="sxs-lookup"><span data-stu-id="25aa0-170">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
