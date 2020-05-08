---
title: 從 ASP.NET Core Blazor WebAssembly 呼叫 Web API
author: guardrex
description: 瞭解如何使用 JSON helper 從Blazor WebAssembly 應用程式呼叫 Web API，包括建立跨原始來源資源分享（CORS）要求。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/07/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/call-web-api
ms.openlocfilehash: 7476e9dce3fa26948d2091235747f893d805d7be
ms.sourcegitcommit: 84b46594f57608f6ac4f0570172c7051df507520
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82967268"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a>從 ASP.NET Core 呼叫 Web APIBlazor

By [Luke Latham](https://github.com/guardrex)、 [Daniel Roth](https://github.com/danroth27)和[Juan De la Cruz](https://github.com/juandelacruz23)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

WebAssembly apps 會使用預先`HttpClient`設定的服務來呼叫 web api。 [ Blazor ](xref:blazor/hosting-models#blazor-webassembly) 撰寫要求，其中可以包含 JavaScript[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)選項、使用Blazor JSON helper 或搭配<xref:System.Net.Http.HttpRequestMessage>。 WebAssembly `HttpClient` apps 中Blazor的服務著重于向原始伺服器提出要求。 本主題中的指導方針僅適用Blazor于 WebAssembly apps。

伺服器應用程式會使用<xref:System.Net.Http.HttpClient>實例呼叫 web api，通常<xref:System.Net.Http.IHttpClientFactory>是使用來建立。 [ Blazor ](xref:blazor/hosting-models#blazor-server) 本主題中的指導方針與Blazor伺服器應用程式無關。 開發Blazor伺服器應用程式時，請遵循中<xref:fundamentals/http-requests>的指導方針。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)（[如何下載](xref:index#how-to-download-a-sample)） &ndash;選取*BlazorWebAssemblySample*應用程式。

請參閱*BlazorWebAssemblySample*範例應用程式中的下列元件：

* 呼叫 Web API （*Pages/CallWebAPI. razor*）
* HTTP 要求測試器（*元件/HTTPRequestTester razor*）

## <a name="packages"></a>套件

在專案檔中參考[系統 .net. Http. Json](https://www.nuget.org/packages/System.Net.Http.Json/) NuGet 套件。

## <a name="add-the-httpclient-service"></a>新增 HttpClient 服務

在`Program.Main`中，新增`HttpClient`服務（如果尚未存在）：

```csharp
builder.Services.AddTransient(sp => 
    new HttpClient
    {
        BaseAddress = new Uri(builder.HostEnvironment.BaseAddress)
    });
```

## <a name="httpclient-and-json-helpers"></a>HttpClient 和 JSON 協助程式

在Blazor WebAssembly 應用程式中， [HttpClient](xref:fundamentals/http-requests)會當做預先設定的服務提供，以便向源伺服器提出要求。

Blazor伺服器應用程式預設不會`HttpClient`包含服務。 使用`HttpClient` [HttpClient factory 基礎結構](xref:fundamentals/http-requests)，將提供給應用程式。

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

  在下列程式碼中， `todoItems`元件會顯示。 當`GetTodoItems`元件完成呈現（[OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)）時，就會觸發方法。 如需完整範例，請參閱範例應用程式。

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] todoItems;

      protected override async Task OnInitializedAsync() => 
          todoItems = await Http.GetFromJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* `PostAsJsonAsync`&ndash;傳送 HTTP POST 要求，包括 json 編碼的內容，並剖析 json 回應主體以建立物件。

  在下列程式碼中`newItemName` ，是由元件的繫結項目所提供。 `AddItem`方法是藉由選取`<button>`元素來觸發。 如需完整範例，請參閱範例應用程式。

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
  
  `PostAsJsonAsync`呼叫會傳回<xref:System.Net.Http.HttpResponseMessage>。 若要從回應訊息還原序列化 JSON 內容，請使用`ReadFromJsonAsync<T>`擴充方法：
  
  ```csharp
  var content = response.content.ReadFromJsonAsync<WeatherForecast>();
  ```

* `PutAsJsonAsync`&ndash;傳送 HTTP PUT 要求，包括 JSON 編碼的內容。

  在下列程式碼中`editItem` ， `Name`和`IsCompleted`的值是由元件的繫結項目所提供。 當專案在`Id` UI 的另一個部分中選取並`EditItem`呼叫時，會設定專案的。 `SaveItem`方法是藉由選取 Save `<button>`元素來觸發。 如需完整範例，請參閱範例應用程式。

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
  
  `PutAsJsonAsync`呼叫會傳回<xref:System.Net.Http.HttpResponseMessage>。 若要從回應訊息還原序列化 JSON 內容，請使用`ReadFromJsonAsync<T>`擴充方法：
  
  ```csharp
  var content = response.content.ReadFromJsonAsync<WeatherForecast>();
  ```

<xref:System.Net.Http>包含用來傳送 HTTP 要求和接收 HTTP 回應的其他擴充方法。 [HttpClient](xref:System.Net.Http.HttpClient.DeleteAsync*)是用來將 HTTP DELETE 要求傳送至 Web API。

在下列程式碼中，Delete `<button>`元素會呼叫`DeleteItem`方法。 綁定`<input>`項會提供要`id`刪除之專案的。 如需完整範例，請參閱範例應用程式。

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

## <a name="handle-errors"></a>處理錯誤

與 Web API 互動時，如果發生錯誤，就可以由開發人員程式碼來處理。 例如， `GetFromJsonAsync`預期來自伺服器 API 的 JSON 回應與`Content-Type`的。 `application/json` 如果回應不是 JSON 格式，則內容驗證會擲<xref:System.NotSupportedException>回。

在下列範例中，氣象預報資料要求的 URI 端點拼錯。 URI 應該是， `WeatherForecast`但在呼叫中會顯示為`WeatherForcast` （遺漏 "e"）。

`GetFromJsonAsync`呼叫預期會傳回 JSON，但是伺服器會針對具有`Content-Type`之的`text/html`伺服器上的未處理例外狀況傳回 HTML。 未處理的例外狀況發生在伺服器上，因為找不到路徑，而且中介軟體無法提供要求的頁面或視圖。

在`OnInitializedAsync`用戶端上， <xref:System.NotSupportedException>當回應內容驗證為非 JSON 時，會擲回。 在`catch`區塊中攔截到例外狀況，其中自訂邏輯可以記錄錯誤，或向使用者呈現易記的錯誤訊息：

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
> 上述範例是為了示範之用。 即使端點不存在，或伺服器上發生未處理的例外，您也可以將 Web API 伺服器應用程式設定為傳回 JSON。

如需詳細資訊，請參閱<xref:blazor/handle-errors>。

## <a name="cross-origin-resource-sharing-cors"></a>跨原始來源資源分享（CORS）

瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。 這種限制稱為「*相同來源原則*」。 相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。 若要將來自瀏覽器的要求傳送至具有不同來源的端點，*端點*必須啟用[跨原始來源資源分享（CORS）](https://www.w3.org/TR/cors/)。

[WebAssembly 範例應用程式（BlazorWebAssemblySample）示範如何在呼叫 Web API 元件（Pages/CallWebAPI razor）中使用 CORS。 Blazor ](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) *Pages/CallWebAPI.razor*

若要允許其他網站對您的應用程式進行跨原始來源資源分享（CORS）要求<xref:security/cors>，請參閱。

## <a name="additional-resources"></a>其他資源

* <xref:security/blazor/webassembly/index>
* <xref:security/blazor/webassembly/additional-scenarios>
* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [Kestrel HTTPS 端點設定](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [位於 W3C 的跨原始來源資源分享（CORS）](https://www.w3.org/TR/cors/)
