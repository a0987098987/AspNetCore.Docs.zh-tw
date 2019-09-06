---
title: ASP.NET Core 中的應用程式啟動
author: rick-anderson
description: 了解 ASP.NET Core 中的 Startup 類別如何設定服務和應用程式的要求管線。
ms.author: riande
ms.custom: mvc
ms.date: 8/7/2019
uid: fundamentals/startup
ms.openlocfilehash: 9407de4ee91ba43b2c95fa98f0cf479bf8539cab
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310502"
---
# <a name="app-startup-in-aspnet-core"></a>ASP.NET Core 中的應用程式啟動

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Tom Dykstra](https://github.com/tdykstra)、[Luke Latham](https://github.com/guardrex) 以及 [Steve Smith](https://ardalis.com)

`Startup` 類別可設定服務和應用程式的要求管線。

## <a name="the-startup-class"></a>Startup 類別

ASP.NET Core 應用程式使用 `Startup` 類別，其依慣例命名為 `Startup`。 `Startup` 類別：

* 選擇性地包含 <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> 方法來設定應用程式的服務。 服務的定義是可提供應用程式功能的可重複使用元件。 服務會在 `ConfigureServices`中經過設定&mdash;也會受描述為「已註冊」&mdash;且會透過 [ 相依性插入 (DI)](xref:fundamentals/dependency-injection) 或 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*> 在應用程式間受取用。
* 包含 <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> 方法來建立應用程式的要求處理管線。

應用程式啟動時，ASP.NET Core 執行階段會呼叫 `ConfigureServices` 與 `Configure`：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

上述範例適用於 [Razor Pages](xref:razor-pages/index)與 MVC 版本類似。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup1.cs)]

::: moniker-end

一旦建置應用程式[主機](xref:fundamentals/index#host)，就會指定 `Startup` 類別。 `Startup` 類別通常會藉由呼叫主機產生器上的 [`WebHostBuilderExtensions.UseStartup<TStartup>`](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) 方法來指定：

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=12)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/Program3.cs?name=snippet_Program&highlight=12)]

主機會提供可供 `Startup` 類別建構函式使用的服務。 應用程式會透過 `ConfigureServices` 新增其他服務。 主機和應用程式服務都可以在 `Configure` 和整個應用程式中使用。

使用 <xref:Microsoft.Extensions.Hosting.IHostBuilder> 時，只能將下列服務型別插入 `Startup` 建構函式：

* `IWebHostEnvironment`
* `IHostEnvironment`
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartUp2.cs?name=snippet)]

在呼叫 `Configure` 方法之前，大部分的服務都無法使用。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

主機會提供可供 `Startup` 類別建構函式使用的服務。 應用程式會透過 `ConfigureServices` 新增其他服務。 然後，主機和應用程式服務都可以在 `Configure` 和整個應用程式中使用。

將[相依性插入](xref:fundamentals/dependency-injection)至 `Startup` 類別的常見用法是插入：

* <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> 來根據環境設定服務。
* <xref:Microsoft.Extensions.Configuration.IConfiguration> 來讀取組態。
* <xref:Microsoft.Extensions.Logging.ILoggerFactory> 來在 `Startup.ConfigureServices` 中建立記錄器。

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

在呼叫 `Configure` 方法之前，大部分的服務都無法使用。

::: moniker-end

### <a name="multiple-startup"></a>多次啟動

