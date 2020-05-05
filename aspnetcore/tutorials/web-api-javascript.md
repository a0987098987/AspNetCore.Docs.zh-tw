---
title: 教學課程：使用 JavaScript 呼叫 ASP.NET Core Web API
author: rick-anderson
description: 了解如何使用 JavaScript 呼叫 ASP.NET Core Web API。
ms.author: riande
ms.custom: mvc
ms.date: 11/26/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/web-api-javascript
ms.openlocfilehash: c3eb003812a31d8cf3168453fcc11601ffba19fb
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82774349"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-javascript"></a>教學課程：使用 JavaScript 呼叫 ASP.NET Core Web API

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

此教學課程會示範如何使用 JavaScript (使用 [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) 呼叫 ASP.NET Core Web API。

::: moniker range="< aspnetcore-3.0"

針對 ASP.NET Core 2.2，請參閱[使用 JavaScript 呼叫 Web API](xref:tutorials/first-web-api#call-the-web-api-with-javascript) 的 2.2 版。

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a>必要條件

* 完成[教學課程：建立 Web API](xref:tutorials/first-web-api)
* 熟悉 CSS、HTML 和 JavaScript

## <a name="call-the-web-api-with-javascript"></a>使用 JavaScript 呼叫 Web API

在此節中，您會新增一個 HTML 網頁，其中包含用於建立及管理待辦事項的表單。 事件處理常式會附加至頁面上的元素。 事件處理常式會產生對 Web API 的動作方法發出的 HTTP 要求。 Fetch API 的 `fetch` 函式會起始每個 HTTP 要求。

函式會傳回[承諾](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)物件，其中包含表示為`Response`物件的 HTTP 回應。 `fetch` 常見的模式是叫用 `Response` 物件上的 `json` 函式，以擷取 JSON 回應主體。 JavaScript 會使用來自 Web API 回應的詳細資料來更新頁面。

最簡單 `fetch` 呼叫會接受代表路由的單一參數。 第二個參數 (稱為 `init` 物件) 是選擇性的。 `init` 是用來設定 HTTP 要求。

1. 設定應用程式來[提供靜態](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)檔案，並[啟用預設檔案對應](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)。 *Startup.cs* 的 `Configure` 方法中需要下列反白顯示的程式碼：

    [!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJavaScript.cs?highlight=8-9&name=snippet_configure)]

1. 在專案根目錄中建立*wwwroot*資料夾。

1. 在*wwwroot*資料夾內建立*js*資料夾。

1. 將名為*index* .html 的 HTML 檔案新增至*wwwroot*資料夾。 將*index*的內容取代為下列標記：

    [!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

1. 將名為*site .js*的 JavaScript 檔案新增至*wwwroot/js*資料夾。 將*site .js*的內容取代為下列程式碼：

    [!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_SiteJs)]

若要在本機測試 HTML 網頁，可能需要變更 ASP.NET Core 專案的啟動設定：

1. 開啟 *Properties\launchSettings.json*。
1. 請移除`launchUrl`屬性，以強制應用程式在*index. html*&mdash;開啟專案的預設檔案。

此範例會呼叫 Web API 的所有 CRUD 方法。 以下是關於 Web API 要求的說明。

### <a name="get-a-list-of-to-do-items"></a>取得待辦事項的清單

在下列程式碼中，會將 HTTP GET 要求傳送至 *api/TodoItems* 路由：

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_GetItems)]

當 Web API 傳回成功狀態碼時，會叫用 `_displayItems` 函式。 `_displayItems` 所接受之陣列參數中的每個待辦事項，都會加入具有 [編輯]**** 和 [刪除]**** 按鈕的表格。 如果 Web API 要求失敗，則會在瀏覽器的主控台中記錄錯誤。

### <a name="add-a-to-do-item"></a>新增待辦事項

在下列程式碼中：

* 系統會宣告 `item` 變數來建構待辦事項的物件常值表示法。
* 您可以使用下列選項來設定 Fetch 要求：
  * `method`&mdash;指定 POST HTTP 動作動詞命令。
  * `body`&mdash;指定要求本文的 JSON 表示法。 JSON 是透過將儲存在 `item` 中的物件常值傳遞至 [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 函式來產生。
  * `headers`&mdash;指定 `Accept` 和 `Content-Type` HTTP 要求標題。 這兩個標題都設定為 `application/json`，以分別指定接收和傳送的媒體類型。
* HTTP POST 要求會傳送至 *api/TodoItems* 路由。

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_AddItem)]

當 Web API 傳回成功狀態碼時，會叫用 `getItems` 函式來更新 HTML 表格 。 如果 Web API 要求失敗，則會在瀏覽器的主控台中記錄錯誤。

### <a name="update-a-to-do-item"></a>更新待辦事項

更新待辦事項類似於新增待辦事項；不過，有兩個重大差異：

* 路由的尾碼為要更新之項目的唯一識別碼。 例如，*api/TodoItems/1*。
* HTTP 動作動詞命令是 PUT，如 `method` 選項所指示。

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_UpdateItem)]

### <a name="delete-a-to-do-item"></a>刪除待辦事項

若要刪除待辦事項，請將要求的 `method` 選項設定為 `DELETE`，並在 URL 中指定項目的唯一識別碼。

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_DeleteItem)]

前進到下一個教學課程來了解如何產生 Web API 說明頁面：

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>

::: moniker-end
