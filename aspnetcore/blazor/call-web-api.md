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
# <a name="call-a-web-api-from-aspnet-core-opno-locblazor"></a><span data-ttu-id="2914e-103">從ASP.NET核心呼叫 Web APIBlazor</span><span class="sxs-lookup"><span data-stu-id="2914e-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="2914e-104">由[盧克·萊瑟姆](https://github.com/guardrex),[丹尼爾·羅斯](https://github.com/danroth27)和[胡安·德拉克魯茲](https://github.com/juandelacruz23)</span><span class="sxs-lookup"><span data-stu-id="2914e-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Juan De la Cruz](https://github.com/juandelacruz23)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="2914e-105">Web組裝應用使用預配置`HttpClient`的服務調用 Web [ Blazor ](xref:blazor/hosting-models#blazor-webassembly) API。</span><span class="sxs-lookup"><span data-stu-id="2914e-105">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="2914e-106">撰寫要求,其中可能包括 JavaScript[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)Blazor選項<xref:System.Net.Http.HttpRequestMessage>、使用 JSON 說明器或與 。</span><span class="sxs-lookup"><span data-stu-id="2914e-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span> <span data-ttu-id="2914e-107">Blazor WebAssembly 應用`HttpClient`中 的服務側重於將請求發回源伺服器。</span><span class="sxs-lookup"><span data-stu-id="2914e-107">The `HttpClient` service in Blazor WebAssembly apps is focused on making requests back to the server of origin.</span></span> <span data-ttu-id="2914e-108">本主題中的指南僅與BlazorWeb 組裝應用相關。</span><span class="sxs-lookup"><span data-stu-id="2914e-108">The guidance in this topic only pertains to Blazor WebAssembly apps.</span></span>

