---
title: 教學課程：使用 ASP.NET Core 建立 Web API
author: rick-anderson
description: 了解如何使用 ASP.NET Core 建置 Web API。
ms.author: riande
ms.custom: mvc
ms.date: 08/27/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 2d0eb24641c3d1f795b9e85ce10d42ee96d30846
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187303"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a>教學課程：使用 ASP.NET Core 建立 Web API

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson) 提供

本教學課程將教導您使用 ASP.NET Core 建立 Web API 的基本概念。

::: moniker range=">= aspnetcore-3.0"

在本教學課程中，您將了解如何：

> [!div class="checklist"]
> * 建立 Web API 專案。
> * 新增模型類別和資料庫內容。
> * 使用 CRUD 方法 Scaffold 控制器。
> * 設定路由、URL 路徑和傳回值。
> * 使用 Postman 呼叫 Web API。

結束時，您會有一個 Web API，可以管理儲存在資料庫中的「待辦事項」。

## <a name="overview"></a>總覽

本教學課程會建立以下 API：

|API | 描述 | 要求本文 | 回應本文 |
|--- | ---- | ---- | ---- |
|GET /api/TodoItems | 取得所有待辦事項 | None | 待辦事項的陣列|
|GET /api/TodoItems/{識別碼} | 依識別碼取得項目 | None | 待辦事項|
|POST /api/TodoItems | 新增記錄 | 待辦事項 | 待辦事項 |
|PUT /api/TodoItems/{識別碼} | 更新現有的項目 &nbsp; | 待辦事項 | None |
|DELETE /api/TodoItems/{識別碼} &nbsp; &nbsp; | 刪除項目 &nbsp; &nbsp; | None | None|

下圖顯示應用程式的設計。

