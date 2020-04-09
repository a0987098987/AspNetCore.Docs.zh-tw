---
title: 從ASP.NET核心呼叫 Web APIBlazor
author: guardrex
description: 瞭解如何使用 JSON 説明Blazor器從 應用呼叫 Web API,包括進行跨源資源分享 (CORS) 請求。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-web-api
ms.openlocfilehash: e6996f0e6731b05038d0a9329152b8afd5f6796d
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78660143"
---
# <a name="call-a-web-api-from-aspnet-core-opno-locblazor"></a>從ASP.NET核心呼叫 Web APIBlazor

由[盧克·萊瑟姆](https://github.com/guardrex),[丹尼爾·羅斯](https://github.com/danroth27)和[胡安·德拉克魯茲](https://github.com/juandelacruz23)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Web組裝應用使用預配置`HttpClient`的服務調用 Web [ Blazor ](xref:blazor/hosting-models#blazor-webassembly) API。 撰寫要求,其中可能包括 JavaScript[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)Blazor選項<xref:System.Net.Http.HttpRequestMessage>、使用 JSON 說明器或與 。 Blazor WebAssembly 應用`HttpClient`中 的服務側重於將請求發回源伺服器。 本主題中的指南僅與BlazorWeb 組裝應用相關。

伺服器應用使用<xref:System.Net.Http.HttpClient>實例調用 Web API,通常使用<xref:System.Net.Http.IHttpClientFactory>[Blazor](xref:blazor/hosting-models#blazor-server)創建實例。 本主題中的指南與Blazor伺服器應用無關。 開發Blazor伺服器應用時,請按照<xref:fundamentals/http-requests>中的指南操作。

[查看或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)([如何下載](xref:index#how-to-download-a-sample))&ndash;選擇*BlazorWebAssemblySample*應用程式。

請參閱*BlazorWebAssembly 範例*應用程式中的以下元件:

* 呼叫 Web API (*頁面/呼叫 WebAPI.razor*)
* HTTP 要求測試器 (*元件/ HTTPRequestTester.razor*)

## <a name="packages"></a>Packages

參考*實驗性*[微軟.AspNetCore.Blazor. . . .HTTPClient](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/) NuGet 套件在專案檔中。 `Microsoft.AspNetCore.Blazor.HttpClient`基於`HttpClient`與[系統.Text.Json](https://www.nuget.org/packages/System.Text.Json/)。

要使用穩定的 API,請使用[Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)套件,該程式使用[Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)/[Json.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm)。 在 中`Microsoft.AspNet.WebApi.Client`使用穩定的 API 不會提供本主題中描述的 JSON`Microsoft.AspNetCore.Blazor.HttpClient`説明程式,這是實驗 包獨有的。

## <a name="httpclient-and-json-helpers"></a>HTTPClient 和 JSON 說明人員

在BlazorWebAssembly 應用中[,HttpClient](xref:fundamentals/http-requests)可作為預先設定的服務,用於將請求發回源伺服器。

默認情況下Blazor,伺服器應用不`HttpClient`包括 服務。 使用`HttpClient`[HTTPClient 工廠基礎結構](xref:fundamentals/http-requests)向應用提供 。

`HttpClient`JSON 幫助器也用於調用第三方 Web API 終結點。 `HttpClient`使用瀏覽器 Fetch [API](https://developer.mozilla.org/docs/Web/API/Fetch_API)實現,並受其限制,包括實施相同的源策略。

用戶端的基本位址設置為原始伺服器的位址。 使用`@inject``HttpClient`指令注入實體:

```razor
@using System.Net.Http
@inject HttpClient Http
```

在以下範例中,Todo Web API 行程創建、讀取、更新和刪除 (CRUD) 操作。 這些範例以儲存`TodoItem`以下項目的類別:

* 項的`Id``long`識別&ndash;碼 ( 、 ) 唯一 ID。
* 項的名稱`Name``string` &ndash; ( 、 ) 項目名稱。
* 狀態`IsComplete` `bool` &ndash; ( 、 ) 指示"Dodo" 項目是否已完成。

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

JSON 說明器方法將要求傳送到 URI(以下範例中的 Web API)並處理回應:

* `GetJsonAsync`&ndash;發送 HTTP GET 請求並分析 JSON 回應正文以創建物件。

  在以下代碼中,`_todoItems`元件顯示 。 `GetTodoItems`當元件完成渲染時([初始化 Async),](xref:blazor/lifecycle#component-initialization-methods)將觸發該方法。 有關完整示例,請參閱示例應用。

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* `PostJsonAsync`&ndash;發送 HTTP POST 請求(包括 JSON 編碼的內容),並分析 JSON 回應正文以創建物件。

  在以下代碼中,`_newItemName`由元件的綁定元素提供。 該方法`AddItem`透過選擇元素觸`<button>`發 。 有關完整示例,請參閱示例應用。

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

* `PutJsonAsync`&ndash;發送 HTTP PUT 請求,包括 JSON 編碼的內容。

  在以下代碼中,`_editItem``IsCompleted``Name`和的值由元件的綁定元素提供。 當在`Id`UI 的另一部分選擇`EditItem`項並 調用該專案時,將設置項。 該方法`SaveItem`通過選擇"保存`<button>`" 元素觸發。 有關完整示例,請參閱示例應用。

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

<xref:System.Net.Http>包括用於發送 HTTP 請求和接收 HTTP 回應的其他擴充方法。 [HTTPClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*)用於向 Web API 發送 HTTP DELETE 請求。

在以下代碼中,Delete`<button>`元素調`DeleteItem`用 方法。 綁定`<input>`元素提供要`id`刪除的項。 有關完整示例,請參閱示例應用。

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

## <a name="cross-origin-resource-sharing-cors"></a>跨源資源分享 (CORS)

流覽器安全性可防止網頁向與服務網頁的域不同的域發出請求。 此限制稱為*同源原則*。 同源策略可防止惡意網站從其他網站讀取敏感數據。 要從瀏覽器向具有不同源的終結點發出請求,*終結點*必須啟用[跨源資源分享 (CORS)。](https://www.w3.org/TR/cors/)

[WebAssembly 範例應用程式 (BlazorWebAssemblySample) 展示在呼叫 Web API 元件(頁面/CallWebAPI.razor)中Blazor使用 CORS。](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) *Pages/CallWebAPI.razor*

要允許其他網站向應用程式發出跨源資源分享 (CORS) 要求,請參<xref:security/cors>閱 。

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a>使用取得 API 要求選項的 HTTPClient 與 HTTPRequestMessage

在BlazorWebAssembly 應用中在 WebAssembly 上運行<xref:System.Net.Http.HttpRequestMessage>時,請使用[HttpClient](xref:fundamentals/http-requests)並自訂請求。 例如,您可以指定請求 URI、HTTP 方法以及任何所需的請求標頭。

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

有關取得 API 選項的詳細資訊,請參閱[MDN Web 文件:WindowOrWorkerGlobalScope.fetch():P參數](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)。

在 CORS 請求上發送認證(授權 Cookie/標`Authorization`頭) 時,CORS 策略必須允許標頭。

以下政策包括以下設定:

* 要求原點`http://localhost:5000`(、 ... `https://localhost:5001`
* 任何方法(動詞)。
* `Content-Type`和`Authorization`標題。 要允許自訂標頭(例如),`x-custom-header`請列出調<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>用 時的標題。
* 由用戶端 JavaScript`credentials`代碼 (`include`屬性設置為 ) 設置的認證。

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

有關詳細資訊,請參閱<xref:security/cors>和範例應用的 HTTP 請求測試元件 (*元件/ HTTPRequestTester.razor*)。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [凱斯特雷爾 HTTPS 端點設定](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [W3C 的跨源資源分享 (CORS)](https://www.w3.org/TR/cors/)
