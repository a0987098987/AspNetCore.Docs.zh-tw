---
title: 從 ASP.NET Core 呼叫 Web APIBlazor WebAssembly
author: guardrex
description: 瞭解如何 Blazor WebAssembly 使用 JSON helper 從應用程式呼叫 Web API，包括建立跨原始來源資源分享（CORS）要求。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/24/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/call-web-api
ms.openlocfilehash: 9c90e2a6c03e02b771cfa8f589e381d52637dc26
ms.sourcegitcommit: f7873c02c1505c99106cbc708f37e18fc0a496d1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/08/2020
ms.locfileid: "86147634"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a><span data-ttu-id="ed754-103">從 ASP.NET Core 呼叫 Web APIBlazor</span><span class="sxs-lookup"><span data-stu-id="ed754-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="ed754-104">By [Luke Latham](https://github.com/guardrex)、 [Daniel Roth](https://github.com/danroth27)和[Juan De la Cruz](https://github.com/juandelacruz23)</span><span class="sxs-lookup"><span data-stu-id="ed754-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Juan De la Cruz](https://github.com/juandelacruz23)</span></span>

> [!NOTE]
> <span data-ttu-id="ed754-105">本主題適用于 Blazor WebAssembly 。</span><span class="sxs-lookup"><span data-stu-id="ed754-105">This topic applies to Blazor WebAssembly.</span></span> <span data-ttu-id="ed754-106">[Blazor Server](xref:blazor/hosting-models#blazor-server)應用程式會使用實例呼叫 web Api <xref:System.Net.Http.HttpClient> ，通常使用建立 <xref:System.Net.Http.IHttpClientFactory> 。</span><span class="sxs-lookup"><span data-stu-id="ed754-106">[Blazor Server](xref:blazor/hosting-models#blazor-server) apps call web APIs using <xref:System.Net.Http.HttpClient> instances, typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="ed754-107">如需適用于的指引 Blazor Server ，請參閱 <xref:fundamentals/http-requests> 。</span><span class="sxs-lookup"><span data-stu-id="ed754-107">For guidance that applies to Blazor Server, see <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="ed754-108">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)應用程式會使用預先設定的服務來呼叫 web Api <xref:System.Net.Http.HttpClient> 。</span><span class="sxs-lookup"><span data-stu-id="ed754-108">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) apps call web APIs using a preconfigured <xref:System.Net.Http.HttpClient> service.</span></span> <span data-ttu-id="ed754-109">撰寫要求，其中可以包含 JavaScript[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)選項、使用 Blazor JSON helper 或搭配 <xref:System.Net.Http.HttpRequestMessage> 。</span><span class="sxs-lookup"><span data-stu-id="ed754-109">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span> <span data-ttu-id="ed754-110"><xref:System.Net.Http.HttpClient>應用程式中的服務著重于將 Blazor WebAssembly 要求傳回給來源伺服器。</span><span class="sxs-lookup"><span data-stu-id="ed754-110">The <xref:System.Net.Http.HttpClient> service in Blazor WebAssembly apps is focused on making requests back to the server of origin.</span></span> <span data-ttu-id="ed754-111">本主題中的指導方針僅適用于 Blazor WebAssembly 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ed754-111">The guidance in this topic only pertains to Blazor WebAssembly apps.</span></span>

<span data-ttu-id="ed754-112">[View or 下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)（[如何下載](xref:index#how-to-download-a-sample)）：選取 `BlazorWebAssemblySample` 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ed754-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample)): Select the `BlazorWebAssemblySample` app.</span></span>

<span data-ttu-id="ed754-113">請參閱範例應用程式中的下列元件 `BlazorWebAssemblySample` ：</span><span class="sxs-lookup"><span data-stu-id="ed754-113">See the following components in the `BlazorWebAssemblySample` sample app:</span></span>

* <span data-ttu-id="ed754-114">呼叫 Web API （ `Pages/CallWebAPI.razor` ）</span><span class="sxs-lookup"><span data-stu-id="ed754-114">Call Web API (`Pages/CallWebAPI.razor`)</span></span>
* <span data-ttu-id="ed754-115">HTTP 要求測試器（ `Components/HTTPRequestTester.razor` ）</span><span class="sxs-lookup"><span data-stu-id="ed754-115">HTTP Request Tester (`Components/HTTPRequestTester.razor`)</span></span>

## <a name="packages"></a><span data-ttu-id="ed754-116">套件</span><span class="sxs-lookup"><span data-stu-id="ed754-116">Packages</span></span>

<span data-ttu-id="ed754-117">參考 [`System.Net.Http.Json`](https://www.nuget.org/packages/System.Net.Http.Json/) 專案檔中的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="ed754-117">Reference the [`System.Net.Http.Json`](https://www.nuget.org/packages/System.Net.Http.Json/) NuGet package in the project file.</span></span>

## <a name="add-the-httpclient-service"></a><span data-ttu-id="ed754-118">新增 HttpClient 服務</span><span class="sxs-lookup"><span data-stu-id="ed754-118">Add the HttpClient service</span></span>

<span data-ttu-id="ed754-119">在中 `Program.Main` ，新增 <xref:System.Net.Http.HttpClient> 服務（如果尚未存在）：</span><span class="sxs-lookup"><span data-stu-id="ed754-119">In `Program.Main`, add an <xref:System.Net.Http.HttpClient> service if it doesn't already exist:</span></span>

```csharp
builder.Services.AddTransient(sp => 
    new HttpClient
    {
        BaseAddress = new Uri(builder.HostEnvironment.BaseAddress)
    });
```

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="ed754-120">HttpClient 和 JSON 協助程式</span><span class="sxs-lookup"><span data-stu-id="ed754-120">HttpClient and JSON helpers</span></span>

<span data-ttu-id="ed754-121">在 Blazor WebAssembly 應用程式中， [`HttpClient`](xref:fundamentals/http-requests) 會以預先設定的服務形式提供，讓要求回到源伺服器。</span><span class="sxs-lookup"><span data-stu-id="ed754-121">In a Blazor WebAssembly app, [`HttpClient`](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span>

<span data-ttu-id="ed754-122">Blazor Server應用程式預設不會包含 <xref:System.Net.Http.HttpClient> 服務。</span><span class="sxs-lookup"><span data-stu-id="ed754-122">A Blazor Server app doesn't include an <xref:System.Net.Http.HttpClient> service by default.</span></span> <span data-ttu-id="ed754-123"><xref:System.Net.Http.HttpClient>使用[ `HttpClient` factory 基礎結構](xref:fundamentals/http-requests)，將提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="ed754-123">Provide an <xref:System.Net.Http.HttpClient> to the app using the [`HttpClient` factory infrastructure](xref:fundamentals/http-requests).</span></span>

<span data-ttu-id="ed754-124"><xref:System.Net.Http.HttpClient>和 JSON 協助程式也用來呼叫協力廠商 Web API 端點。</span><span class="sxs-lookup"><span data-stu-id="ed754-124"><xref:System.Net.Http.HttpClient> and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="ed754-125"><xref:System.Net.Http.HttpClient>會使用瀏覽器[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)來執行，並受限於其限制，包括強制執行相同的來源原則。</span><span class="sxs-lookup"><span data-stu-id="ed754-125"><xref:System.Net.Http.HttpClient> is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="ed754-126">用戶端的基底位址會設定為源伺服器的位址。</span><span class="sxs-lookup"><span data-stu-id="ed754-126">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="ed754-127">使用指示詞插入 <xref:System.Net.Http.HttpClient> 實例 [`@inject`](xref:mvc/views/razor#inject) ：</span><span class="sxs-lookup"><span data-stu-id="ed754-127">Inject an <xref:System.Net.Http.HttpClient> instance using the [`@inject`](xref:mvc/views/razor#inject) directive:</span></span>

```razor
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="ed754-128">在下列範例中，Todo Web API 處理建立、讀取、更新和刪除（CRUD）作業。</span><span class="sxs-lookup"><span data-stu-id="ed754-128">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="ed754-129">這些範例是以 `TodoItem` 儲存的類別為基礎：</span><span class="sxs-lookup"><span data-stu-id="ed754-129">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="ed754-130">ID （ `Id` ， `long` ）：專案的唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="ed754-130">ID (`Id`, `long`): Unique ID of the item.</span></span>
* <span data-ttu-id="ed754-131">名稱（ `Name` ， `string` ）：專案的名稱。</span><span class="sxs-lookup"><span data-stu-id="ed754-131">Name (`Name`, `string`): Name of the item.</span></span>
* <span data-ttu-id="ed754-132">Status （ `IsComplete` ， `bool` ）：表示待辦事項是否已完成。</span><span class="sxs-lookup"><span data-stu-id="ed754-132">Status (`IsComplete`, `bool`): Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="ed754-133">JSON helper 方法會將要求傳送至 URI （下列範例中的 Web API）並處理回應：</span><span class="sxs-lookup"><span data-stu-id="ed754-133">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="ed754-134"><xref:System.Net.Http.Json.HttpClientJsonExtensions.GetFromJsonAsync%2A>：傳送 HTTP GET 要求，並剖析 JSON 回應主體以建立物件。</span><span class="sxs-lookup"><span data-stu-id="ed754-134"><xref:System.Net.Http.Json.HttpClientJsonExtensions.GetFromJsonAsync%2A>: Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="ed754-135">在下列程式碼中， `todoItems` 元件會顯示。</span><span class="sxs-lookup"><span data-stu-id="ed754-135">In the following code, the `todoItems` are displayed by the component.</span></span> <span data-ttu-id="ed754-136">`GetTodoItems`當元件完成呈現（）時，就會觸發方法 [`OnInitializedAsync`](xref:blazor/components/lifecycle#component-initialization-methods) 。</span><span class="sxs-lookup"><span data-stu-id="ed754-136">The `GetTodoItems` method is triggered when the component is finished rendering ([`OnInitializedAsync`](xref:blazor/components/lifecycle#component-initialization-methods)).</span></span> <span data-ttu-id="ed754-137">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="ed754-137">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] todoItems;

      protected override async Task OnInitializedAsync() => 
          todoItems = await Http.GetFromJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <span data-ttu-id="ed754-138"><xref:System.Net.Http.Json.HttpClientJsonExtensions.PostAsJsonAsync%2A>：傳送 HTTP POST 要求，包括 JSON 編碼的內容，並剖析 JSON 回應主體以建立物件。</span><span class="sxs-lookup"><span data-stu-id="ed754-138"><xref:System.Net.Http.Json.HttpClientJsonExtensions.PostAsJsonAsync%2A>: Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="ed754-139">在下列程式碼中， `newItemName` 是由元件的繫結項目所提供。</span><span class="sxs-lookup"><span data-stu-id="ed754-139">In the following code, `newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="ed754-140">`AddItem`方法是藉由選取元素來觸發 `<button>` 。</span><span class="sxs-lookup"><span data-stu-id="ed754-140">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="ed754-141">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="ed754-141">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  <input @bind="newItemName" placeholder="New Todo Item" />
  <button @onclick="@AddItem">Add</button>

  @code {
      private string newItemName;

      private async Task AddItem()
      {
          var addItem = new TodoItem { Name = newItemName, IsComplete = false };
          await Http.PostAsJsonAsync("api/TodoItems", addItem);
      }
  }
  ```
  
  <span data-ttu-id="ed754-142">呼叫會傳回 <xref:System.Net.Http.Json.HttpClientJsonExtensions.PostAsJsonAsync%2A> <xref:System.Net.Http.HttpResponseMessage> 。</span><span class="sxs-lookup"><span data-stu-id="ed754-142">Calls to <xref:System.Net.Http.Json.HttpClientJsonExtensions.PostAsJsonAsync%2A> return an <xref:System.Net.Http.HttpResponseMessage>.</span></span> <span data-ttu-id="ed754-143">若要從回應訊息還原序列化 JSON 內容，請使用 `ReadFromJsonAsync<T>` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="ed754-143">To deserialize the JSON content from the response message, use the `ReadFromJsonAsync<T>` extension method:</span></span>
  
  ```csharp
  var content = response.Content.ReadFromJsonAsync<WeatherForecast>();
  ```

* <span data-ttu-id="ed754-144"><xref:System.Net.Http.Json.HttpClientJsonExtensions.PutAsJsonAsync%2A>：傳送 HTTP PUT 要求，包括 JSON 編碼的內容。</span><span class="sxs-lookup"><span data-stu-id="ed754-144"><xref:System.Net.Http.Json.HttpClientJsonExtensions.PutAsJsonAsync%2A>: Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="ed754-145">在下列程式碼中， `editItem` 和的值 `Name` `IsCompleted` 是由元件的繫結項目所提供。</span><span class="sxs-lookup"><span data-stu-id="ed754-145">In the following code, `editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="ed754-146">`Id`當專案在 UI 的另一個部分中選取並呼叫時，會設定專案的 `EditItem` 。</span><span class="sxs-lookup"><span data-stu-id="ed754-146">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="ed754-147">`SaveItem`方法是藉由選取 Save 元素來觸發 `<button>` 。</span><span class="sxs-lookup"><span data-stu-id="ed754-147">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="ed754-148">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="ed754-148">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  <input type="checkbox" @bind="editItem.IsComplete" />
  <input @bind="editItem.Name" />
  <button @onclick="@SaveItem">Save</button>

  @code {
      private TodoItem editItem = new TodoItem();

      private void EditItem(long id)
      {
          editItem = todoItems.Single(i => i.Id == id);
      }

      private async Task SaveItem() =>
          await Http.PutAsJsonAsync($"api/TodoItems/{editItem.Id}, editItem);
  }
  ```
  
  <span data-ttu-id="ed754-149">呼叫會傳回 <xref:System.Net.Http.Json.HttpClientJsonExtensions.PutAsJsonAsync%2A> <xref:System.Net.Http.HttpResponseMessage> 。</span><span class="sxs-lookup"><span data-stu-id="ed754-149">Calls to <xref:System.Net.Http.Json.HttpClientJsonExtensions.PutAsJsonAsync%2A> return an <xref:System.Net.Http.HttpResponseMessage>.</span></span> <span data-ttu-id="ed754-150">若要從回應訊息還原序列化 JSON 內容，請使用 <xref:System.Net.Http.Json.HttpContentJsonExtensions.ReadFromJsonAsync%2A> 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="ed754-150">To deserialize the JSON content from the response message, use the <xref:System.Net.Http.Json.HttpContentJsonExtensions.ReadFromJsonAsync%2A> extension method:</span></span>
  
  ```csharp
  var content = response.Content.ReadFromJsonAsync<WeatherForecast>();
  ```

<span data-ttu-id="ed754-151"><xref:System.Net.Http>包含用來傳送 HTTP 要求和接收 HTTP 回應的其他擴充方法。</span><span class="sxs-lookup"><span data-stu-id="ed754-151"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="ed754-152"><xref:System.Net.Http.HttpClient.DeleteAsync%2A?displayProperty=nameWithType>是用來將 HTTP DELETE 要求傳送至 Web API。</span><span class="sxs-lookup"><span data-stu-id="ed754-152"><xref:System.Net.Http.HttpClient.DeleteAsync%2A?displayProperty=nameWithType> is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="ed754-153">在下列程式碼中，Delete `<button>` 元素會呼叫 `DeleteItem` 方法。</span><span class="sxs-lookup"><span data-stu-id="ed754-153">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="ed754-154">綁定 `<input>` 項會提供 `id` 要刪除之專案的。</span><span class="sxs-lookup"><span data-stu-id="ed754-154">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="ed754-155">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="ed754-155">See the sample app for a complete example.</span></span>

```razor
@using System.Net.Http
@inject HttpClient Http

<input @bind="id" />
<button @onclick="@DeleteItem">Delete</button>

@code {
    private long id;

    private async Task DeleteItem() =>
        await Http.DeleteAsync($"api/TodoItems/{id}");
}
```

## <a name="named-httpclient-with-ihttpclientfactory"></a><span data-ttu-id="ed754-156">名為 HttpClient 與 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="ed754-156">Named HttpClient with IHttpClientFactory</span></span>

<span data-ttu-id="ed754-157"><xref:System.Net.Http.IHttpClientFactory>支援服務和名為的設定 <xref:System.Net.Http.HttpClient> 。</span><span class="sxs-lookup"><span data-stu-id="ed754-157"><xref:System.Net.Http.IHttpClientFactory> services and the configuration of a named <xref:System.Net.Http.HttpClient> are supported.</span></span>

<span data-ttu-id="ed754-158">參考 [`Microsoft.Extensions.Http`](https://www.nuget.org/packages/Microsoft.Extensions.Http/) 專案檔中的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="ed754-158">Reference the [`Microsoft.Extensions.Http`](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package in the project file.</span></span>

<span data-ttu-id="ed754-159">`Program.Main` (`Program.cs`):</span><span class="sxs-lookup"><span data-stu-id="ed754-159">`Program.Main` (`Program.cs`):</span></span>

```csharp
builder.Services.AddHttpClient("ServerAPI", client => 
    client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress));
```

<span data-ttu-id="ed754-160">`FetchData`component （ `Pages/FetchData.razor` ）：</span><span class="sxs-lookup"><span data-stu-id="ed754-160">`FetchData` component (`Pages/FetchData.razor`):</span></span>

```razor
@inject IHttpClientFactory ClientFactory

...

@code {
    private WeatherForecast[] forecasts;

    protected override async Task OnInitializedAsync()
    {
        var client = ClientFactory.CreateClient("ServerAPI");

        forecasts = await client.GetFromJsonAsync<WeatherForecast[]>(
            "WeatherForecast");
    }
}
```

## <a name="typed-httpclient"></a><span data-ttu-id="ed754-161">具類型的 HttpClient</span><span class="sxs-lookup"><span data-stu-id="ed754-161">Typed HttpClient</span></span>

<span data-ttu-id="ed754-162">型別會 <xref:System.Net.Http.HttpClient> 使用一個或多個應用程式的 <xref:System.Net.Http.HttpClient> 實例（預設或名為），從一或多個 Web API 端點傳回資料。</span><span class="sxs-lookup"><span data-stu-id="ed754-162">Typed <xref:System.Net.Http.HttpClient> uses one or more of the app's <xref:System.Net.Http.HttpClient> instances, default or named, to return data from one or more web API endpoints.</span></span>

<span data-ttu-id="ed754-163">`WeatherForecastClient.cs`:</span><span class="sxs-lookup"><span data-stu-id="ed754-163">`WeatherForecastClient.cs`:</span></span>

```csharp
using System.Net.Http;
using System.Net.Http.Json;
using System.Threading.Tasks;

public class WeatherForecastClient
{
    private readonly HttpClient client;

    public WeatherForecastClient(HttpClient client)
    {
        this.client = client;
    }

    public async Task<WeatherForecast[]> GetForecastAsync()
    {
        var forecasts = new WeatherForecast[0];
    
        try
        {
            forecasts = await client.GetFromJsonAsync<WeatherForecast[]>(
                "WeatherForecast");
        }
        catch
        {
            ...
        }
    
        return forecasts;
    }
}
```

<span data-ttu-id="ed754-164">`Program.Main` (`Program.cs`):</span><span class="sxs-lookup"><span data-stu-id="ed754-164">`Program.Main` (`Program.cs`):</span></span>

```csharp
builder.Services.AddHttpClient<WeatherForecastClient>(client => 
    client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress));
```

<span data-ttu-id="ed754-165">元件會插入具類型的 <xref:System.Net.Http.HttpClient> 以呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="ed754-165">Components inject the typed <xref:System.Net.Http.HttpClient> to call the web API.</span></span>

<span data-ttu-id="ed754-166">`FetchData`component （ `Pages/FetchData.razor` ）：</span><span class="sxs-lookup"><span data-stu-id="ed754-166">`FetchData` component (`Pages/FetchData.razor`):</span></span>

```razor
@inject WeatherForecastClient Client

...

@code {
    private WeatherForecast[] forecasts;

    protected override async Task OnInitializedAsync()
    {
        forecasts = await Client.GetForecastAsync();
    }
}
```

## <a name="handle-errors"></a><span data-ttu-id="ed754-167">處理錯誤</span><span class="sxs-lookup"><span data-stu-id="ed754-167">Handle errors</span></span>

<span data-ttu-id="ed754-168">與 Web API 互動時，如果發生錯誤，就可以由開發人員程式碼來處理。</span><span class="sxs-lookup"><span data-stu-id="ed754-168">When errors occur while interacting with a web API, they can be handled by developer code.</span></span> <span data-ttu-id="ed754-169">例如， <xref:System.Net.Http.Json.HttpClientJsonExtensions.GetFromJsonAsync%2A> 預期來自伺服器 API 的 JSON 回應與 `Content-Type` 的 `application/json` 。</span><span class="sxs-lookup"><span data-stu-id="ed754-169">For example, <xref:System.Net.Http.Json.HttpClientJsonExtensions.GetFromJsonAsync%2A> expects a JSON response from the server API with a `Content-Type` of `application/json`.</span></span> <span data-ttu-id="ed754-170">如果回應不是 JSON 格式，則內容驗證會擲回 <xref:System.NotSupportedException> 。</span><span class="sxs-lookup"><span data-stu-id="ed754-170">If the response isn't in JSON format, content validation throws a <xref:System.NotSupportedException>.</span></span>

<span data-ttu-id="ed754-171">在下列範例中，氣象預報資料要求的 URI 端點拼錯。</span><span class="sxs-lookup"><span data-stu-id="ed754-171">In the following example, the URI endpoint for the weather forecast data request is misspelled.</span></span> <span data-ttu-id="ed754-172">URI 應該是， `WeatherForecast` 但在呼叫中會顯示為 `WeatherForcast` （遺漏 "e"）。</span><span class="sxs-lookup"><span data-stu-id="ed754-172">The URI should be to `WeatherForecast` but appears in the call as `WeatherForcast` (missing "e").</span></span>

<span data-ttu-id="ed754-173"><xref:System.Net.Http.Json.HttpClientJsonExtensions.GetFromJsonAsync%2A>呼叫預期會傳回 JSON，但是伺服器會針對具有之的伺服器上的未處理例外狀況傳回 HTML `Content-Type` `text/html` 。</span><span class="sxs-lookup"><span data-stu-id="ed754-173">The <xref:System.Net.Http.Json.HttpClientJsonExtensions.GetFromJsonAsync%2A> call expects JSON to be returned, but the server returns HTML for an unhandled exception on the server with a `Content-Type` of `text/html`.</span></span> <span data-ttu-id="ed754-174">未處理的例外狀況發生在伺服器上，因為找不到路徑，而且中介軟體無法提供要求的頁面或視圖。</span><span class="sxs-lookup"><span data-stu-id="ed754-174">The unhandled exception occurs on the server because the path isn't found and middleware can't serve a page or view for the request.</span></span>

<span data-ttu-id="ed754-175">在 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> 用戶端上， <xref:System.NotSupportedException> 當回應內容驗證為非 JSON 時，會擲回。</span><span class="sxs-lookup"><span data-stu-id="ed754-175">In <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> on the client, <xref:System.NotSupportedException> is thrown when the response content is validated as non-JSON.</span></span> <span data-ttu-id="ed754-176">在區塊中攔截到例外狀況 `catch` ，其中自訂邏輯可以記錄錯誤，或向使用者呈現易記的錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="ed754-176">The exception is caught in the `catch` block, where custom logic could log the error or present a friendly error message to the user:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    try
    {
        forecasts = await Http.GetFromJsonAsync<WeatherForecast[]>(
            "WeatherForcast");
    }
    catch (NotSupportedException exception)
    {
        ...
    }
}
```

> [!NOTE]
> <span data-ttu-id="ed754-177">上述範例是為了示範之用。</span><span class="sxs-lookup"><span data-stu-id="ed754-177">The preceding example is for demonstration purposes.</span></span> <span data-ttu-id="ed754-178">即使端點不存在或伺服器上發生未處理的例外狀況，Web API 伺服器應用程式也可以設定為傳回 JSON。</span><span class="sxs-lookup"><span data-stu-id="ed754-178">A web API server app can be configured to return JSON even when an endpoint doesn't exist or an unhandled exception on the server occurs.</span></span>

<span data-ttu-id="ed754-179">如需詳細資訊，請參閱 <xref:blazor/fundamentals/handle-errors> 。</span><span class="sxs-lookup"><span data-stu-id="ed754-179">For more information, see <xref:blazor/fundamentals/handle-errors>.</span></span>

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="ed754-180">跨原始來源資源分享（CORS）</span><span class="sxs-lookup"><span data-stu-id="ed754-180">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="ed754-181">瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。</span><span class="sxs-lookup"><span data-stu-id="ed754-181">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="ed754-182">這種限制稱為「*相同來源原則*」。</span><span class="sxs-lookup"><span data-stu-id="ed754-182">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="ed754-183">相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="ed754-183">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="ed754-184">若要將來自瀏覽器的要求傳送至具有不同來源的端點，*端點*必須啟用[跨原始來源資源分享（CORS）](https://www.w3.org/TR/cors/)。</span><span class="sxs-lookup"><span data-stu-id="ed754-184">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="ed754-185">[ Blazor WebAssembly 範例應用程式（ Blazor WebAssemblySample）](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)示範如何在呼叫 Web API 元件（）中使用 CORS `Pages/CallWebAPI.razor` 。</span><span class="sxs-lookup"><span data-stu-id="ed754-185">The [Blazor WebAssembly sample app (BlazorWebAssemblySample)](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) demonstrates the use of CORS in the Call Web API component (`Pages/CallWebAPI.razor`).</span></span>

<span data-ttu-id="ed754-186">如需有關在應用程式中使用安全要求之 CORS 的詳細資訊 Blazor ，請參閱 <xref:blazor/security/webassembly/additional-scenarios#cross-origin-resource-sharing-cors> 。</span><span class="sxs-lookup"><span data-stu-id="ed754-186">For more information on CORS with secure requests in Blazor apps, see <xref:blazor/security/webassembly/additional-scenarios#cross-origin-resource-sharing-cors>.</span></span>

<span data-ttu-id="ed754-187">如需有關 ASP.NET Core 應用程式之 CORS 的一般資訊，請參閱 <xref:security/cors> 。</span><span class="sxs-lookup"><span data-stu-id="ed754-187">For general information on CORS with ASP.NET Core apps, see <xref:security/cors>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed754-188">其他資源</span><span class="sxs-lookup"><span data-stu-id="ed754-188">Additional resources</span></span>

* <span data-ttu-id="ed754-189"><xref:blazor/security/webassembly/additional-scenarios>：包含使用 <xref:System.Net.Http.HttpClient> 來提出安全 Web API 要求的涵蓋範圍。</span><span class="sxs-lookup"><span data-stu-id="ed754-189"><xref:blazor/security/webassembly/additional-scenarios>: Includes coverage on using <xref:System.Net.Http.HttpClient> to make secure web API requests.</span></span>
* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [<span data-ttu-id="ed754-190">Kestrel HTTPS 端點設定</span><span class="sxs-lookup"><span data-stu-id="ed754-190">Kestrel HTTPS endpoint configuration</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [<span data-ttu-id="ed754-191">位於 W3C 的跨原始來源資源分享（CORS）</span><span class="sxs-lookup"><span data-stu-id="ed754-191">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
