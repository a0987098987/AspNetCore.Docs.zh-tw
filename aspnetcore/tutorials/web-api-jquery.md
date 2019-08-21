---
title: 教學課程：使用 ASP.NET Core 以 jQuery 呼叫 Web API
author: rick-anderson
description: 了解如何使用 jQuery 呼叫 ASP.NET Core Web API。
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2019
uid: tutorials/web-api-jquery
ms.openlocfilehash: a319e4b4ce09e9b09afeaff065d5740276deb115
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022564"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-jquery"></a>教學課程：使用 jQuery 呼叫 ASP.NET Core Web API

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本教學課程會示範如何使用 jQuery 呼叫 ASP.NET Core Web API

::: moniker range="< aspnetcore-3.0"

若為 ASP.NET Core 2.2，請參閱[使用 jQuery 呼叫 Web API](xref:tutorials/first-web-api#call-the-api-with-jquery) 的 2.2 版。

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a>必要條件

* 完成[教學課程：建立 Web API](xref:tutorials/first-web-api)
* 熟悉 CSS、HTML、JavaScript 和 jQuery

## <a name="call-the-api-with-jquery"></a>使用 jQuery 呼叫 API

在本節中，將會新增 HTML 網頁，以使用 jQuery 來呼叫 Web API。 jQuery 會起始要求，並使用來自 API 回應的詳細資料更新頁面。

藉由使用下列反白顯示的程式碼更新 *Startup.cs*，來設定應用程式[提供靜態檔案](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)並[啟用預設檔案對應](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)：

[!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJquery.cs?highlight=8-9&name=snippet_configure)]

在專案目錄中建立 *wwwroot* 資料夾。

將名為 *index.html* 的 HTML 檔案新增至 *wwwroot* 目錄。 將其內容取代為下列標記：

[!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

將名為 *site.js* 的 JavaScript 檔案新增至 *wwwroot* 目錄。 將其內容取代為下列程式碼：

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

若要在本機測試 HTML 網頁，可能需要變更 ASP.NET Core 專案的啟動設定：

* 開啟 *Properties\launchSettings.json*。
* 移除 `launchUrl` 屬性，以強制應用程式於 *index.html* 處開啟 &mdash; 專案的預設檔案。

您可以使用幾種方式來取得 jQuery。 在上述的程式碼片段中，從 CDN 載入程式庫。

此範例會呼叫 API 的所有 CRUD 方法。 以下是關於呼叫 API 的說明。

### <a name="get-a-list-of-to-do-items"></a>取得待辦事項的清單

JQuery [ajax](https://api.jquery.com/jquery.ajax/) 函式會將 `GET` 要求傳送至 API，API 則會傳回代表待辦事項陣列的 JSON。 如果要求成功，則會叫用 `success` 回呼函式。 在回呼中，DOM 已使用待辦事項資訊進行更新。

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>新增待辦事項

[ajax](https://api.jquery.com/jquery.ajax/) 函式會傳送 `POST` 要求，並在要求本文中包含待辦事項。 `accepts` 和 `contentType` 選項都設定為 `application/json`，以指定接收和傳送的媒體類型。 待辦事項會使用 [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 轉換成 JSON。 當 API 傳回成功狀態碼時，會叫用 `getData` 函式來更新 HTML 資料表。

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>更新待辦事項

更新待辦事項類似於新增待辦事項。 `url` 會變更為新增項目的唯一識別碼，而 `type` 是 `PUT`。

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>刪除待辦事項

刪除待辦事項的達成方法是將 AJAX 呼叫的 `type` 設定為 `DELETE`，並在 URL 中指定項目的唯一識別碼。

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

前進到下一個教學課程來了解如何產生 API 說明頁面：

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
::: moniker-end
