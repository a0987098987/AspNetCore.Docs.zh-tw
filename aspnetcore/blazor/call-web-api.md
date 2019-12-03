---
title: 從 ASP.NET Core 呼叫 Web API Blazor
author: guardrex
description: 瞭解如何使用 JSON helper 從 Blazor 應用程式呼叫 Web API，包括建立跨原始來源資源分享（CORS）要求。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2019
no-loc:
- Blazor
uid: blazor/call-web-api
ms.openlocfilehash: d4c69e8be2d4f6295c7177bf5d00aed596d0ead2
ms.sourcegitcommit: 169ea5116de729c803685725d96450a270bc55b7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2019
ms.locfileid: "74733852"
---
# <a name="call-a-web-api-from-aspnet-core-opno-locblazor"></a><span data-ttu-id="3b06e-103">從 ASP.NET Core 呼叫 Web API Blazor</span><span class="sxs-lookup"><span data-stu-id="3b06e-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="3b06e-104">By [Luke Latham](https://github.com/guardrex)、 [Daniel Roth](https://github.com/danroth27)和[Juan De la Cruz](https://github.com/juandelacruz23)</span><span class="sxs-lookup"><span data-stu-id="3b06e-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Juan De la Cruz](https://github.com/juandelacruz23)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="3b06e-105">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) apps 會使用預先設定的 `HttpClient` 服務來呼叫 web api。</span><span class="sxs-lookup"><span data-stu-id="3b06e-105">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="3b06e-106">撰寫要求，其中可包含 JavaScript[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)選項、使用 Blazor JSON helper 或 <xref:System.Net.Http.HttpRequestMessage>。</span><span class="sxs-lookup"><span data-stu-id="3b06e-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span>

