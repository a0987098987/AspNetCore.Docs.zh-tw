---
title: "使用 Swagger 的 ASP.NET Core Web API 說明頁面"
author: spboyer
description: "本教學課程提供新增 Swagger，以產生 Web API 應用程式之文件和說明頁面的逐步解說。"
keywords: "ASP.NET Core, Swagger, Swashbuckle, 說明頁面, Web API"
ms.author: spboyer
manager: wpickett
ms.date: 09/01/2017
ms.topic: article
ms.assetid: 54bb961d-29d9-4dee-8e2c-a93fc33c16f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 647ab48fb83c5e2c79b5de371173bc644c65d831
ms.sourcegitcommit: 98ecb0f1bae4886507b090c84ecd99ff1e5c46ed
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2017
---
# <a name="aspnet-web-api-help-pages-using-swagger"></a>使用 Swagger 的 ASP.NET Web API 說明頁面

<a name=web-api-help-pages-using-swagger></a>

由 [Shayne Boyer](https://twitter.com/spboyer) 和 [Scott Addie](https://twitter.com/Scott_Addie) 提供

建置使用的應用程式時，了解 API 的各種方法對於開發人員而言可能是一項挑戰。

搭配使用 [Swagger](https://swagger.io/) 與 .NET Core 實作 [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore)，為您的 Web API 產生優良的文件與說明頁面，就像新增幾個 NuGet 套件和修改 *Startup.cs* 一樣簡單。

* [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) 是用來為 ASP.NET Core Web API 產生 Swagger 文件的開放原始碼專案。

* [Swagger](https://swagger.io/) 是 RESTful API 的電腦可讀取表示法，可支援互動式文件、產生用戶端 SDK，以及可探索性。

本教學課程是基於[使用 ASP.NET Core MVC 和 Visual Studio 建置第一個 Web API](xref:tutorials/first-web-api) 的範例。 如果您想要跟著做，請下載 [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) 上的範例。

## <a name="getting-started"></a>快速入門

Swashbuckle 有三個主要元件：

* `Swashbuckle.AspNetCore.Swagger`：Swagger 物件模型和中介軟體，用来公開 `SwaggerDocument` 物件作為 JSON 端點。

* `Swashbuckle.AspNetCore.SwaggerGen`：Swagger 產生器，可直接從您的路由、控制器和模型建置 `SwaggerDocument` 物件。 它通常會結合 Swagger 端點中介軟體，以便自動公開 Swagger JSON。

* `Swashbuckle.AspNetCore.SwaggerUI`：Swagger UI 工具的內嵌版本，可解譯 Swagger JSON 來建置描述 Web API 功能的可自訂豐富體驗。 它包含公用方法的內建測試載入器。

## <a name="nuget-packages"></a>NuGet 封裝

可使用下列方法新增 Swashbuckle：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 [套件管理員主控台] 視窗中：

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* 從 [管理 NuGet 套件] 對話方塊中：

     * 在方案總管 > [管理 NuGet 套件] 中，以滑鼠右鍵按一下您的專案
     * 將 [套件來源] 設定為 "nuget.org"
     * 在搜尋方塊中輸入 "Swashbuckle.AspNetCore"
     * 從 [瀏覽] 索引標籤中選取 "Swashbuckle.AspNetCore" 套件，並按一下 [安裝]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 在 [Solution Pad] > [新增套件...] 中，以滑鼠右鍵按一下 *Packages* 資料夾
* 將 [新增套件] 視窗的 [來源] 下拉式清單設定為 "nuget.org"
* 在搜尋方塊中輸入 Swashbuckle.AspNetCore
* 從結果窗格中選取 Swashbuckle.AspNetCore 套件，並按一下 [新增套件]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

從 [整合式終端機] 執行下列命令：

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

執行下列命令：

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a>新增 Swagger 並將其設定為中介軟體

將 Swagger 產生器新增至 *Startup.cs* 的 `ConfigureServices` 方法中的服務集合：

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

在 `Info` 類別新增下列 using 陳述式：

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

在 *Startup.cs* 的 `Configure` 方法中，啟用中介軟體為產生的 JSON 文件和 SwaggerUI 提供服務：

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

啟動應用程式，並巡覽至 `http://localhost:<random_port>/swagger/v1/swagger.json`。 描述端點的已產生文件隨即出現。

**注意：**Microsoft Edge、Google Chrome 和 Firefox 會以原生方式顯示 JSON 文件。 Chrome 具有延伸模組，可格式化文件以方便閱讀。 *為求簡單明瞭，已減少下列範例。*

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
           "type": "object",
            "properties": {
                "id": {
                    "format": "int64",
                    "type": "integer"
                },
                "name": {
                    "type": "string"
                },
                "isComplete": {
                    "default": false,
                    "type": "boolean"
                }
            }
       }
   },
   "securityDefinitions": {}
}
```

這份文件會驅動 Swagger UI，而此 Swagger UI 可透過巡覽至 `http://localhost:<random_port>/swagger` 進行檢視：

![Swagger UI](web-api-help-pages-using-swagger/_static/swagger-ui.png)

`TodoController` 中的每個公用動作方法都可以從 UI 進行測試。 按一下方法名稱以展開該區段。 新增任何必要的參數，然後按一下 [試試看!]。

