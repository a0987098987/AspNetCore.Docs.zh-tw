---
title: 使用 Swagger/OpenAPI 的 ASP.NET Core Web API 說明頁面
author: RicoSuter
description: 本教學課程提供新增 Swagger，以產生 Web API 應用程式之文件和說明頁面的逐步解說。
ms.author: scaddie
ms.custom: mvc
ms.date: 12/07/2019
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 4408e02996b958bf009903aa1e4eeda9ad4f457c
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78658470"
---
# <a name="aspnet-core-web-api-help-pages-with-swagger--openapi"></a>使用 Swagger/OpenAPI 的 ASP.NET Core Web API 說明頁面

作者：[Christoph Nienaber](https://twitter.com/zuckerthoben) 和 [Rico Suter](https://blog.rsuter.com/)

使用 Web API 時，了解其各種不同的方法對開對發人員來說可能是一項挑戰。 [Swagger](https://swagger.io/) (也稱為 [Open API](https://www.openapis.org/)) 解決為 Web API 產生有用文件與說明頁面的問題。 它提供如互動式文件、用戶端 SDK 產生作業和 API 發現性等優點。

在本文中，展示了 [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) 和 [NSwag](https://github.com/RicoSuter/NSwag) .NET Swagger 實作：

* **Swashbuckle.AspNetCore** 是用來為 ASP.NET Core Web API 產生 Swagger 文件的開放原始碼專案。

* **NSwag** 是用來產生 Swagger 文件，並將 [Swagger UI](https://swagger.io/swagger-ui/) 或 [ReDoc](https://github.com/Rebilly/ReDoc) 整合到 ASP.NET Core Web API 的其他開放原始碼專案。 此外，NSwag 提供方法來為您的 API 產生 C# 和 TypeScript 用戶端程式碼。

## <a name="what-is-swagger--openapi"></a>什麼是 Swagger/OpenAPI？

Swagger 是用來描述 [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) API 的語言無關規格。 Swagger 專案已捐贈給 [OpenAPI Initiative](https://www.openapis.org/) (OpenAPI 方案)，目前在該方案中將其稱為 OpenAPI。 這兩個名稱可交替使用；不過，OpenAPI 是慣用名稱。 它可讓電腦和人類都了解服務的功能，而不必直接存取實作 (原始碼、網路存取、文件)。 其中一個目標是要將連線未相關聯之服務所需的工作量減到最少。 另一個目標則是減少正確記錄服務所需的時間量。

## <a name="swagger-specification-swaggerjson"></a>Swagger 規格 (swagger.json)

Swagger 流程的核心是 Swagger 規格&mdash;根據預設，文件名為 *swagger.json*。 它是由 Swagger 工具鏈 (或其協力廠商實作) 根據您的服務所產生。 它會描述 API 的功能，以及如何使用 HTTP 來進行存取。 它可驅動 Swagger UI，並由工具鏈用來啟用探索和產生用戶端程式碼功能。 以下是 Swagger 規格的範例，為求簡單明瞭，其已經過縮減：

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

## <a name="swagger-ui"></a>Swagger UI

[Swagger UI](https://swagger.io/swagger-ui/) 使用產生的 Swagger 規格提供 Web UI ，以提供服務的相關資訊。 Swashbuckle 和 NSwag 都包含內嵌的 Swagger UI 版本，因此它可以使用中介軟體的登錄呼叫裝載在 ASP.NET Core 應用程式中。 Web UI 顯示如下：

![Swagger UI](web-api-help-pages-using-swagger/_static/swagger-ui.png)

控制器中的每個公用動作方法都可以從 UI 進行測試。 按一下方法名稱以展開該區段。 添加任何必要的參數,然後單擊 **「試用」!**

![範例 Swagger GET 測試](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> 用於螢幕擷取畫面的 Swagger UI 版本為第 2 版。 如需第 3 版的範例，請參閱 [Petstore 範例](https://petstore.swagger.io/)。

## <a name="next-steps"></a>後續步驟

* [開始使用 Swashbuckle](xref:tutorials/get-started-with-swashbuckle)
* [開始使用 NSwag](xref:tutorials/get-started-with-nswag)
