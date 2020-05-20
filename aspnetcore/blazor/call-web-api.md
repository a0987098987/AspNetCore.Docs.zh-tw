---
<span data-ttu-id="d246f-101">標題： ' 從 ASP.NET Core WebAssembly 呼叫 Web API Blazor ' 作者：描述： ' 瞭解如何 Blazor 使用 JSON helper 從 WebAssembly 應用程式呼叫 Web API，包括建立跨原始來源資源分享（CORS）要求。</span><span class="sxs-lookup"><span data-stu-id="d246f-101">title: 'Call a web API from ASP.NET Core Blazor WebAssembly' author: description: 'Learn how to call a web API from a Blazor WebAssembly app using JSON helpers, including making cross-origin resource sharing (CORS) requests.'</span></span>
<span data-ttu-id="d246f-102">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d246f-102">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d246f-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d246f-103">'Blazor'</span></span>
- <span data-ttu-id="d246f-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d246f-104">'Identity'</span></span>
- <span data-ttu-id="d246f-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d246f-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="d246f-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d246f-106">'Razor'</span></span>
- <span data-ttu-id="d246f-107">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d246f-107">'SignalR' uid:</span></span> 

---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a><span data-ttu-id="d246f-108">從 ASP.NET Core 呼叫 Web APIBlazor</span><span class="sxs-lookup"><span data-stu-id="d246f-108">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="d246f-109">By [Luke Latham](https://github.com/guardrex)、 [Daniel Roth](https://github.com/danroth27)和[Juan De la Cruz](https://github.com/juandelacruz23)</span><span class="sxs-lookup"><span data-stu-id="d246f-109">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Juan De la Cruz](https://github.com/juandelacruz23)</span></span>

<span data-ttu-id="d246f-110">[ Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) apps 會使用預先設定的服務來呼叫 web api `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="d246f-110">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="d246f-111">撰寫要求，其中可以包含 JavaScript[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)選項、使用 Blazor JSON helper 或搭配 <xref:System.Net.Http.HttpRequestMessage> 。</span><span class="sxs-lookup"><span data-stu-id="d246f-111">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span> <span data-ttu-id="d246f-112">`HttpClient`WebAssembly apps 中的服務著重于向 Blazor 原始伺服器提出要求。</span><span class="sxs-lookup"><span data-stu-id="d246f-112">The `HttpClient` service in Blazor WebAssembly apps is focused on making requests back to the server of origin.</span></span> <span data-ttu-id="d246f-113">本主題中的指導方針僅適用于 Blazor WebAssembly apps。</span><span class="sxs-lookup"><span data-stu-id="d246f-113">The guidance in this topic only pertains to Blazor WebAssembly apps.</span></span>

<span data-ttu-id="d246f-114">[ Blazor 伺服器](xref:blazor/hosting-models#blazor-server)應用程式會使用實例呼叫 web api <xref:System.Net.Http.HttpClient> ，通常是使用來建立 <xref:System.Net.Http.IHttpClientFactory> 。</span><span class="sxs-lookup"><span data-stu-id="d246f-114">[Blazor Server](xref:blazor/hosting-models#blazor-server) apps call web APIs using <xref:System.Net.Http.HttpClient> instances, typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="d246f-115">本主題中的指導方針與 Blazor 伺服器應用程式無關。</span><span class="sxs-lookup"><span data-stu-id="d246f-115">The guidance in this topic doesn't pertain to Blazor Server apps.</span></span> <span data-ttu-id="d246f-116">開發 Blazor 伺服器應用程式時，請遵循中的指導方針 <xref:fundamentals/http-requests> 。</span><span class="sxs-lookup"><span data-stu-id="d246f-116">When developing Blazor Server apps, follow the guidance in <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="d246f-117">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)（[如何下載](xref:index#how-to-download-a-sample)） &ndash; 選取*BlazorWebAssemblySample*應用程式。</span><span class="sxs-lookup"><span data-stu-id="d246f-117">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample)) &ndash; Select the *BlazorWebAssemblySample* app.</span></span>

<span data-ttu-id="d246f-118">請參閱*BlazorWebAssemblySample*範例應用程式中的下列元件：</span><span class="sxs-lookup"><span data-stu-id="d246f-118">See the following components in the *BlazorWebAssemblySample* sample app:</span></span>

* <span data-ttu-id="d246f-119">呼叫 Web API （*Pages/CallWebAPI. razor*）</span><span class="sxs-lookup"><span data-stu-id="d246f-119">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="d246f-120">HTTP 要求測試器（*元件/HTTPRequestTester razor*）</span><span class="sxs-lookup"><span data-stu-id="d246f-120">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="packages"></a><span data-ttu-id="d246f-121">套件</span><span class="sxs-lookup"><span data-stu-id="d246f-121">Packages</span></span>

<span data-ttu-id="d246f-122">在專案檔中參考[系統 .net. Http. Json](https://www.nuget.org/packages/System.Net.Http.Json/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="d246f-122">Reference the [System.Net.Http.Json](https://www.nuget.org/packages/System.Net.Http.Json/) NuGet package in the project file.</span></span>

## <a name="add-the-httpclient-service"></a><span data-ttu-id="d246f-123">新增 HttpClient 服務</span><span class="sxs-lookup"><span data-stu-id="d246f-123">Add the HttpClient service</span></span>

<span data-ttu-id="d246f-124">在中 `Program.Main` ，新增 `HttpClient` 服務（如果尚未存在）：</span><span class="sxs-lookup"><span data-stu-id="d246f-124">In `Program.Main`, add an `HttpClient` service if it doesn't already exist:</span></span>

```csharp
builder.Services.AddTransient(sp => 
    new HttpClient
    {
        BaseAddress = new Uri(builder.HostEnvironment.BaseAddress)
    });
```

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="d246f-125">HttpClient 和 JSON 協助程式</span><span class="sxs-lookup"><span data-stu-id="d246f-125">HttpClient and JSON helpers</span></span>

<span data-ttu-id="d246f-126">在 Blazor WebAssembly 應用程式中[，HttpClient](xref:fundamentals/http-requests)會當做預先設定的服務提供，以便向源伺服器提出要求。</span><span class="sxs-lookup"><span data-stu-id="d246f-126">In a Blazor WebAssembly app, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span>

<span data-ttu-id="d246f-127">Blazor伺服器應用程式預設不會包含 `HttpClient` 服務。</span><span class="sxs-lookup"><span data-stu-id="d246f-127">A Blazor Server app doesn't include an `HttpClient` service by default.</span></span> <span data-ttu-id="d246f-128">`HttpClient`使用[HttpClient factory 基礎結構](xref:fundamentals/http-requests)，將提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="d246f-128">Provide an `HttpClient` to the app using the [HttpClient factory infrastructure](xref:fundamentals/http-requests).</span></span>

<span data-ttu-id="d246f-129">`HttpClient`和 JSON 協助程式也用來呼叫協力廠商 Web API 端點。</span><span class="sxs-lookup"><span data-stu-id="d246f-129">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="d246f-130">`HttpClient`會使用瀏覽器[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)來執行，並受限於其限制，包括強制執行相同的來源原則。</span><span class="sxs-lookup"><span data-stu-id="d246f-130">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="d246f-131">用戶端的基底位址會設定為源伺服器的位址。</span><span class="sxs-lookup"><span data-stu-id="d246f-131">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="d246f-132">使用指示詞插入 `HttpClient` 實例 `@inject` ：</span><span class="sxs-lookup"><span data-stu-id="d246f-132">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```razor
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="d246f-133">在下列範例中，Todo Web API 處理建立、讀取、更新和刪除（CRUD）作業。</span><span class="sxs-lookup"><span data-stu-id="d246f-133">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="d246f-134">這些範例是以 `TodoItem` 儲存的類別為基礎：</span><span class="sxs-lookup"><span data-stu-id="d246f-134">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="d246f-135">`Id`專案的識別碼（， `long` ） &ndash; 唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="d246f-135">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="d246f-136">`Name`專案的名稱（， `string` ） &ndash; 名稱。</span><span class="sxs-lookup"><span data-stu-id="d246f-136">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="d246f-137">Status （ `IsComplete` ， `bool` ） &ndash; 表示待辦事項是否已完成。</span><span class="sxs-lookup"><span data-stu-id="d246f-137">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="d246f-138">JSON helper 方法會將要求傳送至 URI （下列範例中的 Web API）並處理回應：</span><span class="sxs-lookup"><span data-stu-id="d246f-138">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="d246f-139">`GetFromJsonAsync`傳送 &ndash; HTTP GET 要求，並剖析 JSON 回應主體以建立物件。</span><span class="sxs-lookup"><span data-stu-id="d246f-139">`GetFromJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="d246f-140">在下列程式碼中， `todoItems` 元件會顯示。</span><span class="sxs-lookup"><span data-stu-id="d246f-140">In the following code, the `todoItems` are displayed by the component.</span></span> <span data-ttu-id="d246f-141">`GetTodoItems`當元件完成呈現（[OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)）時，就會觸發方法。</span><span class="sxs-lookup"><span data-stu-id="d246f-141">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)).</span></span> <span data-ttu-id="d246f-142">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="d246f-142">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] todoItems;

      protected override async Task OnInitializedAsync() => 
          todoItems = await Http.GetFromJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <span data-ttu-id="d246f-143">`PostAsJsonAsync`傳送 &ndash; HTTP POST 要求，包括 json 編碼的內容，並剖析 json 回應主體以建立物件。</span><span class="sxs-lookup"><span data-stu-id="d246f-143">`PostAsJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="d246f-144">在下列程式碼中， `newItemName` 是由元件的繫結項目所提供。</span><span class="sxs-lookup"><span data-stu-id="d246f-144">In the following code, `newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="d246f-145">`AddItem`方法是藉由選取元素來觸發 `<button>` 。</span><span class="sxs-lookup"><span data-stu-id="d246f-145">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="d246f-146">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="d246f-146">See the sample app for a complete example.</span></span>

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
  
  <span data-ttu-id="d246f-147">呼叫會傳回 `PostAsJsonAsync` <xref:System.Net.Http.HttpResponseMessage> 。</span><span class="sxs-lookup"><span data-stu-id="d246f-147">Calls to `PostAsJsonAsync` return an <xref:System.Net.Http.HttpResponseMessage>.</span></span> <span data-ttu-id="d246f-148">若要從回應訊息還原序列化 JSON 內容，請使用 `ReadFromJsonAsync<T>` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="d246f-148">To deserialize the JSON content from the response message, use the `ReadFromJsonAsync<T>` extension method:</span></span>
  
  ```csharp
  var content = response.content.ReadFromJsonAsync<WeatherForecast>();
  ```

* <span data-ttu-id="d246f-149">`PutAsJsonAsync`傳送 &ndash; HTTP PUT 要求，包括 JSON 編碼的內容。</span><span class="sxs-lookup"><span data-stu-id="d246f-149">`PutAsJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="d246f-150">在下列程式碼中， `editItem` 和的值 `Name` `IsCompleted` 是由元件的繫結項目所提供。</span><span class="sxs-lookup"><span data-stu-id="d246f-150">In the following code, `editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="d246f-151">`Id`當專案在 UI 的另一個部分中選取並呼叫時，會設定專案的 `EditItem` 。</span><span class="sxs-lookup"><span data-stu-id="d246f-151">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="d246f-152">`SaveItem`方法是藉由選取 Save 元素來觸發 `<button>` 。</span><span class="sxs-lookup"><span data-stu-id="d246f-152">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="d246f-153">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="d246f-153">See the sample app for a complete example.</span></span>

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
          var editItem = todoItems.Single(i => i.Id == id);
          editItem = new TodoItem { Id = editItem.Id, Name = editItem.Name, 
              IsComplete = editItem.IsComplete };
      }

      private async Task SaveItem() =>
          await Http.PutAsJsonAsync($"api/TodoItems/{editItem.Id}, editItem);
  }
  ```
  
  <span data-ttu-id="d246f-154">呼叫會傳回 `PutAsJsonAsync` <xref:System.Net.Http.HttpResponseMessage> 。</span><span class="sxs-lookup"><span data-stu-id="d246f-154">Calls to `PutAsJsonAsync` return an <xref:System.Net.Http.HttpResponseMessage>.</span></span> <span data-ttu-id="d246f-155">若要從回應訊息還原序列化 JSON 內容，請使用 `ReadFromJsonAsync<T>` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="d246f-155">To deserialize the JSON content from the response message, use the `ReadFromJsonAsync<T>` extension method:</span></span>
  
  ```csharp
  var content = response.content.ReadFromJsonAsync<WeatherForecast>();
  ```

<span data-ttu-id="d246f-156"><xref:System.Net.Http>包含用來傳送 HTTP 要求和接收 HTTP 回應的其他擴充方法。</span><span class="sxs-lookup"><span data-stu-id="d246f-156"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="d246f-157">[HttpClient](xref:System.Net.Http.HttpClient.DeleteAsync*)是用來將 HTTP DELETE 要求傳送至 Web API。</span><span class="sxs-lookup"><span data-stu-id="d246f-157">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="d246f-158">在下列程式碼中，Delete `<button>` 元素會呼叫 `DeleteItem` 方法。</span><span class="sxs-lookup"><span data-stu-id="d246f-158">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="d246f-159">綁定 `<input>` 項會提供 `id` 要刪除之專案的。</span><span class="sxs-lookup"><span data-stu-id="d246f-159">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="d246f-160">如需完整範例，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="d246f-160">See the sample app for a complete example.</span></span>

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

## <a name="named-httpclient-with-ihttpclientfactory"></a><span data-ttu-id="d246f-161">名為 HttpClient 與 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="d246f-161">Named HttpClient with IHttpClientFactory</span></span>

<span data-ttu-id="d246f-162"><xref:System.Net.Http.IHttpClientFactory>支援服務和名為的設定 <xref:System.Net.Http.HttpClient> 。</span><span class="sxs-lookup"><span data-stu-id="d246f-162"><xref:System.Net.Http.IHttpClientFactory> services and the configuration of a named <xref:System.Net.Http.HttpClient> are supported.</span></span>

<span data-ttu-id="d246f-163">`Program.Main`（*Program.cs*）：</span><span class="sxs-lookup"><span data-stu-id="d246f-163">`Program.Main` (*Program.cs*):</span></span>

```csharp
builder.Services.AddHttpClient("ServerAPI", client => 
    client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress));
```

<span data-ttu-id="d246f-164">`FetchData`元件（*Pages/FetchData. razor*）：</span><span class="sxs-lookup"><span data-stu-id="d246f-164">`FetchData` component (*Pages/FetchData.razor*):</span></span>

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

## <a name="typed-httpclient"></a><span data-ttu-id="d246f-165">具類型的 HttpClient</span><span class="sxs-lookup"><span data-stu-id="d246f-165">Typed HttpClient</span></span>

<span data-ttu-id="d246f-166">型別會 <xref:System.Net.Http.HttpClient> 使用一個或多個應用程式的 <xref:System.Net.Http.HttpClient> 實例（預設或名為），從一或多個 Web API 端點傳回資料。</span><span class="sxs-lookup"><span data-stu-id="d246f-166">Typed <xref:System.Net.Http.HttpClient> uses one or more of the app's <xref:System.Net.Http.HttpClient> instances, default or named, to return data from one or more web API endpoints.</span></span>

<span data-ttu-id="d246f-167">*WeatherForecastClient.cs*：</span><span class="sxs-lookup"><span data-stu-id="d246f-167">*WeatherForecastClient.cs*:</span></span>

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

<span data-ttu-id="d246f-168">`Program.Main`（*Program.cs*）：</span><span class="sxs-lookup"><span data-stu-id="d246f-168">`Program.Main` (*Program.cs*):</span></span>

```csharp
builder.Services.AddHttpClient<WeatherForecastClient>(client => 
    client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress));
```

<span data-ttu-id="d246f-169">元件會插入具類型的 `HttpClient` 以呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="d246f-169">Components inject the typed `HttpClient` to call the web API.</span></span>

<span data-ttu-id="d246f-170">`FetchData`元件（*Pages/FetchData. razor*）：</span><span class="sxs-lookup"><span data-stu-id="d246f-170">`FetchData` component (*Pages/FetchData.razor*):</span></span>

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

## <a name="handle-errors"></a><span data-ttu-id="d246f-171">處理錯誤</span><span class="sxs-lookup"><span data-stu-id="d246f-171">Handle errors</span></span>

<span data-ttu-id="d246f-172">與 Web API 互動時，如果發生錯誤，就可以由開發人員程式碼來處理。</span><span class="sxs-lookup"><span data-stu-id="d246f-172">When errors occur while interacting with a web API, they can be handled by developer code.</span></span> <span data-ttu-id="d246f-173">例如， `GetFromJsonAsync` 預期來自伺服器 API 的 JSON 回應與 `Content-Type` 的 `application/json` 。</span><span class="sxs-lookup"><span data-stu-id="d246f-173">For example, `GetFromJsonAsync` expects a JSON response from the server API with a `Content-Type` of `application/json`.</span></span> <span data-ttu-id="d246f-174">如果回應不是 JSON 格式，則內容驗證會擲回 <xref:System.NotSupportedException> 。</span><span class="sxs-lookup"><span data-stu-id="d246f-174">If the response isn't in JSON format, content validation throws a <xref:System.NotSupportedException>.</span></span>

<span data-ttu-id="d246f-175">在下列範例中，氣象預報資料要求的 URI 端點拼錯。</span><span class="sxs-lookup"><span data-stu-id="d246f-175">In the following example, the URI endpoint for the weather forecast data request is misspelled.</span></span> <span data-ttu-id="d246f-176">URI 應該是， `WeatherForecast` 但在呼叫中會顯示為 `WeatherForcast` （遺漏 "e"）。</span><span class="sxs-lookup"><span data-stu-id="d246f-176">The URI should be to `WeatherForecast` but appears in the call as `WeatherForcast` (missing "e").</span></span>

<span data-ttu-id="d246f-177">`GetFromJsonAsync`呼叫預期會傳回 JSON，但是伺服器會針對具有之的伺服器上的未處理例外狀況傳回 HTML `Content-Type` `text/html` 。</span><span class="sxs-lookup"><span data-stu-id="d246f-177">The `GetFromJsonAsync` call expects JSON to be returned, but the server returns HTML for an unhandled exception on the server with a `Content-Type` of `text/html`.</span></span> <span data-ttu-id="d246f-178">未處理的例外狀況發生在伺服器上，因為找不到路徑，而且中介軟體無法提供要求的頁面或視圖。</span><span class="sxs-lookup"><span data-stu-id="d246f-178">The unhandled exception occurs on the server because the path isn't found and middleware can't serve a page or view for the request.</span></span>

<span data-ttu-id="d246f-179">在 `OnInitializedAsync` 用戶端上， <xref:System.NotSupportedException> 當回應內容驗證為非 JSON 時，會擲回。</span><span class="sxs-lookup"><span data-stu-id="d246f-179">In `OnInitializedAsync` on the client, <xref:System.NotSupportedException> is thrown when the response content is validated as non-JSON.</span></span> <span data-ttu-id="d246f-180">在區塊中攔截到例外狀況 `catch` ，其中自訂邏輯可以記錄錯誤，或向使用者呈現易記的錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="d246f-180">The exception is caught in the `catch` block, where custom logic could log the error or present a friendly error message to the user:</span></span>

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
> <span data-ttu-id="d246f-181">上述範例是為了示範之用。</span><span class="sxs-lookup"><span data-stu-id="d246f-181">The preceding example is for demonstration purposes.</span></span> <span data-ttu-id="d246f-182">即使端點不存在，或伺服器上發生未處理的例外，您也可以將 Web API 伺服器應用程式設定為傳回 JSON。</span><span class="sxs-lookup"><span data-stu-id="d246f-182">A web API server app can be configured to return JSON even when an endpoint doesn't exist or an unhandled excpetion on the server occurs.</span></span>

<span data-ttu-id="d246f-183">如需詳細資訊，請參閱<xref:blazor/handle-errors>。</span><span class="sxs-lookup"><span data-stu-id="d246f-183">For more information, see <xref:blazor/handle-errors>.</span></span>

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="d246f-184">跨原始來源資源分享（CORS）</span><span class="sxs-lookup"><span data-stu-id="d246f-184">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="d246f-185">瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。</span><span class="sxs-lookup"><span data-stu-id="d246f-185">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="d246f-186">這種限制稱為「*相同來源原則*」。</span><span class="sxs-lookup"><span data-stu-id="d246f-186">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="d246f-187">相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="d246f-187">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="d246f-188">若要將來自瀏覽器的要求傳送至具有不同來源的端點，*端點*必須啟用[跨原始來源資源分享（CORS）](https://www.w3.org/TR/cors/)。</span><span class="sxs-lookup"><span data-stu-id="d246f-188">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="d246f-189">[ Blazor WebAssembly 範例應用程式（BlazorWebAssemblySample）](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)示範如何在呼叫 Web API 元件（*Pages/CallWebAPI RAZOR*）中使用 CORS。</span><span class="sxs-lookup"><span data-stu-id="d246f-189">The [Blazor WebAssembly sample app (BlazorWebAssemblySample)](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="d246f-190">若要允許其他網站對您的應用程式進行跨原始來源資源分享（CORS）要求，請參閱 <xref:security/cors> 。</span><span class="sxs-lookup"><span data-stu-id="d246f-190">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d246f-191">其他資源</span><span class="sxs-lookup"><span data-stu-id="d246f-191">Additional resources</span></span>

* <span data-ttu-id="d246f-192"><xref:security/blazor/webassembly/additional-scenarios>&ndash;包含使用 `HttpClient` 來提出安全 Web API 要求的涵蓋範圍。</span><span class="sxs-lookup"><span data-stu-id="d246f-192"><xref:security/blazor/webassembly/additional-scenarios> &ndash; Includes coverage on using `HttpClient` to make secure web API requests.</span></span>
* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [<span data-ttu-id="d246f-193">Kestrel HTTPS 端點設定</span><span class="sxs-lookup"><span data-stu-id="d246f-193">Kestrel HTTPS endpoint configuration</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [<span data-ttu-id="d246f-194">位於 W3C 的跨原始來源資源分享（CORS）</span><span class="sxs-lookup"><span data-stu-id="d246f-194">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
