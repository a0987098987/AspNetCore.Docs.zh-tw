---
title: ASP.NET Core 2.2 的新功能
author: tdykstra
description: 了解 ASP.NET Core 2.2 的新功能。
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/18/2018
uid: aspnetcore-2.2
ms.openlocfilehash: 6dcdf71ec5271690718dd1fe750a9a74d498a0f8
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410332"
---
# <a name="whats-new-in-aspnet-core-22"></a>ASP.NET Core 2.2 的新功能

此文章重點說明 ASP.NET Core 2.2 最重要的變更，附有相關文件的連結。

## <a name="open-api-analyzers--conventions"></a>Open API 分析器與慣例

Open API (也稱為 Swagger) 是用來描述 REST API 的語言無關規格。 Open API 生態系統中已有工具，可讓您使用此規格來探索、測試和產生用戶端程式碼。 您可以透過社群導向專案 (例如 [NSwag](https://github.com/RSuter/NSwag) 和 [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore))，在 ASP.NET Core MVC 中產生和視覺化 Open API 文件。 ASP.NET Core 2.2 提供改良的工具和執行階段體驗來建立 Open API 文件。

如需詳細資訊，請參閱下列資源：

* <xref:web-api/advanced/analyzers>
* <xref:web-api/advanced/conventions>
* [ASP.NET Core 2.2.0-preview1:Open API Analyzers & Conventions](https://blogs.msdn.microsoft.com/webdev/2018/08/23/asp-net-core-2-20-preview1-open-api-analyzers-conventions/) (ASP.NET Core 2.2.0-preview1：Open API 分析器與慣例)

## <a name="problem-details-support"></a>問題詳細資料支援

ASP.NET Core 2.1 引進了 `ProblemDetails`，它採用 [RFC 7807](https://tools.ietf.org/html/rfc7807) 規格在 HTTP 回應中包含錯誤的詳細資料。 在 2.2 中，`ProblemDetails` 是對具有 `ApiControllerAttribute` 屬性之控制器中用戶端錯誤碼的標準回應。 傳回用戶端錯誤狀態碼 (4xx) 的 `IActionResult` 現在會傳回 `ProblemDetails` 主體。 結果中也會包含相互關聯識別碼，可用來透過要求記錄檔與錯誤相互關聯。 對於用戶端錯誤，`ProducesResponseType` 預設會使用 `ProblemDetails` 作為回應類型。 這會記載於使用 NSwag 或 Swashbuckle.AspNetCore 產生的 Open API/Swagger 輸出中。

## <a name="endpoint-routing"></a>端點路由

ASP.NET Core 2.2 使用新的「端點路由」系統來改善要求的分派。 其變更包括新的連結產生 API 成員和路由參數轉換器。

如需詳細資訊，請參閱下列資源：

* [Endpoint routing in 2.2](https://blogs.msdn.microsoft.com/webdev/2018/08/27/asp-net-core-2-2-0-preview1-endpoint-routing/) (2.2 中的端點路由)
* [路由參數轉換器](https://www.hanselman.com/blog/ASPNETCore22ParameterTransformersForCleanURLGenerationAndSlugsInRazorPagesOrMVC.aspx) (請參閱＜Routing＞(路由) 一節)
* [IRouter 路由與端點路由之間的差異](xref:fundamentals/routing?view=aspnetcore-2.2#differences-from-earlier-versions-of-routing)

## <a name="health-checks"></a>健康狀態檢查

新的健康狀態檢查服務可讓您更輕鬆地在需要健康狀態檢查的環境中 (例如 Kubernetes) 使用 ASP.NET Core。 健康狀態檢查包括中介軟體，以及定義 `IHealthCheck` 抽象概念和服務的一組程式庫。

容器協調器或負載平衡器使用健康狀態檢查，來快速判斷系統是否正常回應要求。 容器協調器可能會暫停輪流部署或重新啟動容器，來回應失敗的健康狀態檢查。 負載平衡器可能會從服務的失敗執行個體路由出流量，來回應健康狀態檢查。

應用程式會將健康狀態檢查公開為監控系統所使用的 HTTP 端點。 您可以針對各種即時監控案例和監控系統來設定健康狀態檢查。 健康狀態檢查服務與 [BeatPulse 專案](https://github.com/Xabaril/BeatPulse)整合。 這可讓您更輕鬆地新增數十個熱門系統和相依性的檢查。

如需詳細資訊，請參閱 [ASP.NET Core 中的健康狀態檢查](xref:host-and-deploy/health-checks)。

## <a name="http2-in-kestrel"></a>Kestrel 中的 HTTP/2

ASP.NET Core 2.2 新增 HTTP/2 支援。 

HTTP/2 是 HTTP 通訊協定的主要版本。 一些值得注意的 HTTP/2 功能包括支援標頭壓縮，以及透過單一連線進行完全多工的資料流。 雖然 HTTP/2 會保留 HTTP 的語意 (HTTP 標頭、方法等)，但它是自 HTTP/1.x 以來的重大變更，對於此資料如何進行框架處理及透過網路傳送皆有所著墨。

這項框架處理變更導致伺服器和用戶端必須交涉所使用的通訊協定版本。 Application-Layer Protocol Negotiation (ALPN) 是 TLS 延伸模組，可讓伺服器和用戶端交涉在其 TLS 信號交換過程中所使用的通訊協定版本。 雖然您可能事先知道伺服器與用戶端之間的通訊協定，但所有主要瀏覽器都支援 ALPN 作為建立 HTTP/2 連線的唯一方式。

如需詳細資訊，請參閱 [HTTP/2 支援](xref:fundamentals/servers/index?view=aspnetcore-2.2#http2-support)。

## <a name="kestrel-configuration"></a>Kestrel 組態

在舊版的 ASP.NET Core 中，Kestrel 選項是透過呼叫 `UseKestrel` 設定。 在 2.2 中，Kestrel 選項是透過在主機產生器上呼叫 `ConfigureKestrel` 設定。 這項變更解決了同處理序裝載的 `IServer` 註冊順序問題。 如需詳細資訊，請參閱下列資源：

* [Mitigate UseIIS conflict](https://github.com/aspnet/KestrelHttpServer/issues/2760) (降低 UseIIS 衝突)
* [使用 ConfigureKestrel 設定 Kestrel 伺服器選項](xref:fundamentals/servers/kestrel?view=aspnetcore-2.2#how-to-use-kestrel-in-aspnet-core-apps)

## <a name="iis-in-process-hosting"></a>IIS 同處理序裝載

在舊版的 ASP.NET Core 中，IIS 是作為反向 Proxy。 在 2.2 中，ASP.NET Core 模組可啟動 CoreCLR，並在 IIS 背景工作處理序 (*w3wp.exe*) 內裝載應用程式。 同處理序裝載以 IIS 執行時可改善效能和診斷。

如需詳細資訊，請參閱 [IIS 的同處理序裝載](xref:host-and-deploy/aspnet-core-module?view=aspnetcore-2.2#in-process-hosting-model)。

## <a name="signalr-java-client"></a>SignalR Java 用戶端

ASP.NET Core 2.2 引進適用於 SignalR 的 Java 用戶端。 此用戶端支援從 Java 程式碼連線到 ASP.NET Core SignalR 伺服器，包括 Android 應用程式。

如需詳細資訊，請參閱 [ASP.NET Core SignalR Java 用戶端](https://docs.microsoft.com/aspnet/core/signalr/java-client?view=aspnetcore-2.2)。

## <a name="cors-improvements"></a>CORS 改善

在舊版的 ASP.NET Core 中，CORS 中介軟體允許傳送 `Accept`、`Accept-Language`、`Content-Language` 和 `Origin` 標頭，無論 `CorsPolicy.Headers` 中設定的值為何。 在 2.2 中，只有 `Access-Control-Request-Headers` 中傳送的標頭與 `WithHeaders` 中指定的標頭完全相符時，才可能符合 CORS 中介軟體原則。

如需詳細資訊，請參閱 [CORS 中介軟體](xref:security/cors?view=aspnetcore-2.2#set-the-allowed-request-headers)。

## <a name="response-compression"></a>回應壓縮

ASP.NET Core 2.2 可以使用 [Brotli 壓縮格式](https://tools.ietf.org/html/rfc7932)來壓縮回應。

如需詳細資訊，請參閱[回應壓縮中介軟體支援 Brotli 壓縮](xref:performance/response-compression?view=aspnetcore-2.2#brotli-compression-provider)。

## <a name="project-templates"></a>專案範本

ASP.NET Core Web 專案範本已更新為 [Bootstrap 4](https://getbootstrap.com/docs/4.1/migration/) 和 [Angular 6](https://blog.angular.io/version-6-of-angular-now-available-cc56b0efa7a4)。 新的外觀看起來比較簡單，因此更容易看出應用程式的重要結構。

![Home 或 Index 頁面](~/tutorials/razor-pages/razor-pages-start/_static/home2.2.png)

## <a name="validation-performance"></a>驗證效能

MVC 的驗證系統設計成可延伸且具有彈性，因此您可以根據每個要求判斷要套用至指定模型的驗證程式。 這有助於撰寫複雜的驗證提供者。 不過，在最常見的情況下，應用程式只會使用內建驗證程式，而不需要此額外的彈性。 內建驗證程式包括 DataAnnotations (例如 [Required] 和 [StringLength]) 以及 `IValidatableObject`。

在 ASP.NET Core 2.2 中，若 MVC 判斷指定的模型圖形不需要驗證，則會跳過驗證。 當驗證無法驗證或沒有任何驗證程式的模型時，跳過驗證可大幅改善效能。 這包括諸如基本類型集合 (例如 `byte[]`、`string[]`、`Dictionary<string, string>`) 的物件，或沒有許多驗證程式的複雜物件圖形。

## <a name="http-client-performance"></a>HTTP 用戶端效能

在 ASP.NET Core 2.2 中，由於降低連接集區鎖定競爭情況，因此改善了 `SocketsHttpHandler` 的效能。 針對進行許多傳出 HTTP 要求的應用程式 (例如某些微服務架構)，輸送量已獲得改善。 在負載情況下，`HttpClient` 輸送量在 Linux 上可提升高達 60%，在 Windows 上可提升高達 20%。

如需詳細資訊，請參閱[已做出這項改善的提取要求](https://github.com/dotnet/corefx/pull/32568)。

## <a name="additional-information"></a>其他資訊

如需完整的變更清單，請參閱 [ASP.NET Core 2.2 版本資訊](https://github.com/aspnet/Home/releases/tag/2.2.0)。
