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
ms.openlocfilehash: 1417056beac99a8dfee47131c2cb6ab7ec52ad1e
ms.sourcegitcommit: 384833762c614851db653b841cc09fbc944da463
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86445264"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a>從 ASP.NET Core 呼叫 Web APIBlazor

By [Luke Latham](https://github.com/guardrex)、 [Daniel Roth](https://github.com/danroth27)和[Juan De la Cruz](https://github.com/juandelacruz23)

> [!NOTE]
> 本主題適用于 Blazor WebAssembly 。 [Blazor Server](xref:blazor/hosting-models#blazor-server)應用程式會使用實例呼叫 web Api <xref:System.Net.Http.HttpClient> ，通常使用建立 <xref:System.Net.Http.IHttpClientFactory> 。 如需適用于的指引 Blazor Server ，請參閱 <xref:fundamentals/http-requests> 。

[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)應用程式會使用預先設定的服務來呼叫 web Api <xref:System.Net.Http.HttpClient> 。 撰寫要求，其中可以包含 JavaScript[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)選項、使用 Blazor JSON helper 或搭配 <xref:System.Net.Http.HttpRequestMessage> 。 <xref:System.Net.Http.HttpClient>應用程式中的服務著重于將 Blazor WebAssembly 要求傳回給來源伺服器。 本主題中的指導方針僅適用于 Blazor WebAssembly 應用程式。

[View or 下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)（[如何下載](xref:index#how-to-download-a-sample)）：選取 `BlazorWebAssemblySample` 應用程式。

請參閱範例應用程式中的下列元件 `BlazorWebAssemblySample` ：

* 呼叫 Web API （ `Pages/CallWebAPI.razor` ）
* HTTP 要求測試器（ `Components/HTTPRequestTester.razor` ）

## <a name="packages"></a>套件

參考 [`System.Net.Http.Json`](https://www.nuget.org/packages/System.Net.Http.Json/) 專案檔中的 NuGet 套件。

## <a name="add-the-httpclient-service"></a>新增 HttpClient 服務

在中 `Program.Main` ，新增 <xref:System.Net.Http.HttpClient> 服務（如果尚未存在）：

```csharp
builder.Services.AddScoped(sp => 
    new HttpClient
    {
        BaseAddress = new Uri(builder.HostEnvironment.BaseAddress)
    });
```

## <a name="httpclient-and-json-helpers"></a>HttpClient 和 JSON 協助程式

在 Blazor WebAssembly 應用程式中， [`HttpClient`](xref:fundamentals/http-requests) 會以預先設定的服務形式提供，讓要求回到源伺服器。

Blazor Server應用程式預設不會包含 <xref:System.Net.Http.HttpClient> 服務。 <xref:System.Net.Http.HttpClient>使用[ `HttpClient` factory 基礎結構](xref:fundamentals/http-requests)，將提供給應用程式。

<xref:System.Net.Http.HttpClient>和 JSON 協助程式也用來呼叫協力廠商 Web API 端點。 <xref:System.Net.Http.HttpClient>會使用瀏覽器[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)來執行，並受限於其限制，包括強制執行相同的來源原則。

用戶端的基底位址會設定為源伺服器的位址。 使用指示詞插入 <xref:System.Net.Http.HttpClient> 實例 [`@inject`](xref:mvc/views/razor#inject) ：

```razor
@using System.Net.Http
@inject HttpClient Http
```

在下列範例中，Todo Web API 處理建立、讀取、更新和刪除（CRUD）作業。 這些範例是以 `TodoItem` 儲存的類別為基礎：

* ID （ `Id` ， `long` ）：專案的唯一識別碼。
* 名稱（ `Name` ， `string` ）：專案的名稱。
* Status （ `IsComplete` ， `bool` ）：表示待辦事項是否已完成。

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

JSON helper 方法會將要求傳送至 URI （下列範例中的 Web API）並處理回應：

* <xref:System.Net.Http.Json.HttpClientJsonExtensions.GetFromJsonAsync%2A>：傳送 HTTP GET 要求，並剖析 JSON 回應主體以建立物件。

  在下列程式碼中， `todoItems` 元件會顯示。 `GetTodoItems`當元件完成呈現（）時，就會觸發方法 [`OnInitializedAsync`](xref:blazor/components/lifecycle#component-initialization-methods) 。 如需完整範例，請參閱範例應用程式。

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] todoItems;

      protected override async Task OnInitializedAsync() => 
          todoItems = await Http.GetFromJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <xref:System.Net.Http.Json.HttpClientJsonExtensions.PostAsJsonAsync%2A>：傳送 HTTP POST 要求，包括 JSON 編碼的內容，並剖析 JSON 回應主體以建立物件。

  在下列程式碼中， `newItemName` 是由元件的繫結項目所提供。 `AddItem`方法是藉由選取元素來觸發 `<button>` 。 如需完整範例，請參閱範例應用程式。

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
  
  呼叫會傳回 <xref:System.Net.Http.Json.HttpClientJsonExtensions.PostAsJsonAsync%2A> <xref:System.Net.Http.HttpResponseMessage> 。 若要從回應訊息還原序列化 JSON 內容，請使用 `ReadFromJsonAsync<T>` 擴充方法：
  
  ```csharp
  var content = response.Content.ReadFromJsonAsync<WeatherForecast>();
  ```

* <xref:System.Net.Http.Json.HttpClientJsonExtensions.PutAsJsonAsync%2A>：傳送 HTTP PUT 要求，包括 JSON 編碼的內容。

  在下列程式碼中， `editItem` 和的值 `Name` `IsCompleted` 是由元件的繫結項目所提供。 `Id`當專案在 UI 的另一個部分中選取並呼叫時，會設定專案的 `EditItem` 。 `SaveItem`方法是藉由選取 Save 元素來觸發 `<button>` 。 如需完整範例，請參閱範例應用程式。

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
  
  呼叫會傳回 <xref:System.Net.Http.Json.HttpClientJsonExtensions.PutAsJsonAsync%2A> <xref:System.Net.Http.HttpResponseMessage> 。 若要從回應訊息還原序列化 JSON 內容，請使用 <xref:System.Net.Http.Json.HttpContentJsonExtensions.ReadFromJsonAsync%2A> 擴充方法：
  
  ```csharp
  var content = response.Content.ReadFromJsonAsync<WeatherForecast>();
  ```

<xref:System.Net.Http>包含用來傳送 HTTP 要求和接收 HTTP 回應的其他擴充方法。 <xref:System.Net.Http.HttpClient.DeleteAsync%2A?displayProperty=nameWithType>是用來將 HTTP DELETE 要求傳送至 Web API。

在下列程式碼中，Delete `<button>` 元素會呼叫 `DeleteItem` 方法。 綁定 `<input>` 項會提供 `id` 要刪除之專案的。 如需完整範例，請參閱範例應用程式。

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

## <a name="named-httpclient-with-ihttpclientfactory"></a>名為 HttpClient 與 IHttpClientFactory

<xref:System.Net.Http.IHttpClientFactory>支援服務和名為的設定 <xref:System.Net.Http.HttpClient> 。

參考 [`Microsoft.Extensions.Http`](https://www.nuget.org/packages/Microsoft.Extensions.Http/) 專案檔中的 NuGet 套件。

`Program.Main` (`Program.cs`):

```csharp
builder.Services.AddHttpClient("ServerAPI", client => 
    client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress));
```

`FetchData`component （ `Pages/FetchData.razor` ）：

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

## <a name="typed-httpclient"></a>具類型的 HttpClient

型別會 <xref:System.Net.Http.HttpClient> 使用一個或多個應用程式的 <xref:System.Net.Http.HttpClient> 實例（預設或名為），從一或多個 Web API 端點傳回資料。

`WeatherForecastClient.cs`:

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

`Program.Main` (`Program.cs`):

```csharp
builder.Services.AddHttpClient<WeatherForecastClient>(client => 
    client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress));
```

元件會插入具類型的 <xref:System.Net.Http.HttpClient> 以呼叫 Web API。

`FetchData`component （ `Pages/FetchData.razor` ）：

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

## <a name="handle-errors"></a>處理錯誤

與 Web API 互動時，如果發生錯誤，就可以由開發人員程式碼來處理。 例如， <xref:System.Net.Http.Json.HttpClientJsonExtensions.GetFromJsonAsync%2A> 預期來自伺服器 API 的 JSON 回應與 `Content-Type` 的 `application/json` 。 如果回應不是 JSON 格式，則內容驗證會擲回 <xref:System.NotSupportedException> 。

在下列範例中，氣象預報資料要求的 URI 端點拼錯。 URI 應該是， `WeatherForecast` 但在呼叫中會顯示為 `WeatherForcast` （遺漏 "e"）。

<xref:System.Net.Http.Json.HttpClientJsonExtensions.GetFromJsonAsync%2A>呼叫預期會傳回 JSON，但是伺服器會針對具有之的伺服器上的未處理例外狀況傳回 HTML `Content-Type` `text/html` 。 未處理的例外狀況發生在伺服器上，因為找不到路徑，而且中介軟體無法提供要求的頁面或視圖。

在 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> 用戶端上， <xref:System.NotSupportedException> 當回應內容驗證為非 JSON 時，會擲回。 在區塊中攔截到例外狀況 `catch` ，其中自訂邏輯可以記錄錯誤，或向使用者呈現易記的錯誤訊息：

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
> 上述範例是為了示範之用。 即使端點不存在或伺服器上發生未處理的例外狀況，Web API 伺服器應用程式也可以設定為傳回 JSON。

如需詳細資訊，請參閱 <xref:blazor/fundamentals/handle-errors> 。

## <a name="cross-origin-resource-sharing-cors"></a>跨原始來源資源分享（CORS）

瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。 這種限制稱為「*相同來源原則*」。 相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。 若要將來自瀏覽器的要求傳送至具有不同來源的端點，*端點*必須啟用[跨原始來源資源分享（CORS）](https://www.w3.org/TR/cors/)。

[ Blazor WebAssembly 範例應用程式（ Blazor WebAssemblySample）](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)示範如何在呼叫 Web API 元件（）中使用 CORS `Pages/CallWebAPI.razor` 。

如需有關在應用程式中使用安全要求之 CORS 的詳細資訊 Blazor ，請參閱 <xref:blazor/security/webassembly/additional-scenarios#cross-origin-resource-sharing-cors> 。

如需有關 ASP.NET Core 應用程式之 CORS 的一般資訊，請參閱 <xref:security/cors> 。

## <a name="additional-resources"></a>其他資源

* <xref:blazor/security/webassembly/additional-scenarios>：包含使用 <xref:System.Net.Http.HttpClient> 來提出安全 Web API 要求的涵蓋範圍。
* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [Kestrel HTTPS 端點設定](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [位於 W3C 的跨原始來源資源分享（CORS）](https://www.w3.org/TR/cors/)
