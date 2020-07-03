---
title: 在瀏覽器應用程式中使用 gRPC
author: jamesnk
description: 瞭解如何在 ASP.NET Core 上設定 gRPC 服務，以使用 gRPC-Web 從瀏覽器應用程式呼叫。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 06/30/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: grpc/browser
ms.openlocfilehash: 05ff343f7116509128b7370a50bcfa3c67ffb9fe
ms.sourcegitcommit: 66fca14611eba141d455fe0bd2c37803062e439c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2020
ms.locfileid: "85944234"
---
# <a name="use-grpc-in-browser-apps"></a>在瀏覽器應用程式中使用 gRPC

依[James 牛頓-王](https://twitter.com/jamesnk)

 瞭解如何使用[gRPC-Web](https://github.com/grpc/grpc/blob/2a388793792cc80944334535b7c729494d209a7e/doc/PROTOCOL-WEB.md)通訊協定，將現有的 ASP.NET Core gRPC 服務設定為可從瀏覽器應用程式呼叫。 gRPC-Web 可讓瀏覽器 JavaScript 和 Blazor 應用程式呼叫 gRPC 服務。 您無法從以瀏覽器為基礎的應用程式呼叫 HTTP/2 gRPC 服務。 ASP.NET Core 中裝載的 gRPC 服務可以設定為支援 gRPC-Web 與 HTTP/2 gRPC。


如需將 gRPC 服務新增至現有 ASP.NET Core 應用程式的指示，請參閱[將 gRPC 服務新增至 ASP.NET Core 應用程式](xref:grpc/aspnetcore#add-grpc-services-to-an-aspnet-core-app)。

如需建立 gRPC 專案的指示，請參閱 <xref:tutorials/grpc/grpc-start> 。

## <a name="grpc-web-in-aspnet-core-vs-envoy"></a>gRPC-Web in ASP.NET Core 與 Envoy 的比較

有兩種方式可讓您選擇如何將 gRPC 新增至 ASP.NET Core 應用程式：

* 在 ASP.NET Core 中支援 gRPC 和 gRPC HTTP/2。 此選項會使用封裝所提供的中介軟體 `Grpc.AspNetCore.Web` 。
* 使用[Envoy proxy 的](https://www.envoyproxy.io/)GRPC-web 支援，將 GRPC-web 轉譯為 gRPC HTTP/2。 然後，將已轉譯的呼叫轉送到 ASP.NET Core 應用程式。

每種方法都有優缺點。 如果應用程式的環境已經使用 Envoy 做為 proxy，則也可以使用 Envoy 來提供 gRPC-Web 支援。 針對只需要 ASP.NET Core 的 gRPC Web 基本解決方案， `Grpc.AspNetCore.Web` 是不錯的選擇。

## <a name="configure-grpc-web-in-aspnet-core"></a>在 ASP.NET Core 中設定 gRPC-Web

ASP.NET Core 中裝載的 gRPC 服務可以設定為支援 gRPC-Web 與 HTTP/2 gRPC。 gRPC-Web 不需要對服務進行任何變更。 唯一的修改是啟動設定。

若要使用 ASP.NET Core gRPC 服務啟用 gRPC-Web：

* 新增對[Grpc. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web)封裝的參考。
* 藉由將 `UseGrpcWeb` 和新增 `EnableGrpcWeb` 至*Startup.cs*，將應用程式設定為使用 gRPC Web：

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

上述程式碼：

* 加入 gRPC-Web 中介軟體，在 `UseGrpcWeb` 路由之後和結束點之前。
* 指定 `endpoints.MapGrpcService<GreeterService>()` 方法支援使用的 GRPC Web `EnableGrpcWeb` 。 

或者，您可以設定 gRPC-Web 中介軟體，讓所有服務預設都支援 gRPC-Web，而 `EnableGrpcWeb` 不需要。 指定 `new GrpcWebOptions { DefaultEnabled = true }` 加入中介軟體的時機。

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=12)]

> [!NOTE]
> 已知的問題會導致 gRPC 在 .NET Core 3.x 中[由 Http.sys](xref:fundamentals/servers/httpsys)裝載時失敗。
>
> 您可以在[這裡](https://github.com/grpc/grpc-dotnet/issues/853#issuecomment-610078202)找到 gRPC Web 運作 Http.sys 的因應措施。

### <a name="grpc-web-and-cors"></a>gRPC-Web 和 CORS

瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。 這種限制適用于使用瀏覽器應用程式進行 gRPC Web 呼叫。 例如，服務所提供的瀏覽器應用程式 `https://www.contoso.com` 遭到封鎖，無法呼叫裝載于的 GRPC Web 服務 `https://services.contoso.com` 。 跨原始來源資源分享（CORS）可以用來放寬這種限制。

若要允許瀏覽器應用程式進行跨原始來源的 gRPC 呼叫，請[在 ASP.NET Core 中設定 CORS](xref:security/cors)。 使用內建的 CORS 支援，並使用公開 gRPC 特有的標頭 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders%2A> 。

[!code-csharp[](~/grpc/browser/sample/CORS_Startup.cs?name=snippet_1&highlight=5-11,19,24)]

上述程式碼：

* 呼叫 `AddCors` 以新增 cors 服務，並設定會公開 gRPC 特定標頭的 CORS 原則。
* 呼叫 `UseCors` ，以在路由和結束端點之後加入 CORS 中介軟體。
* 指定 `endpoints.MapGrpcService<GreeterService>()` 方法支援 CORS 與 `RequiresCors` 。

### <a name="grpc-web-and-streaming"></a>gRPC-Web 和串流

傳統 gRPC over HTTP/2 支援所有方向的串流。 gRPC-Web 提供有限的串流支援：

* gRPC-網頁瀏覽器用戶端不支援呼叫用戶端資料流程和雙向串流方法。
* Azure App Service 和 IIS 上裝載的 ASP.NET Core gRPC 服務不支援雙向串流。

使用 gRPC-Web 時，我們只建議使用一元方法和伺服器資料流程方法。

## <a name="call-grpc-web-from-the-browser"></a>從瀏覽器呼叫 gRPC-Web

瀏覽器應用程式可以使用 gRPC 來呼叫 gRPC 服務。 從瀏覽器呼叫具有 gRPC 的 gRPC 服務時，有一些需求和限制：

* 伺服器必須已設定為支援 gRPC-Web。
* 不支援用戶端串流和雙向串流呼叫。 支援伺服器串流。
* 若要在不同的網域上呼叫 gRPC 服務，必須在伺服器上設定[CORS](xref:security/cors) 。

### <a name="javascript-grpc-web-client"></a>JavaScript gRPC-Web 用戶端

有一個 JavaScript gRPC Web 用戶端。 如需如何從 JavaScript 使用 gRPC Web 的指示，請參閱[使用 gRPC 撰寫 javascript 用戶端程式代碼](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code)。

### <a name="configure-grpc-web-with-the-net-grpc-client"></a>使用 .NET gRPC 用戶端設定 gRPC-Web

您可以設定 .NET gRPC 用戶端進行 gRPC-Web 呼叫。 這適用于在 [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) 瀏覽器中裝載並具有 JavaScript 程式碼相同 HTTP 限制的應用程式。 使用 .NET 用戶端呼叫 gRPC Web 與[HTTP/2 gRPC 相同](xref:grpc/client)。 唯一的修改是建立通道的方式。

若要使用 gRPC-Web：

* 將參考新增至[Grpc 的 .net. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web)封裝。
* 請確定[Grpc .net. 用戶端](https://www.nuget.org/packages/Grpc.Net.Client)封裝的參考已2.29.0 或更高。
* 設定通道以使用 `GrpcWebHandler` ：

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

上述程式碼：

* 設定通道以使用 gRPC-Web。
* 建立用戶端，並使用通道進行呼叫。

`GrpcWebHandler`具有下列設定選項：

* **InnerHandler**： <xref:System.Net.Http.HttpMessageHandler> 發出 gRPC HTTP 要求的基礎，例如 `HttpClientHandler` 。
* **GrpcWebMode**：指定 gRPC HTTP 要求是否為或的列舉類型 `Content-Type` `application/grpc-web` `application/grpc-web-text` 。
    * `GrpcWebMode.GrpcWeb`設定要在沒有編碼的情況下傳送的內容。 預設值。
    * `GrpcWebMode.GrpcWebText`將內容設定為 base64 編碼。 瀏覽器中的伺服器串流呼叫所需。
* **HttpVersion**： `Version` 用來在基礎 gRPC HTTP 要求上設定[HttpRequestMessage](xref:System.Net.Http.HttpRequestMessage.Version)的 HTTP 通訊協定。 gRPC-Web 不需要特定版本，除非指定，否則不會覆寫預設值。

> [!IMPORTANT]
> 產生的 gRPC 用戶端具有呼叫一元方法的同步和非同步方法。 例如， `SayHello` 會同步處理，而且 `SayHelloAsync` 是非同步。 在應用程式中呼叫同步方法 Blazor WebAssembly 會導致應用程式變得沒有回應。 非同步方法必須一律在中使用 Blazor WebAssembly 。

## <a name="additional-resources"></a>其他資源

* [適用于 Web 用戶端的 gRPC GitHub 專案](https://github.com/grpc/grpc-web)
* <xref:security/cors>