<span data-ttu-id="3b06e-107">[Blazor 伺服器](xref:blazor/hosting-models#blazor-server)應用程式會使用通常使用 <xref:System.Net.Http.IHttpClientFactory>建立的 <xref:System.Net.Http.HttpClient> 實例來呼叫 web api。</span><span class="sxs-lookup"><span data-stu-id="3b06e-107">[Blazor Server](xref:blazor/hosting-models#blazor-server) apps call web APIs using <xref:System.Net.Http.HttpClient> instances typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="3b06e-108">如需詳細資訊，請參閱<xref:fundamentals/http-requests>。</span><span class="sxs-lookup"><span data-stu-id="3b06e-108">For more information, see <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="3b06e-109">[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)（[如何下載](xref:index#how-to-download-a-sample)） &ndash; 選取*BlazorWebAssemblySample*應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b06e-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample)) &ndash; Select the *BlazorWebAssemblySample* app.</span></span>

<span data-ttu-id="3b06e-110">請參閱*BlazorWebAssemblySample*範例應用程式中的下列元件：</span><span class="sxs-lookup"><span data-stu-id="3b06e-110">See the following components in the *BlazorWebAssemblySample* sample app:</span></span>

* <span data-ttu-id="3b06e-111">呼叫 Web API （*Pages/CallWebAPI. razor*）</span><span class="sxs-lookup"><span data-stu-id="3b06e-111">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="3b06e-112">HTTP 要求測試器（*元件/HTTPRequestTester razor*）</span><span class="sxs-lookup"><span data-stu-id="3b06e-112">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="packages"></a><span data-ttu-id="3b06e-113">package</span><span class="sxs-lookup"><span data-stu-id="3b06e-113">Packages</span></span>

<span data-ttu-id="3b06e-114">參考*實驗*性[AspNetCore.Blazor。HttpClient](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/)專案檔中的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="3b06e-114">Reference the *experimental* [Microsoft.AspNetCore.Blazor.HttpClient](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/) NuGet package in the project file.</span></span> <span data-ttu-id="3b06e-115">`Microsoft.AspNetCore.Blazor.HttpClient` 是以 `HttpClient` 和[system.web](https://www.nuget.org/packages/System.Text.Json/)為基礎。</span><span class="sxs-lookup"><span data-stu-id="3b06e-115">`Microsoft.AspNetCore.Blazor.HttpClient` is based on `HttpClient` and [System.Text.Json](https://www.nuget.org/packages/System.Text.Json/).</span></span>

<span data-ttu-id="3b06e-116">若要使用穩定的 API，請使用[WebApi. 用戶端](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)套件，其使用[Newtonsoft](https://www.nuget.org/packages/Newtonsoft.Json/)/[Json.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm)。</span><span class="sxs-lookup"><span data-stu-id="3b06e-116">To use a stable API, use the [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) package, which uses [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)/[Json.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span> <span data-ttu-id="3b06e-117">在 `Microsoft.AspNet.WebApi.Client` 中使用穩定 API，並不會提供本主題中所述的 JSON helper，這對實驗性 `Microsoft.AspNetCore.Blazor.HttpClient` 套件而言是唯一的。</span><span class="sxs-lookup"><span data-stu-id="3b06e-117">Using the stable API in `Microsoft.AspNet.WebApi.Client` doesn't provide the JSON helpers described in this topic, which are unique to the experimental `Microsoft.AspNetCore.Blazor.HttpClient` package.</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="3b06e-118">HttpClient 和 JSON 協助程式</span><span class="sxs-lookup"><span data-stu-id="3b06e-118">HttpClient and JSON helpers</span></span>

<span data-ttu-id="3b06e-119">在 Blazor WebAssembly 應用程式中， [HttpClient](xref:fundamentals/http-requests)是以預先設定的服務形式提供，可讓要求回到源伺服器。</span><span class="sxs-lookup"><span data-stu-id="3b06e-119">In a Blazor WebAssembly app, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span>

<span data-ttu-id="3b06e-120">Blazor 伺服器應用程式預設不包含 `HttpClient` 服務。</span><span class="sxs-lookup"><span data-stu-id="3b06e-120">A Blazor Server app doesn't include an `HttpClient` service by default.</span></span> <span data-ttu-id="3b06e-121">使用[HttpClient factory 基礎結構](xref:fundamentals/http-requests)提供應用程式的 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="3b06e-121">Provide an `HttpClient` to the app using the [HttpClient factory infrastructure](xref:fundamentals/http-requests).</span></span>

<span data-ttu-id="3b06e-122">`HttpClient` 和 JSON 協助程式也用來呼叫協力廠商 Web API 端點。</span><span class="sxs-lookup"><span data-stu-id="3b06e-122">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="3b06e-123">`HttpClient` 是使用瀏覽器[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)來執行，並受限於其限制，包括強制執行相同的來源原則。</span><span class="sxs-lookup"><span data-stu-id="3b06e-123">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="3b06e-124">用戶端的基底位址會設定為源伺服器的位址。</span><span class="sxs-lookup"><span data-stu-id="3b06e-124">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="3b06e-125">使用 `@inject` 指示詞插入 `HttpClient` 實例：</span><span class="sxs-lookup"><span data-stu-id="3b06e-125">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="3b06e-126">在下列範例中，Todo Web API 處理建立、讀取、更新和刪除（CRUD）作業。</span><span class="sxs-lookup"><span data-stu-id="3b06e-126">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="3b06e-127">這些範例是以儲存的 `TodoItem` 類別為基礎：</span><span class="sxs-lookup"><span data-stu-id="3b06e-127">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="3b06e-128">識別碼（`Id`，`long`） &ndash; 專案的唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="3b06e-128">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="3b06e-129">專案的名稱（`Name`、`string`） &ndash; 名稱。</span><span class="sxs-lookup"><span data-stu-id="3b06e-129">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="3b06e-130">如果 Todo 專案已完成，則為狀態（`IsComplete`、`bool`） &ndash; 指示。</span><span class="sxs-lookup"><span data-stu-id="3b06e-130">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="3b06e-131">JSON helper 方法會將要求傳送至 URI （下列範例中的 Web API）並處理回應：</span><span class="sxs-lookup"><span data-stu-id="3b06e-131">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="3b06e-132">`GetJsonAsync` &ndash; 傳送 HTTP GET 要求，並剖析 JSON 回應主體以建立物件。</span><span class="sxs-lookup"><span data-stu-id="3b06e-132">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="3b06e-133">在下列程式碼中，元件會顯示 `_todoItems`。</span><span class="sxs-lookup"><span data-stu-id="3b06e-133">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="3b06e-134">當元件完成呈現（[OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)）時，就會觸發 `GetTodoItems` 方法。</span><span class="sxs-lookup"><span data-stu-id="3b06e-134">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)).</span></span> <span data-ttu-id="3b06e-135">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b06e-135">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <span data-ttu-id="3b06e-136">`PostJsonAsync` &ndash; 傳送 HTTP POST 要求，包括 JSON 編碼的內容，並剖析 JSON 回應主體以建立物件。</span><span class="sxs-lookup"><span data-stu-id="3b06e-136">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="3b06e-137">在下列程式碼中，`_newItemName` 是由元件的繫結項目所提供。</span><span class="sxs-lookup"><span data-stu-id="3b06e-137">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="3b06e-138">`AddItem` 方法是藉由選取 `<button>` 元素來觸發。</span><span class="sxs-lookup"><span data-stu-id="3b06e-138">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="3b06e-139">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b06e-139">See the sample app for a complete example.</span></span>

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
          await Http.PostJsonAsync("api/TodoItems", addItem);
      }
  }
  ```

* <span data-ttu-id="3b06e-140">`PutJsonAsync` &ndash; 傳送 HTTP PUT 要求，包括 JSON 編碼的內容。</span><span class="sxs-lookup"><span data-stu-id="3b06e-140">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="3b06e-141">在下列程式碼中，`Name` 和 `IsCompleted` 的 `_editItem` 值是由元件的繫結項目所提供。</span><span class="sxs-lookup"><span data-stu-id="3b06e-141">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="3b06e-142">當專案在 UI 的另一個部分中選取，且呼叫 `EditItem` 時，會設定專案的 `Id`。</span><span class="sxs-lookup"><span data-stu-id="3b06e-142">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="3b06e-143">藉由選取 [儲存 `<button>`] 元素，即可觸發 `SaveItem` 方法。</span><span class="sxs-lookup"><span data-stu-id="3b06e-143">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="3b06e-144">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b06e-144">See the sample app for a complete example.</span></span>

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
          await Http.PutJsonAsync($"api/TodoItems/{_editItem.Id}, _editItem);
  }
  ```

