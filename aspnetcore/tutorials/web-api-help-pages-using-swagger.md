---
title: 使用 Swagger/OpenAPI 的 ASP.NET Core Web API 說明頁面
author: RicoSuter
description: 本教學課程提供新增 Swagger，以產生 Web API 應用程式之文件和說明頁面的逐步解說。
ms.author: scaddie
ms.custom: mvc
ms.date: 07/06/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 66b8278e84df5ee56582254ebe2dc99ada98a9dc
ms.sourcegitcommit: fa89d6553378529ae86b388689ac2c6f38281bb9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2020
ms.locfileid: "86060302"
---
# <a name="aspnet-core-web-api-help-pages-with-swagger--openapi"></a>使用 Swagger/OpenAPI 的 ASP.NET Core Web API 說明頁面

作者：[Christoph Nienaber](https://twitter.com/zuckerthoben) 和 [Rico Suter](https://blog.rsuter.com/)

當您使用 Web API 時，瞭解其各種方法對於開發人員而言可能是一項挑戰。 [Swagger](https://swagger.io/)（也稱為[OpenAPI](https://www.openapis.org/)）可解決為 web api 產生有用檔和說明頁面的問題。 它提供如互動式文件、用戶端 SDK 產生作業和 API 發現性等優點。

在本文中，展示了 [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) 和 [NSwag](https://github.com/RicoSuter/NSwag) .NET Swagger 實作：

* **Swashbuckle.AspNetCore** 是用來為 ASP.NET Core Web API 產生 Swagger 文件的開放原始碼專案。

* **NSwag** 是用來產生 Swagger 文件，並將 [Swagger UI](https://swagger.io/swagger-ui/) 或 [ReDoc](https://github.com/Rebilly/ReDoc) 整合到 ASP.NET Core Web API 的其他開放原始碼專案。 此外，NSwag 提供方法來為您的 API 產生 C# 和 TypeScript 用戶端程式碼。

## <a name="what-is-swagger--openapi"></a>什麼是 Swagger/OpenAPI？

Swagger 是用來描述 [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) API 的語言無關規格。 Swagger 專案已捐贈給 [OpenAPI Initiative](https://www.openapis.org/) (OpenAPI 方案)，目前在該方案中將其稱為 OpenAPI。 這兩個名稱可交替使用；不過，OpenAPI 是慣用名稱。 它可讓電腦和人類都了解服務的功能，而不必直接存取實作 (原始碼、網路存取、文件)。 其中一個目標是要將連線未相關聯之服務所需的工作量減到最少。 另一個目標則是減少正確記錄服務所需的時間量。

## <a name="openapi-specification-openapijson"></a>OpenAPI 規格（openapi.js）

OpenAPI 流程的核心預設為規格，這是 &mdash; 名為*openapi.js*的檔。 它是由 OpenAPI 工具鏈（或它的協力廠商執行）依據您的服務所產生。 它會描述 API 的功能，以及如何使用 HTTP 來進行存取。 它可驅動 Swagger UI，並由工具鏈用來啟用探索和產生用戶端程式碼功能。 以下是 OpenAPI 規格的範例，為了簡潔起見，減少了：

```json
{
  "openapi": "3.0.1",
  "info": {
    "title": "API V1",
    "version": "v1"
  },
  "paths": {
    "/api/Todo": {
      "get": {
        "tags": [
          "Todo"
        ],
        "operationId": "ApiTodoGet",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "text/plain": {
                "schema": {
                  "type": "array",
                  "items": {
                    "$ref": "#/components/schemas/ToDoItem"
                  }
                }
              },
              "application/json": {
                "schema": {
                  "type": "array",
                  "items": {
                    "$ref": "#/components/schemas/ToDoItem"
                  }
                }
              },
              "text/json": {
                "schema": {
                  "type": "array",
                  "items": {
                    "$ref": "#/components/schemas/ToDoItem"
                  }
                }
              }
            }
          }
        }
      },
      "post": {
        …
      }
    },
    "/api/Todo/{id}": {
      "get": {
        …
      },
      "put": {
        …
      },
      "delete": {
        …
      }
    }
  },
  "components": {
    "schemas": {
      "ToDoItem": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "name": {
            "type": "string",
            "nullable": true
          },
          "isCompleted": {
            "type": "boolean"
          }
        },
        "additionalProperties": false
      }
    }
  }
}
```

## <a name="swagger-ui"></a>Swagger UI

[SWAGGER ui](https://swagger.io/swagger-ui/)提供 WEB 型 ui，提供服務的相關資訊，並使用產生的 OpenAPI 規格。 Swashbuckle 和 NSwag 都包含內嵌的 Swagger UI 版本，因此它可以使用中介軟體的登錄呼叫裝載在 ASP.NET Core 應用程式中。 Web UI 顯示如下：

![Swagger UI](web-api-help-pages-using-swagger/_static/swagger-ui.png)

控制器中的每個公用動作方法都可以從 UI 進行測試。 按一下方法名稱以展開該區段。 新增任何必要的參數，然後按一下 [**試試看！**]。

![範例 Swagger GET 測試](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> 用於螢幕擷取畫面的 Swagger UI 版本為第 2 版。 如需第 3 版的範例，請參閱 [Petstore 範例](https://petstore.swagger.io/)。

## <a name="next-steps"></a>後續步驟

* [開始使用 Swashbuckle](xref:tutorials/get-started-with-swashbuckle)
* [開始使用 NSwag](xref:tutorials/get-started-with-nswag)
