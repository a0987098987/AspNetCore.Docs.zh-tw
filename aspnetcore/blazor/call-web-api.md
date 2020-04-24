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
# <a name="call-a-web-api-from-aspnet-core-blazor"></a>從 ASP.NET Core Blazor 呼叫 Web API

By [Luke Latham](https://github.com/guardrex)、 [Daniel Roth](https://github.com/danroth27)和[Juan De la Cruz](https://github.com/juandelacruz23)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) apps 會使用預先`HttpClient`設定的服務來呼叫 web api。 撰寫要求，其中可以包含 JavaScript[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)選項、使用 Blazor JSON helper 或搭配<xref:System.Net.Http.HttpRequestMessage>。 Blazor `HttpClient` WebAssembly apps 中的服務著重于向原始伺服器提出要求。 本主題中的指導方針僅適用于 Blazor WebAssembly apps。

[Blazor 伺服器](xref:blazor/hosting-models#blazor-server)應用程式會使用<xref:System.Net.Http.HttpClient>實例呼叫 web api，通常<xref:System.Net.Http.IHttpClientFactory>是使用來建立。 本主題中的指導方針與 Blazor 伺服器應用程式無關。 開發 Blazor 伺服器應用程式時，請遵循中<xref:fundamentals/http-requests>的指導方針。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)（[如何下載](xref:index#how-to-download-a-sample)） &ndash;選取*BlazorWebAssemblySample*應用程式。

請參閱*BlazorWebAssemblySample*範例應用程式中的下列元件：

* 呼叫 Web API （*Pages/CallWebAPI. razor*）
* HTTP 要求測試器（*元件/HTTPRequestTester razor*）

## <a name="packages"></a>Packages

在專案檔中參考[系統 .net. Http. Json](https://www.nuget.org/packages/System.Net.Http.Json/) NuGet 套件。

## <a name="add-the-httpclient-service"></a>新增 HttpClient 服務

在`Program.Main`中，新增`HttpClient`服務（如果尚未存在）：

```csharp
builder.Services.AddSingleton(
    new HttpClient
    {
        BaseAddress = new Uri(builder.HostEnvironment.BaseAddress)
    });
```

## <a name="httpclient-and-json-helpers"></a>HttpClient 和 JSON 協助程式

在 Blazor WebAssembly 應用程式中， [HttpClient](xref:fundamentals/http-requests)會當做預先設定的服務提供，以便向源伺服器提出要求。

Blazor 伺服器應用程式預設不會`HttpClient`包含服務。 使用`HttpClient` [HttpClient factory 基礎結構](xref:fundamentals/http-requests)，將提供給應用程式。

`HttpClient`和 JSON 協助程式也用來呼叫協力廠商 Web API 端點。 `HttpClient`會使用瀏覽器[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)來執行，並受限於其限制，包括強制執行相同的來源原則。

用戶端的基底位址會設定為源伺服器的位址。 使用`@inject`指示`HttpClient`詞插入實例：

```razor
@using System.Net.Http
@inject HttpClient Http
```

在下列範例中，Todo Web API 處理建立、讀取、更新和刪除（CRUD）作業。 這些範例是以儲存的`TodoItem`類別為基礎：

* 專案的`Id`識別碼`long`（ &ndash; ，）唯一識別碼。
* 專案的`Name`名稱`string`（ &ndash; ，）名稱。
* Status （`IsComplete`， `bool`） &ndash;表示待辦事項是否已完成。

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

JSON helper 方法會將要求傳送至 URI （下列範例中的 Web API）並處理回應：

* `GetFromJsonAsync`&ndash;傳送 HTTP GET 要求，並剖析 JSON 回應主體以建立物件。

  在下列程式碼中， `_todoItems`元件會顯示。 當`GetTodoItems`元件完成呈現（[OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)）時，就會觸發方法。 如需完整範例，請參閱範例應用程式。

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetFromJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* `PostAsJsonAsync`&ndash;傳送 HTTP POST 要求，包括 json 編碼的內容，並剖析 json 回應主體以建立物件。

  在下列程式碼中`_newItemName` ，是由元件的繫結項目所提供。 `AddItem`方法是藉由選取`<button>`元素來觸發。 如需完整範例，請參閱範例應用程式。

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
  
  `PostAsJsonAsync`呼叫會傳回<xref:System.Net.Http.HttpResponseMessage>。 若要從回應訊息還原序列化 JSON 內容，請使用`ReadFromJsonAsync<T>`擴充方法：
  
  ```csharp
  var content = response.content.ReadFromJsonAsync<WeatherForecast>();
  ```

* `PutAsJsonAsync`&ndash;傳送 HTTP PUT 要求，包括 JSON 編碼的內容。

  在下列程式碼中`_editItem` ， `Name`和`IsCompleted`的值是由元件的繫結項目所提供。 當專案在`Id` UI 的另一個部分中選取並`EditItem`呼叫時，會設定專案的。 `SaveItem`方法是藉由選取 Save `<button>`元素來觸發。 如需完整範例，請參閱範例應用程式。

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
  
  `PutAsJsonAsync`呼叫會傳回<xref:System.Net.Http.HttpResponseMessage>。 若要從回應訊息還原序列化 JSON 內容，請使用`ReadFromJsonAsync<T>`擴充方法：
  
  ```csharp
  var content = response.content.ReadFromJsonAsync<WeatherForecast>();
  ```

<xref:System.Net.Http>包含用來傳送 HTTP 要求和接收 HTTP 回應的其他擴充方法。 [HttpClient](xref:System.Net.Http.HttpClient.DeleteAsync*)是用來將 HTTP DELETE 要求傳送至 Web API。

在下列程式碼中，Delete `<button>`元素會呼叫`DeleteItem`方法。 綁定`<input>`項會提供要`id`刪除之專案的。 如需完整範例，請參閱範例應用程式。

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

## <a name="cross-origin-resource-sharing-cors"></a>跨原始來源資源分享（CORS）

瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。 這種限制稱為「*相同來源原則*」。 相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。 若要將來自瀏覽器的要求傳送至具有不同來源的端點，*端點*必須啟用[跨原始來源資源分享（CORS）](https://www.w3.org/TR/cors/)。

[Blazor WebAssembly 範例應用程式（BlazorWebAssemblySample）](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)示範如何在呼叫 Web API 元件（*Pages/CallWebAPI. razor*）中使用 CORS。

若要允許其他網站對您的應用程式進行跨原始來源資源分享（CORS）要求<xref:security/cors>，請參閱。

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a>具有 Fetch API 要求選項的 HttpClient 和 HttpRequestMessage

在 Blazor WebAssembly 應用程式的 WebAssembly 上執行時， [HttpClient](xref:fundamentals/http-requests)請使用<xref:System.Net.Http.HttpRequestMessage> HttpClient 和來自訂要求。 例如，您可以指定要求 URI、HTTP 方法，以及任何所需的要求標頭。

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

.NET WebAssembly 的執行會`HttpClient`使用[WindowOrWorkerGlobalScope。 fetch （）](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch)。 Fetch 可讓您設定數個[要求特有的選項](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)。 

您可以使用`HttpRequestMessage`下表所示的擴充方法來設定 HTTP 提取要求選項。

| `HttpRequestMessage`擴充方法 | Fetch 要求屬性 |
| ------------------------------------- | ---------------------- |
| `SetBrowserRequestCredentials`        | [憑證](https://developer.mozilla.org/docs/Web/API/Request/credentials) |
| `SetBrowserRequestCache`              | [高速](https://developer.mozilla.org/docs/Web/API/Request/cache) |
| `SetBrowserRequestMode`               | [mode](https://developer.mozilla.org/docs/Web/API/Request/mode) |
| `SetBrowserRequestIntegrity`          | [完整性](https://developer.mozilla.org/docs/Web/API/Request/integrity) |

您可以使用更泛型`SetBrowserRequestOption`的擴充方法來設定其他選項。
 
HTTP 回應通常會在Blazor WebAssembly 應用程式中進行緩衝處理，以啟用回應內容的同步讀取支援。 若要啟用回應串流的支援，請`SetBrowserResponseStreamingEnabled`在要求上使用擴充方法。

若要在跨原始來源要求中包含認證，請`SetBrowserRequestCredentials`使用擴充方法：

```csharp
requestMessage.SetBrowserRequestCredentials(BrowserRequestCredentials.Include);
```

如需有關提取 API 選項的詳細資訊，請參閱[MDN web 檔： WindowOrWorkerGlobalScope。 Fetch （）:P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)。

在 CORS 要求上傳送認證（授權 cookie/標頭）時`Authorization` ，cors 原則必須允許標頭。

下列原則包含的設定：

* 要求來源（`http://localhost:5000`、 `https://localhost:5001`）。
* Any 方法（動詞）。
* `Content-Type`和`Authorization`標頭。 若要允許自訂標頭（例如`x-custom-header`），請在呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>時列出標頭。
* 用戶端 JavaScript 程式碼（`credentials`屬性設為`include`）所設定的認證。

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

如需詳細資訊， <xref:security/cors>請參閱和範例應用程式的 HTTP 要求測試器元件（*Components/HTTPRequestTester*）。

## <a name="additional-resources"></a>其他資源

* [要求其他存取權杖](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* [將權杖附加到連出要求](xref:security/blazor/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)
* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [Kestrel HTTPS 端點設定](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [位於 W3C 的跨原始來源資源分享（CORS）](https://www.w3.org/TR/cors/)
