---
title: ASP.NET Core 2.2 的新功能
author: rick-anderson
description: 了解 ASP.NET Core 2.2 的新功能。
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- SignalR
uid: aspnetcore-2.2
ms.openlocfilehash: 54d3f1e7b0c94d69781c052694305a389a675019
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977167"
---
# <a name="whats-new-in-aspnet-core-22"></a>ASP.NET Core 2.2 的新功能

本文會重點說明 ASP.NET Core 2.2 最重要的變更，附有相關文件的連結。

## <a name="openapi-analyzers--conventions"></a>OpenAPI 分析器與慣例

OpenAPI (之前稱為 Swagger) 是用來描述 REST API 的語言無關規格。 OpenAPI 生態系統中已有工具，可讓您使用此規格來探索、測試和產生用戶端程式碼。 支援在ASP.NET核心MVC中生成和可視化OpenAPI文檔,通過社區驅動的專案(如[NSwag](https://github.com/RicoSuter/NSwag)和[Swashbuckle)](https://github.com/domaindrivendev/Swashbuckle.AspNetCore)提供。 ASP.NET Core 2.2 提供改善的工具和執行階段體驗來建立 OpenAPI 文件。

如需詳細資訊，請參閱下列資源：

* <xref:web-api/advanced/analyzers>
* <xref:web-api/advanced/conventions>
* [ASP.NET核心 2.2.0 預覽1:openAPI 分析儀&約定](https://blogs.msdn.microsoft.com/webdev/2018/08/23/asp-net-core-2-20-preview1-open-api-analyzers-conventions/)

## <a name="problem-details-support"></a>問題詳細資料支援

ASP.NET核心 2.1`ProblemDetails`引入了 ,基於[RFC 7807](https://tools.ietf.org/html/rfc7807)規範,用於攜帶帶有 HTTP 回應的錯誤詳細資訊。 在 2.2 中，`ProblemDetails` 是對具有 `ApiControllerAttribute` 屬性之控制器中用戶端錯誤碼的標準回應。 傳回用戶端錯誤狀態碼 (4xx) 的 `IActionResult` 現在會傳回 `ProblemDetails` 主體。 結果中也會包含相互關聯識別碼，可用來透過要求記錄檔與錯誤相互關聯。 對於用戶端錯誤，`ProducesResponseType` 預設會使用 `ProblemDetails` 作為回應類型。 這會記載於使用 NSwag 或 Swashbuckle.AspNetCore 產生的 OpenAPI/Swagger 輸出中。

## <a name="endpoint-routing"></a>端點路由

ASP.NET Core 2.2 使用新的「端點路由」** 系統來改善要求的分派。 其變更包括新的連結產生 API 成員和路由參數轉換器。

如需詳細資訊，請參閱下列資源：

* [Endpoint routing in 2.2](https://blogs.msdn.microsoft.com/webdev/2018/08/27/asp-net-core-2-2-0-preview1-endpoint-routing/) (2.2 中的端點路由)
* [路由參數轉換器](https://www.hanselman.com/blog/ASPNETCore22ParameterTransformersForCleanURLGenerationAndSlugsInRazorPagesOrMVC.aspx) (請參閱＜Routing＞(路由)**** 一節)
* [IRouter 路由與端點路由之間的差異](xref:fundamentals/routing?view=aspnetcore-2.2#differences-from-earlier-versions-of-routing)

## <a name="health-checks"></a>健康情況檢查

新的健康狀態檢查服務可讓您更輕鬆地在需要健康狀態檢查的環境中 (例如 Kubernetes) 使用 ASP.NET Core。 健康狀態檢查包括中介軟體，以及定義 `IHealthCheck` 抽象概念和服務的一組程式庫。

容器協調器或負載平衡器使用健康狀態檢查，來快速判斷系統是否正常回應要求。 容器協調器可能會暫停輪流部署或重新啟動容器，來回應失敗的健康狀態檢查。 負載平衡器可能會從服務的失敗執行個體路由出流量，來回應健康狀態檢查。

應用程式會將健康狀態檢查公開為監控系統所使用的 HTTP 端點。 您可以針對各種即時監控案例和監控系統來設定健康狀態檢查。 健康狀態檢查服務與 [BeatPulse 專案](https://github.com/Xabaril/BeatPulse)整合。 這可讓您更輕鬆地新增數十個熱門系統和相依性的檢查。

如需詳細資訊，請參閱 [ASP.NET Core 中的健康狀態檢查](xref:host-and-deploy/health-checks)。

## <a name="http2-in-kestrel"></a>Kestrel 中的 HTTP/2

ASP.NET Core 2.2 新增 HTTP/2 支援。

HTTP/2 是 HTTP 通訊協定的主要版本。 HTTP/2 的顯著功能包括:

* 支持標頭壓縮。
* 通過單個連接進行完全多路複用的流。

雖然 HTTP/2 保留 HTTP 的語義(例如 HTTP 標頭和方法),但它是 HTTP/1.x 對用戶端和伺服器之間數據框架和發送方式的一個重大更改。

這項框架處理變更導致伺服器和用戶端必須交涉所使用的通訊協定版本。 Application-Layer Protocol Negotiation (ALPN) 是 TLS 延伸模組，可讓伺服器和用戶端交涉在其 TLS 信號交換過程中所使用的通訊協定版本。 雖然您可能事先知道伺服器與用戶端之間的通訊協定，但所有主要瀏覽器都支援 ALPN 作為建立 HTTP/2 連線的唯一方式。

如需詳細資訊，請參閱 [HTTP/2 支援](xref:fundamentals/servers/index?view=aspnetcore-2.2#http2-support)。

## <a name="kestrel-configuration"></a>Kestrel 組態

在舊版的 ASP.NET Core 中，Kestrel 選項是透過呼叫 `UseKestrel` 設定。 在 2.2 中，Kestrel 選項是透過在主機產生器上呼叫 `ConfigureKestrel` 設定。 這項變更解決了同處理序裝載的 `IServer` 註冊順序問題。 如需詳細資訊，請參閱下列資源：

* [Mitigate UseIIS conflict](https://github.com/aspnet/KestrelHttpServer/issues/2760) (降低 UseIIS 衝突)
* [使用 ConfigureKestrel 設定 Kestrel 伺服器選項](xref:fundamentals/servers/kestrel?view=aspnetcore-2.2#how-to-use-kestrel-in-aspnet-core-apps)

## <a name="iis-in-process-hosting"></a>IIS 同處理序裝載

在舊版的 ASP.NET Core 中，IIS 是作為反向 Proxy。 在 2.2 中，ASP.NET Core 模組可啟動 CoreCLR，並在 IIS 背景工作處理序 (*w3wp.exe*) 內裝載應用程式。 同處理序裝載以 IIS 執行時可改善效能和診斷。

如需詳細資訊，請參閱 [IIS 的同處理序裝載](xref:host-and-deploy/aspnet-core-module?view=aspnetcore-2.2#in-process-hosting-model)。

## <a name="opno-locsignalr-java-client"></a>SignalRJava 用戶端

ASP.NET核心 2.2 引入SignalR了 的 Java 用戶端。 此用戶端支援從 JAVASignalR代碼 (包括 Android 應用)連接到 ASP.NET 核心伺服器。

有關詳細資訊,請參閱[ASP.NETSignalR核心 Java 用戶端](https://docs.microsoft.com/aspnet/core/signalr/java-client?view=aspnetcore-2.2)。

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

MVC 的驗證系統設計為可擴展且靈活,允許您根據請求確定哪些驗證器適用於給定模型。 這有助於撰寫複雜的驗證提供者。 但是,在最常見的情況下,應用程式僅使用內置驗證器,並且不需要這種額外的靈活性。 內建驗證程式包括 DataAnnotations (例如 [Required] 和 [StringLength]) 以及 `IValidatableObject`。

在 ASP.NET Core 2.2 中，若 MVC 判斷指定的模型圖形不需要驗證，則會跳過驗證。 當驗證無法驗證或沒有任何驗證程式的模型時，跳過驗證可大幅改善效能。 這包括諸如基本類型集合 (例如 `byte[]`、`string[]`、`Dictionary<string, string>`) 的物件，或沒有許多驗證程式的複雜物件圖形。

## <a name="http-client-performance"></a>HTTP 用戶端效能

在 ASP.NET Core 2.2 中，由於降低連接集區鎖定競爭情況，因此改善了 `SocketsHttpHandler` 的效能。 針對進行許多傳出 HTTP 要求的應用程式 (例如某些微服務架構)，輸送量已獲得改善。 在負載情況下，`HttpClient` 輸送量在 Linux 上可提升高達 60%，在 Windows 上可提升高達 20%。

如需詳細資訊，請參閱[已做出這項改善的提取要求](https://github.com/dotnet/corefx/pull/32568)。

## <a name="additional-information"></a>其他資訊

如需完整的變更清單，請參閱 [ASP.NET Core 2.2 版本資訊](https://github.com/dotnet/aspnetcore/releases/tag/2.2.0)。