<span data-ttu-id="2914e-109">伺服器應用使用<xref:System.Net.Http.HttpClient>實例調用 Web API,通常使用<xref:System.Net.Http.IHttpClientFactory>[Blazor](xref:blazor/hosting-models#blazor-server)創建實例。</span><span class="sxs-lookup"><span data-stu-id="2914e-109">[Blazor Server](xref:blazor/hosting-models#blazor-server) apps call web APIs using <xref:System.Net.Http.HttpClient> instances, typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="2914e-110">本主題中的指南與Blazor伺服器應用無關。</span><span class="sxs-lookup"><span data-stu-id="2914e-110">The guidance in this topic doesn't pertain to Blazor Server apps.</span></span> <span data-ttu-id="2914e-111">開發Blazor伺服器應用時,請按照<xref:fundamentals/http-requests>中的指南操作。</span><span class="sxs-lookup"><span data-stu-id="2914e-111">When developing Blazor Server apps, follow the guidance in <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="2914e-112">[查看或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)([如何下載](xref:index#how-to-download-a-sample))&ndash;選擇*BlazorWebAssemblySample*應用程式。</span><span class="sxs-lookup"><span data-stu-id="2914e-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample)) &ndash; Select the *BlazorWebAssemblySample* app.</span></span>

<span data-ttu-id="2914e-113">請參閱*BlazorWebAssembly 範例*應用程式中的以下元件:</span><span class="sxs-lookup"><span data-stu-id="2914e-113">See the following components in the *BlazorWebAssemblySample* sample app:</span></span>

* <span data-ttu-id="2914e-114">呼叫 Web API (*頁面/呼叫 WebAPI.razor*)</span><span class="sxs-lookup"><span data-stu-id="2914e-114">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="2914e-115">HTTP 要求測試器 (*元件/ HTTPRequestTester.razor*)</span><span class="sxs-lookup"><span data-stu-id="2914e-115">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="packages"></a><span data-ttu-id="2914e-116">Packages</span><span class="sxs-lookup"><span data-stu-id="2914e-116">Packages</span></span>

<span data-ttu-id="2914e-117">參考*實驗性*[微軟.AspNetCore.Blazor. . . .HTTPClient](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/) NuGet 套件在專案檔中。</span><span class="sxs-lookup"><span data-stu-id="2914e-117">Reference the *experimental* [Microsoft.AspNetCore.Blazor.HttpClient](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/) NuGet package in the project file.</span></span> <span data-ttu-id="2914e-118">`Microsoft.AspNetCore.Blazor.HttpClient`基於`HttpClient`與[系統.Text.Json](https://www.nuget.org/packages/System.Text.Json/)。</span><span class="sxs-lookup"><span data-stu-id="2914e-118">`Microsoft.AspNetCore.Blazor.HttpClient` is based on `HttpClient` and [System.Text.Json](https://www.nuget.org/packages/System.Text.Json/).</span></span>

<span data-ttu-id="2914e-119">要使用穩定的 API,請使用[Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)套件,該程式使用[Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)/[Json.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm)。</span><span class="sxs-lookup"><span data-stu-id="2914e-119">To use a stable API, use the [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) package, which uses [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)/[Json.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span> <span data-ttu-id="2914e-120">在 中`Microsoft.AspNet.WebApi.Client`使用穩定的 API 不會提供本主題中描述的 JSON`Microsoft.AspNetCore.Blazor.HttpClient`説明程式,這是實驗 包獨有的。</span><span class="sxs-lookup"><span data-stu-id="2914e-120">Using the stable API in `Microsoft.AspNet.WebApi.Client` doesn't provide the JSON helpers described in this topic, which are unique to the experimental `Microsoft.AspNetCore.Blazor.HttpClient` package.</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="2914e-121">HTTPClient 和 JSON 說明人員</span><span class="sxs-lookup"><span data-stu-id="2914e-121">HttpClient and JSON helpers</span></span>

<span data-ttu-id="2914e-122">在BlazorWebAssembly 應用中[,HttpClient](xref:fundamentals/http-requests)可作為預先設定的服務,用於將請求發回源伺服器。</span><span class="sxs-lookup"><span data-stu-id="2914e-122">In a Blazor WebAssembly app, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span>

<span data-ttu-id="2914e-123">默認情況下Blazor,伺服器應用不`HttpClient`包括 服務。</span><span class="sxs-lookup"><span data-stu-id="2914e-123">A Blazor Server app doesn't include an `HttpClient` service by default.</span></span> <span data-ttu-id="2914e-124">使用`HttpClient`[HTTPClient 工廠基礎結構](xref:fundamentals/http-requests)向應用提供 。</span><span class="sxs-lookup"><span data-stu-id="2914e-124">Provide an `HttpClient` to the app using the [HttpClient factory infrastructure](xref:fundamentals/http-requests).</span></span>

<span data-ttu-id="2914e-125">`HttpClient`JSON 幫助器也用於調用第三方 Web API 終結點。</span><span class="sxs-lookup"><span data-stu-id="2914e-125">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="2914e-126">`HttpClient`使用瀏覽器 Fetch [API](https://developer.mozilla.org/docs/Web/API/Fetch_API)實現,並受其限制,包括實施相同的源策略。</span><span class="sxs-lookup"><span data-stu-id="2914e-126">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="2914e-127">用戶端的基本位址設置為原始伺服器的位址。</span><span class="sxs-lookup"><span data-stu-id="2914e-127">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="2914e-128">使用`@inject``HttpClient`指令注入實體:</span><span class="sxs-lookup"><span data-stu-id="2914e-128">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```razor
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="2914e-129">在以下範例中,Todo Web API 行程創建、讀取、更新和刪除 (CRUD) 操作。</span><span class="sxs-lookup"><span data-stu-id="2914e-129">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="2914e-130">這些範例以儲存`TodoItem`以下項目的類別:</span><span class="sxs-lookup"><span data-stu-id="2914e-130">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="2914e-131">項的`Id``long`識別&ndash;碼 ( 、 ) 唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="2914e-131">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="2914e-132">項的名稱`Name``string` &ndash; ( 、 ) 項目名稱。</span><span class="sxs-lookup"><span data-stu-id="2914e-132">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="2914e-133">狀態`IsComplete` `bool` &ndash; ( 、 ) 指示"Dodo" 項目是否已完成。</span><span class="sxs-lookup"><span data-stu-id="2914e-133">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="2914e-134">JSON 說明器方法將要求傳送到 URI(以下範例中的 Web API)並處理回應:</span><span class="sxs-lookup"><span data-stu-id="2914e-134">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="2914e-135">`GetJsonAsync`&ndash;發送 HTTP GET 請求並分析 JSON 回應正文以創建物件。</span><span class="sxs-lookup"><span data-stu-id="2914e-135">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="2914e-136">在以下代碼中,`_todoItems`元件顯示 。</span><span class="sxs-lookup"><span data-stu-id="2914e-136">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="2914e-137">`GetTodoItems`當元件完成渲染時([初始化 Async),](xref:blazor/lifecycle#component-initialization-methods)將觸發該方法。</span><span class="sxs-lookup"><span data-stu-id="2914e-137">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)).</span></span> <span data-ttu-id="2914e-138">有關完整示例,請參閱示例應用。</span><span class="sxs-lookup"><span data-stu-id="2914e-138">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <span data-ttu-id="2914e-139">`PostJsonAsync`&ndash;發送 HTTP POST 請求(包括 JSON 編碼的內容),並分析 JSON 回應正文以創建物件。</span><span class="sxs-lookup"><span data-stu-id="2914e-139">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="2914e-140">在以下代碼中,`_newItemName`由元件的綁定元素提供。</span><span class="sxs-lookup"><span data-stu-id="2914e-140">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="2914e-141">該方法`AddItem`透過選擇元素觸`<button>`發 。</span><span class="sxs-lookup"><span data-stu-id="2914e-141">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="2914e-142">有關完整示例,請參閱示例應用。</span><span class="sxs-lookup"><span data-stu-id="2914e-142">See the sample app for a complete example.</span></span>

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

* <span data-ttu-id="2914e-143">`PutJsonAsync`&ndash;發送 HTTP PUT 請求,包括 JSON 編碼的內容。</span><span class="sxs-lookup"><span data-stu-id="2914e-143">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="2914e-144">在以下代碼中,`_editItem``IsCompleted``Name`和的值由元件的綁定元素提供。</span><span class="sxs-lookup"><span data-stu-id="2914e-144">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="2914e-145">當在`Id`UI 的另一部分選擇`EditItem`項並 調用該專案時,將設置項。</span><span class="sxs-lookup"><span data-stu-id="2914e-145">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="2914e-146">該方法`SaveItem`通過選擇"保存`<button>`" 元素觸發。</span><span class="sxs-lookup"><span data-stu-id="2914e-146">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="2914e-147">有關完整示例,請參閱示例應用。</span><span class="sxs-lookup"><span data-stu-id="2914e-147">See the sample app for a complete example.</span></span>

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

<span data-ttu-id="2914e-148"><xref:System.Net.Http>包括用於發送 HTTP 請求和接收 HTTP 回應的其他擴充方法。</span><span class="sxs-lookup"><span data-stu-id="2914e-148"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="2914e-149">[HTTPClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*)用於向 Web API 發送 HTTP DELETE 請求。</span><span class="sxs-lookup"><span data-stu-id="2914e-149">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="2914e-150">在以下代碼中,Delete`<button>`元素調`DeleteItem`用 方法。</span><span class="sxs-lookup"><span data-stu-id="2914e-150">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="2914e-151">綁定`<input>`元素提供要`id`刪除的項。</span><span class="sxs-lookup"><span data-stu-id="2914e-151">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="2914e-152">有關完整示例,請參閱示例應用。</span><span class="sxs-lookup"><span data-stu-id="2914e-152">See the sample app for a complete example.</span></span>

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

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="2914e-153">跨源資源分享 (CORS)</span><span class="sxs-lookup"><span data-stu-id="2914e-153">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="2914e-154">流覽器安全性可防止網頁向與服務網頁的域不同的域發出請求。</span><span class="sxs-lookup"><span data-stu-id="2914e-154">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="2914e-155">此限制稱為*同源原則*。</span><span class="sxs-lookup"><span data-stu-id="2914e-155">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="2914e-156">同源策略可防止惡意網站從其他網站讀取敏感數據。</span><span class="sxs-lookup"><span data-stu-id="2914e-156">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="2914e-157">要從瀏覽器向具有不同源的終結點發出請求,*終結點*必須啟用[跨源資源分享 (CORS)。](https://www.w3.org/TR/cors/)</span><span class="sxs-lookup"><span data-stu-id="2914e-157">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="2914e-158">[WebAssembly 範例應用程式 (BlazorWebAssemblySample) 展示在呼叫 Web API 元件(頁面/CallWebAPI.razor)中Blazor使用 CORS。](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) *Pages/CallWebAPI.razor*</span><span class="sxs-lookup"><span data-stu-id="2914e-158">The [Blazor WebAssembly sample app (BlazorWebAssemblySample)](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="2914e-159">要允許其他網站向應用程式發出跨源資源分享 (CORS) 要求,請參<xref:security/cors>閱 。</span><span class="sxs-lookup"><span data-stu-id="2914e-159">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="2914e-160">使用取得 API 要求選項的 HTTPClient 與 HTTPRequestMessage</span><span class="sxs-lookup"><span data-stu-id="2914e-160">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="2914e-161">在BlazorWebAssembly 應用中在 WebAssembly 上運行<xref:System.Net.Http.HttpRequestMessage>時,請使用[HttpClient](xref:fundamentals/http-requests)並自訂請求。</span><span class="sxs-lookup"><span data-stu-id="2914e-161">When running on WebAssembly in a Blazor WebAssembly app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="2914e-162">例如,您可以指定請求 URI、HTTP 方法以及任何所需的請求標頭。</span><span class="sxs-lookup"><span data-stu-id="2914e-162">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

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

<span data-ttu-id="2914e-163">有關取得 API 選項的詳細資訊,請參閱[MDN Web 文件:WindowOrWorkerGlobalScope.fetch():P參數](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)。</span><span class="sxs-lookup"><span data-stu-id="2914e-163">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="2914e-164">在 CORS 請求上發送認證(授權 Cookie/標`Authorization`頭) 時,CORS 策略必須允許標頭。</span><span class="sxs-lookup"><span data-stu-id="2914e-164">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="2914e-165">以下政策包括以下設定:</span><span class="sxs-lookup"><span data-stu-id="2914e-165">The following policy includes configuration for:</span></span>

* <span data-ttu-id="2914e-166">要求原點`http://localhost:5000`(、 ... `https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="2914e-166">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="2914e-167">任何方法(動詞)。</span><span class="sxs-lookup"><span data-stu-id="2914e-167">Any method (verb).</span></span>
* <span data-ttu-id="2914e-168">`Content-Type`和`Authorization`標題。</span><span class="sxs-lookup"><span data-stu-id="2914e-168">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="2914e-169">要允許自訂標頭(例如),`x-custom-header`請列出調<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>用 時的標題。</span><span class="sxs-lookup"><span data-stu-id="2914e-169">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="2914e-170">由用戶端 JavaScript`credentials`代碼 (`include`屬性設置為 ) 設置的認證。</span><span class="sxs-lookup"><span data-stu-id="2914e-170">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="2914e-171">有關詳細資訊,請參閱<xref:security/cors>和範例應用的 HTTP 請求測試元件 (*元件/ HTTPRequestTester.razor*)。</span><span class="sxs-lookup"><span data-stu-id="2914e-171">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2914e-172">其他資源</span><span class="sxs-lookup"><span data-stu-id="2914e-172">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [<span data-ttu-id="2914e-173">凱斯特雷爾 HTTPS 端點設定</span><span class="sxs-lookup"><span data-stu-id="2914e-173">Kestrel HTTPS endpoint configuration</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [<span data-ttu-id="2914e-174">W3C 的跨源資源分享 (CORS)</span><span class="sxs-lookup"><span data-stu-id="2914e-174">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
