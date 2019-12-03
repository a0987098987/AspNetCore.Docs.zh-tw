---
title: 從 ASP.NET Core 呼叫 Web API Blazor
author: guardrex
description: 瞭解如何使用 JSON helper 從 Blazor 應用程式呼叫 Web API，包括建立跨原始來源資源分享（CORS）要求。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/23/2019
no-loc:
- Blazor
uid: blazor/call-web-api
ms.openlocfilehash: ffc9904c5746fbf0fafa10cf054666608942650c
ms.sourcegitcommit: 0dd224b2b7efca1fda0041b5c3f45080327033f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2019
ms.locfileid: "74680898"
---
# <a name="call-a-web-api-from-aspnet-core-opno-locblazor"></a>從 ASP.NET Core 呼叫 Web API Blazor

By [Luke Latham](https://github.com/guardrex)、 [Daniel Roth](https://github.com/danroth27)和[Juan De la Cruz](https://github.com/juandelacruz23)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor WebAssembly apps 會使用預先設定的 `HttpClient` 服務來呼叫 web Api。 撰寫要求，其中可包含 JavaScript[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)選項、使用 Blazor JSON helper 或 <xref:System.Net.Http.HttpRequestMessage>。

Blazor 伺服器應用程式會使用通常使用 <xref:System.Net.Http.IHttpClientFactory>建立的 <xref:System.Net.Http.HttpClient> 實例來呼叫 web Api。 如需詳細資訊，請參閱<xref:fundamentals/http-requests>。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

如 Blazor WebAssembly 範例，請參閱範例應用程式中的下列元件：

* 呼叫 Web API （*Pages/CallWebAPI. razor*）
* HTTP 要求測試器（*元件/HTTPRequestTester razor*）

## <a name="httpclient-and-json-helpers"></a>HttpClient 和 JSON 協助程式

在 Blazor WebAssembly apps 中， [HttpClient](xref:fundamentals/http-requests)會當做預先設定的服務提供，讓要求回到源伺服器。 若要使用 `HttpClient` JSON helper，請將套件參考新增至 `Microsoft.AspNetCore.Blazor.HttpClient`。 `HttpClient` 和 JSON 協助程式也用來呼叫協力廠商 Web API 端點。 `HttpClient` 是使用瀏覽器[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)來執行，並受限於其限制，包括強制執行相同的來源原則。

用戶端的基底位址會設定為源伺服器的位址。 使用 `@inject` 指示詞插入 `HttpClient` 實例：

```cshtml
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

  ```cshtml
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

* `PutJsonAsync` &ndash; 傳送 HTTP PUT 要求，包括 JSON 編碼的內容。

  在下列程式碼中，`Name` 和 `IsCompleted` 的 `_editItem` 值是由元件的繫結項目所提供。 當專案在 UI 的另一個部分中選取，且呼叫 `EditItem` 時，會設定專案的 `Id`。 藉由選取 [儲存 `<button>`] 元素，即可觸發 `SaveItem` 方法。 如需完整範例，請參閱範例應用程式。

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

<xref:System.Net.Http> 包含傳送 HTTP 要求和接收 HTTP 回應的其他擴充方法。 [HttpClient](xref:System.Net.Http.HttpClient.DeleteAsync*)是用來將 HTTP DELETE 要求傳送至 Web API。

在下列程式碼中，Delete `<button>` 元素會呼叫 `DeleteItem` 方法。 系結的 `<input>` 元素會提供要刪除之專案的 `id`。 如需完整範例，請參閱範例應用程式。

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

## <a name="cross-origin-resource-sharing-cors"></a>跨原始來源資源分享（CORS）

瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。 這種限制稱為「*相同來源原則*」。 相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。 若要將來自瀏覽器的要求傳送至具有不同來源的端點，*端點*必須啟用[跨原始來源資源分享（CORS）](https://www.w3.org/TR/cors/)。

範例應用程式會示範如何在呼叫 Web API 元件（*Pages/CallWebAPI razor*）中使用 CORS。

若要允許其他網站對您的應用程式進行跨原始來源資源分享（CORS）要求，請參閱 <xref:security/cors>。

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a>具有 Fetch API 要求選項的 HttpClient 和 HttpRequestMessage

在 Blazor WebAssembly 應用程式的 WebAssembly 上執行時，請使用[HttpClient](xref:fundamentals/http-requests)和 <xref:System.Net.Http.HttpRequestMessage> 來自訂要求。 例如，您可以指定要求 URI、HTTP 方法，以及任何所需的要求標頭。

使用要求上的 `WebAssemblyHttpMessageHandler.FetchArgs` 屬性，將要求選項提供給基礎 JavaScript[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API) 。 如下列範例所示，`credentials` 屬性會設定為下列任何值：

* `FetchCredentialsOption.Include` （"include"） &ndash; 會建議瀏覽器傳送認證（例如 cookie 或 HTTP 驗證標頭），即使是跨原始來源要求也一樣。 只有在 CORS 原則設定為允許認證時才允許。
* `FetchCredentialsOption.Omit` （「省略」） &ndash; 會建議瀏覽器永遠不會傳送認證（例如 cookie 或 HTTP 驗證標頭）。
* `FetchCredentialsOption.SameOrigin` （「相同來源」） &ndash; 只有在目標 URL 與呼叫應用程式位於相同的來源時，才會建議瀏覽器傳送認證（例如 cookie 或 HTTP 驗證標頭）。

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