應用程式針對不同的環境定義個別的 `Startup` 類別 (例如 `StartupDevelopment`) 時，會在執行階段選取適當的 `Startup` 類別。 將優先使用其名稱尾碼符合目前環境的類別。 如果應用程式是在開發環境中執行，且同時包含 `Startup` 類別和 `StartupDevelopment` 類別，則會使用 `StartupDevelopment` 類別。 如需詳細資訊，請參閱[使用多重環境](xref:fundamentals/environments#environment-based-startup-class-and-methods)。

如需主機的詳細資訊，請參閱[主機](xref:fundamentals/index#host)。 如需在啟動期間處理錯誤的資訊，請參閱[啟動例外狀況處理](xref:fundamentals/error-handling#startup-exception-handling)。

## <a name="the-configureservices-method"></a>ConfigureServices 方法

<xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> 方法為：

* 選擇性。
* 由主機在 `Configure` 方法之前呼叫，來設定應用程式的服務。
* [組態選項](xref:fundamentals/configuration/index)依慣例設定的位置。

主機可能會在呼叫 `Startup` 方法之前，設定一些服務。 如需詳細資訊，請參閱[主機](xref:fundamentals/index#host)。

對於需要大量安裝的功能，可從 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 上取得 `Add{Service}` 擴充方法。 例如 **Add**DbContext、**Add**DefaultIdentity、**Add**EntityFrameworkStores 和 **Add**RazorPages：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartupIdentity.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup3.cs)]

::: moniker-end

將服務新增至服務容器，使其可在應用程式和 `Configure` 方法內使用。 服務可透過[相依性插入](xref:fundamentals/dependency-injection)或從 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*> 獲得解析。

::: moniker range="< aspnetcore-3.0"

請參閱 [SetCompatibilityVersion](xref:mvc/compatibility-version) 以取得 `SetCompatibilityVersion` 的詳細資訊。

::: moniker-end

## <a name="the-configure-method"></a>Configure 方法

<xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> 方法可用來指定應用程式對 HTTP 要求的回應方式。 藉由將[中介軟體](xref:fundamentals/middleware/index)元件新增至 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 執行個體，即可設定要求管線。 `IApplicationBuilder` 可用於 `Configure` 方法，但它未註冊在服務容器中。 裝載會建立 `IApplicationBuilder`，並將它直接傳遞到 `Configure`。

[ASP.NET Core 範本](/dotnet/core/tools/dotnet-new)會以下列支援設定管線：

* [開發人員例外狀況頁面](xref:fundamentals/error-handling#developer-exception-page)
* [例外處理常式](xref:fundamentals/error-handling#exception-handler-page)
* [HTTP 嚴格的傳輸安全性 (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [HTTPS 重新導向](xref:security/enforcing-ssl)
* [靜態檔案](xref:fundamentals/static-files)
* ASP.NET Core [MVC](xref:mvc/overview) 及 [Razor Pages](xref:razor-pages/index)

::: moniker range="< aspnetcore-3.0"

* [一般資料保護規定 (GDPR)](xref:security/gdpr)

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

上述範例適用於 [Razor Pages](xref:razor-pages/index)與 MVC 版本類似。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

::: moniker-end

各 `Use` 擴充方法會將一或多個中介軟體元件新增到要求管線。 例如，<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> 會設定[中介軟體](xref:fundamentals/middleware/index)來提供[靜態檔案](xref:fundamentals/static-files)。

要求管線中的每個中介軟體元件負責叫用管線中的下一個元件，或於適當時，對鏈結執行最少運算。

::: moniker range=">= aspnetcore-3.0"

`IWebHostEnvironment`、`ILoggerFactory` 或任何在 `ConfigureServices` 中定義的其他服務，也可以在 `Configure` 方法簽章中指定。 這些服務在可用時即會插入。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

`IHostingEnvironment` 和 `ILoggerFactory` 等其他服務，或任何在 `ConfigureServices` 中定義的項目，也可以在 `Configure` 方法簽章中指定。 這些服務在可用時即會插入。

::: moniker-end

如需 `IApplicationBuilder` 的使用方式及中介軟體處理順序的詳細資訊，請參閱 <xref:fundamentals/middleware/index>。

<a name="convenience-methods"></a>

## <a name="configure-services-without-startup"></a>在不啟動的情況下設定服務

若要設定服務及要求處理管線，且不使用 `Startup` 類別，請在主機建立器上呼叫 `ConfigureServices` 及 `Configure` 便利方法。 多次呼叫 `ConfigureServices` 會彼此附加。 若多個 `Configure` 方法呼叫存在，則會使用最後的 `Configure` 呼叫。

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program1.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=16,20)]

::: moniker-end

## <a name="extend-startup-with-startup-filters"></a>使用啟動篩選條件來擴充啟動

使用 <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> 在應用程式的 [Configure](#the-configure-method) 中介軟體管線的開頭或結尾設定中介軟體。 `IStartupFilter` 用來建立 `Configure` 方法的管線。 [IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) 可以將中介軟體設為在程式庫新增中介軟體之前或之後執行。

`IStartupFilter` 會實作 <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>，其會接收並傳回 `Action<IApplicationBuilder>`。 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 會定義類別，以設定應用程式的要求管線。 如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)。

每個 `IStartupFilter` 都可在要求管線中新增一或多個中介軟體。 篩選條件將依照它們新增至服務容器的順序叫用。 篩選條件可能會在控制權傳給下一個篩選條件之前或之後新增中介軟體，因此它們會附加至應用程式管線的開頭或結尾。

以下範例示範如何使用 `IStartupFilter` 註冊中介軟體。 此 `RequestSetOptionsMiddleware` 中介軟體會從查詢字串參數設定選項值：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` 設定在 `RequestSetOptionsStartupFilter` 類別中：

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

`RequestSetOptionsMiddleware` 設定在 `RequestSetOptionsStartupFilter` 類別中：

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

`IStartupFilter` 註冊在 <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> 的服務容器中。

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program.cs?name=snippet&highlight=19-20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

::: moniker-end

提供 `option` 的查詢字串參數時，中介軟體會在 ASP.NET Core 中介軟體轉譯回應之前，先處理值指派。

中介軟體執行順序是依照 `IStartupFilter` 註冊順序來設定：

* 多個 `IStartupFilter` 實作可能會與相同的物件互動。 如果順序很重要，請排列其 `IStartupFilter` 服務註冊順序，以符合其中介軟體執行應該依照的順序。
* 程式庫可能使用一或多個 `IStartupFilter` 實作 (其在使用 `IStartupFilter` 註冊的其他應用程式中介軟體之前或之後執行) 來新增中介軟體。 在由程式庫的 `IStartupFilter` 所新增中介軟體之前叫用 `IStartupFilter` 中介軟體：

  * 先放置服務註冊，再將程式庫新增至服務容器。
  * 若要在之後叫用，請在新增程式庫之後放置服務註冊。

## <a name="add-configuration-at-startup-from-an-external-assembly"></a>在啟動時從外部組件新增組態

<xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。 如需詳細資訊，請參閱 <xref:fundamentals/configuration/platform-specific-configuration>。

## <a name="additional-resources"></a>其他資源

* [主機](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
