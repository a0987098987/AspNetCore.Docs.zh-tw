---
title: 教學課程：使用 ASP.NET Core 建立 Web API
author: rick-anderson
description: 了解如何使用 ASP.NET Core 建置 Web API。
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: tutorials/first-web-api
ms.openlocfilehash: b4c88f5dc7853396448a2a6122f3652f92079e68
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72541781"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a>教學課程：使用 ASP.NET Core 建立 Web API

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson) 提供

本教學課程將教導您使用 ASP.NET Core 建立 Web API 的基本概念。

在本教學課程中，您將了解如何：

> [!div class="checklist"]
> * 建立 Web API 專案。
> * 新增模型類別和資料庫內容。
> * 使用 CRUD 方法 Scaffold 控制器。
> * 設定路由、URL 路徑和傳回值。
> * 使用 Postman 呼叫 Web API。

結束時，您會有一個 Web API，可以管理儲存在資料庫中的「待辦事項」。

## <a name="overview"></a>總覽

本教學課程會建立包含下列端點的 Web API：

|端點                  |描述            |要求本文|回應本文       |
|--------------------------|-----------------------|------------|--------------------|
|GET /api/TodoItems        |取得所有待辦事項    |None        |待辦事項的陣列|
|GET /api/TodoItems/{識別碼}   |依識別碼取得項目      |None        |待辦事項          |
|POST /api/TodoItems       |新增記錄         |待辦事項  |待辦事項          |
|PUT /api/TodoItems/{識別碼}   |更新現有的專案|待辦事項  |None                |
|刪除/api/TodoItems/{id}|刪除項目         |None        |None                |

下圖顯示應用程式的設計。

