---
title: 從 ASP.NET Core Blazor WebAssembly 呼叫 Web API
author: guardrex
description: 瞭解如何使用 JSON helper 從Blazor WebAssembly 應用程式呼叫 Web API，包括建立跨原始來源資源分享（CORS）要求。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/23/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-web-api
ms.openlocfilehash: abc546cc0079a01e3999b2c7c083235d3fff9b06
ms.sourcegitcommit: e94ecfae6a3ef568fa197da791c8bc595917d17a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2020
ms.locfileid: "82122224"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a><span data-ttu-id="8070e-103">從 ASP.NET Core Blazor 呼叫 Web API</span><span class="sxs-lookup"><span data-stu-id="8070e-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="8070e-104">By [Luke Latham](https://github.com/guardrex)、 [Daniel Roth](https://github.com/danroth27)和[Juan De la Cruz](https://github.com/juandelacruz23)</span><span class="sxs-lookup"><span data-stu-id="8070e-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Juan De la Cruz](https://github.com/juandelacruz23)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="8070e-105">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) apps 會使用預先`HttpClient`設定的服務來呼叫 web api。</span><span class="sxs-lookup"><span data-stu-id="8070e-105">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="8070e-106">撰寫要求，其中可以包含 JavaScript[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)選項、使用 Blazor JSON helper 或搭配<xref:System.Net.Http.HttpRequestMessage>。</span><span class="sxs-lookup"><span data-stu-id="8070e-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span> <span data-ttu-id="8070e-107">Blazor `HttpClient` WebAssembly apps 中的服務著重于向原始伺服器提出要求。</span><span class="sxs-lookup"><span data-stu-id="8070e-107">The `HttpClient` service in Blazor WebAssembly apps is focused on making requests back to the server of origin.</span></span> <span data-ttu-id="8070e-108">本主題中的指導方針僅適用于 Blazor WebAssembly apps。</span><span class="sxs-lookup"><span data-stu-id="8070e-108">The guidance in this topic only pertains to Blazor WebAssembly apps.</span></span>

<span data-ttu-id="8070e-109">[Blazor 伺服器](xref:blazor/hosting-models#blazor-server)應用程式會使用<xref:System.Net.Http.HttpClient>實例呼叫 web api，通常<xref:System.Net.Http.IHttpClientFactory>是使用來建立。</span><span class="sxs-lookup"><span data-stu-id="8070e-109">[Blazor Server](xref:blazor/hosting-models#blazor-server) apps call web APIs using <xref:System.Net.Http.HttpClient> instances, typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="8070e-110">本主題中的指導方針與 Blazor 伺服器應用程式無關。</span><span class="sxs-lookup"><span data-stu-id="8070e-110">The guidance in this topic doesn't pertain to Blazor Server apps.</span></span> <span data-ttu-id="8070e-111">開發 Blazor 伺服器應用程式時，請遵循中<xref:fundamentals/http-requests>的指導方針。</span><span class="sxs-lookup"><span data-stu-id="8070e-111">When developing Blazor Server apps, follow the guidance in <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="8070e-112">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)（[如何下載](xref:index#how-to-download-a-sample)） &ndash;選取*BlazorWebAssemblySample*應用程式。</span><span class="sxs-lookup"><span data-stu-id="8070e-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample)) &ndash; Select the *BlazorWebAssemblySample* app.</span></span>

<span data-ttu-id="8070e-113">請參閱*BlazorWebAssemblySample*範例應用程式中的下列元件：</span><span class="sxs-lookup"><span data-stu-id="8070e-113">See the following components in the *BlazorWebAssemblySample* sample app:</span></span>

* <span data-ttu-id="8070e-114">呼叫 Web API （*Pages/CallWebAPI. razor*）</span><span class="sxs-lookup"><span data-stu-id="8070e-114">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="8070e-115">HTTP 要求測試器（*元件/HTTPRequestTester razor*）</span><span class="sxs-lookup"><span data-stu-id="8070e-115">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="packages"></a><span data-ttu-id="8070e-116">Packages</span><span class="sxs-lookup"><span data-stu-id="8070e-116">Packages</span></span>

<span data-ttu-id="8070e-117">在專案檔中參考[系統 .net. Http. Json](https://www.nuget.org/packages/System.Net.Http.Json/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="8070e-117">Reference the [System.Net.Http.Json](https://www.nuget.org/packages/System.Net.Http.Json/) NuGet package in the project file.</span></span>

## <a name="add-the-httpclient-service"></a><span data-ttu-id="8070e-118">新增 HttpClient 服務</span><span class="sxs-lookup"><span data-stu-id="8070e-118">Add the HttpClient service</span></span>

<span data-ttu-id="8070e-119">在`Program.Main`中，新增`HttpClient`服務（如果尚未存在）：</span><span class="sxs-lookup"><span data-stu-id="8070e-119">In `Program.Main`, add an `HttpClient` service if it doesn't already exist:</span></span>

```csharp
builder.Services.AddSingleton(
    new HttpClient
    {
        BaseAddress = new Uri(builder.HostEnvironment.BaseAddress)
    });
```

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="8070e-120">HttpClient 和 JSON 協助程式</span><span class="sxs-lookup"><span data-stu-id="8070e-120">HttpClient and JSON helpers</span></span>

<span data-ttu-id="8070e-121">在 Blazor WebAssembly 應用程式中， [HttpClient](xref:fundamentals/http-requests)會當做預先設定的服務提供，以便向源伺服器提出要求。</span><span class="sxs-lookup"><span data-stu-id="8070e-121">In a Blazor WebAssembly app, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span>

<span data-ttu-id="8070e-122">Blazor 伺服器應用程式預設不會`HttpClient`包含服務。</span><span class="sxs-lookup"><span data-stu-id="8070e-122">A Blazor Server app doesn't include an `HttpClient` service by default.</span></span> <span data-ttu-id="8070e-123">使用`HttpClient` [HttpClient factory 基礎結構](xref:fundamentals/http-requests)，將提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="8070e-123">Provide an `HttpClient` to the app using the [HttpClient factory infrastructure](xref:fundamentals/http-requests).</span></span>

<span data-ttu-id="8070e-124">`HttpClient`和 JSON 協助程式也用來呼叫協力廠商 Web API 端點。</span><span class="sxs-lookup"><span data-stu-id="8070e-124">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="8070e-125">`HttpClient`會使用瀏覽器[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)來執行，並受限於其限制，包括強制執行相同的來源原則。</span><span class="sxs-lookup"><span data-stu-id="8070e-125">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="8070e-126">用戶端的基底位址會設定為源伺服器的位址。</span><span class="sxs-lookup"><span data-stu-id="8070e-126">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="8070e-127">使用`@inject`指示`HttpClient`詞插入實例：</span><span class="sxs-lookup"><span data-stu-id="8070e-127">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```razor
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="8070e-128">在下列範例中，Todo Web API 處理建立、讀取、更新和刪除（CRUD）作業。</span><span class="sxs-lookup"><span data-stu-id="8070e-128">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="8070e-129">這些範例是以儲存的`TodoItem`類別為基礎：</span><span class="sxs-lookup"><span data-stu-id="8070e-129">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="8070e-130">專案的`Id`識別碼`long`（ &ndash; ，）唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="8070e-130">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="8070e-131">專案的`Name`名稱`string`（ &ndash; ，）名稱。</span><span class="sxs-lookup"><span data-stu-id="8070e-131">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="8070e-132">Status （`IsComplete`， `bool`） &ndash;表示待辦事項是否已完成。</span><span class="sxs-lookup"><span data-stu-id="8070e-132">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="8070e-133">JSON helper 方法會將要求傳送至 URI （下列範例中的 Web API）並處理回應：</span><span class="sxs-lookup"><span data-stu-id="8070e-133">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="8070e-134">`GetFromJsonAsync`&ndash;傳送 HTTP GET 要求，並剖析 JSON 回應主體以建立物件。</span><span class="sxs-lookup"><span data-stu-id="8070e-134">`GetFromJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="8070e-135">在下列程式碼中， `_todoItems`元件會顯示。</span><span class="sxs-lookup"><span data-stu-id="8070e-135">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="8070e-136">當`GetTodoItems`元件完成呈現（[OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)）時，就會觸發方法。</span><span class="sxs-lookup"><span data-stu-id="8070e-136">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)).</span></span> <span data-ttu-id="8070e-137">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="8070e-137">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetFromJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <span data-ttu-id="8070e-138">`PostAsJsonAsync`&ndash;傳送 HTTP POST 要求，包括 json 編碼的內容，並剖析 json 回應主體以建立物件。</span><span class="sxs-lookup"><span data-stu-id="8070e-138">`PostAsJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="8070e-139">在下列程式碼中`_newItemName` ，是由元件的繫結項目所提供。</span><span class="sxs-lookup"><span data-stu-id="8070e-139">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="8070e-140">`AddItem`方法是藉由選取`<button>`元素來觸發。</span><span class="sxs-lookup"><span data-stu-id="8070e-140">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="8070e-141">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="8070e-141">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  <input @bind="_newItemName" placeholder="New Todo Item" />
  <button @onclick="@AddItem">Add</button>

  @code {
      private string _newItemName;

      private async Task AddItem()
      {
          var addItem = new TodoItem { Name = _newItemName, IsComplete = false };
          await Http.PostAsJsonAsync("api/TodoItems", addItem);
      }
  }
  ```
  
  <span data-ttu-id="8070e-142">`PostAsJsonAsync`呼叫會傳回<xref:System.Net.Http.HttpResponseMessage>。</span><span class="sxs-lookup"><span data-stu-id="8070e-142">Calls to `PostAsJsonAsync` return an <xref:System.Net.Http.HttpResponseMessage>.</span></span> <span data-ttu-id="8070e-143">若要從回應訊息還原序列化 JSON 內容，請使用`ReadFromJsonAsync<T>`擴充方法：</span><span class="sxs-lookup"><span data-stu-id="8070e-143">To deserialize the JSON content from the response message, use the `ReadFromJsonAsync<T>` extension method:</span></span>
  
  ```csharp
  var content = response.content.ReadFromJsonAsync<WeatherForecast>();
  ```

* <span data-ttu-id="8070e-144">`PutAsJsonAsync`&ndash;傳送 HTTP PUT 要求，包括 JSON 編碼的內容。</span><span class="sxs-lookup"><span data-stu-id="8070e-144">`PutAsJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="8070e-145">在下列程式碼中`_editItem` ， `Name`和`IsCompleted`的值是由元件的繫結項目所提供。</span><span class="sxs-lookup"><span data-stu-id="8070e-145">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="8070e-146">當專案在`Id` UI 的另一個部分中選取並`EditItem`呼叫時，會設定專案的。</span><span class="sxs-lookup"><span data-stu-id="8070e-146">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="8070e-147">`SaveItem`方法是藉由選取 Save `<button>`元素來觸發。</span><span class="sxs-lookup"><span data-stu-id="8070e-147">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="8070e-148">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="8070e-148">See the sample app for a complete example.</span></span>

  ```razor
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
          await Http.PutAsJsonAsync($"api/TodoItems/{_editItem.Id}, _editItem);
  }
  ```
  
  <span data-ttu-id="8070e-149">`PutAsJsonAsync`呼叫會傳回<xref:System.Net.Http.HttpResponseMessage>。</span><span class="sxs-lookup"><span data-stu-id="8070e-149">Calls to `PutAsJsonAsync` return an <xref:System.Net.Http.HttpResponseMessage>.</span></span> <span data-ttu-id="8070e-150">若要從回應訊息還原序列化 JSON 內容，請使用`ReadFromJsonAsync<T>`擴充方法：</span><span class="sxs-lookup"><span data-stu-id="8070e-150">To deserialize the JSON content from the response message, use the `ReadFromJsonAsync<T>` extension method:</span></span>
  
  ```csharp
  var content = response.content.ReadFromJsonAsync<WeatherForecast>();
  ```

<span data-ttu-id="8070e-151"><xref:System.Net.Http>包含用來傳送 HTTP 要求和接收 HTTP 回應的其他擴充方法。</span><span class="sxs-lookup"><span data-stu-id="8070e-151"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="8070e-152">[HttpClient](xref:System.Net.Http.HttpClient.DeleteAsync*)是用來將 HTTP DELETE 要求傳送至 Web API。</span><span class="sxs-lookup"><span data-stu-id="8070e-152">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="8070e-153">在下列程式碼中，Delete `<button>`元素會呼叫`DeleteItem`方法。</span><span class="sxs-lookup"><span data-stu-id="8070e-153">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="8070e-154">綁定`<input>`項會提供要`id`刪除之專案的。</span><span class="sxs-lookup"><span data-stu-id="8070e-154">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="8070e-155">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="8070e-155">See the sample app for a complete example.</span></span>

```razor
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

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="8070e-156">跨原始來源資源分享（CORS）</span><span class="sxs-lookup"><span data-stu-id="8070e-156">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="8070e-157">瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。</span><span class="sxs-lookup"><span data-stu-id="8070e-157">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="8070e-158">這種限制稱為「*相同來源原則*」。</span><span class="sxs-lookup"><span data-stu-id="8070e-158">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="8070e-159">相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="8070e-159">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="8070e-160">若要將來自瀏覽器的要求傳送至具有不同來源的端點，*端點*必須啟用[跨原始來源資源分享（CORS）](https://www.w3.org/TR/cors/)。</span><span class="sxs-lookup"><span data-stu-id="8070e-160">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="8070e-161">[Blazor WebAssembly 範例應用程式（BlazorWebAssemblySample）](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)示範如何在呼叫 Web API 元件（*Pages/CallWebAPI. razor*）中使用 CORS。</span><span class="sxs-lookup"><span data-stu-id="8070e-161">The [Blazor WebAssembly sample app (BlazorWebAssemblySample)](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="8070e-162">若要允許其他網站對您的應用程式進行跨原始來源資源分享（CORS）要求<xref:security/cors>，請參閱。</span><span class="sxs-lookup"><span data-stu-id="8070e-162">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="8070e-163">具有 Fetch API 要求選項的 HttpClient 和 HttpRequestMessage</span><span class="sxs-lookup"><span data-stu-id="8070e-163">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="8070e-164">在 Blazor WebAssembly 應用程式的 WebAssembly 上執行時， [HttpClient](xref:fundamentals/http-requests)請使用<xref:System.Net.Http.HttpRequestMessage> HttpClient 和來自訂要求。</span><span class="sxs-lookup"><span data-stu-id="8070e-164">When running on WebAssembly in a Blazor WebAssembly app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="8070e-165">例如，您可以指定要求 URI、HTTP 方法，以及任何所需的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="8070e-165">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

```razor
@using System.Net.Http
@using System.Net.Http.Headers
@using System.Net.Http.Json
@inject HttpClient Http

@code {
    private async Task PostRequest()
    {
        var requestMessage = new HttpRequestMessage()
        {
            Method = new HttpMethod("POST"),
            RequestUri = new Uri("https://localhost:10000/api/TodoItems"),
            Content = 
                JsonContent.Create(new TodoItem
                { 
                    Name: "A New Todo Item",
                    IsComplete: false
                })
        };
        
        requestMessage.Headers.Authorization = 
           new AuthenticationHeaderValue("Bearer", "{OAUTH TOKEN}");

        requestMessage.Content.Headers.TryAddWithoutValidation(
            "x-custom-header", "value");

        var response = await Http.SendAsync(requestMessage);
        var responseStatusCode = response.StatusCode;
        var responseBody = await response.Content.ReadAsStringAsync();
    }
}
```

<span data-ttu-id="8070e-166">.NET WebAssembly 的執行會`HttpClient`使用[WindowOrWorkerGlobalScope。 fetch （）](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch)。</span><span class="sxs-lookup"><span data-stu-id="8070e-166">.NET WebAssembly's implementation of `HttpClient` uses [WindowOrWorkerGlobalScope.fetch()](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch).</span></span> <span data-ttu-id="8070e-167">Fetch 可讓您設定數個[要求特有的選項](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)。</span><span class="sxs-lookup"><span data-stu-id="8070e-167">Fetch allows configuring several [request-specific options](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span> 

<span data-ttu-id="8070e-168">您可以使用`HttpRequestMessage`下表所示的擴充方法來設定 HTTP 提取要求選項。</span><span class="sxs-lookup"><span data-stu-id="8070e-168">HTTP fetch request options can be configured with `HttpRequestMessage` extension methods shown in the following table.</span></span>

| <span data-ttu-id="8070e-169">`HttpRequestMessage`擴充方法</span><span class="sxs-lookup"><span data-stu-id="8070e-169">`HttpRequestMessage` extension method</span></span> | <span data-ttu-id="8070e-170">Fetch 要求屬性</span><span class="sxs-lookup"><span data-stu-id="8070e-170">Fetch request property</span></span> |
| ------------------------------------- | ---------------------- |
| `SetBrowserRequestCredentials`        | [<span data-ttu-id="8070e-171">憑證</span><span class="sxs-lookup"><span data-stu-id="8070e-171">credentials</span></span>](https://developer.mozilla.org/docs/Web/API/Request/credentials) |
| `SetBrowserRequestCache`              | [<span data-ttu-id="8070e-172">高速</span><span class="sxs-lookup"><span data-stu-id="8070e-172">cache</span></span>](https://developer.mozilla.org/docs/Web/API/Request/cache) |
| `SetBrowserRequestMode`               | [<span data-ttu-id="8070e-173">mode</span><span class="sxs-lookup"><span data-stu-id="8070e-173">mode</span></span>](https://developer.mozilla.org/docs/Web/API/Request/mode) |
| `SetBrowserRequestIntegrity`          | [<span data-ttu-id="8070e-174">完整性</span><span class="sxs-lookup"><span data-stu-id="8070e-174">integrity</span></span>](https://developer.mozilla.org/docs/Web/API/Request/integrity) |

<span data-ttu-id="8070e-175">您可以使用更泛型`SetBrowserRequestOption`的擴充方法來設定其他選項。</span><span class="sxs-lookup"><span data-stu-id="8070e-175">You can set additional options using the more generic `SetBrowserRequestOption` extension method.</span></span>
 
<span data-ttu-id="8070e-176">HTTP 回應通常會在Blazor WebAssembly 應用程式中進行緩衝處理，以啟用回應內容的同步讀取支援。</span><span class="sxs-lookup"><span data-stu-id="8070e-176">The HTTP response is typically buffered in a Blazor WebAssembly app to enable support for sync reads on the response content.</span></span> <span data-ttu-id="8070e-177">若要啟用回應串流的支援，請`SetBrowserResponseStreamingEnabled`在要求上使用擴充方法。</span><span class="sxs-lookup"><span data-stu-id="8070e-177">To enable support for response streaming, use the `SetBrowserResponseStreamingEnabled` extension method on the request.</span></span>

<span data-ttu-id="8070e-178">若要在跨原始來源要求中包含認證，請`SetBrowserRequestCredentials`使用擴充方法：</span><span class="sxs-lookup"><span data-stu-id="8070e-178">To include credentials in a cross-origin request, use the `SetBrowserRequestCredentials` extension method:</span></span>

```csharp
requestMessage.SetBrowserRequestCredentials(BrowserRequestCredentials.Include);
```

<span data-ttu-id="8070e-179">如需有關提取 API 選項的詳細資訊，請參閱[MDN web 檔： WindowOrWorkerGlobalScope。 Fetch （）:P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)。</span><span class="sxs-lookup"><span data-stu-id="8070e-179">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="8070e-180">在 CORS 要求上傳送認證（授權 cookie/標頭）時`Authorization` ，cors 原則必須允許標頭。</span><span class="sxs-lookup"><span data-stu-id="8070e-180">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="8070e-181">下列原則包含的設定：</span><span class="sxs-lookup"><span data-stu-id="8070e-181">The following policy includes configuration for:</span></span>

* <span data-ttu-id="8070e-182">要求來源（`http://localhost:5000`、 `https://localhost:5001`）。</span><span class="sxs-lookup"><span data-stu-id="8070e-182">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="8070e-183">Any 方法（動詞）。</span><span class="sxs-lookup"><span data-stu-id="8070e-183">Any method (verb).</span></span>
* <span data-ttu-id="8070e-184">`Content-Type`和`Authorization`標頭。</span><span class="sxs-lookup"><span data-stu-id="8070e-184">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="8070e-185">若要允許自訂標頭（例如`x-custom-header`），請在呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>時列出標頭。</span><span class="sxs-lookup"><span data-stu-id="8070e-185">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="8070e-186">用戶端 JavaScript 程式碼（`credentials`屬性設為`include`）所設定的認證。</span><span class="sxs-lookup"><span data-stu-id="8070e-186">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="8070e-187">如需詳細資訊， <xref:security/cors>請參閱和範例應用程式的 HTTP 要求測試器元件（*Components/HTTPRequestTester*）。</span><span class="sxs-lookup"><span data-stu-id="8070e-187">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8070e-188">其他資源</span><span class="sxs-lookup"><span data-stu-id="8070e-188">Additional resources</span></span>

* [<span data-ttu-id="8070e-189">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="8070e-189">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* [<span data-ttu-id="8070e-190">將權杖附加到連出要求</span><span class="sxs-lookup"><span data-stu-id="8070e-190">Attach tokens to outgoing requests</span></span>](xref:security/blazor/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)
* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [<span data-ttu-id="8070e-191">Kestrel HTTPS 端點設定</span><span class="sxs-lookup"><span data-stu-id="8070e-191">Kestrel HTTPS endpoint configuration</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [<span data-ttu-id="8070e-192">位於 W3C 的跨原始來源資源分享（CORS）</span><span class="sxs-lookup"><span data-stu-id="8070e-192">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
