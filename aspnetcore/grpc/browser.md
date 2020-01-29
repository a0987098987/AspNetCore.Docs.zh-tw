---
title: 瀏覽器應用程式中的 gRPC
author: jamesnk
description: 瞭解如何在 ASP.NET Core 上設定 gRPC 服務，以使用 gRPC-Web 從瀏覽器應用程式呼叫。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 01/24/2020
uid: grpc/browser
ms.openlocfilehash: 6359c3b76b3cb1ba2b6d9f9a989f64cbf4c4379d
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2020
ms.locfileid: "76830657"
---
# <a name="grpc-in-browser-apps"></a>瀏覽器應用程式中的 gRPC

依[James 牛頓-王](https://twitter.com/jamesnk)

> [!IMPORTANT]
> **gRPC-.NET 中的 Web 支援為實驗性**
>
> gRPC-適用于 .NET 的 Web 是實驗性專案，而不是認可的產品。 我們想要：
>
> * 測試我們用來執行 gRPC 的方法-Web works。
> * 如果這種方法對 .NET 開發人員來說很有用，請取得意見反應，相較于傳統的方法是透過 proxy 設定 gRPC Web。
>
> 請在[https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet)留下意見反應，以確保我們建立了開發人員所需的專案，並提高生產力。

您無法從以瀏覽器為基礎的應用程式呼叫 HTTP/2 gRPC 服務。 [gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md)是一種通訊協定，可讓瀏覽器 JavaScript 和 Blazor 應用程式呼叫 gRPC 服務。 本文說明如何在 .NET Core 中使用 gRPC-Web。

## <a name="configure-grpc-web-in-aspnet-core"></a>在 ASP.NET Core 中設定 gRPC-Web

ASP.NET Core 中裝載的 gRPC 服務可以設定為支援 gRPC-Web 與 HTTP/2 gRPC。 gRPC-Web 不需要對服務進行任何變更。 唯一的修改是啟動設定。

若要使用 ASP.NET Core gRPC 服務啟用 gRPC-Web：

* 新增對[Grpc. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web)封裝的參考。
* 藉由將 `AddGrpcWeb` 和 `UseGrpcWeb` 新增至*Startup.cs*，將應用程式設定為使用 gRPC：

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=3,10,14)]

上述程式碼：

* 在路由和結束端點之後，新增 gRPC Web 中介軟體、`UseGrpcWeb`。
* 指定 `endpoints.MapGrpcService<GreeterService>()` 方法支援具有 `EnableGrpcWeb`的 gRPC Web。 

或者，將 `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` 新增至 ConfigureServices，以設定所有服務以支援 gRPC Web。

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=5,12,16)]

可能需要一些額外的設定，才能從瀏覽器呼叫 gRPC-Web，例如設定 ASP.NET Core 以支援 CORS。 如需詳細資訊，請參閱[支援 CORS](xref:security/cors)。

## <a name="call-grpc-web-from-the-browser"></a>從瀏覽器呼叫 gRPC-Web

瀏覽器應用程式可以使用 gRPC 來呼叫 gRPC 服務。 從瀏覽器呼叫具有 gRPC 的 gRPC 服務時，有一些需求和限制：

* 伺服器必須已設定為支援 gRPC-Web。
* 不支援用戶端串流和雙向串流呼叫。 支援伺服器串流。
* 若要在不同的網域上呼叫 gRPC 服務，必須在伺服器上設定[CORS](xref:security/cors) 。

### <a name="javascript-grpc-web-client"></a>JavaScript gRPC-Web 用戶端

有一個 JavaScript gRPC Web 用戶端。 如需如何從 JavaScript 使用 gRPC Web 的指示，請參閱[使用 gRPC 撰寫 javascript 用戶端程式代碼](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code)。

### <a name="configure-grpc-web-with-the-net-grpc-client"></a>使用 .NET gRPC 用戶端設定 gRPC-Web

您可以設定 .NET gRPC 用戶端進行 gRPC-Web 呼叫。 這適用于[Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps，這些應用程式裝載于瀏覽器中，並具有 JavaScript 程式碼的相同 HTTP 限制。 使用 .NET 用戶端呼叫 gRPC Web 與[HTTP/2 gRPC 相同](xref:grpc/client)。 唯一的修改是建立通道的方式。

若要使用 gRPC-Web：

* 將參考新增至[Grpc 的 .net. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web)封裝。
* 設定通道以使用 `GrpcWebHandler`：

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

上述程式碼：

* 設定通道以使用 gRPC-Web。
* 建立用戶端，並使用通道進行呼叫。

建立時，`GrpcWebHandler` 具有下列設定選項：

* **InnerHandler**：進行 HTTP 呼叫的基礎 <xref:System.Net.Http.HttpMessageHandler>，例如 `HttpClientHandler`。
* **模式**： `GrpcWebMode` 列舉。 `GrpcWebMode.GrpcWebText` 會將內容設定為以 base64 編碼，這是支援伺服器串流呼叫所需的設定。
* **HttpVersion**： HTTP 通訊協定 `Version`。 gRPC-Web 不需要特定的通訊協定，也不會在提出要求時指定一個，除非已設定。

## <a name="additional-resources"></a>其他資源

* [適用于 Web 用戶端的 gRPC GitHub 專案](https://github.com/grpc/grpc-web)
* <xref:security/cors>
