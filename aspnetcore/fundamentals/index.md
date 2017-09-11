---
title: "ASP.NET Core 基本概念"
author: rick-anderson
description: "本文提供了建置 ASP.NET Core 應用程式時需了解之基本概念的高階概觀。"
keywords: "ASP.NET Core, 基本概念, 概觀"
ms.author: riande
manager: wpickett
ms.date: 08/18/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 99fbe0e02be27a0fbbb7ff65bc15713aab58c003
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/25/2017
---
# <a name="aspnet-core-fundamentals-overview"></a>ASP.NET Core 基本概念的概觀

ASP.NET Core 應用程式是一種主控台應用程式，可使用其 `Main` 方法建立網頁伺服器：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

`Main` 方法會叫用 `WebHost.CreateDefaultBuilder`，這會遵循產生器模式來建立 Web 應用程式主機。 產生器具有定義網頁伺服器 (例如，`UseKestrel`) 和啟動類別 (`UseStartup`) 的方法。 在上述範例中，會自動配置 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器。 ASP.NET Core 的 Web 主機將嘗試在 IIS 上執行 (如果有的話)。 其他網頁伺服器 (例如 [HTTP.sys](xref:fundamentals/servers/httpsys)) 則可透過叫用適當的擴充方法來使用。 `UseStartup` 將於下一節進一步說明。

`IWebHostBuilder` 是 `WebHost.CreateDefaultBuilder` 叫用的傳回型別，提供了許多選擇性方法。 其中某些方法包括用來在 HTTP.sys 中裝載應用程式的 `UseHttpSys`，以及用於指定內容根目錄的 `UseContentRoot`。 `Build` 和 `Run` 方法則會建置 `IWebHost` 物件，以裝載應用程式並開始接聽 HTTP 要求。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

`Main` 方法會使用 `WebHostBuilder`，這會遵循產生器模式來建立 Web 應用程式主機。 產生器具有定義網頁伺服器 (例如，`UseKestrel`) 和啟動類別 (`UseStartup`) 的方法。 在上述範例中，會使用 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器。 其他網頁伺服器 (例如 [WebListener](xref:fundamentals/servers/weblistener)) 則可透過叫用適當的擴充方法來使用。 `UseStartup` 將於下一節進一步說明。

`WebHostBuilder` 提供許多選擇性方法，包括用來在 IIS 和 IIS Express 中進行裝載的 `UseIISIntegration`，以及用於指定內容根目錄的 `UseContentRoot`。 `Build` 和 `Run` 方法則會建置 `IWebHost` 物件，以裝載應用程式並開始接聽 HTTP 要求。

---

## <a name="startup"></a>啟動

`WebHostBuilder` 上的 `UseStartup` 方法可為應用程式指定 `Startup` 類別：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

`Startup` 類別是您用來定義要求處理管線和設定應用程式所需之任何服務的位置。 `Startup` 必須是公用類別，而且包含下列方法：

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

