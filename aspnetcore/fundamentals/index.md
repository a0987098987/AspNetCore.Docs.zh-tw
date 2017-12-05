---
title: "ASP.NET Core 基本概念"
author: rick-anderson
description: "探索用於建置 ASP.NET Core 應用程式的基本概念。"
keywords: "ASP.NET Core, 基本概念, 概觀"
ms.author: riande
manager: wpickett
ms.date: 09/30/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 83bed4676be3ca752442da3fe560f1f2a4d728a1
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2017
---
# <a name="aspnet-core-fundamentals"></a>ASP.NET Core 基本概念

ASP.NET Core 應用程式是一種主控台應用程式，可使用其 `Main` 方法建立網頁伺服器：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

`Main` 方法會叫用 `WebHost.CreateDefaultBuilder`，這會遵循產生器模式來建立 Web 應用程式主機。 產生器具有定義網頁伺服器 (例如，`UseKestrel`) 和啟動類別 (`UseStartup`) 的方法。 以前述範例而言，會自動配置 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器。 ASP.NET Core 的 Web 主機會嘗試在 IIS 上執行 (如果有的話)。 其他網頁伺服器 (例如 [HTTP.sys](xref:fundamentals/servers/httpsys)) 則可透過叫用適當的擴充方法來使用。 `UseStartup` 將於下一節進一步說明。

`IWebHostBuilder` 是 `WebHost.CreateDefaultBuilder` 叫用的傳回型別，提供了許多選擇性方法。 其中某些方法包括用來在 HTTP.sys 中裝載應用程式的 `UseHttpSys`，以及用於指定根內容目錄的 `UseContentRoot`。 `Build` 與 `Run` 方法則會建置 `IWebHost` 物件，裝載應用程式並開始接聽 HTTP 要求。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

`Main` 方法會使用 `WebHostBuilder`，這會遵循產生器模式來建立 Web 應用程式主機。 產生器具有定義網頁伺服器 (例如，`UseKestrel`) 和啟動類別 (`UseStartup`) 的方法。 在上述範例中，會使用 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器。 其他網頁伺服器 (例如 [WebListener](xref:fundamentals/servers/weblistener)) 則可透過叫用適當的擴充方法來使用。 `UseStartup` 將於下一節進一步說明。

`WebHostBuilder` 提供許多選擇性方法，包括用來裝載於 IIS 和 IIS Express 中的 `UseIISIntegration`，以及用於指定根內容目錄的 `UseContentRoot`。 `Build` 與 `Run` 方法則會建置 `IWebHost` 物件，裝載應用程式並開始接聽 HTTP 要求。

---

## <a name="startup"></a>啟動

`WebHostBuilder` 上的 `UseStartup` 方法可為應用程式指定 `Startup` 類別：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

`Startup` 類別是您用來定義要求處理管線以及設定應用程式所需之所有服務的位置。 `Startup` 必須是公用類別，而且包含下列方法：

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

