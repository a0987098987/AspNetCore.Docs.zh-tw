---
title: ASP.NET Core 基本概念
author: rick-anderson
description: 探索 ASP.NET Core 應用程式的基本建置概念。
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2018
uid: fundamentals/index
ms.openlocfilehash: 83dfb5707700da01c45bae3c0c00e67ca397d402
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325467"
---
# <a name="aspnet-core-fundamentals"></a>ASP.NET Core 基本概念

ASP.NET Core 應用程式是一種主控台應用程式，可使用其 `Main` 方法建立網頁伺服器：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

`Main` 方法會叫用 [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*)，這會遵循[建立器模式](https://wikipedia.org/wiki/Builder_pattern)來建立 Web 主機。 產生器具有定義網頁伺服器 (例如，<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) 和啟動類別 (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>) 的方法。 以前述範例而言，會自動配置 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器。 ASP.NET Core 的 Web 主機會嘗試在 IIS 上執行 (如果有的話)。 其他網頁伺服器 (例如 [HTTP.sys](xref:fundamentals/servers/httpsys)) 則可透過叫用適當的擴充方法來使用。 `UseStartup` 將於下一節進一步說明。

<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> 是 `WebHost.CreateDefaultBuilder` 叫用的傳回型別，提供了許多選擇性方法。 其中某些方法包括用來在 HTTP.sys 中裝載應用程式的 `UseHttpSys`，以及用於指定根內容目錄的 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*>。 <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> 與 <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> 方法則會建置 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 物件，裝載應用程式並開始接聽 HTTP 要求。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

`Main` 方法會使用 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>，這會遵循[建立器模式](https://wikipedia.org/wiki/Builder_pattern)來建立 Web 應用程式主機。 產生器具有定義網頁伺服器 (例如，<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) 和啟動類別 (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>) 的方法。 在上述範例中，會使用 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器。 其他網頁伺服器 (例如 [WebListener](xref:fundamentals/servers/weblistener)) 則可透過叫用適當的擴充方法來使用。 `UseStartup` 將於下一節進一步說明。

`WebHostBuilder` 提供許多選擇性方法，包括用來裝載於 IIS 和 IIS Express 中的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*>，以及用於指定根內容目錄的 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*>。 <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> 與 <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> 方法則會建置 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 物件，裝載應用程式並開始接聽 HTTP 要求。

::: moniker-end

## <a name="startup"></a>啟動

`WebHostBuilder` 上的 `UseStartup` 方法可為應用程式指定 `Startup` 類別：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

`Startup` 類別是您用來定義要求處理管線以及設定應用程式所需之所有服務的位置。 `Startup` 必須是公用類別，而且包含下列方法：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 會定義應用程式所使用的[服務](#dependency-injection-services) (例如 ASP.NET Core MVC、Entity Framework Core、Identity 等)。 <xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> 定義在要求管線中呼叫的[中介軟體](xref:fundamentals/middleware/index)。

如需詳細資訊，請參閱<xref:fundamentals/startup>。

## <a name="content-root"></a>內容根目錄

內容根目錄是應用程式所使用任何內容的基底路徑，例如 [Razor 頁面](xref:razor-pages/index)、MVC 檢視和靜態資產。 根據預設，內容根目錄與裝載應用程式之可執行檔的應用程式基底路徑相同。

## <a name="web-root"></a>Web 根目錄

應用程式的 Web 根目錄是專案中包含公用、靜態資源 (例如 CSS、JavaScript 和影像檔) 的目錄。

## <a name="dependency-injection-services"></a>相依性插入 (服務)

「服務」是一種在應用程式中常用的元件。 服務可透過[相依性插入 (DI)](xref:fundamentals/dependency-injection) 提供。 ASP.NET Core 包含原生逆轉控制 (IoC) 容器，其根據預設支援[建構函式插入](xref:mvc/controllers/dependency-injection#constructor-injection)。 您可視需要取代預設容器。 DI 除了具有[鬆散結合的優點](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation)之外，還能夠讓整個應用程式皆可使用服務，例如[記錄](xref:fundamentals/logging/index)。

如需詳細資訊，請參閱<xref:fundamentals/dependency-injection>。

## <a name="middleware"></a>中介軟體

在 ASP.NET Core 中，您可以使用[中介軟體](xref:fundamentals/middleware/index)來撰寫要求管線。 ASP.NET Core 中介軟體會在 `HttpContext` 上執行非同步作業，然後叫用管線中下一個中介軟體或終止要求。

依照慣例，將稱為 "XYZ" 之中介軟體元件新增至管線的方法是，在 `Configure` 方法中叫用 `UseXYZ` 擴充方法。

ASP.NET Core 包含一組豐富的內建中介軟體，您也可以自行撰寫自訂中介軟體。 ASP.NET Core 應用程式支援 [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin)，這可讓 Web 應用程式與網頁伺服器分離。

如需詳細資訊，請參閱 <xref:fundamentals/middleware/index> 與 <xref:fundamentals/owin>。

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a>初始化 HTTP 要求

<xref:System.Net.Http.IHttpClientFactory> 可用來存取 <xref:System.Net.Http.HttpClient> 執行個體以發出 HTTP 要求。

如需詳細資訊，請參閱<xref:fundamentals/http-requests>。

::: moniker-end

## <a name="environments"></a>環境

「開發」與「生產」等環境是 ASP.NET Core 中的第一級概念，並可使用環境變數、設定檔案和命令列引數加以設定。

如需詳細資訊，請參閱<xref:fundamentals/environments>。

## <a name="hosting"></a>裝載

ASP.NET Core 應用程式會設定並啟動*主機*，其負責啟動應用程式以及管理存留期。

如需詳細資訊，請參閱<xref:fundamentals/host/index>。

## <a name="servers"></a>伺服器

ASP.NET Core 裝載模型不會直接接聽要求。 裝載模型需透過 HTTP 伺服器實作，才可將要求轉寄至應用程式。 轉寄的要求會包裝成一組可透過介面來存取的功能物件。 ASP.NET Core 包含一個受管理的跨平台網頁伺服器，稱為 [Kestrel](xref:fundamentals/servers/kestrel)。 Kestrel 通常會在生產網頁伺服器 (例如反向 Proxy 組態中的 [IIS](https://www.iis.net/) 或 [Nginx](http://nginx.org)) 後面執行。 Kestrel 也可在 ASP.NET Core 2.0 或更新版本中作為直接向網際網路公開的公眾 Edge Server 執行。

如需詳細資訊，請參閱<xref:fundamentals/servers/index>。

## <a name="configuration"></a>Configuration

ASP.NET Core 會使用以成對的名稱/值為基礎的組態模型。 而非以 <xref:System.Configuration> 或 *web.config* 為基礎的組態模型。組態會從組態提供者經排序的集合中取得設定。 內建的組態提供者支援各種檔案格式 (XML、JSON、INI)、環境變數和命令列引數。 您也可以撰寫您自己的自訂組態提供者。

如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。

## <a name="logging"></a>記錄

ASP.NET Core 支援可搭配各種記錄提供者的記錄 API。 內建提供者支援將記錄檔傳送至一或多個目的地。 可以使用協力廠商記錄架構。

如需詳細資訊，請參閱<xref:fundamentals/logging/index>。

## <a name="error-handling"></a>錯誤處理

ASP.NET Core 具有內建案例，可處理應用程式中的錯誤，包括開發人員例外狀況頁面、自訂錯誤頁面、靜態狀態碼頁面，以及啟動例外狀況處理。

如需詳細資訊，請參閱<xref:fundamentals/error-handling>。

## <a name="routing"></a>路由

ASP.NET Core 提供用來將應用程式要求路由至路由處理常式的案例。

如需詳細資訊，請參閱<xref:fundamentals/routing>。

## <a name="file-providers"></a>檔案提供者

ASP.NET Core 透過使用檔案提供者，將檔案系統存取抽象化，而檔案提供者則提供通用介面，讓您可跨平台使用檔案。

如需詳細資訊，請參閱<xref:fundamentals/file-providers>。

## <a name="static-files"></a>靜態檔案

靜態檔案中介軟體負責提供靜態檔案，例如 HTML、CSS、影像和 JavaScript 檔案。

如需詳細資訊，請參閱<xref:fundamentals/static-files>。

## <a name="session-and-app-state"></a>工作階段和應用程式狀態

ASP.NET Core 提供數種方法，可在使用者瀏覽 Web 應用程式時保留工作階段與應用程式狀態。

如需詳細資訊，請參閱<xref:fundamentals/app-state>。

## <a name="globalization-and-localization"></a>全球化和當地語系化

使用 ASP.NET Core 建立多語系網站，讓更廣大的群眾得以使用您的網站。 ASP.NET Core 可提供服務與中介軟體，用以將內容當地語系化成不同的語言與文化特性。

如需詳細資訊，請參閱<xref:fundamentals/localization>。

## <a name="request-features"></a>要求功能

有關 HTTP 要求和回應的網頁伺服器實作詳細資料，定義於介面中。 伺服器實作與中介軟體會使用這些介面，建立及修改應用程式的裝載管線。

如需詳細資訊，請參閱<xref:fundamentals/request-features>。

## <a name="background-tasks"></a>背景工作

背景工作會實作為*託管服務*。 託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。

如需詳細資訊，請參閱<xref:fundamentals/host/hosted-services>。

## <a name="access-httpcontext"></a>存取 HttpContext

在使用 Razor 頁面和 MVC 處理要求時，會自動提供 `HttpContext`。 在 `HttpContext` 尚無法使用的情況下，您可以透過 <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> 介面及其預設實作 <xref:Microsoft.AspNetCore.Http.HttpContextAccessor> 存取 `HttpContext`。

如需詳細資訊，請參閱<xref:fundamentals/httpcontext>。

## <a name="websockets"></a>WebSockets

[WebSocket](https://wikipedia.org/wiki/WebSocket) 為通訊協定，其可在 TCP 連線下啟用雙向的持續性通訊通道。 其可用於像是聊天、股票行情、遊戲等應用程式，以及您希望在 Web 應用程式中使用即時功能的任何位置。 ASP.NET Core 支援 Web 通訊端案例。

如需詳細資訊，請參閱<xref:fundamentals/websockets>。

::: moniker range=">= aspnetcore-2.1"

## <a name="microsoftaspnetcoreapp-metapackage"></a>Microsoft.AspNetCore.App 中繼套件

[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) 中繼套件可簡化套件管理。

如需詳細資訊，請參閱<xref:fundamentals/metapackage-app>。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="microsoftaspnetcoreall-metapackage"></a>Microsoft.AspNetCore.All 中繼套件

ASP.NET Core 的 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) 中繼套件包括：

* 所有由 ASP.NET Core 小組支援的套件。
* Entity Framework Core 支援的所有套件。
* ASP.NET Core 與 Entity Framework Core 所使用的內部與第三人相依性。

如需詳細資訊，請參閱<xref:fundamentals/metapackage>。

::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a>.NET Core 與 .NET Framework 執行階段

ASP.NET Core 應用程式可將目標設為 .NET Core 或 .NET Framework 執行階段。

如需詳細資訊，請參閱[在 .NET Core 和 .NET Framework 之間進行選擇](/dotnet/articles/standard/choosing-core-framework-server)。

## <a name="choose-between-aspnet-core-and-aspnet"></a>在 ASP.NET Core 與 ASP.NET 之間選擇

如需在 ASP.NET Core 與 ASP.NET 之間選擇的詳細資訊，請參閱 <xref:fundamentals/choose-between-aspnet-and-aspnetcore>。