![範例 Swagger GET 測試](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a>自訂與擴充性

Swagger 提供用來記載物件模型和自訂 UI 以符合佈景主題的選項。

### <a name="api-info-and-description"></a>API 資訊與描述

傳遞至 `AddSwaggerGen` 方法的組態動作可用來新增資訊，例如作者、授權和描述：

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

下列影像說明顯示版本資訊的 Swagger UI：

![含有版本資訊的 Swagger UI：描述、作者，以及查看更多連結](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a>XML 註解

XML 註解可以使用下列方式啟用：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 以滑鼠右鍵按一下方案總管中的專案，然後選取 [屬性]
* 檢查 [組建] 索引標籤的 [輸出] 區段下方的 [XML 文件檔]：

![專案屬性的 [組建] 索引標籤](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 開啟 [專案選項] 對話方塊 > [組建] > [編譯器]
* 檢查 [一般選項] 區段下方的 [產生 XML 文件] 方塊：

![專案選項的 [一般選項] 區段](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

將下列程式碼片段手動新增至 *.csproj* 檔案：

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

---

設定 Swagger 來使用產生的 XML 檔案。 對於 Linux 或非 Windows 作業系統，檔案名稱和路徑可以區分大小寫。 例如，*ToDoApi.XML* 檔案可位於 Windows，但不是 CentOS。

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

在上述程式碼中，`ApplicationBasePath` 會取得應用程式的基底路徑。 基底路徑用來找出 XML 註解檔案。 *TodoApi.xml* 僅適用於此範例，因為所產生之 XML 註解檔案的名稱是以應用程式名稱為基礎。

將三斜線註解新增至方法，即可透過將描述加入區段標頭來增強 Swagger UI：

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![顯示 XML 註解 'Deletes a specific TodoItem.' 的 Swagger UI 適用於 Delete 方法](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

UI 是由產生的 JSON 檔案所驅動，該檔案中也包含這些註解：

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

請將 [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) 標記新增至 `Create` 動作方法文件。 它會補充 `<summary>` 標記中指定的資訊，並提供更強固的 Swagger UI。 `<remarks>` 標記內容可以包含文字、JSON 或 XML。

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

請注意使用這些額外註解的 UI 增強功能。

![顯示額外註解的 Swagger UI](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a>資料註釋

使用 `System.ComponentModel.DataAnnotations` 中的屬性來裝飾模型，以協助驅動 Swagger UI 元件。

將 `[Required]` 屬性 (attribute) 新增至 `TodoItem` 類別的 `Name` 屬性 (property)：

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

這個屬性的存在會變更 UI 行為，並改變基礎的 JSON 結構描述：

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

將 `[Produces("application/json")]` 屬性新增至 API 控制器。 其目的是要宣告控制器的動作支援傳回的內容類型為 *application/json*：

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

[回應內容類型] 下拉式清單會選取此內容類型，作為控制器 GET 動作的預設值：

![含有預設回應內容類型的 Swagger UI](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

當 Web API 中的資料註釋使用量增加時，UI 和 API 說明頁面會變得更清楚且更有用。

### <a name="describing-response-types"></a>描述回應類型

取用的開發人員最關心的是傳回的 &mdash; 特別回應類型和錯誤碼 (如果不是標準的話)。 這些是以 XML 註解和資料註釋進行處理。

`Create` 動作會在成功時傳回 `201 Created`，或在發佈的要求主體為 null 時傳回 `400 Bad Request`。 如果 Swagger UI 中沒有適當的文件，取用者便會缺乏對這些預期結果的了解。 在下列範例中新增強調顯示的那幾行來修正該問題：

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

現在，Swagger UI 清楚記載了預期的 HTTP 回應碼：

![顯示 POST 回應類別描述 'Returns the newly created Todo item'，並針對回應訊息下方的狀態碼和原因顯示 '400 - If the item is null' 的 Swagger UI](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a>自訂 UI

股票 UI 既可運作又能呈現；不過，在建置 API 的文件頁面時，您希望它表示您的品牌或佈景主題。 使用 Swashbuckle 元件完成該工作，需要新增資源來處理靜態檔案，然後建置裝載這些檔案的資料夾結構。

如果目標設為 .NET Framework，請將 `Microsoft.AspNetCore.StaticFiles` NuGet 套件新增至專案：

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

啟用靜態檔案中介軟體：

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

從 [Swagger UI GitHub 儲存機制](https://github.com/swagger-api/swagger-ui/tree/2.x/dist)中取得 *dist* 資料夾的內容。 此資料夾包含 Swagger UI 頁面所需的資產。 將該資料夾的內容複製到 *wwwroot/swagger/ui* 資料夾。

使用下列 CSS 建立 *wwwroot/swagger/ui/css/custom.css* 檔案，以自訂頁面標頭：

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

在 *index.html* 檔案中參考 *custom.css*：

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

瀏覽至 `http://localhost:<random_port>/swagger/ui/index.html` 上的 *index.html* 頁面。 在標頭的文字方塊中輸入 `http://localhost:<random_port>/swagger/v1/swagger.json`，然後按一下 [瀏覽] 按鈕。 結果頁面如下所示：

![含有自訂標頭標題的 Swagger UI](web-api-help-pages-using-swagger/_static/custom-header.png)

還有更多可以在頁面中執行的功能。 請在 [Swagger UI GitHub 儲存機制](https://github.com/swagger-api/swagger-ui)中查看 UI 資源的完整功能。
