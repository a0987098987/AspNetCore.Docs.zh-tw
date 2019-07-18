---
title: 使用 Swagger/OpenAPI 的 ASP.NET Core Web API 說明頁面
author: rsuter
description: 本教學課程提供新增 Swagger，以產生 Web API 應用程式之文件和說明頁面的逐步解說。
ms.author: scaddie
ms.custom: mvc
ms.date: 09/20/2018
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 06a942ca213c7c0a58932494be0ed23cb6896d48
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856097"
---
# <a name="aspnet-core-web-api-help-pages-with-swagger--openapi"></a><span data-ttu-id="102ba-103">使用 Swagger/OpenAPI 的 ASP.NET Core Web API 說明頁面</span><span class="sxs-lookup"><span data-stu-id="102ba-103">ASP.NET Core web API help pages with Swagger / OpenAPI</span></span>

<span data-ttu-id="102ba-104">作者：[Christoph Nienaber](https://twitter.com/zuckerthoben) 和 [Rico Suter](https://blog.rsuter.com/)</span><span class="sxs-lookup"><span data-stu-id="102ba-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://blog.rsuter.com/)</span></span>

<span data-ttu-id="102ba-105">使用 Web API 時，了解其各種不同的方法對開對發人員來說可能是一項挑戰。</span><span class="sxs-lookup"><span data-stu-id="102ba-105">When consuming a Web API, understanding its various methods can be challenging for a developer.</span></span> <span data-ttu-id="102ba-106">[Swagger](https://swagger.io/) (也稱為 [Open API](https://www.openapis.org/)) 解決為 Web API 產生有用文件與說明頁面的問題。</span><span class="sxs-lookup"><span data-stu-id="102ba-106">[Swagger](https://swagger.io/), also known as [OpenAPI](https://www.openapis.org/), solves the problem of generating useful documentation and help pages for Web APIs.</span></span> <span data-ttu-id="102ba-107">它提供如互動式文件、用戶端 SDK 產生作業和 API 發現性等優點。</span><span class="sxs-lookup"><span data-stu-id="102ba-107">It provides benefits such as interactive documentation, client SDK generation, and API discoverability.</span></span>

<span data-ttu-id="102ba-108">在本文中，展示了 [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) 和 [NSwag](https://github.com/RicoSuter/NSwag) .NET Swagger 實作：</span><span class="sxs-lookup"><span data-stu-id="102ba-108">In this article, the [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) and [NSwag](https://github.com/RicoSuter/NSwag) .NET Swagger implementations are showcased:</span></span>

* <span data-ttu-id="102ba-109">**Swashbuckle.AspNetCore** 是用來為 ASP.NET Core Web API 產生 Swagger 文件的開放原始碼專案。</span><span class="sxs-lookup"><span data-stu-id="102ba-109">**Swashbuckle.AspNetCore** is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="102ba-110">**NSwag** 是用來產生 Swagger 文件，並將 [Swagger UI](https://swagger.io/swagger-ui/) 或 [ReDoc](https://github.com/Rebilly/ReDoc) 整合到 ASP.NET Core Web API 的其他開放原始碼專案。</span><span class="sxs-lookup"><span data-stu-id="102ba-110">**NSwag** is another open source project for generating Swagger documents and integrating [Swagger UI](https://swagger.io/swagger-ui/) or [ReDoc](https://github.com/Rebilly/ReDoc) into ASP.NET Core web APIs.</span></span> <span data-ttu-id="102ba-111">此外，NSwag 提供方法來為您的 API 產生 C# 和 TypeScript 用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="102ba-111">Additionally, NSwag offers approaches to generate C# and TypeScript client code for your API.</span></span>

## <a name="what-is-swagger--openapi"></a><span data-ttu-id="102ba-112">什麼是 Swagger/OpenAPI？</span><span class="sxs-lookup"><span data-stu-id="102ba-112">What is Swagger / OpenAPI?</span></span>

<span data-ttu-id="102ba-113">Swagger 是用來描述 [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) API 的語言無關規格。</span><span class="sxs-lookup"><span data-stu-id="102ba-113">Swagger is a language-agnostic specification for describing [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) APIs.</span></span> <span data-ttu-id="102ba-114">Swagger 專案已捐贈給 [OpenAPI Initiative](https://www.openapis.org/) (OpenAPI 方案)，目前在該方案中將其稱為 OpenAPI。</span><span class="sxs-lookup"><span data-stu-id="102ba-114">The Swagger project was donated to the [OpenAPI Initiative](https://www.openapis.org/), where it's now referred to as OpenAPI.</span></span> <span data-ttu-id="102ba-115">這兩個名稱可交替使用；不過，OpenAPI 是慣用名稱。</span><span class="sxs-lookup"><span data-stu-id="102ba-115">Both names are used interchangeably; however, OpenAPI is preferred.</span></span> <span data-ttu-id="102ba-116">它可讓電腦和人類都了解服務的功能，而不必直接存取實作 (原始碼、網路存取、文件)。</span><span class="sxs-lookup"><span data-stu-id="102ba-116">It allows both computers and humans to understand the capabilities of a service without any direct access to the implementation (source code, network access, documentation).</span></span> <span data-ttu-id="102ba-117">其中一個目標是要將連線未相關聯之服務所需的工作量減到最少。</span><span class="sxs-lookup"><span data-stu-id="102ba-117">One goal is to minimize the amount of work needed to connect disassociated services.</span></span> <span data-ttu-id="102ba-118">另一個目標則是減少正確記錄服務所需的時間量。</span><span class="sxs-lookup"><span data-stu-id="102ba-118">Another goal is to reduce the amount of time needed to accurately document a service.</span></span>

## <a name="swagger-specification-swaggerjson"></a><span data-ttu-id="102ba-119">Swagger 規格 (swagger.json)</span><span class="sxs-lookup"><span data-stu-id="102ba-119">Swagger specification (swagger.json)</span></span>

<span data-ttu-id="102ba-120">Swagger 流程的核心是 Swagger 規格&mdash;根據預設，文件名為 *swagger.json*。</span><span class="sxs-lookup"><span data-stu-id="102ba-120">The core to the Swagger flow is the Swagger specification&mdash;by default, a document named *swagger.json*.</span></span> <span data-ttu-id="102ba-121">它是由 Swagger 工具鏈 (或其協力廠商實作) 根據您的服務所產生。</span><span class="sxs-lookup"><span data-stu-id="102ba-121">It's generated by the Swagger tool chain (or third-party implementations of it) based on your service.</span></span> <span data-ttu-id="102ba-122">它會描述 API 的功能，以及如何使用 HTTP 來進行存取。</span><span class="sxs-lookup"><span data-stu-id="102ba-122">It describes the capabilities of your API and how to access it with HTTP.</span></span> <span data-ttu-id="102ba-123">它可驅動 Swagger UI，並由工具鏈用來啟用探索和產生用戶端程式碼功能。</span><span class="sxs-lookup"><span data-stu-id="102ba-123">It drives the Swagger UI and is used by the tool chain to enable discovery and client code generation.</span></span> <span data-ttu-id="102ba-124">以下是 Swagger 規格的範例，為求簡單明瞭，其已經過縮減：</span><span class="sxs-lookup"><span data-stu-id="102ba-124">Here's an example of a Swagger specification, reduced for brevity:</span></span>

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

## <a name="swagger-ui"></a><span data-ttu-id="102ba-125">Swagger UI</span><span class="sxs-lookup"><span data-stu-id="102ba-125">Swagger UI</span></span>

<span data-ttu-id="102ba-126">[Swagger UI](https://swagger.io/swagger-ui/) 使用產生的 Swagger 規格提供 Web UI ，以提供服務的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="102ba-126">[Swagger UI](https://swagger.io/swagger-ui/) offers a web-based UI that provides information about the service, using the generated Swagger specification.</span></span> <span data-ttu-id="102ba-127">Swashbuckle 和 NSwag 都包含內嵌的 Swagger UI 版本，因此它可以使用中介軟體的登錄呼叫裝載在 ASP.NET Core 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="102ba-127">Both Swashbuckle and NSwag include an embedded version of Swagger UI, so that it can be hosted in your ASP.NET Core app using a middleware registration call.</span></span> <span data-ttu-id="102ba-128">Web UI 顯示如下：</span><span class="sxs-lookup"><span data-stu-id="102ba-128">The web UI looks like this:</span></span>

![Swagger UI](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="102ba-130">控制器中的每個公用動作方法都可以從 UI 進行測試。</span><span class="sxs-lookup"><span data-stu-id="102ba-130">Each public action method in your controllers can be tested from the UI.</span></span> <span data-ttu-id="102ba-131">按一下方法名稱以展開該區段。</span><span class="sxs-lookup"><span data-stu-id="102ba-131">Click a method name to expand the section.</span></span> <span data-ttu-id="102ba-132">新增任何必要的參數，然後按一下 [試試看！]  。</span><span class="sxs-lookup"><span data-stu-id="102ba-132">Add any necessary parameters, and click **Try it out!**.</span></span>

![範例 Swagger GET 測試](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> <span data-ttu-id="102ba-134">用於螢幕擷取畫面的 Swagger UI 版本為第 2 版。</span><span class="sxs-lookup"><span data-stu-id="102ba-134">The Swagger UI version used for the screenshots is version 2.</span></span> <span data-ttu-id="102ba-135">如需第 3 版的範例，請參閱 [Petstore 範例](https://petstore.swagger.io/)。</span><span class="sxs-lookup"><span data-stu-id="102ba-135">For a version 3 example, see [Petstore example](https://petstore.swagger.io/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="102ba-136">後續步驟</span><span class="sxs-lookup"><span data-stu-id="102ba-136">Next steps</span></span>

* [<span data-ttu-id="102ba-137">開始使用 Swashbuckle</span><span class="sxs-lookup"><span data-stu-id="102ba-137">Get started with Swashbuckle</span></span>](xref:tutorials/get-started-with-swashbuckle)
* [<span data-ttu-id="102ba-138">開始使用 NSwag</span><span class="sxs-lookup"><span data-stu-id="102ba-138">Get started with NSwag</span></span>](xref:tutorials/get-started-with-nswag)
