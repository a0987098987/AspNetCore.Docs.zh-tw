---
title: 使用 ASP.NET Core 和 Visual Studio for Mac 建立 Web API
author: rick-anderson
description: 使用 ASP.NET Core MVC 和 Visual Studio for Mac 建立 Web API
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 40f9bd9c57b97826edfddeb00cb4fb38a026d46e
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011621"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a>使用 ASP.NET Core 和 Visual Studio for Mac 建立 Web API

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson) 提供

在本教學課程中，請建置 Web API 來管理「待辦事項」項目清單。 不會建構 UI。

本教學課程有 3 個版本：

* macOS：使用 Visual Studio for Mac 建立 Web API (本教學課程)
* Windows：[使用 Visual Studio for Windows 建立 Web API](xref:tutorials/first-web-api)
* macOS、Linux、Windows：[使用 Visual Studio Code 建立 Web API](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

如需使用持續性資料庫的範例，請參閱 [ macOS 或 Linux 上的 ASP.NET Core MVC 簡介](xref:tutorials/first-mvc-app-xplat/index)。

## <a name="prerequisites"></a>必要條件

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a>建立專案

從 Visual Studio 中，選取 [檔案] > [新增方案]。

![macOS 新增方案](first-web-api-mac/_static/sln.png)

選取 [.NET Core 應用程式] > [ASP.NET Core Web API] > [下一步]。

![macOS [新增專案] 對話方塊](first-web-api-mac/_static/1.png)

為 [專案名稱] 輸入 *TodoApi*，然後按一下 [建立]。

![設定對話方塊](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>啟動應用程式

在 Visual Studio 中，選取 [執行] > [啟動並偵錯] 來啟動應用程式。 Visual Studio 會啟動瀏覽器並巡覽至 `http://localhost:5000`。 您收到 HTTP 404 (找不到) 錯誤。 將 URL 變更為 `http://localhost:<port>/api/values`。 隨即顯示 `ValuesController` 資料：

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>新增 Entity Framework Core 的支援

安裝 [Entity Framework Core InMemory](/ef/core/providers/in-memory/) 資料庫提供者。 此資料庫提供者可讓 Entity Framework Core 搭配使用記憶體內部資料庫。

* 從 [專案] 功能表中，選取 [新增 NuGet 套件]。

  * 或者，您可以用滑鼠右鍵按一下 [相依性]，然後選取 [新增套件]。

* 在搜尋方塊中輸入 `EntityFrameworkCore.InMemory`。
* 選取 `Microsoft.EntityFrameworkCore.InMemory`，然後選取 [新增套件]。

### <a name="add-a-model-class"></a>新增模型類別

模型是代表應用程式中資料的物件。 在此情況下，唯一的模型是待辦事項。

在方案總管中，以滑鼠右鍵按一下專案。 選取 [新增] > [新增資料夾]。 將資料夾命名為 *Models*。

![新增資料夾](first-web-api-mac/_static/folder.png)

> [!NOTE]
> 您可以將模型類別放在專案中的任何位置，但依照慣例會使用 *Models* 資料夾。

以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [新增檔案] > [一般] > [空類別]。 將類別命名為 *TodoItem*，然後按一下 [新增]。

使用下列程式碼取代產生的程式碼：

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

此資料庫會在建立 `TodoItem` 時產生 `Id`。

### <a name="create-the-database-context"></a>建立資料庫內容

「資料庫內容」是為指定的資料模型協調 Entity Framework 功能的主要類別。 您可以透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立此類別。

將 `TodoContext` 類別新增至 *Models* 資料夾。

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>新增控制器

在方案總管的 *Controllers* 資料夾中新增 `TodoController` 類別。

使用下列程式碼取代產生的程式碼：

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>啟動應用程式

在 Visual Studio 中，選取 [執行] > [啟動並偵錯] 來啟動應用程式。 Visual Studio 會啟動瀏覽器並巡覽至 `http://localhost:<port>`，其中 `<port>` 是隨機選擇的通訊埠編號。 您收到 HTTP 404 (找不到) 錯誤。 將 URL 變更為 `http://localhost:<port>/api/values`。 隨即顯示 `ValuesController` 資料：

```json
["value1","value2"]
```

瀏覽至位於 `http://localhost:<port>/api/todo` 的 `Todo` 控制器。 即會傳回下列 JSON：

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>實作其他的 CRUD 作業

我們會將 `Create`、`Update` 和 `Delete` 方法新增至控制器。 這些方法是佈景主題的變化，因此我只會顯示程式碼，並強調顯示主要的差異。 請在新增或變更程式碼之後建置專案。

### <a name="create"></a>建立

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

上述方法會回應 HTTP POST，如 [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性所示。 [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) 屬性會告知 MVC 從 HTTP 要求的主體取得待辦事項的值。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

上述方法會回應 HTTP POST，如 [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性所示。 MVC 會從 HTTP 要求的主體取得待辦事項的值。

::: moniker-end

`CreatedAtRoute` 方法會傳回 201 回應。 對於可在伺服器上建立新資源的 HTTP POST 方法，這是標準回應。 `CreatedAtRoute` 也會將位置標頭新增至回應。 位置標頭指定新建立之待辦事項的 URI。 請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 已建立)。

### <a name="use-postman-to-send-a-create-request"></a>使用 Postman 來傳送建立要求

* 啟動應用程式 ([執行] > [啟動並偵錯])。
* 開啟 Postman。

![Postman 主控台](first-web-api/_static/pmc.png)

* 更新 localhost URL 中的通訊埠號碼。
* 將 HTTP 方法設定為 *POST*。
* 按一下 [本文] 索引標籤。
* 選取 [原始] 選項按鈕。
* 將類型設定為 *JSON (application/json)*。
* 輸入要求本文與待辦項目，類似於下列 JSON：

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* 按一下 [傳送] 按鈕。

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> 如果按一下 [傳送] 之後沒有顯示任何回應，請停用 [SSL 憑證驗證] 選項。 這位於 [檔案] > [設定] 下。 停用設定之後，再按一下 [傳送] 按鈕。

::: moniker-end

按一下 [回應] 窗格中的 [標頭] 索引標籤，並複製 [位置] 標頭值：

![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/pmc2.png)

您可以使用 [位置] 標頭 URI 來存取您建立的資源。 `Create` 方法會傳回 [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_)。 第一個傳遞至 `CreatedAtRoute` 的參數代表用來產生 URL 的具名路由。 請記住，`GetById` 方法建立了 `"GetTodo"` 具名路由：

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a>更新

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

`Update` 類似於 `Create`，但是會使用 HTTP PUT。 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。 根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是差異。 若要支援部分更新，請使用 HTTP PATCH。

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/pmcput.png)

### <a name="delete"></a>刪除

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。

![顯示 204 (沒有內容) 回應的 Postman 主控台](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
