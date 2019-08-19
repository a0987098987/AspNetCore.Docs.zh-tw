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
# <a name="call-a-web-api-from-aspnet-core-blazor"></a>從 ASP.NET Core Blazor 呼叫 Web API

By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)

Blazor 用戶端應用程式會使用預先`HttpClient`設定的服務來呼叫 web api。 撰寫要求, 其中可以包含 JavaScript[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)選項、使用 Blazor JSON helper 或<xref:System.Net.Http.HttpRequestMessage>搭配。

Blazor 伺服器端應用程式會使用通常使用<xref:System.Net.Http.HttpClient> <xref:System.Net.Http.IHttpClientFactory>建立的實例來呼叫 web api。 如需詳細資訊，請參閱 <xref:fundamentals/http-requests>。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

如需 Blazor 用戶端範例, 請參閱範例應用程式中的下列元件:

* 呼叫 Web API (*Pages/CallWebAPI. razor*)
* HTTP 要求測試器 (*元件/HTTPRequestTester razor*)

## <a name="httpclient-and-json-helpers"></a>HttpClient 和 JSON 協助程式

在 Blazor 用戶端應用程式中, [HttpClient](xref:fundamentals/http-requests)會當做預先設定的服務提供, 以便向源伺服器提出要求。 若要`HttpClient`使用 JSON helper, 請將套件參考`Microsoft.AspNetCore.Blazor.HttpClient`新增至。 `HttpClient`和 JSON 協助程式也用來呼叫協力廠商 Web API 端點。 `HttpClient`會使用瀏覽器[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)來執行, 並受限於其限制, 包括強制執行相同的來源原則。

用戶端的基底位址會設定為源伺服器的位址。 使用指示詞插入`HttpClient`實例 `@inject` :

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

在下列範例中, Todo Web API 處理建立、讀取、更新和刪除 (CRUD) 作業。 這些範例是以儲存的`TodoItem`類別為基礎:

* 專案的`Id`識別碼`long`( &ndash; ,) 唯一識別碼。
* 專案的`Name`名稱`string`( &ndash; ,) 名稱。
* Status (`IsComplete`, `bool`) &ndash;表示待辦事項是否已完成。

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

JSON helper 方法會將要求傳送至 URI (下列範例中的 Web API) 並處理回應:

* `GetJsonAsync`&ndash;傳送 HTTP GET 要求, 並剖析 JSON 回應主體以建立物件。

  在下列程式碼中, `_todoItems`元件會顯示。 當`GetTodoItems`元件完成呈現 ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)) 時, 就會觸發方法。 如需完整範例, 請參閱範例應用程式。

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/todo");
  }
  ```

* `PostJsonAsync`&ndash;傳送 HTTP POST 要求, 包括 json 編碼的內容, 並剖析 json 回應主體以建立物件。

  在下列程式碼中`_newItemName` , 是由元件的繫結項目所提供。 方法是藉由`<button>`選取元素來觸發。 `AddItem` 如需完整範例, 請參閱範例應用程式。

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

* `PutJsonAsync`&ndash;傳送 HTTP PUT 要求, 包括 JSON 編碼的內容。

  在下列程式碼中`_editItem` , `Name`和`IsCompleted`的值是由元件的繫結項目所提供。 當專案在`Id` UI 的另一個部分中選取並`EditItem`呼叫時, 會設定專案的。 方法是藉由選取 Save `<button>`元素來觸發。 `SaveItem` 如需完整範例, 請參閱範例應用程式。

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

<xref:System.Net.Http>包含用來傳送 HTTP 要求和接收 HTTP 回應的其他擴充方法。 [HttpClient](xref:System.Net.Http.HttpClient.DeleteAsync*)是用來將 HTTP DELETE 要求傳送至 Web API。

在下列程式碼中, Delete `<button>`元素會`DeleteItem`呼叫方法。 綁定`<input>`項會`id`提供要刪除之專案的。 如需完整範例, 請參閱範例應用程式。

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

## <a name="cross-origin-resource-sharing-cors"></a>跨原始來源資源分享 (CORS)

瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。 這種限制稱為「*相同來源原則*」。 相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。 若要將來自瀏覽器的要求傳送至具有不同來源的端點,*端點*必須啟用[跨原始來源資源分享 (CORS)](https://www.w3.org/TR/cors/)。

範例應用程式會示範如何在呼叫 Web API 元件 (*Pages/CallWebAPI razor*) 中使用 CORS。

若要允許其他網站對您的應用程式進行跨原始來源資源分享 (CORS) 要求<xref:security/cors>, 請參閱。

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a>具有 Fetch API 要求選項的 HttpClient 和 HttpRequestMessage

在 Blazor 用戶端應用程式的 WebAssembly 上執行時, 請 [HttpClient](xref:fundamentals/http-requests) 使用 <xref:System.Net.Http.HttpRequestMessage>和來自訂要求。 例如, 您可以指定要求 URI、HTTP 方法, 以及任何所需的要求標頭。

使用要求上的`WebAssemblyHttpMessageHandler.FetchArgs`屬性, 將要求選項提供給基礎 JavaScript[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API) 。 如下列範例所示, `credentials`屬性會設定為下列任何值:

* `FetchCredentialsOption.Include`(「包含」)&ndash;建議瀏覽器傳送認證 (例如 cookie 或 HTTP 驗證標頭), 即使是跨原始來源要求也一樣。 只有在 CORS 原則設定為允許認證時才允許。
* `FetchCredentialsOption.Omit`(「省略」)&ndash;建議瀏覽器永遠不會傳送認證 (例如 cookie 或 HTTP 驗證標頭)。
* `FetchCredentialsOption.SameOrigin`(「相同來源」)&ndash;只有當目標 URL 與呼叫應用程式位於相同的來源時, 才會建議瀏覽器傳送認證 (例如 cookie 或 HTTP 驗證標頭)。

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

如需有關 Fetch API 選項的詳細資訊[, 請參閱 MDN web 檔:WindowOrWorkerGlobalScope ():P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)。

在 cors 要求上傳送認證 (授權 cookie/標頭) 時`Authorization` , cors 原則必須允許標頭。

下列原則包含的設定:

* 要求來源 (`http://localhost:5000`、 `https://localhost:5001`)。
* Any 方法 (動詞)。
* `Content-Type`和`Authorization`標頭。 若要允許自訂標頭 ( `x-custom-header`例如), 請在呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>時列出標頭。
* 用戶端 JavaScript 程式碼 (`credentials`屬性設為`include`) 所設定的認證。

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

如需詳細資訊, <xref:security/cors>請參閱和範例應用程式的 HTTP 要求測試器元件 (*Components/HTTPRequestTester*)。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/http-requests>
* [位於 W3C 的跨原始來源資源分享 (CORS)](https://www.w3.org/TR/cors/)
