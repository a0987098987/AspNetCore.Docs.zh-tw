---
title: 在瀏覽器應用程式中使用 gRPC
author: jamesnk
description: 瞭解如何在ASP.NET酷睿配置 gRPC 服務,以便使用 gRPC-Web 從瀏覽器應用調用 gRPC 服務。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 02/16/2020
uid: grpc/browser
ms.openlocfilehash: 0bb8157525ccd32991d8925816c1b599c3d21a92
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977141"
---
# <a name="use-grpc-in-browser-apps"></a>在瀏覽器應用程式中使用 gRPC

由[詹姆斯·牛頓-金](https://twitter.com/jamesnk)

> [!IMPORTANT]
> **.NET 中的 gRPC-Web 支援是實驗性的**
>
> gRPC-Web for .NET 是一個實驗專案,而不是一個承諾的產品。 我們想要：
>
> * 測試我們實現 gRPC-Web 的方法有效。
> * 與通過代理設定 gRPC-Web 的傳統方式相比,獲取此方法對 .NET 開發人員是否有用的回饋。
>
> 請留下回饋,[https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet)以確保我們建構開發者喜歡並高效使用的東西。

無法從基於瀏覽器的應用調用 HTTP/2 gRPC 服務。 [gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md)是一種協定,允許瀏覽器 JavaScript 和 Blazor 應用調用 gRPC 服務。 本文介紹如何在 .NET 核心中使用 gRPC-Web。

## <a name="configure-grpc-web-in-aspnet-core"></a>在 ASP.NET核心中設定 gRPC-Web

ASP.NET核心中託管的 gRPC 服務可以配置為支援 gRPC-Web 以及 HTTP/2 gRPC。 gRPC-Web 不需要對服務進行任何更改。 唯一的修改是啟動配置。

要啟用具有ASP.NET核心 gRPC 服務的 gRPC-Web:

* 添加對[Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web)包的引用。
* 使用 gRPC-Web,透過`AddGrpcWeb`新增`UseGrpcWeb`與*Startup.cs*:

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

上述程式碼：

* 在路由後與終結點之前加入 gRPC-Web`UseGrpcWeb`中間件 。
* 指定方法`endpoints.MapGrpcService<GreeterService>()`支援 gRPC-Web。 `EnableGrpcWeb` 

或者,通過添加到`services.AddGrpcWeb(o => o.GrpcWebEnabled = true);`配置服務,將所有服務配置為支援 gRPC-Web。

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=6,13)]

> [!NOTE]
> 在 .NET Core [3.x 中由 Http.sys 託管](xref:fundamentals/servers/httpsys)時,存在導致 gRPC-Web 失敗這一已知問題。
>
> 此處[提供了一](https://github.com/grpc/grpc-dotnet/issues/853#issuecomment-610078202)種解決方法,用於使 gRPC-Web 在 Http.sys 上工作。

### <a name="grpc-web-and-cors"></a>gRPC-Web 和 CORS

流覽器安全性可防止網頁向與服務網頁的域不同的域發出請求。 此限制適用於使用瀏覽器應用進行 gRPC-Web 調用。 例如,所`https://www.contoso.com`服務的瀏覽器應用被阻止調用 託管在的`https://services.contoso.com`gRPC-Web 服務。 跨源資源分享 (CORS) 可用於放寬此限制。

要允許瀏覽器應用進行交叉源 gRPC-Web 調用,請[ASP.NET核心 中設置 CORS。](xref:security/cors) 使用內建的 CORS 支援,並使用 公開特定於<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>gRPC 的標頭。

[!code-csharp[](~/grpc/browser/sample/CORS_Startup.cs?name=snippet_1&highlight=5-11,19,24)]

上述程式碼：

* 呼叫`AddCors`以添加 CORS 服務並配置一個 CORS 策略,該策略公開 gRPC 特定的標頭。
* 在`UseCors`路由後和終結點之前添加 CORS 中間件的調用。
* 指定方法`endpoints.MapGrpcService<GreeterService>()`支援`RequiresCors`具有的 CORS。

## <a name="call-grpc-web-from-the-browser"></a>從瀏覽器呼叫 gRPC-Web

瀏覽器應用可以使用 gRPC-Web 調用 gRPC 服務。 從瀏覽器使用 gRPC-Web 呼叫 gRPC 服務時有一些要求和限制:

* 伺服器必須配置為支援 gRPC-Web。
* 不支援用戶端流式處理和雙向流式處理調用。 支援伺服器流。
* 在不同的域上調用 gRPC 服務需要在伺服器上配置[CORS。](xref:security/cors)

### <a name="javascript-grpc-web-client"></a>JavaScript gRPC-Web 用戶端

有一個JavaScriptgRPC-Web用戶端。 有關如何使用 JavaScript 中的 gRPC-Web 的說明,請參閱[使用 gRPC-Web 撰寫 JavaScript 客戶端碼](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code)。

### <a name="configure-grpc-web-with-the-net-grpc-client"></a>使用 .NET gRPC 用戶端設定 gRPC-Web

.NET gRPC 用戶端可以配置為進行 gRPC-Web 調用。 這對於[Blazor WebAssembly](xref:blazor/index#blazor-webassembly)應用很有用,這些應用託管在瀏覽器中,並且具有相同的 JavaScript 代碼的 HTTP 限制。 使用 .NET 用戶端呼叫 gRPC-Web[與 HTTP/2 gRPC 相同](xref:grpc/client)。 唯一的修改是如何創建通道。

要使用 gRPC-Web:

* 添加對[Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web)包的引用。
* 確保對[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client)包的引用為 2.27.0 或更高。
* 將通道設定為使用`GrpcWebHandler`:

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

上述程式碼：

* 配置通道以使用 gRPC-Web。
* 創建用戶端並使用通道進行呼叫。

建立`GrpcWebHandler`時具有以下設定選項:

* **內部處理程式**:<xref:System.Net.Http.HttpMessageHandler>使 gRPC HTTP 要求`HttpClientHandler`的基礎,例如 。
* **模式**:指定 gRPC`Content-Type``application/grpc-web`HTTP`application/grpc-web-text`請求 是 或的枚舉類型。
    * `GrpcWebMode.GrpcWeb`配置內容,無需編碼即可發送。 預設值。
    * `GrpcWebMode.GrpcWebText`將內容配置為基本編碼。 瀏覽器中的伺服器流式調用是必需的。
* **HTTPVersion** `Version` : HTTP 協定用於在基礎 gRPC HTTP 請求上設定[HTTPRequestMessage.Version。](xref:System.Net.Http.HttpRequestMessage.Version) gRPC-Web 不需要特定版本,除非指定,否則不會覆蓋預設值。

> [!IMPORTANT]
> 生成的 gRPC 用戶端具有用於調用一元方法的同步和非同步方法。 例如,`SayHello`是同步`SayHelloAsync`和非同步。 在 Blazor WebAssembly 應用中調用同步方法將導致應用無回應。 在 Blazor WebAssembly 中必須始終使用非同步方法。

## <a name="additional-resources"></a>其他資源

* [GRPC](https://github.com/grpc/grpc-web)
* <xref:security/cors>
