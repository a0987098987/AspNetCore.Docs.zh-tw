---
title: 從 ASP.NET Core Blazor WebAssembly 呼叫 Web API
author: guardrex
description: 瞭解如何使用 JSON helper 從Blazor WebAssembly 應用程式呼叫 Web API，包括建立跨原始來源資源分享（CORS）要求。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/04/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/call-web-api
ms.openlocfilehash: d823db3688e05f6befefacc9f390e0dcdbf329a7
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82767144"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a><span data-ttu-id="75e0a-103">從 ASP.NET Core 呼叫 Web APIBlazor</span><span class="sxs-lookup"><span data-stu-id="75e0a-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="75e0a-104">By [Luke Latham](https://github.com/guardrex)、 [Daniel Roth](https://github.com/danroth27)和[Juan De la Cruz](https://github.com/juandelacruz23)</span><span class="sxs-lookup"><span data-stu-id="75e0a-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Juan De la Cruz](https://github.com/juandelacruz23)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="75e0a-105">WebAssembly apps 會使用預先`HttpClient`設定的服務來呼叫 web api。 [ Blazor ](xref:blazor/hosting-models#blazor-webassembly)</span><span class="sxs-lookup"><span data-stu-id="75e0a-105">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="75e0a-106">撰寫要求，其中可以包含 JavaScript[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)選項、使用Blazor JSON helper 或搭配<xref:System.Net.Http.HttpRequestMessage>。</span><span class="sxs-lookup"><span data-stu-id="75e0a-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span> <span data-ttu-id="75e0a-107">WebAssembly `HttpClient` apps 中Blazor的服務著重于向原始伺服器提出要求。</span><span class="sxs-lookup"><span data-stu-id="75e0a-107">The `HttpClient` service in Blazor WebAssembly apps is focused on making requests back to the server of origin.</span></span> <span data-ttu-id="75e0a-108">本主題中的指導方針僅適用Blazor于 WebAssembly apps。</span><span class="sxs-lookup"><span data-stu-id="75e0a-108">The guidance in this topic only pertains to Blazor WebAssembly apps.</span></span>

<span data-ttu-id="75e0a-109">伺服器應用程式會使用<xref:System.Net.Http.HttpClient>實例呼叫 web api，通常<xref:System.Net.Http.IHttpClientFactory>是使用來建立。 [ Blazor ](xref:blazor/hosting-models#blazor-server)</span><span class="sxs-lookup"><span data-stu-id="75e0a-109">[Blazor Server](xref:blazor/hosting-models#blazor-server) apps call web APIs using <xref:System.Net.Http.HttpClient> instances, typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="75e0a-110">本主題中的指導方針與Blazor伺服器應用程式無關。</span><span class="sxs-lookup"><span data-stu-id="75e0a-110">The guidance in this topic doesn't pertain to Blazor Server apps.</span></span> <span data-ttu-id="75e0a-111">開發Blazor伺服器應用程式時，請遵循中<xref:fundamentals/http-requests>的指導方針。</span><span class="sxs-lookup"><span data-stu-id="75e0a-111">When developing Blazor Server apps, follow the guidance in <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="75e0a-112">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)（[如何下載](xref:index#how-to-download-a-sample)） &ndash;選取*BlazorWebAssemblySample*應用程式。</span><span class="sxs-lookup"><span data-stu-id="75e0a-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample)) &ndash; Select the *BlazorWebAssemblySample* app.</span></span>

<span data-ttu-id="75e0a-113">請參閱*BlazorWebAssemblySample*範例應用程式中的下列元件：</span><span class="sxs-lookup"><span data-stu-id="75e0a-113">See the following components in the *BlazorWebAssemblySample* sample app:</span></span>

* <span data-ttu-id="75e0a-114">呼叫 Web API （*Pages/CallWebAPI. razor*）</span><span class="sxs-lookup"><span data-stu-id="75e0a-114">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="75e0a-115">HTTP 要求測試器（*元件/HTTPRequestTester razor*）</span><span class="sxs-lookup"><span data-stu-id="75e0a-115">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="packages"></a><span data-ttu-id="75e0a-116">套件</span><span class="sxs-lookup"><span data-stu-id="75e0a-116">Packages</span></span>

<span data-ttu-id="75e0a-117">在專案檔中參考[系統 .net. Http. Json](https://www.nuget.org/packages/System.Net.Http.Json/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="75e0a-117">Reference the [System.Net.Http.Json](https://www.nuget.org/packages/System.Net.Http.Json/) NuGet package in the project file.</span></span>

## <a name="add-the-httpclient-service"></a><span data-ttu-id="75e0a-118">新增 HttpClient 服務</span><span class="sxs-lookup"><span data-stu-id="75e0a-118">Add the HttpClient service</span></span>

<span data-ttu-id="75e0a-119">在`Program.Main`中，新增`HttpClient`服務（如果尚未存在）：</span><span class="sxs-lookup"><span data-stu-id="75e0a-119">In `Program.Main`, add an `HttpClient` service if it doesn't already exist:</span></span>

```csharp
builder.Services.AddTransient(sp => 
    new HttpClient
    {
        BaseAddress = new Uri(builder.HostEnvironment.BaseAddress)
    });
```

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="75e0a-120">HttpClient 和 JSON 協助程式</span><span class="sxs-lookup"><span data-stu-id="75e0a-120">HttpClient and JSON helpers</span></span>

<span data-ttu-id="75e0a-121">在Blazor WebAssembly 應用程式中， [HttpClient](xref:fundamentals/http-requests)會當做預先設定的服務提供，以便向源伺服器提出要求。</span><span class="sxs-lookup"><span data-stu-id="75e0a-121">In a Blazor WebAssembly app, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span>

<span data-ttu-id="75e0a-122">Blazor伺服器應用程式預設不會`HttpClient`包含服務。</span><span class="sxs-lookup"><span data-stu-id="75e0a-122">A Blazor Server app doesn't include an `HttpClient` service by default.</span></span> <span data-ttu-id="75e0a-123">使用`HttpClient` [HttpClient factory 基礎結構](xref:fundamentals/http-requests)，將提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="75e0a-123">Provide an `HttpClient` to the app using the [HttpClient factory infrastructure](xref:fundamentals/http-requests).</span></span>

<span data-ttu-id="75e0a-124">`HttpClient`和 JSON 協助程式也用來呼叫協力廠商 Web API 端點。</span><span class="sxs-lookup"><span data-stu-id="75e0a-124">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="75e0a-125">`HttpClient`會使用瀏覽器[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)來執行，並受限於其限制，包括強制執行相同的來源原則。</span><span class="sxs-lookup"><span data-stu-id="75e0a-125">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="75e0a-126">用戶端的基底位址會設定為源伺服器的位址。</span><span class="sxs-lookup"><span data-stu-id="75e0a-126">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="75e0a-127">使用`@inject`指示`HttpClient`詞插入實例：</span><span class="sxs-lookup"><span data-stu-id="75e0a-127">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```razor
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="75e0a-128">在下列範例中，Todo Web API 處理建立、讀取、更新和刪除（CRUD）作業。</span><span class="sxs-lookup"><span data-stu-id="75e0a-128">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="75e0a-129">這些範例是以儲存的`TodoItem`類別為基礎：</span><span class="sxs-lookup"><span data-stu-id="75e0a-129">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="75e0a-130">專案的`Id`識別碼`long`（ &ndash; ，）唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="75e0a-130">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="75e0a-131">專案的`Name`名稱`string`（ &ndash; ，）名稱。</span><span class="sxs-lookup"><span data-stu-id="75e0a-131">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="75e0a-132">Status （`IsComplete`， `bool`） &ndash;表示待辦事項是否已完成。</span><span class="sxs-lookup"><span data-stu-id="75e0a-132">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="75e0a-133">JSON helper 方法會將要求傳送至 URI （下列範例中的 Web API）並處理回應：</span><span class="sxs-lookup"><span data-stu-id="75e0a-133">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="75e0a-134">`GetFromJsonAsync`&ndash;傳送 HTTP GET 要求，並剖析 JSON 回應主體以建立物件。</span><span class="sxs-lookup"><span data-stu-id="75e0a-134">`GetFromJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="75e0a-135">在下列程式碼中， `_todoItems`元件會顯示。</span><span class="sxs-lookup"><span data-stu-id="75e0a-135">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="75e0a-136">當`GetTodoItems`元件完成呈現（[OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)）時，就會觸發方法。</span><span class="sxs-lookup"><span data-stu-id="75e0a-136">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)).</span></span> <span data-ttu-id="75e0a-137">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="75e0a-137">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetFromJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <span data-ttu-id="75e0a-138">`PostAsJsonAsync`&ndash;傳送 HTTP POST 要求，包括 json 編碼的內容，並剖析 json 回應主體以建立物件。</span><span class="sxs-lookup"><span data-stu-id="75e0a-138">`PostAsJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="75e0a-139">在下列程式碼中`_newItemName` ，是由元件的繫結項目所提供。</span><span class="sxs-lookup"><span data-stu-id="75e0a-139">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="75e0a-140">`AddItem`方法是藉由選取`<button>`元素來觸發。</span><span class="sxs-lookup"><span data-stu-id="75e0a-140">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="75e0a-141">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="75e0a-141">See the sample app for a complete example.</span></span>

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
  
  <span data-ttu-id="75e0a-142">`PostAsJsonAsync`呼叫會傳回<xref:System.Net.Http.HttpResponseMessage>。</span><span class="sxs-lookup"><span data-stu-id="75e0a-142">Calls to `PostAsJsonAsync` return an <xref:System.Net.Http.HttpResponseMessage>.</span></span> <span data-ttu-id="75e0a-143">若要從回應訊息還原序列化 JSON 內容，請使用`ReadFromJsonAsync<T>`擴充方法：</span><span class="sxs-lookup"><span data-stu-id="75e0a-143">To deserialize the JSON content from the response message, use the `ReadFromJsonAsync<T>` extension method:</span></span>
  
  ```csharp
  var content = response.content.ReadFromJsonAsync<WeatherForecast>();
  ```

* <span data-ttu-id="75e0a-144">`PutAsJsonAsync`&ndash;傳送 HTTP PUT 要求，包括 JSON 編碼的內容。</span><span class="sxs-lookup"><span data-stu-id="75e0a-144">`PutAsJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="75e0a-145">在下列程式碼中`_editItem` ， `Name`和`IsCompleted`的值是由元件的繫結項目所提供。</span><span class="sxs-lookup"><span data-stu-id="75e0a-145">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="75e0a-146">當專案在`Id` UI 的另一個部分中選取並`EditItem`呼叫時，會設定專案的。</span><span class="sxs-lookup"><span data-stu-id="75e0a-146">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="75e0a-147">`SaveItem`方法是藉由選取 Save `<button>`元素來觸發。</span><span class="sxs-lookup"><span data-stu-id="75e0a-147">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="75e0a-148">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="75e0a-148">See the sample app for a complete example.</span></span>

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
  
  <span data-ttu-id="75e0a-149">`PutAsJsonAsync`呼叫會傳回<xref:System.Net.Http.HttpResponseMessage>。</span><span class="sxs-lookup"><span data-stu-id="75e0a-149">Calls to `PutAsJsonAsync` return an <xref:System.Net.Http.HttpResponseMessage>.</span></span> <span data-ttu-id="75e0a-150">若要從回應訊息還原序列化 JSON 內容，請使用`ReadFromJsonAsync<T>`擴充方法：</span><span class="sxs-lookup"><span data-stu-id="75e0a-150">To deserialize the JSON content from the response message, use the `ReadFromJsonAsync<T>` extension method:</span></span>
  
  ```csharp
  var content = response.content.ReadFromJsonAsync<WeatherForecast>();
  ```

<span data-ttu-id="75e0a-151"><xref:System.Net.Http>包含用來傳送 HTTP 要求和接收 HTTP 回應的其他擴充方法。</span><span class="sxs-lookup"><span data-stu-id="75e0a-151"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="75e0a-152">[HttpClient](xref:System.Net.Http.HttpClient.DeleteAsync*)是用來將 HTTP DELETE 要求傳送至 Web API。</span><span class="sxs-lookup"><span data-stu-id="75e0a-152">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="75e0a-153">在下列程式碼中，Delete `<button>`元素會呼叫`DeleteItem`方法。</span><span class="sxs-lookup"><span data-stu-id="75e0a-153">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="75e0a-154">綁定`<input>`項會提供要`id`刪除之專案的。</span><span class="sxs-lookup"><span data-stu-id="75e0a-154">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="75e0a-155">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="75e0a-155">See the sample app for a complete example.</span></span>

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

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="75e0a-156">跨原始來源資源分享（CORS）</span><span class="sxs-lookup"><span data-stu-id="75e0a-156">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="75e0a-157">瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。</span><span class="sxs-lookup"><span data-stu-id="75e0a-157">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="75e0a-158">這種限制稱為「*相同來源原則*」。</span><span class="sxs-lookup"><span data-stu-id="75e0a-158">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="75e0a-159">相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="75e0a-159">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="75e0a-160">若要將來自瀏覽器的要求傳送至具有不同來源的端點，*端點*必須啟用[跨原始來源資源分享（CORS）](https://www.w3.org/TR/cors/)。</span><span class="sxs-lookup"><span data-stu-id="75e0a-160">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="75e0a-161">[WebAssembly 範例應用程式（BlazorWebAssemblySample）示範如何在呼叫 Web API 元件（Pages/CallWebAPI razor）中使用 CORS。 Blazor ](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) *Pages/CallWebAPI.razor*</span><span class="sxs-lookup"><span data-stu-id="75e0a-161">The [Blazor WebAssembly sample app (BlazorWebAssemblySample)](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="75e0a-162">若要允許其他網站對您的應用程式進行跨原始來源資源分享（CORS）要求<xref:security/cors>，請參閱。</span><span class="sxs-lookup"><span data-stu-id="75e0a-162">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="75e0a-163">其他資源</span><span class="sxs-lookup"><span data-stu-id="75e0a-163">Additional resources</span></span>

* <xref:security/blazor/webassembly/index>
* <xref:security/blazor/webassembly/additional-scenarios>
* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [<span data-ttu-id="75e0a-164">Kestrel HTTPS 端點設定</span><span class="sxs-lookup"><span data-stu-id="75e0a-164">Kestrel HTTPS endpoint configuration</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [<span data-ttu-id="75e0a-165">位於 W3C 的跨原始來源資源分享（CORS）</span><span class="sxs-lookup"><span data-stu-id="75e0a-165">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