<span data-ttu-id="3b06e-145"><xref:System.Net.Http> 包含傳送 HTTP 要求和接收 HTTP 回應的其他擴充方法。</span><span class="sxs-lookup"><span data-stu-id="3b06e-145"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="3b06e-146">[HttpClient](xref:System.Net.Http.HttpClient.DeleteAsync*)是用來將 HTTP DELETE 要求傳送至 Web API。</span><span class="sxs-lookup"><span data-stu-id="3b06e-146">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="3b06e-147">在下列程式碼中，Delete `<button>` 元素會呼叫 `DeleteItem` 方法。</span><span class="sxs-lookup"><span data-stu-id="3b06e-147">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="3b06e-148">系結的 `<input>` 元素會提供要刪除之專案的 `id`。</span><span class="sxs-lookup"><span data-stu-id="3b06e-148">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="3b06e-149">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b06e-149">See the sample app for a complete example.</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http

<input @bind="_id" />
<button @onclick="@DeleteItem">Delete</button>

@code {
    private long _id;

    private async Task DeleteItem() =>
        await Http.DeleteAsync($"api/TodoItems/{_id}");
}
```

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="3b06e-150">跨原始來源資源分享（CORS）</span><span class="sxs-lookup"><span data-stu-id="3b06e-150">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="3b06e-151">瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。</span><span class="sxs-lookup"><span data-stu-id="3b06e-151">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="3b06e-152">這種限制稱為「*相同來源原則*」。</span><span class="sxs-lookup"><span data-stu-id="3b06e-152">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="3b06e-153">相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="3b06e-153">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="3b06e-154">若要將來自瀏覽器的要求傳送至具有不同來源的端點，*端點*必須啟用[跨原始來源資源分享（CORS）](https://www.w3.org/TR/cors/)。</span><span class="sxs-lookup"><span data-stu-id="3b06e-154">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="3b06e-155">[Blazor WebAssembly 範例應用程式（BlazorWebAssemblySample）](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)示範如何在呼叫 Web API 元件（*Pages/CallWebAPI razor*）中使用 CORS。</span><span class="sxs-lookup"><span data-stu-id="3b06e-155">The [Blazor WebAssembly sample app (BlazorWebAssemblySample)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="3b06e-156">若要允許其他網站對您的應用程式進行跨原始來源資源分享（CORS）要求，請參閱 <xref:security/cors>。</span><span class="sxs-lookup"><span data-stu-id="3b06e-156">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="3b06e-157">具有 Fetch API 要求選項的 HttpClient 和 HttpRequestMessage</span><span class="sxs-lookup"><span data-stu-id="3b06e-157">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="3b06e-158">在 Blazor WebAssembly 應用程式的 WebAssembly 上執行時，請使用[HttpClient](xref:fundamentals/http-requests)和 <xref:System.Net.Http.HttpRequestMessage> 來自訂要求。</span><span class="sxs-lookup"><span data-stu-id="3b06e-158">When running on WebAssembly in a Blazor WebAssembly app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="3b06e-159">例如，您可以指定要求 URI、HTTP 方法，以及任何所需的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="3b06e-159">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

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
            RequestUri = new Uri("https://localhost:10000/api/TodoItems"),
            Content = 
                new StringContent(
                    @"{""name"":""A New Todo Item"",""isComplete"":false}")
        };

        requestMessage.Content.Headers.ContentType = 
            new System.Net.Http.Headers.MediaTypeHeaderValue(
                "application/json");

        requestMessage.Content.Headers.TryAddWithoutValidation(
            "x-custom-header", "value");

        var response = await Http.SendAsync(requestMessage);
        var responseStatusCode = response.StatusCode;
        var responseBody = await response.Content.ReadAsStringAsync();
    }
}
```