`ConfigureServices` 會定義應用程式所使用的[服務](#dependency-injection-services) (例如 ASP.NET Core MVC、Entity Framework Core、Identity 等)。 `Configure` 則會定義要求管線的[中介軟體](xref:fundamentals/middleware)。

如需詳細資訊，請參閱[應用程式啟動](xref:fundamentals/startup)。

## <a name="content-root"></a>內容根目錄

內容根目錄是應用程式所使用的任何內容的基底路徑，例如檢視、[Razor 頁面](xref:mvc/razor-pages/index)，以及靜態資產。 根據預設，內容根目錄會與裝載應用程式之可執行檔的應用程式基底路徑相同。

## <a name="web-root"></a>Web 根目錄

應用程式的 Web 根目錄是專案中包含公用、靜態資源 (例如 CSS、JavaScript 和影像檔) 的目錄。

## <a name="dependency-injection-services"></a>相依性插入 (服務)

服務是一種在應用程式中常用的元件。 服務可透過[相依性插入 (DI)](xref:fundamentals/dependency-injection) 提供。 ASP.NET Core 包含原生逆轉控制 (IoC) 容器，根據預設，其會支援[建構函式插入](xref:mvc/controllers/dependency-injection#constructor-injection)。 您可視需要取代掉預設的原生容器。 DI 除了具有鬆散結合的優點之外，還能夠讓整個應用程式皆可使用服務。(例如：[記錄](xref:fundamentals/logging/index))。

如需詳細資訊，請參閱[相依性插入](xref:fundamentals/dependency-injection)。

## <a name="middleware"></a>中介軟體

在 ASP.NET Core 中，您可以使用[中介軟體](xref:fundamentals/middleware)來撰寫要求管線。 ASP.NET Core 中介軟體會對 `HttpContext` 執行非同步邏輯，然後叫用序列中的下一個中介軟體或直接終止要求。 透過在 `Configure` 方法中叫用 `UseXYZ` 擴充方法，新增稱為 "XYZ" 的中介軟體元件。

ASP.NET Core 隨附一組豐富的內建中介軟體：

* [靜態檔案](xref:fundamentals/static-files)
* [路由傳送](xref:fundamentals/routing)
* [驗證](xref:security/authentication/index)
* [回應壓縮中介軟體](xref:performance/response-compression)
* [URL 重寫中介軟體](xref:fundamentals/url-rewriting)

ASP.NET Core 應用程式可使用以 [OWIN](http://owin.org) 為基礎的中介軟體，您也可以自行撰寫自訂的中介軟體。

如需詳細資訊，請參閱[中介軟體](xref:fundamentals/middleware)和 [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin)。

## <a name="environments"></a>環境

如「開發」與「生產」等環境是 ASP.NET Core 中的第一級概念，可使用環境變數加以設定。

如需詳細資訊，請參閱[使用多個環境](xref:fundamentals/environments)。

## <a name="configuration"></a>組態

ASP.NET Core 會使用以成對的名稱/值為基礎的組態模型。 而非以 `System.Configuration` 或 *web.config* 為基礎的組態模型。組態會從組態提供者經排序的集合中取得設定。 內建的組態提供者支援各種檔案格式 (XML、JSON、INI) 和環境變數，可啟用以環境為基礎的組態。 您也可以撰寫您自己的自訂組態提供者。

如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。

## <a name="logging"></a>記錄

ASP.NET Core 支援可搭配各種記錄提供者的記錄 API。 內建提供者支援將記錄檔傳送至一或多個目的地。 可以使用協力廠商記錄架構。

[記錄](xref:fundamentals/logging/index)

## <a name="error-handling"></a>錯誤處理

ASP.NET CoreASP.NET Core 具有內建功能，可處理應用程式中的錯誤，包括開發人員例外狀況頁面、自訂錯誤頁面、靜態狀態字碼頁，以及啟動例外狀況處理。

如需詳細資訊，請參閱[錯誤處理](xref:fundamentals/error-handling)。

## <a name="routing"></a>路由

ASP.NET Core 提供用來將應用程式要求路由至路由處理常式的功能。

如需詳細資訊，請參閱[路由](xref:fundamentals/routing)。

## <a name="file-providers"></a>檔案提供者

ASP.NET Core 透過使用檔案提供者，將檔案系統存取抽象化，而檔案提供者則提供通用介面，讓您可跨平台使用檔案。

如需詳細資訊，請參閱[檔案提供者](xref:fundamentals/file-providers)。

## <a name="static-files"></a>靜態檔案

靜態檔案中介軟體負責提供靜態檔案，例如 HTML、CSS、影像和 JavaScript。

如需詳細資訊，請參閱[使用靜態檔案](xref:fundamentals/static-files)。

## <a name="hosting"></a>裝載

ASP.NET Core 應用程式會設定並啟動*主機*，其負責啟動應用程式以及管理存留期。

如需詳細資訊，請參閱[裝載](xref:fundamentals/hosting)。

## <a name="session-and-application-state"></a>工作階段與應用程式狀態

在 ASP.NET Core 中的工作階段狀態功能，可用於在使用者瀏覽您的 Web 應用程式時，儲存及存放使用者資料。

如需詳細資訊，請參閱[工作階段與應用程式狀態](xref:fundamentals/app-state)。

## <a name="servers"></a>伺服器

ASP.NET Core 裝載模型不會直接接聽要求。 裝載模型需透過 HTTP 伺服器實作，才可將要求轉寄至應用程式。 轉寄的要求會包裝成一組可透過介面來存取的功能物件。 ASP.NET Core 包含一個受管理的跨平台網頁伺服器，稱為 [Kestrel](xref:fundamentals/servers/kestrel)。 Kestrel 通常會在生產網頁伺服器 (例如 [IIS](https://www.iis.net/) 或 [nginx](http://nginx.org)) 背後執行。 Kestrel 可執行為 Edge Server。

如需詳細資訊，請參閱[伺服器](xref:fundamentals/servers/index)及下列主題：

* [Kestrel](xref:fundamentals/servers/kestrel)
* [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)
* [HTTP.sys](xref:fundamentals/servers/httpsys) (先前稱為 [WebListener](xref:fundamentals/servers/weblistener))

## <a name="globalization-and-localization"></a>全球化和當地語系化

使用 ASP.NET Core 建立多語系網站，讓更廣大的群眾得以使用您的網站。 ASP.NET Core 提供服務與中介軟體，可將網站當地語系化成不同的語言與文化特性。

如需詳細資訊，請參閱[全球化與當地語系化](xref:fundamentals/localization)。

## <a name="request-features"></a>要求功能

有關 HTTP 要求和回應的網頁伺服器實作詳細資料，定義於介面中。 伺服器實作與中介軟體會使用這些介面，建立及修改應用程式的裝載管線。

如需詳細資訊，請參閱[要求功能](xref:fundamentals/request-features)。

## <a name="open-web-interface-for-net-owin"></a>Open Web Interface for .NET (OWIN)

ASP.NET Core 支援Open Web Interface for .NET (OWIN)。 OWIN 可讓 Web 應用程式獨立於網頁伺服器。

如需詳細資訊，請參閱 [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin)。

## <a name="websockets"></a>WebSockets

[WebSocket](https://wikipedia.org/wiki/WebSocket) 為通訊協定，其可在 TCP 連線下啟用雙向的持續性通訊通道。 其可用於像是聊天、股票行情、遊戲等應用程式，以及您希望在 Web 應用程式中使用即時功能的任何位置。 ASP.NET Core 支援網路通訊端功能。

如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。

## <a name="microsoftaspnetcoreall-metapackage"></a>Microsoft.AspNetCore.All 中繼套件

ASP.NET Core 的 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) 中繼套件包括：

* 所有由 ASP.NET Core 小組支援的套件。
* 所有由 Entity Framework Core 支援的套件。 
* ASP.NET Core 與 Entity Framework Core 所使用的內部與第三人相依性。

如需詳細資訊，請參閱 [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)。

## <a name="net-core-vs-net-framework-runtime"></a>.NET Core 與 .NET Framework 執行階段

ASP.NET Core 應用程式可將目標設為 .NET Core 或 .NET Framework 執行階段。

如需詳細資訊，請參閱[在 .NET Core 和 .NET Framework 之間進行選擇](/dotnet/articles/standard/choosing-core-framework-server)。

## <a name="choose-between-aspnet-core-and-aspnet"></a>在 ASP.NET Core 與 ASP.NET 之間選擇

如需在 ASP.NET Core 與 ASP.NET 之間選擇的詳細資訊，請參閱[在 ASP.NET Core 與 ASP.NET 之間選擇](xref:fundamentals/choose-between-aspnet-and-aspnetcore)。
