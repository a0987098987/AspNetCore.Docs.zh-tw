---
title: 從 ASP.NET Core 呼叫 Web API Blazor
author: guardrex
description: 瞭解如何使用 JSON helper 從 Blazor 應用程式呼叫 Web API，包括建立跨原始來源資源分享（CORS）要求。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-web-api
ms.openlocfilehash: 345fb6962e3376c22551eb7914c70c89cb7100d5
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213271"
---
# <a name="call-a-web-api-from-aspnet-core-opno-locblazor"></a>從 ASP.NET Core 呼叫 Web API Blazor

By [Luke Latham](https://github.com/guardrex)、 [Daniel Roth](https://github.com/danroth27)和[Juan De la Cruz](https://github.com/juandelacruz23)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) apps 會使用預先設定的 `HttpClient` 服務來呼叫 web api。 撰寫要求，其中可包含 JavaScript[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)選項、使用 Blazor JSON helper 或 <xref:System.Net.Http.HttpRequestMessage>。 Blazor WebAssembly 應用程式中的 `HttpClient` 服務，著重于向原始伺服器提出要求。 本主題中的指導方針僅適用于 Blazor WebAssembly 應用程式。

[Blazor 伺服器](xref:blazor/hosting-models#blazor-server)應用程式會使用 <xref:System.Net.Http.HttpClient> 實例呼叫 web api，通常是使用 <xref:System.Net.Http.IHttpClientFactory>來建立的。 本主題中的指導方針與 Blazor 伺服器應用程式無關。 在開發 Blazor 伺服器應用程式時，請遵循 <xref:fundamentals/http-requests>中的指引。

[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)（[如何下載](xref:index#how-to-download-a-sample)） &ndash; 選取*BlazorWebAssemblySample*應用程式。

請參閱*BlazorWebAssemblySample*範例應用程式中的下列元件：

* 呼叫 Web API （*Pages/CallWebAPI. razor*）
* HTTP 要求測試器（*元件/HTTPRequestTester razor*）

## <a name="packages"></a>Packages

參考*實驗*性[AspNetCore.Blazor。HttpClient](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/)專案檔中的 NuGet 套件。 `Microsoft.AspNetCore.Blazor.HttpClient` 是以 `HttpClient` 和[system.web](https://www.nuget.org/packages/System.Text.Json/)為基礎。

若要使用穩定的 API，請使用[WebApi. 用戶端](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)套件，其使用[Newtonsoft](https://www.nuget.org/packages/Newtonsoft.Json/)/[Json.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm)。 在 `Microsoft.AspNet.WebApi.Client` 中使用穩定 API，並不會提供本主題中所述的 JSON helper，這對實驗性 `Microsoft.AspNetCore.Blazor.HttpClient` 套件而言是唯一的。

## <a name="httpclient-and-json-helpers"></a>HttpClient 和 JSON 協助程式

在 Blazor WebAssembly 應用程式中， [HttpClient](xref:fundamentals/http-requests)是以預先設定的服務形式提供，可讓要求回到源伺服器。

Blazor 伺服器應用程式預設不包含 `HttpClient` 服務。 使用[HttpClient factory 基礎結構](xref:fundamentals/http-requests)提供應用程式的 `HttpClient`。

`HttpClient` 和 JSON 協助程式也用來呼叫協力廠商 Web API 端點。 `HttpClient` 是使用瀏覽器[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)來執行，並受限於其限制，包括強制執行相同的來源原則。

用戶端的基底位址會設定為源伺服器的位址。 使用 `@inject` 指示詞插入 `HttpClient` 實例：

```razor
@using System.Net.Http
@inject HttpClient Http
```

在下列範例中，Todo Web API 處理建立、讀取、更新和刪除（CRUD）作業。 這些範例是以儲存的 `TodoItem` 類別為基礎：

* 識別碼（`Id`，`long`） &ndash; 專案的唯一識別碼。
* 專案的名稱（`Name`、`string`） &ndash; 名稱。
* 如果 Todo 專案已完成，則為狀態（`IsComplete`、`bool`） &ndash; 指示。

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

JSON helper 方法會將要求傳送至 URI （下列範例中的 Web API）並處理回應：

* `GetJsonAsync` &ndash; 傳送 HTTP GET 要求，並剖析 JSON 回應主體以建立物件。

  在下列程式碼中，元件會顯示 `_todoItems`。 當元件完成呈現（[OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)）時，就會觸發 `GetTodoItems` 方法。 如需完整範例，請參閱範例應用程式。

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* `PostJsonAsync` &ndash; 傳送 HTTP POST 要求，包括 JSON 編碼的內容，並剖析 JSON 回應主體以建立物件。

  在下列程式碼中，`_newItemName` 是由元件的繫結項目所提供。 `AddItem` 方法是藉由選取 `<button>` 元素來觸發。 如需完整範例，請參閱範例應用程式。

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
          await Http.PostJsonAsync("api/TodoItems", addItem);
      }
  }
  ```

* `PutJsonAsync` &ndash; 傳送 HTTP PUT 要求，包括 JSON 編碼的內容。

  在下列程式碼中，`Name` 和 `IsCompleted` 的 `_editItem` 值是由元件的繫結項目所提供。 當專案在 UI 的另一個部分中選取，且呼叫 `EditItem` 時，會設定專案的 `Id`。 藉由選取 [儲存 `<button>`] 元素，即可觸發 `SaveItem` 方法。 如需完整範例，請參閱範例應用程式。

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
          await Http.PutJsonAsync($"api/TodoItems/{_editItem.Id}, _editItem);
  }
  ```

<xref:System.Net.Http> 包含傳送 HTTP 要求和接收 HTTP 回應的其他擴充方法。 [HttpClient](xref:System.Net.Http.HttpClient.DeleteAsync*)是用來將 HTTP DELETE 要求傳送至 Web API。

在下列程式碼中，Delete `<button>` 元素會呼叫 `DeleteItem` 方法。 系結的 `<input>` 元素會提供要刪除之專案的 `id`。 如需完整範例，請參閱範例應用程式。

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

[Blazor WebAssembly 範例應用程式（BlazorWebAssemblySample）](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)示範如何在呼叫 Web API 元件（*Pages/CallWebAPI razor*）中使用 CORS。

若要允許其他網站對您的應用程式進行跨原始來源資源分享（CORS）要求，請參閱 <xref:security/cors>。

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a>具有 Fetch API 要求選項的 HttpClient 和 HttpRequestMessage

在 Blazor WebAssembly 應用程式的 WebAssembly 上執行時，請使用[HttpClient](xref:fundamentals/http-requests)和 <xref:System.Net.Http.HttpRequestMessage> 來自訂要求。 例如，您可以指定要求 URI、HTTP 方法，以及任何所需的要求標頭。

```razor
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

如需有關提取 API 選項的詳細資訊，請參閱[MDN web 檔： WindowOrWorkerGlobalScope。 Fetch （）:P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)。

在 CORS 要求上傳送認證（授權 cookie/標頭）時，CORS 原則必須允許 `Authorization` 標頭。

下列原則包含的設定：

* 要求來源（`http://localhost:5000`、`https://localhost:5001`）。
* Any 方法（動詞）。
* `Content-Type` 和 `Authorization` 標頭。 若要允許自訂標頭（例如 `x-custom-header`），請在呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>時列出標頭。
* 用戶端 JavaScript 程式碼（`credentials` 屬性設定為 `include`）所設定的認證。

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

如需詳細資訊，請參閱 <xref:security/cors> 和範例應用程式的 HTTP 要求測試器元件（*Components/HTTPRequestTester*）。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [Kestrel HTTPS 端點設定](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [位於 W3C 的跨原始來源資源分享（CORS）](https://www.w3.org/TR/cors/)
