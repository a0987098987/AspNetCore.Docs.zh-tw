---
title: 使用 Swagger/Open API 的 ASP.NET Core Web API 說明頁面
author: rsuter
description: 本教學課程提供新增 Swagger，以產生 Web API 應用程式之文件和說明頁面的逐步解說。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: e44b491fd5265e12646efa42f12eb0662e287f04
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
---
# <a name="aspnet-core-web-api-help-pages-with-swagger--open-api"></a><span data-ttu-id="fa64d-103">使用 Swagger/Open API 的 ASP.NET Core Web API 說明頁面</span><span class="sxs-lookup"><span data-stu-id="fa64d-103">ASP.NET Core Web API help pages with Swagger / Open API</span></span>

<span data-ttu-id="fa64d-104">作者：[Christoph Nienaber](https://twitter.com/zuckerthoben) 和 [Rico Suter](http://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="fa64d-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](http://rsuter.com)</span></span>

<span data-ttu-id="fa64d-105">使用 Web API 時，了解其各種不同的方法對開對發人員來說可能是一項挑戰。</span><span class="sxs-lookup"><span data-stu-id="fa64d-105">When consuming a Web API, understanding its various methods can be challenging for a developer.</span></span> <span data-ttu-id="fa64d-106">[Swagger](https://swagger.io/) (也稱為 Open API) 解決為 Web API 產生有用文件與說明頁面的問題。</span><span class="sxs-lookup"><span data-stu-id="fa64d-106">[Swagger](https://swagger.io/), also known as Open API, solves the problem of generating useful documentation and help pages for Web APIs.</span></span> <span data-ttu-id="fa64d-107">它提供如互動式文件、用戶端 SDK 產生作業和 API 發現性等優點。</span><span class="sxs-lookup"><span data-stu-id="fa64d-107">It provides benefits such as interactive documentation, client SDK generation, and API discoverability.</span></span>

<span data-ttu-id="fa64d-108">在本文中，展示了 [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) 和 [NSwag](https://github.com/RSuter/NSwag) .NET Swagger 實作：</span><span class="sxs-lookup"><span data-stu-id="fa64d-108">In this article, the [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) and [NSwag](https://github.com/RSuter/NSwag) .NET Swagger implementations are showcased:</span></span>

* <span data-ttu-id="fa64d-109">**Swashbuckle.AspNetCore** 是用來為 ASP.NET Core Web API 產生 Swagger 文件的開放原始碼專案。</span><span class="sxs-lookup"><span data-stu-id="fa64d-109">**Swashbuckle.AspNetCore** is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="fa64d-110">**NSwag** 是用來將 [Swagger UI](https://swagger.io/swagger-ui/) 或 [ReDoc](https://github.com/Rebilly/ReDoc) 整合到 ASP.NET Core Web API 的其他開放原始碼專案。</span><span class="sxs-lookup"><span data-stu-id="fa64d-110">**NSwag** is another open source project for integrating [Swagger UI](https://swagger.io/swagger-ui/) or [ReDoc](https://github.com/Rebilly/ReDoc) into ASP.NET Core Web APIs.</span></span> <span data-ttu-id="fa64d-111">它提供方法來為您的 API 產生 C# 和 TypeScript 用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="fa64d-111">It offers approaches to generate C# and TypeScript client code for your API.</span></span>

## <a name="what-is-swagger--open-api"></a><span data-ttu-id="fa64d-112">何謂 Swagger/Open API？</span><span class="sxs-lookup"><span data-stu-id="fa64d-112">What is Swagger / Open API?</span></span>

<span data-ttu-id="fa64d-113">Swagger 是用來描述 [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) API 的語言無關規格。</span><span class="sxs-lookup"><span data-stu-id="fa64d-113">Swagger is a language-agnostic specification for describing [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) APIs.</span></span> <span data-ttu-id="fa64d-114">Swagger 專案已捐贈給 [OpenAPI Initiative](https://www.openapis.org/) (OpenAPI 方案)，目前在該方案中它稱為 Open API。</span><span class="sxs-lookup"><span data-stu-id="fa64d-114">The Swagger project was donated to the [OpenAPI Initiative](https://www.openapis.org/), where it's now referred to as Open API.</span></span> <span data-ttu-id="fa64d-115">這兩個名稱可交替使用；不過，Open API 是慣用名稱。</span><span class="sxs-lookup"><span data-stu-id="fa64d-115">Both names are used interchangeably; however, Open API is preferred.</span></span> <span data-ttu-id="fa64d-116">它可讓電腦和人類都了解服務的功能，而不必直接存取實作 (原始碼、網路存取、文件)。</span><span class="sxs-lookup"><span data-stu-id="fa64d-116">It allows both computers and humans to understand the capabilities of a service without any direct access to the implementation (source code, network access, documentation).</span></span> <span data-ttu-id="fa64d-117">其中一個目標是要將連線未相關聯之服務所需的工作量減到最少。</span><span class="sxs-lookup"><span data-stu-id="fa64d-117">One goal is to minimize the amount of work needed to connect disassociated services.</span></span> <span data-ttu-id="fa64d-118">另一個目標則是減少正確記錄服務所需的時間量。</span><span class="sxs-lookup"><span data-stu-id="fa64d-118">Another goal is to reduce the amount of time needed to accurately document a service.</span></span>

## <a name="swagger-specification-swaggerjson"></a><span data-ttu-id="fa64d-119">Swagger 規格 (swagger.json)</span><span class="sxs-lookup"><span data-stu-id="fa64d-119">Swagger specification (swagger.json)</span></span>

<span data-ttu-id="fa64d-120">Swagger 流程的核心是 Swagger 規格&mdash;根據預設，文件名為 *swagger.json*。</span><span class="sxs-lookup"><span data-stu-id="fa64d-120">The core to the Swagger flow is the Swagger specification&mdash;by default, a document named *swagger.json*.</span></span> <span data-ttu-id="fa64d-121">它是由 Swagger 工具鏈 (或其協力廠商實作) 根據您的服務所產生。</span><span class="sxs-lookup"><span data-stu-id="fa64d-121">It's generated by the Swagger tool chain (or third-party implementations of it) based on your service.</span></span> <span data-ttu-id="fa64d-122">它會描述 API 的功能，以及如何使用 HTTP 來進行存取。</span><span class="sxs-lookup"><span data-stu-id="fa64d-122">It describes the capabilities of your API and how to access it with HTTP.</span></span> <span data-ttu-id="fa64d-123">它可驅動 Swagger UI，並由工具鏈用來啟用探索和產生用戶端程式碼功能。</span><span class="sxs-lookup"><span data-stu-id="fa64d-123">It drives the Swagger UI and is used by the tool chain to enable discovery and client code generation.</span></span> <span data-ttu-id="fa64d-124">以下是 Swagger 規格的範例，為求簡單明瞭，其已經過縮減：</span><span class="sxs-lookup"><span data-stu-id="fa64d-124">Here's an example of a Swagger specification, reduced for brevity:</span></span>

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

## <a name="swagger-ui"></a><span data-ttu-id="fa64d-125">Swagger UI</span><span class="sxs-lookup"><span data-stu-id="fa64d-125">Swagger UI</span></span>

<span data-ttu-id="fa64d-126">[Swagger UI](https://swagger.io/swagger-ui/) 使用產生的 Swagger 規格提供 Web UI ，以提供服務的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="fa64d-126">[Swagger UI](https://swagger.io/swagger-ui/) offers a web-based UI that provides information about the service, using the generated Swagger specification.</span></span> <span data-ttu-id="fa64d-127">Swashbuckle 和 NSwag 都包含內嵌的 Swagger UI 版本，因此它可以使用中介軟體的登錄呼叫裝載在 ASP.NET Core 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="fa64d-127">Both Swashbuckle and NSwag include an embedded version of Swagger UI, so that it can be hosted in your ASP.NET Core app using a middleware registration call.</span></span> <span data-ttu-id="fa64d-128">Web UI 顯示如下：</span><span class="sxs-lookup"><span data-stu-id="fa64d-128">The web UI looks like this:</span></span>

![Swagger UI](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="fa64d-130">控制器中的每個公用動作方法都可以從 UI 進行測試。</span><span class="sxs-lookup"><span data-stu-id="fa64d-130">Each public action method in your controllers can be tested from the UI.</span></span> <span data-ttu-id="fa64d-131">按一下方法名稱以展開該區段。</span><span class="sxs-lookup"><span data-stu-id="fa64d-131">Click a method name to expand the section.</span></span> <span data-ttu-id="fa64d-132">新增任何必要的參數，然後按一下 [試試看！]。</span><span class="sxs-lookup"><span data-stu-id="fa64d-132">Add any necessary parameters, and click **Try it out!**.</span></span>

![範例 Swagger GET 測試](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> <span data-ttu-id="fa64d-134">用於螢幕擷取畫面的 Swagger UI 版本為第 2 版。</span><span class="sxs-lookup"><span data-stu-id="fa64d-134">The Swagger UI version used for the screenshots is version 2.</span></span> <span data-ttu-id="fa64d-135">如需第 3 版的範例，請參閱 [Petstore 範例](http://petstore.swagger.io/)。</span><span class="sxs-lookup"><span data-stu-id="fa64d-135">For a version 3 example, see [Petstore example](http://petstore.swagger.io/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fa64d-136">後續步驟</span><span class="sxs-lookup"><span data-stu-id="fa64d-136">Next steps</span></span>

* [<span data-ttu-id="fa64d-137">開始使用 Swashbuckle</span><span class="sxs-lookup"><span data-stu-id="fa64d-137">Get started with Swashbuckle</span></span>](xref:tutorials/get-started-with-swashbuckle)
* [<span data-ttu-id="fa64d-138">開始使用 NSwag</span><span class="sxs-lookup"><span data-stu-id="fa64d-138">Get started with NSwag</span></span>](xref:tutorials/get-started-with-nswag)