![左側方塊代表用戶端。 它會送出要求並接收來自應用程式 (右側繪製的方塊) 的回應。 在應用程式方塊中，三個方塊代表控制器、模型以及資料存取層。 要求進入應用程式的控制器，而在控制器與資料存取層之間進行讀取/寫入作業。 模型會序列化並在回應中傳回至用戶端。](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a>必要條件

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a>建立 Web 專案

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 [檔案] 功能表選取 [新增] > [專案]。
* 選取 **ASP.NET Core Web 應用程式**範本，然後按一下 [下一步]。
* 將專案命名為 *TodoApi*，然後按一下 [建立]。
* 在 [建立新的 ASP.NET Core Web 應用程式] 對話方塊中，確認選取 [.NET Core] 和 [ASP.NET Core 3.0]。 選取 **API** 範本，然後按一下 [建立]。 請**勿**選取 [Enable Docker Support] \(啟用 Docker 支援\)。

![VS 新增專案對話方塊](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。
* 將目錄 (`cd`) 變更為包含專案資料夾的資料夾。
* 執行下列命令：

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoAPI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* 當出現對話方塊詢問您是否要將所需的資產新增至專案時，選取 [是]。

  上述命令：

  * 建立新的 Web API 專案，然後在 Visual Studio Code 中予以開啟。
  * 新增下一節需要的 NuGet 套件。

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 選取 [檔案] > [新增解決方案]。

  ![macOS 新增方案](first-web-api-mac/_static/sln.png)

* 選取[.Net Core] > [應用程式] > [API] > [下一步]。

  ![macOS [新增專案] 對話方塊](first-web-api-mac/_static/1.png)
  
* 在 [設定您的新 ASP.NET Core Web API] 對話方塊中，選取 * *.NET Core 3.0* 的 [目標 Framework]。

* 針對 [專案名稱] 輸入 *TodoApi*，然後選取 [建立]。

  ![設定對話方塊](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

在專案資料夾中開啟命令終端機，然後執行下列命令：

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a>測試 API

專案範本會建立 `WeatherForecast` API。 從瀏覽器呼叫 `Get` 方法來測試應用程式。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

按 Ctrl+F5 執行應用程式。 Visual Studio 會啟動瀏覽器並巡覽至 `https://localhost:<port>/WeatherForecast`，其中 `<port>` 是隨機選擇的通訊埠編號。

如果出現對話方塊詢問您是否應該信任 IIS Express 憑證，請選取 [是]。 在接著出現的 [安全性警告] 對話方塊中，選取 [是]。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

按 Ctrl+F5 執行應用程式。 在瀏覽器中，前往下列 URL：[https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast)。

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

選取 [執行] > [開始偵錯] 來啟動應用程式。 Visual Studio for Mac 會啟動瀏覽器並巡覽至 `https://localhost:<port>`，其中 `<port>` 是隨機選擇的連接埠號碼。 傳回 HTTP 404 (找不到) 錯誤。 將 `/WeatherForecast` 附加至 URL (將 URL 變更為 `https://localhost:<port>/WeatherForecast`)。

---

系統會傳回與下列類似的 JSON：

```json
[
    {
        "date": "2019-07-16T19:04:05.7257911-06:00",
        "temperatureC": 52,
        "temperatureF": 125,
        "summary": "Mild"
    },
    {
        "date": "2019-07-17T19:04:05.7258461-06:00",
        "temperatureC": 36,
        "temperatureF": 96,
        "summary": "Warm"
    },
    {
        "date": "2019-07-18T19:04:05.7258467-06:00",
        "temperatureC": 39,
        "temperatureF": 102,
        "summary": "Cool"
    },
    {
        "date": "2019-07-19T19:04:05.7258471-06:00",
        "temperatureC": 10,
        "temperatureF": 49,
        "summary": "Bracing"
    },
    {
        "date": "2019-07-20T19:04:05.7258474-06:00",
        "temperatureC": -1,
        "temperatureF": 31,
        "summary": "Chilly"
    }
]
```

## <a name="add-a-model-class"></a>新增模型類別

「模型」是代表應用程式所管理資料的一組類別。 此應用程式的模型是單一 `TodoItem` 類別。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 在 [方案總管] 中，以滑鼠右鍵按一下專案。 選取 [新增] > [新增資料夾]。 將資料夾命名為 *Models*。

* 以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [類別]。 將類別命名為 *TodoItem*，然後選取 [新增]。

* 使用下列程式碼取代範本程式碼：

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 新增名為 *Models* 的資料夾。

* 將 `TodoItem` 類別新增至具有下列程式碼的 *Models* 資料夾：

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 以滑鼠右鍵按一下專案。 選取 [新增] > [新增資料夾]。 將資料夾命名為 *Models*。

  ![新增資料夾](first-web-api-mac/_static/folder.png)

* 以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [新增檔案] > [一般] > [空類別]。

* 將類別命名為 *TodoItem*，然後按一下 [新增]。

* 使用下列程式碼取代範本程式碼：

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

`Id` 屬性的功能相當於關聯式資料庫中的唯一索引鍵。

模型類別可位於專案中的任何位置，但依照慣例會使用 *Models* 資料夾。

## <a name="add-a-database-context"></a>新增資料庫內容

「資料庫內容」是為資料模型協調 Entity Framework 功能的主要類別。 此類別是透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a>新增 Microsoft.EntityFrameworkCore.SqlServer

* 在 [工具] 功能表上，選取 [NuGet 套件管理員] > [管理解決方案的 NuGet 套件]。
* 選取 [包括發行前版本] 核取方塊。
* 選取 [瀏覽] 索引標籤，然後在搜尋方塊中輸入 **Microsoft.EntityFrameworkCore.SqlServer**。
* 選取左窗格中的 [Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview]。
* 選取右窗格中的 [專案] 核取方塊，然後選取 [安裝]。
* 使用上述指示來新增 `Microsoft.EntityFrameworkCore.InMemory` NuGet 套件。

![NuGet 封裝管理員](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a>新增 TodoCoNtext 資料庫內容

* 以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [類別]。 將類別命名為 *TodoContext*，然後按一下 [新增]。

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* 將 `TodoContext` 類別新增至 *Models* 資料夾。

---

* 輸入下列程式碼：

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a>登錄資料庫內容

在 ASP.NET Core 中，資料庫內容等服務必須向[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器註冊。 此容器會將服務提供給控制器。

使用下列醒目提示的程式碼更新 *Startup.cs*：

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

上述程式碼：

* 移除未使用的 `using` 宣告。
* 將資料庫內容新增至 DI 容器。
* 指定資料庫內容將會使用記憶體內部資料庫。

## <a name="scaffold-a-controller"></a>Scaffold 控制器

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 以滑鼠右鍵按一下 *Controllers* 資料夾。
* 選取 [新增] > [新增 Scaffold 項目]。
* 選取 [使用 Entity Framework 執行動作的 API 控制器]，然後選取 [新增]。
* 在 [使用 Entity Framework 執行動作的 API 控制器] 對話方塊中：

  * 在 [模型類別] 中選取 [TodoItem (TodoAPI.Models)]。
  * 在 [資料內容類別] 中選取 [TodoContext (TodoAPI.Models)]。
  * 選取 [新增]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

執行下列命令：

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

上述命令：

* 新增 Scaffolding 所需的 NuGet 套件。
* 安裝 Scaffolding 引擎 (`dotnet-aspnet-codegenerator`)。
* Scaffold `TodoItemsController`。

---

產生的程式碼：

* 定義不含方法的 API 控制器類別。
* 使用 [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 屬性來裝飾類別。 這個屬性表示控制器會回應 Web API 要求。 如需屬性所啟用之特定行為的相關資訊，請參閱 <xref:web-api/index>。
* 使用 DI 將資料庫內容 (`TodoContext`) 插入到控制器中。 控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。

## <a name="examine-the-posttodoitem-create-method"></a>檢查 PostTodoItem 建立方法

取代 `PostTodoItem` 中的 return 陳述式，以使用 [nameof](/dotnet/csharp/language-reference/operators/nameof) 運算子：

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

上述程式碼是 HTTP POST 方法，如 [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性所示。 該方法會從 HTTP 要求本文取得待辦事項的值。

<xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> 方法：

* 成功時會傳回 HTTP 201 狀態碼。 對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是 HTTP 201。
* 將 [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location)標頭新增到回應。 `Location` 標頭會指定新建待辦事項的 [URI](https://developer.mozilla.org/docs/Glossary/URI)。 如需詳細資訊，請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (已建立 10.2.2 201)。
* 參考 `GetTodoItem` 動作以建立 `Location` 標頭的 URI。 C# `nameof` 關鍵字是用來避免在 `CreatedAtAction` 呼叫中以硬式編碼方式寫入動作名稱。

### <a name="install-postman"></a>安裝 Postman

本教學課程使用 Postman 來測試 Web API。

* 安裝 [Postman](https://www.getpostman.com/downloads/)
* 啟動 Web 應用程式。
* 啟動 Postman。
* 停用 [SSL certificate verification] \(SSL 憑證驗證\)
* 從 [檔案] > [設定] (* *[一般]* 索引標籤)，停用 [SSL certificate verification] \(SSL 憑證驗證\)。
    > [!WARNING]
    > 在測試控制器之後，請重新啟用 [SSL certificate verification] \(SSL 憑證驗證\)。

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a>使用 Postman 測試 PostTodoItem

* 建立新的要求。
* 將 HTTP 方法設為 `POST`。
* 選取 [本文] 索引標籤。
* 選取 [原始] 選項按鈕。
* 將類型設定為 **JSON (application/json)** 。
* 在要求本文中，針對待辦項目輸入 JSON：

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* 選取 [傳送]。

  ![Postman 與建立要求](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a>測試位置標頭 URI

* 在 [回應] 窗格中選取 [標頭] 索引標籤。
* 複製 [位置] 標頭值：

  ![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/3/create.png)

* 將方法設定為 GET。
* 貼上 URI (例如 `https://localhost:5001/api/TodoItems/1`)
* 選取 [傳送]。

## <a name="examine-the-get-methods"></a>檢查 GET 方法

這些方法會實作兩個 GET 端點：

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

從瀏覽器或 Postman 呼叫這兩個端點來測試應用程式。 例如：

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

`GetTodoItems` 的呼叫會產生類似下列的回應：

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a>使用 Postman 測試 Get

* 建立新的要求。
* 將 HTTP 方法設定為 **GET**。
* 將要求 URL 設定為 `https://localhost:<port>/api/TodoItems`。 例如，`https://localhost:5001/api/TodoItems`。
* 在 Postman 中，設定 [Two pane view] \(雙窗格檢視\)。
* 選取 [傳送]。

這個應用程式會使用記憶體內部資料庫。 如果應用程式在停止後再啟動，上述 GET 要求將不會傳回任何資料。 如果沒有傳回任何資料，請將資料 [POST](#post) 到應用程式。

## <a name="routing-and-url-paths"></a>傳送和 URL 路徑

[`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) 屬性代表回應 HTTP GET 要求的方法。 每個方法的 URL 路徑的建構方式如下：

* 一開始在控制器的 `Route` 屬性中使用範本字串：

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* 以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。 在此範例中，控制器類別名稱是 **TodoItems**Controller，因此控制器名稱是 "TodoItems"。 ASP.NET Core [路由](xref:mvc/controllers/routing)不區分大小寫。
* 如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("products")]`)，請將其附加到路徑。 此範例不使用範本。 如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。

在下列 `GetTodoItem` 方法中，`"{id}"` 是待辦事項唯一識別碼的預留位置變數。 在叫用 `GetTodoItem` 時，會將 URL 中的 `"{id}"` 值提供給方法的 `id` 參數。

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a>傳回值

`GetTodoItems` 和 `GetTodoItem` 方法的傳回型別為 [ActionResult\<T> 類型](xref:web-api/action-return-types#actionresultt-type)。 ASP.NET Core 會自動將物件序列化為 [JSON](https://www.json.org/)，並將 JSON 寫入至回應訊息的本文。 此傳回型別的回應碼為 200，假設沒有任何未處理的例外狀況。 未處理的例外狀況會轉譯成 5xx 錯誤。

`ActionResult` 傳回型別可代表各種 HTTP 狀態碼。 例如，`GetTodoItem` 可傳回兩個不同的狀態值：

* 如果沒有項目符合所要求的識別碼，方法會傳回 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 錯誤碼。
* 否則，方法會傳回 200 與 JSON 回應本文。 傳回 `item` 會導致 HTTP 200 回應。

## <a name="the-puttodoitem-method"></a>PutTodoItem 方法

檢查 `PutTodoItem` 方法：

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

`PutTodoItem` 類似於 `PostTodoItem`，但是會使用 HTTP PUT。 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。 根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是變更。 若要支援部分更新，請使用 [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)。

如果在呼叫 `PutTodoItem` 時發生錯誤，請呼叫 `GET` 以確保資料庫中有項目。

### <a name="test-the-puttodoitem-method"></a>測試 PutTodoItem 方法

此範例使用在每次應用程式啟動都必須起始的記憶體內部資料庫。 資料庫中必須有項目，您才能進行 PUT 呼叫。 在發出 PUT 呼叫之前，請先呼叫 GET 以確保資料庫中有項目。

更新識別碼為 1 的待辦事項，並將其名稱設定為 "feed fish"：

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

下圖顯示 Postman 更新：

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a>DeleteTodoItem 方法

檢查 `DeleteTodoItem` 方法：

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

`DeleteTodoItem` 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) \(204 (沒有內容)\)。

### <a name="test-the-deletetodoitem-method"></a>測試 DeleteTodoItem 方法

使用 Postman 刪除待辦事項：

* 將方法設定為 `DELETE`。
* 設定要刪除的物件 URI，例如 `https://localhost:5001/api/TodoItems/1`
* 選取 [傳送]

## <a name="call-the-web-api-with-javascript"></a>使用 JavaScript 呼叫 Web API

請[參閱教學課程：使用 JavaScript 呼叫 ASP.NET Core Web API](xref:tutorials/web-api-javascript)。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

在本教學課程中，您將了解如何：

> [!div class="checklist"]
> * 建立 Web API 專案。
> * 新增模型類別和資料庫內容。
> * 新增控制器。
> * 新增 CRUD 方法。
> * 設定路由和 URL 路徑。
> * 指定傳回值。
> * 使用 Postman 呼叫 Web API。
> * 使用 JavaScript 呼叫 Web API。

結束時，您將會有一個可管理關聯式資料庫中所儲存「待辦事項」的 Web API。

## <a name="overview"></a>總覽

本教學課程會建立以下 API：

|API | 描述 | 要求本文 | 回應本文 |
|--- | ---- | ---- | ---- |
|GET /api/TodoItems | 取得所有待辦事項 | None | 待辦事項的陣列|
|GET /api/TodoItems/{識別碼} | 依識別碼取得項目 | None | 待辦事項|
|POST /api/TodoItems | 新增記錄 | 待辦事項 | 待辦事項 |
|PUT /api/TodoItems/{識別碼} | 更新現有的項目 &nbsp; | 待辦事項 | None |
|DELETE /api/TodoItems/{識別碼} &nbsp; &nbsp; | 刪除項目 &nbsp; &nbsp; | None | None|

下圖顯示應用程式的設計。

![左側方塊代表用戶端。 它會送出要求並接收來自應用程式 (右側繪製的方塊) 的回應。 在應用程式方塊中，三個方塊代表控制器、模型以及資料存取層。 要求進入應用程式的控制器，而在控制器與資料存取層之間進行讀取/寫入作業。 模型會序列化並在回應中傳回至用戶端。](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a>必要條件

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a>建立 Web 專案

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 [檔案] 功能表選取 [新增] > [專案]。
* 選取 **ASP.NET Core Web 應用程式**範本，然後按一下 [下一步]。
* 將專案命名為 *TodoApi*，然後按一下 [建立]。
* 在 [建立新的 ASP.NET Core Web 應用程式] 對話方塊中，確認選取 [.NET Core] 和 [ASP.NET Core 2.2]。 選取 **API** 範本，然後按一下 [建立]。 請**勿**選取 [Enable Docker Support] \(啟用 Docker 支援\)。

![VS 新增專案對話方塊](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。
* 將目錄 (`cd`) 變更為包含專案資料夾的資料夾。
* 執行下列命令：

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  這些命令會建立新的 Web API 專案，並開啟新專案資料夾中的新 Visual Studio Code 執行個體。

* 當出現對話方塊詢問您是否要將所需的資產新增至專案時，選取 [是]。

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 選取 [檔案] > [新增解決方案]。

  ![macOS 新增方案](first-web-api-mac/_static/sln.png)

* 選取[.Net Core] > [應用程式] > [API] > [下一步]。

  ![macOS [新增專案] 對話方塊](first-web-api-mac/_static/1.png)
  
* 在 [設定您的新 ASP.NET Core Web API] 對話方塊中，接受 [目標 Framework] 的預設 * *.NET Core 2.2*。

* 針對 [專案名稱] 輸入 *TodoApi*，然後選取 [建立]。

  ![設定對話方塊](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a>測試 API

專案範本會建立 `values` API。 從瀏覽器呼叫 `Get` 方法來測試應用程式。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

按 Ctrl+F5 執行應用程式。 Visual Studio 會啟動瀏覽器並巡覽至 `https://localhost:<port>/api/values`，其中 `<port>` 是隨機選擇的通訊埠編號。

如果出現對話方塊詢問您是否應該信任 IIS Express 憑證，請選取 [是]。 在接著出現的 [安全性警告] 對話方塊中，選取 [是]。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

按 Ctrl+F5 執行應用程式。 在瀏覽器中，前往下列 URL：[https://localhost:5001/api/values](https://localhost:5001/api/values)。

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

選取 [執行] > [開始偵錯] 來啟動應用程式。 Visual Studio for Mac 會啟動瀏覽器並巡覽至 `https://localhost:<port>`，其中 `<port>` 是隨機選擇的連接埠號碼。 傳回 HTTP 404 (找不到) 錯誤。 將 `/api/values` 附加至 URL (將 URL 變更為 `https://localhost:<port>/api/values`)。

---

即會傳回下列 JSON：

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a>新增模型類別

「模型」是代表應用程式所管理資料的一組類別。 此應用程式的模型是單一 `TodoItem` 類別。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 在 [方案總管] 中，以滑鼠右鍵按一下專案。 選取 [新增] > [新增資料夾]。 將資料夾命名為 *Models*。

* 以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [類別]。 將類別命名為 *TodoItem*，然後選取 [新增]。

* 使用下列程式碼取代範本程式碼：

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 新增名為 *Models* 的資料夾。

* 將 `TodoItem` 類別新增至具有下列程式碼的 *Models* 資料夾：

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 以滑鼠右鍵按一下專案。 選取 [新增] > [新增資料夾]。 將資料夾命名為 *Models*。

  ![新增資料夾](first-web-api-mac/_static/folder.png)

* 以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [新增檔案] > [一般] > [空類別]。

* 將類別命名為 *TodoItem*，然後按一下 [新增]。

* 使用下列程式碼取代範本程式碼：

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

`Id` 屬性的功能相當於關聯式資料庫中的唯一索引鍵。

模型類別可位於專案中的任何位置，但依照慣例會使用 *Models* 資料夾。

## <a name="add-a-database-context"></a>新增資料庫內容

「資料庫內容」是為資料模型協調 Entity Framework 功能的主要類別。 此類別是透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [類別]。 將類別命名為 *TodoContext*，然後按一下 [新增]。

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* 將 `TodoContext` 類別新增至 *Models* 資料夾。

---

* 使用下列程式碼取代範本程式碼：

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a>登錄資料庫內容

在 ASP.NET Core 中，資料庫內容等服務必須向[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器註冊。 此容器會將服務提供給控制器。

使用下列醒目提示的程式碼更新 *Startup.cs*：

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

上述程式碼：

* 移除未使用的 `using` 宣告。
* 將資料庫內容新增至 DI 容器。
* 指定資料庫內容將會使用記憶體內部資料庫。

## <a name="add-a-controller"></a>新增控制器

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 以滑鼠右鍵按一下 *Controllers* 資料夾。
* 選取 [新增] > [新增項目]。
* 在 [新增項目] 對話方塊中，選取 [API 控制器類別] 範本。
* 將類別命名為 *TodoController*，然後選取 [新增]。

  ![在搜尋方塊中輸入 controller 且已選取 Web API 控制器的 [新增項目] 對話方塊](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* 在 *Controllers* 資料夾中，建立名為 `TodoController` 的類別。

---

* 使用下列程式碼取代範本程式碼：

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

上述程式碼：

* 定義不含方法的 API 控制器類別。
* 使用 [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 屬性來裝飾類別。 這個屬性表示控制器會回應 Web API 要求。 如需屬性所啟用之特定行為的相關資訊，請參閱 <xref:web-api/index>。
* 使用 DI 將資料庫內容 (`TodoContext`) 插入到控制器中。 控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。
* 如果資料庫是空的，請將名為 `Item1` 的項目新增至資料庫。 此程式碼是在建構函式中，因此每次執行都會有新的 HTTP 要求。 如果您刪除所有項目，則建構函式會在下次呼叫 API 方法時重新建立 `Item1`。 因此看起來雖然像是刪除失敗，但實際為成功。

## <a name="add-get-methods"></a>新增 Get 方法

若要提供擷取待辦事項的 API，請在 `TodoController` 類別中新增下列方法：

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

這些方法會實作兩個 GET 端點：

* `GET /api/todo`
* `GET /api/todo/{id}`

如果應用程式仍在執行，請將其停止。 然後重新予以執行以包含最新的變更。

從瀏覽器呼叫這兩個端點來測試應用程式。 例如：

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

以下是呼叫 `GetTodoItems` 所產生的 HTTP 回應：

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a>傳送和 URL 路徑

[`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) 屬性代表回應 HTTP GET 要求的方法。 每個方法的 URL 路徑的建構方式如下：

* 一開始在控制器的 `Route` 屬性中使用範本字串：

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* 以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。 在此範例中，控制器類別名稱是 **Todo**Controller，因此容器名稱是 "todo"。 ASP.NET Core [路由](xref:mvc/controllers/routing)不區分大小寫。
* 如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("products")]`)，請將其附加到路徑。 此範例不使用範本。 如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。

在下列 `GetTodoItem` 方法中，`"{id}"` 是待辦事項唯一識別碼的預留位置變數。 在叫用 `GetTodoItem` 時，會將 URL 中的 `"{id}"` 值提供給方法的 `id` 參數。

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a>傳回值

`GetTodoItems` 和 `GetTodoItem` 方法的傳回型別為 [ActionResult\<T> 類型](xref:web-api/action-return-types#actionresultt-type)。 ASP.NET Core 會自動將物件序列化為 [JSON](https://www.json.org/)，並將 JSON 寫入至回應訊息的本文。 此傳回型別的回應碼為 200，假設沒有任何未處理的例外狀況。 未處理的例外狀況會轉譯成 5xx 錯誤。

`ActionResult` 傳回型別可代表各種 HTTP 狀態碼。 例如，`GetTodoItem` 可傳回兩個不同的狀態值：

* 如果沒有項目符合所要求的識別碼，方法會傳回 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 錯誤碼。
* 否則，方法會傳回 200 與 JSON 回應本文。 傳回 `item` 會導致 HTTP 200 回應。

## <a name="test-the-gettodoitems-method"></a>測試 GetTodoItems 方法

本教學課程使用 Postman 來測試 Web API。

* 安裝 [Postman](https://www.getpostman.com/downloads/)
* 啟動 Web 應用程式。
* 啟動 Postman。
* 停用 [SSL certificate verification] \(SSL 憑證驗證\)

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 [檔案] > [設定] ([一般] 索引標籤)，停用 [SSL 憑證驗證]。

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* 從 [Postman]  >  [喜好設定] ([一般] 索引標籤)，停用 [SSL 憑證驗證]。 或者，選取扳手並選取 [設定]，然後停用 [SSL 憑證驗證]。

---
  
> [!WARNING]
> 在測試控制器之後，請重新啟用 [SSL certificate verification] \(SSL 憑證驗證\)。

* 建立新的要求。
  * 將 HTTP 方法設定為 **GET**。
  * 將要求 URL 設定為 `https://localhost:<port>/api/todo`。 例如，`https://localhost:5001/api/todo`。
* 在 Postman 中，設定 [Two pane view] \(雙窗格檢視\)。
* 選取 [傳送]。

![Postman 與 GET 要求](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a>新增 Create 方法

在 *Controllers/TodoController.cs* 內部新增下列 `PostTodoItem` 方法： 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

上述程式碼是 HTTP POST 方法，如 [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性所示。 該方法會從 HTTP 要求本文取得待辦事項的值。

`CreatedAtAction` 方法：

* 成功時會傳回 HTTP 201 狀態碼。 對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是 HTTP 201。
* 將 `Location` 標頭加到回應中。 `Location` 標頭指定新建立之待辦事項的 URI。 如需詳細資訊，請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (已建立 10.2.2 201)。
* 參考 `GetTodoItem` 動作以建立 `Location` 標頭的 URI。 C# `nameof` 關鍵字是用來避免在 `CreatedAtAction` 呼叫中以硬式編碼方式寫入動作名稱。

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a>測試 PostTodoItem 方法

* 建置專案。
* 在 Postman 中，將 HTTP 方法設定為 `POST`。
* 選取 [本文] 索引標籤。
* 選取 [原始] 選項按鈕。
* 將類型設定為 **JSON (application/json)** 。
* 在要求本文中，針對待辦項目輸入 JSON：

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* 選取 [傳送]。

  ![Postman 與建立要求](first-web-api/_static/create.png)

  如果您收到 405「不允許的方法」錯誤，可能是由於新增 `PostTodoItem` 方法之後未編譯專案所導致。

### <a name="test-the-location-header-uri"></a>測試位置標頭 URI

* 在 [回應] 窗格中選取 [標頭] 索引標籤。
* 複製 [位置] 標頭值：

  ![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/pmc2.png)

* 將方法設定為 GET。
* 貼上 URI (例如 `https://localhost:5001/api/Todo/2`)
* 選取 [傳送]。

## <a name="add-a-puttodoitem-method"></a>新增 PutTodoItem 方法

新增以下 `PutTodoItem` 方法：

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`PutTodoItem` 類似於 `PostTodoItem`，但是會使用 HTTP PUT。 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。 根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是變更。 若要支援部分更新，請使用 [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)。

如果在呼叫 `PutTodoItem` 時發生錯誤，請呼叫 `GET` 以確保資料庫中有項目。

### <a name="test-the-puttodoitem-method"></a>測試 PutTodoItem 方法

此範例使用在每次應用程式啟動都必須起始的記憶體內部資料庫。 資料庫中必須有項目，您才能進行 PUT 呼叫。 在發出 PUT 呼叫之前，請先呼叫 GET 以確保資料庫中有項目。

更新識別碼為 1 的待辦事項，並將其名稱設定為 "feed fish"：

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

下圖顯示 Postman 更新：

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a>新增 DeleteTodoItem 方法

新增以下 `DeleteTodoItem` 方法：

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

`DeleteTodoItem` 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) \(204 (沒有內容)\)。

### <a name="test-the-deletetodoitem-method"></a>測試 DeleteTodoItem 方法

使用 Postman 刪除待辦事項：

* 將方法設定為 `DELETE`。
* 設定要刪除的物件 URI，例如 `https://localhost:5001/api/todo/1`
* 選取 [傳送]

範例應用程式可讓您刪除所有項目。 但刪除最後一個項目之後，模型類別建構函式會在下次呼叫 API 時建立新的項目。

## <a name="call-the-web-api-with-javascript"></a>使用 JavaScript 呼叫 Web API

在此節中，將會新增 HTML 網頁，以使用 JavaScript 來呼叫 Web API。 Fetch API 會起始要求。 JavaScript 會使用來自 Web API 回應的詳細資料來更新頁面。

藉由使用下列反白顯示的程式碼更新 *Startup.cs*，來設定應用程式[提供靜態檔案](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)並[啟用預設檔案對應](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)：

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

在專案目錄中建立 *wwwroot* 資料夾。

將名為 *index.html* 的 HTML 檔案新增至 *wwwroot* 目錄。 將其內容取代為下列標記：

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

將名為 *site.js* 的 JavaScript 檔案新增至 *wwwroot* 目錄。 將其內容取代為下列程式碼：

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

若要在本機測試 HTML 網頁，可能需要變更 ASP.NET Core 專案的啟動設定：

* 開啟 *Properties\launchSettings.json*。
* 移除 `launchUrl` 屬性，以強制應用程式於 *index.html* 處開啟 &mdash; 專案的預設檔案。

此範例會呼叫 Web API 的所有 CRUD 方法。 以下是關於呼叫 API 的說明。

### <a name="get-a-list-of-to-do-items"></a>取得待辦事項的清單

Fetch 會將 HTTP GET 要求傳送至 Web API，API 則會傳回代表待辦事項陣列的 JSON。 如果要求成功，則會叫用 `success` 回呼函式。 在回呼中，DOM 已使用待辦事項資訊進行更新。

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>新增待辦事項

Fetch 會傳送 HTTP POST 要求，並在要求本文中包含待辦事項。 `accepts` 和 `contentType` 選項都設定為 `application/json`，以指定接收和傳送的媒體類型。 待辦事項會使用 [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 轉換成 JSON。 當 API 傳回成功狀態碼時，會叫用 `getData` 函式來更新 HTML 資料表。

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>更新待辦事項

更新待辦事項類似於新增待辦事項。 `url` 會變更為新增項目的唯一識別碼，而 `type` 是 `PUT`。

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>刪除待辦事項

刪除待辦事項的達成方法是將 AJAX 呼叫的 `type` 設定為 `DELETE`，並在 URL 中指定項目的唯一識別碼。

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

## <a name="additional-resources"></a>其他資源

[檢視或下載本教學課程的範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples)。 請參閱[如何下載](xref:index#how-to-download-a-sample)。

如需詳細資訊，請參閱下列資源：

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [這個教學課程的 YouTube 版本](https://www.youtube.com/watch?v=TTkhEyGBfAk)