<span data-ttu-id="3b06e-160">如需有關提取 API 選項的詳細資訊，請參閱[MDN web 檔： WindowOrWorkerGlobalScope。 Fetch （）:P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)。</span><span class="sxs-lookup"><span data-stu-id="3b06e-160">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="3b06e-161">在 CORS 要求上傳送認證（授權 cookie/標頭）時，CORS 原則必須允許 `Authorization` 標頭。</span><span class="sxs-lookup"><span data-stu-id="3b06e-161">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="3b06e-162">下列原則包含的設定：</span><span class="sxs-lookup"><span data-stu-id="3b06e-162">The following policy includes configuration for:</span></span>

* <span data-ttu-id="3b06e-163">要求來源（`http://localhost:5000`、`https://localhost:5001`）。</span><span class="sxs-lookup"><span data-stu-id="3b06e-163">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="3b06e-164">Any 方法（動詞）。</span><span class="sxs-lookup"><span data-stu-id="3b06e-164">Any method (verb).</span></span>
* <span data-ttu-id="3b06e-165">`Content-Type` 和 `Authorization` 標頭。</span><span class="sxs-lookup"><span data-stu-id="3b06e-165">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="3b06e-166">若要允許自訂標頭（例如 `x-custom-header`），請在呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>時列出標頭。</span><span class="sxs-lookup"><span data-stu-id="3b06e-166">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="3b06e-167">用戶端 JavaScript 程式碼（`credentials` 屬性設定為 `include`）所設定的認證。</span><span class="sxs-lookup"><span data-stu-id="3b06e-167">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="3b06e-168">如需詳細資訊，請參閱 <xref:security/cors> 和範例應用程式的 HTTP 要求測試器元件（*Components/HTTPRequestTester*）。</span><span class="sxs-lookup"><span data-stu-id="3b06e-168">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3b06e-169">其他資源</span><span class="sxs-lookup"><span data-stu-id="3b06e-169">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [<span data-ttu-id="3b06e-170">Kestrel HTTPS 端點設定</span><span class="sxs-lookup"><span data-stu-id="3b06e-170">Kestrel HTTPS endpoint configuration</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [<span data-ttu-id="3b06e-171">位於 W3C 的跨原始來源資源分享（CORS）</span><span class="sxs-lookup"><span data-stu-id="3b06e-171">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