* `ConfigureServices` 定義應用程式所使用的[服務](#services) (例如 ASP.NET Core MVC、Entity Framework Core、Identity 等)。

* `Configure` 定義要求管線中的[中介軟體](xref:fundamentals/middleware)。

如需詳細資訊，請參閱[應用程式啟動](xref:fundamentals/startup)。

## <a name="services"></a>服務

服務是一種可在應用程式中共用使用的元件。 服務是透過[相依性插入](xref:fundamentals/dependency-injection) (DI) 提供。 ASP.NET Core 包含原生控制反轉 (IoC) 容器，預設支援[建構函式插入](xref:mvc/controllers/dependency-injection#constructor-injection)。 原生容器可取代為您所選擇的容器。 除了其鬆散結合的益處之外，DI 還能夠讓服務可以在整個應用程式中使用。 例如，可以在整個應用程式中使用[記錄](xref:fundamentals/logging)。

如需詳細資訊，請參閱[相依性插入](xref:fundamentals/dependency-injection)。

## <a name="middleware"></a>中介軟體

在 ASP.NET Core 中，您可以使用[中介軟體](xref:fundamentals/middleware)來撰寫要求管線。 ASP.NET Core 中介軟體會對 `HttpContext` 執行非同步邏輯，然後叫用序列中的下一個中介軟體或直接終止要求。 透過在 `Configure` 方法中叫用 `UseXYZ` 擴充方法，新增稱為 "XYZ" 的中介軟體元件。

ASP.NET Core 隨附一組豐富的內建中介軟體：

* [靜態檔案](xref:fundamentals/static-files)

* [路由傳送](xref:fundamentals/routing)

* [驗證](xref:security/authentication/index)

您可以搭配使用任何以 [OWIN](http://owin.org) 為基礎的中介軟體與 ASP.NET Core，也可以撰寫您自己的自訂中介軟體。

如需詳細資訊，請參閱[中介軟體](xref:fundamentals/middleware)和 [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin)。

## <a name="servers"></a>伺服器

裝載模型的 ASP.NET Core 不會直接接聽要求；相反地，它依賴 HTTP 伺服器實作將要求轉送至應用程式。 轉送的要求會包裝成一組您可以透過介面存取的功能物件。 應用程式會將此設定撰寫成 `HttpContext`。 ASP.NET Core 包含一個受管理的跨平台網頁伺服器，稱為 [Kestrel](xref:fundamentals/servers/kestrel)。 Kestrel 通常會在生產網頁伺服器 (例如 [IIS](https://iis.net) 或 [nginx](http://nginx.org)) 背後執行。

如需詳細資訊，請參閱[伺服器](xref:fundamentals/servers/index)和[裝載](xref:fundamentals/hosting)。

## <a name="content-root"></a>內容根目錄

內容根目錄是應用程式所使用的任何內容的基底路徑，例如檢視、[Razor 頁面](xref:mvc/razor-pages/index)，以及靜態資產。 根據預設，內容根目錄與裝載應用程式之可執行檔的應用程式基底路徑相同。 內容根目錄的其他位置則由 `WebHostBuilder` 指定。

## <a name="web-root"></a>Web 根目錄

應用程式的 Web 根目錄是專案中包含公用、靜態資源 (例如 CSS、JavaScript 和影像檔) 的目錄。 根據預設，靜態檔案中介軟體只會處理來自 Web 根目錄及其子目錄的檔案。 如需詳細資訊，請參閱[使用靜態檔案](xref:fundamentals/static-files)。 Web 根目錄的路徑預設為 */wwwroot*，但是您可以使用 `WebHostBuilder` 指定不同的位置。

## <a name="configuration"></a>組態

ASP.NET Core 使用新的組態模型來處理簡單的名稱/值對。 新的組態模型不是根據 `System.Configuration` 或 *web.config*；相反地，它會從組態提供者的已排序集合中提取。 內建的組態提供者支援各種檔案格式 (XML、JSON、INI) 和環境變數，可啟用以環境為基礎的組態。 您也可以撰寫您自己的自訂組態提供者。

如需詳細資訊，請參閱[組態](xref:fundamentals/configuration)。

## <a name="environments"></a>環境

如「開發」和「生產」等環境是 ASP.NET Core 中的第一級概念，可以使用環境變數進行設定。

如需詳細資訊，請參閱[使用多個環境](xref:fundamentals/environments)。

## <a name="net-core-vs-net-framework-runtime"></a>.NET Core 與 .NET Framework 執行階段

ASP.NET Core 應用程式可將目標設為 .NET Core 或 .NET Framework 執行階段。 如需詳細資訊，[選擇 .NET Core 或 .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)。

## <a name="additional-information"></a>其他資訊

另請參閱下列主題：

- [錯誤處理](xref:fundamentals/error-handling)
- [檔案提供者](xref:fundamentals/file-providers)
- [全球化和當地語系化](xref:fundamentals/localization)
- [記錄](xref:fundamentals/logging)
- [管理應用程式狀態](xref:fundamentals/app-state)