![左側方塊代表用戶端。 它會送出要求並接收來自應用程式 (右側繪製的方塊) 的回應。 在應用程式方塊中，三個方塊代表控制器、模型以及資料存取層。 要求進入應用程式的控制器，而在控制器與資料存取層之間進行讀取/寫入作業。 模型會序列化並在回應中傳回至用戶端。](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a>Prerequisites

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a>建立 Web 專案

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 從 [檔案] 功能表選取 [新增] > [專案]。
1. 選取 **ASP.NET Core Web 應用程式**範本，然後按一下 [下一步]。
1. 將專案命名為 *TodoApi*，然後按一下 [建立]。
1. 在 [建立新的 ASP.NET Core Web 應用程式] 對話方塊中，確認選取 [.NET Core] 和 [ASP.NET Core 3.0]。 選取 **API** 範本，然後按一下 [建立]。

![VS 新增專案對話方塊](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. 開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。
1. 將目錄 (`cd`) 變更為包含專案資料夾的資料夾。
1. 執行下列命令：

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

1. 當出現對話方塊詢問您是否要將所需的資產新增至專案時，選取 [是]。

  上述命令：

  * 建立新的 Web API 專案，並在 Visual Studio Code 中開啟它。
  * 新增下一節中所需的 NuGet 套件。

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 選取 [檔案] > [新增方案]。

    ![macOS 新增方案](first-web-api-mac/_static/sln.png)

1. 選取 .NET Core > 應用程式 > API > 下一步.

    ![macOS [新增專案] 對話方塊](first-web-api-mac/_static/1.png)
  
1. 在 [設定您的新 ASP.NET Core Web API] 對話方塊中，選取 * *.NET Core 3.0* 的 [目標 Framework]。
1. 針對 [專案名稱] 輸入 *TodoApi*，然後選取 [建立]。

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

按<kbd>Ctrl + F5</kbd>執行應用程式。 Visual Studio 會啟動瀏覽器並巡覽至 `https://localhost:<port>/WeatherForecast`，其中 `<port>` 是隨機選擇的通訊埠編號。

如果出現對話方塊詢問您是否應該信任 IIS Express 憑證，請選取 [是]。 在接著出現的 [安全性警告] 對話方塊中，選取 [是]。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

按<kbd>Ctrl + F5</kbd>執行應用程式。 在瀏覽器中，前往下列 URL：[https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast)。

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

1. 在 [方案總管] 中，以滑鼠右鍵按一下專案。 選取 [新增] > [新增資料夾]。 將資料夾命名為 *Models*。
1. 以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [類別]。 將類別命名為 *TodoItem*，然後選取 [新增]。
1. 使用下列程式碼取代範本程式碼：

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. 新增名為 *Models* 的資料夾。
1. 將 `TodoItem` 類別新增至具有下列程式碼的 *Models* 資料夾：

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 以滑鼠右鍵按一下專案。 選取 [新增] > [新增資料夾]。 將資料夾命名為 *Models*。

    ![新增資料夾](first-web-api-mac/_static/folder.png)

1. 以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [新增檔案] > [一般] > [空類別]。
1. 將類別命名為 *TodoItem*，然後按一下 [新增]。
1. 使用下列程式碼取代範本程式碼：

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

`Id` 屬性的功能相當於關聯式資料庫中的唯一索引鍵。

模型類別可位於專案中的任何位置，但依照慣例會使用 *Models* 資料夾。

## <a name="add-a-database-context"></a>新增資料庫內容

「資料庫內容」是為資料模型協調 Entity Framework 功能的主要類別。 此類別是透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a>新增 Microsoft.EntityFrameworkCore.SqlServer

1. 在 [工具] 功能表上，選取 [NuGet 套件管理員] > [管理解決方案的 NuGet 套件]。
1. 選取 [瀏覽] 索引標籤，然後在搜尋方塊中輸入 **Microsoft.EntityFrameworkCore.SqlServer**。
1. 在左窗格中選取 [ **microsoft.entityframeworkcore** ]。
1. 選取右窗格中的 [專案] 核取方塊，然後選取 [安裝]。
1. 使用上述指示來新增 `Microsoft.EntityFrameworkCore.InMemory` NuGet 套件。

![NuGet 封裝管理員](first-web-api/_static/vs3nuget.png)

## <a name="add-the-todocontext-database-context"></a>新增 TodoCoNtext 資料庫內容

* 以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [類別]。 將類別命名為 *TodoContext*，然後按一下 [新增]。

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* 將 `TodoContext` 類別新增至 *Models* 資料夾。

---

* 輸入下列程式碼：

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a>登錄資料庫內容

在 ASP.NET Core 中，服務（例如資料庫內容）必須向相依性[插入（DI）](xref:fundamentals/dependency-injection)容器註冊。 此容器會將服務提供給控制器。

使用下列醒目提示的程式碼更新 *Startup.cs*：

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

上述程式碼：

* 移除未使用的 `using` 宣告。
* 將資料庫內容新增至 DI 容器。
* 指定資料庫內容將會使用記憶體內部資料庫。

## <a name="scaffold-a-controller"></a>Scaffold 控制器

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 以滑鼠右鍵按一下 *Controllers* 資料夾。
1. 選取 [**加入** > **新增 scaffold 專案**]。
1. 選取 [使用 Entity Framework 執行動作的 API 控制器]，然後選取 [新增]。
1. 在 [使用 Entity Framework 執行動作的 API 控制器] 對話方塊中：
    * 在**模型類別**中選取 [ **TodoItem （TodoApi）** ]。
    * 選取**資料內容類別**中的 [ **TodoCoNtext （TodoApi）** ]。
    * 選取 [新增]。

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

執行下列命令：

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

上述命令：

* 新增 Scaffolding 所需的 NuGet 套件。
* 安裝 Scaffolding 引擎 (`dotnet-aspnet-codegenerator`)。
* Scaffold `TodoItemsController`。

---

產生的程式碼：

* 定義不含方法的 API 控制器類別。
* 使用 [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 屬性來裝飾類別。 這個屬性表示控制器會回應 Web API 要求。 如需屬性所啟用之特定行為的相關資訊，請參閱 <xref:web-api/index>。
* 使用 DI 將資料庫內容 (`TodoContext`) 插入到控制器中。 控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。

## <a name="examine-the-posttodoitem-create-method"></a>檢查 PostTodoItem 建立方法

取代 `PostTodoItem` 中的 return 陳述式，以使用 [nameof](/dotnet/csharp/language-reference/operators/nameof) 運算子：

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

上述程式碼是 HTTP POST 方法，如 [[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) 屬性所示。 該方法會從 HTTP 要求本文取得待辦事項的值。

<xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> 方法：

* 成功時會傳回 HTTP 201 狀態碼。 對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是 HTTP 201。
* 將 [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location)標頭新增到回應。 `Location` 標頭會指定新建待辦事項的 [URI](https://developer.mozilla.org/docs/Glossary/URI)。 如需詳細資訊，請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (已建立 10.2.2 201)。
* 參考 `GetTodoItem` 動作以建立 `Location` 標頭的 URI。 C# `nameof` 關鍵字是用來避免在 `CreatedAtAction` 呼叫中以硬式編碼方式寫入動作名稱。

### <a name="install-postman"></a>安裝 Postman

本教學課程使用 Postman 來測試 Web API。

1. 安裝 [Postman](https://www.getpostman.com/downloads/)
1. 啟動 Web 應用程式。
1. 啟動 Postman。
1. 停用 [SSL certificate verification] \(SSL 憑證驗證\)
1. 從 [檔案 ** >  設定**] （ **[一般**] 索引標籤），停用**SSL 憑證驗證**。

    > [!WARNING]
    > 在測試控制器之後，請重新啟用 [SSL certificate verification] \(SSL 憑證驗證\)。

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a>使用 Postman 測試 PostTodoItem

1. 建立新的要求。
1. 將 HTTP 方法設為 `POST`。
1. 選取 [本文] 索引標籤。
1. 選取 [原始] 選項按鈕。
1. 將類型設定為 **JSON (application/json)** 。
1. 在 [要求本文] 中，輸入 JSON 做為待辦事項：

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

1. 選取 [傳送]。

  ![Postman 與建立要求](first-web-api/_static/create.png)

### <a name="test-the-location-header-uri"></a>測試位置標頭 URI

1. 在 [回應] 窗格中選取 [標頭] 索引標籤。
1. 複製 [位置] 標頭值：

    ![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/create.png)

1. 將方法設定為 GET。
1. 貼上 URI （例如 `https://localhost:5001/api/TodoItems/1`）。
1. 選取 [傳送]。

## <a name="examine-the-get-methods"></a>檢查 GET 方法

這些方法會實作兩個 GET 端點：

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

從瀏覽器或 Postman 呼叫這兩個端點來測試應用程式。 例如:

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

1. 建立新的要求。
1. 將 HTTP 方法設定為 **GET**。
1. 將要求 URL 設定為 `https://localhost:<port>/api/TodoItems`。 例如，`https://localhost:5001/api/TodoItems`。
1. 在 Postman 中，設定 [Two pane view] \(雙窗格檢視\)。
1. 選取 [傳送]。

這個應用程式會使用記憶體內部資料庫。 如果應用程式已停止並啟動，上述 GET 要求將不會傳回任何資料。 如果沒有傳回任何資料，請將資料 [POST](#post) 到應用程式。

## <a name="routing-and-url-paths"></a>傳送和 URL 路徑

[[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)屬性代表回應 HTTP GET 要求的方法。 每個方法的 URL 路徑的建構方式如下：

* 一開始在控制器的 `Route` 屬性中使用範本字串：

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* 以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。 在此範例中，控制器類別名稱是 **TodoItems**Controller，因此控制器名稱是 "TodoItems"。 ASP.NET Core [路由](xref:mvc/controllers/routing)不區分大小寫。
* 如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("products")]`)，請將其附加到路徑。 此範例不使用範本。 如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。

在下列 `GetTodoItem` 方法中，`"{id}"` 是待辦事項唯一識別碼的預留位置變數。 叫用 `GetTodoItem` 時，URL 中 `"{id}"` 的值會在其 `id` 參數中提供給方法。

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a>傳回值

`GetTodoItems` 和 `GetTodoItem` 方法的傳回型別為 [ActionResult\<T> 類型](xref:web-api/action-return-types#actionresultt-type)。 ASP.NET Core 會自動將物件序列化為 [JSON](https://www.json.org/)，並將 JSON 寫入至回應訊息的本文。 此傳回型別的回應碼為 200，假設沒有任何未處理的例外狀況。 未處理的例外狀況會轉譯成 5xx 錯誤。

`ActionResult` 傳回型別可代表各種 HTTP 狀態碼。 例如，`GetTodoItem` 可傳回兩個不同的狀態值：

* 如果沒有專案符合所要求的識別碼，方法會傳回 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound> 錯誤碼。
* 否則，方法會傳回 200 與 JSON 回應本文。 傳回 `item` 會導致 HTTP 200 回應。

## <a name="the-puttodoitem-method"></a>PutTodoItem 方法

檢查 `PutTodoItem` 方法：

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

`PutTodoItem` 類似於 `PostTodoItem`，但是會使用 HTTP PUT。 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。 根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是變更。 若要支援部分更新，請使用 [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)。

如果在呼叫 `PutTodoItem` 時發生錯誤，請呼叫 `GET` 以確保資料庫中有項目。

### <a name="test-the-puttodoitem-method"></a>測試 PutTodoItem 方法

這個範例會使用必須在每次啟動應用程式時初始化的記憶體內部資料庫。 資料庫中必須有項目，您才能進行 PUT 呼叫。 在發出 PUT 呼叫之前，請先呼叫 GET 以確保資料庫中有項目。

更新識別碼為1的待辦事項。 將其名稱設為 "feed 魚"：

```json
{
  "ID":1,
  "name":"feed fish",
  "isComplete":true
}
```

下圖顯示 Postman 更新：

![顯示 204 (沒有內容) 回應的 Postman 主控台](first-web-api/_static/pmcput.png)

## <a name="the-deletetodoitem-method"></a>DeleteTodoItem 方法

檢查 `DeleteTodoItem` 方法：

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

`DeleteTodoItem` 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) \(204 (沒有內容)\)。

### <a name="test-the-deletetodoitem-method"></a>測試 DeleteTodoItem 方法

使用 Postman 刪除待辦事項：

1. 將方法設定為 `DELETE`。
1. 設定要刪除之物件的 URI （例如，`https://localhost:5001/api/TodoItems/1`）。
1. 選取 [傳送]。

## <a name="call-the-web-api-with-javascript"></a>使用 JavaScript 呼叫 Web API

請參閱[教學課程：使用 JavaScript 呼叫 ASP.NET Core Web API](xref:tutorials/web-api-javascript)。

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a>將驗證支援新增至 Web API

請參閱[IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html)教學課程。

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
