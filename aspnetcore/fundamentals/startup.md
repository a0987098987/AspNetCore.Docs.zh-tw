---
title: ASP.NET Core 中的應用程式啟動
author: ardalis
description: 探索 Startup 類別如何在 ASP.NET Core 中設定服務和應用程式的要求管線。
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 4/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/startup
ms.openlocfilehash: 58ced0ae11f462bc309526b7db7bda7897c33009
ms.sourcegitcommit: 40b102ecf88e53d9d872603ce6f3f7044bca95ce
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/18/2018
ms.locfileid: "34233166"
---
# <a name="application-startup-in-aspnet-core"></a>ASP.NET Core 中的應用程式啟動

作者：[Steve Smith](https://ardalis.com)、[Tom Dykstra](https://github.com/tdykstra) 和 [Luke Latham](https://github.com/guardrex)

`Startup` 類別可設定服務和應用程式的要求管線。

## <a name="the-startup-class"></a>Startup 類別

ASP.NET Core 應用程式使用 `Startup` 類別，其依慣例命名為 `Startup`。 `Startup` 類別：

* 可以選擇性地包含 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) 方法來設定應用程式的服務。
* 必須包含 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) 方法來建立應用程式的要求處理管線。

應用程式啟動時，執行階段就會呼叫 `ConfigureServices` 和 `Configure`：

[!code-csharp[](startup/snapshot_sample/Startup1.cs)]

使用 [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) 方法來指定 `Startup` 類別：

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

`Startup` 類別建構函式接受主機所定義的相依性。 將[相依性插入](xref:fundamentals/dependency-injection)至 `Startup` 類別的常見用法是插入：

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment)，用來依環境設定服務。
* [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration)，用來在啟動期間設定應用程式。

[!code-csharp[](startup/snapshot_sample/Startup2.cs)]

插入 `IHostingEnvironment` 的替代方式是使用基於慣例的方法。 應用程式可以針對不同的環境定義個別的 `Startup` 類別 (例如，`StartupDevelopment`)，並在執行階段選取適當的啟動類別。 將優先使用其名稱尾碼符合目前環境的類別。 如果應用程式是在開發環境中執行，且同時包含 `Startup` 類別和 `StartupDevelopment` 類別，則會使用 `StartupDevelopment` 類別。 如需詳細資訊，請參閱[使用多重環境](xref:fundamentals/environments#startup-conventions)。

若要深入了解 `WebHostBuilder`，請參閱[裝載](xref:fundamentals/host/index)主題。 如需在啟動期間處理錯誤的資訊，請參閱[啟動例外狀況處理](xref:fundamentals/error-handling#startup-exception-handling)。

## <a name="the-configureservices-method"></a>ConfigureServices 方法

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) 方法是：

* Optional
* 由 Web 主機在 `Configure` 方法之前呼叫，用來設定應用程式的服務。
* [組態選項](xref:fundamentals/configuration/index)依慣例設定的位置。

將服務新增至服務容器，使其可在應用程式和 `Configure` 方法內使用。 這些服務會透過[相依性插入](xref:fundamentals/dependency-injection)或從 [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) 加以解析。

Web 主機可能會在呼叫 `Startup` 方法之前設定一些服務。 詳細資料可於[在 ASP.NET Core 中代管](xref:fundamentals/host/index)主題中取得。

對於需要大量安裝的功能，[IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection) 上有 `Add[Service]` 擴充方法。 適用於 Entity Framework、身分識別和 MVC 的典型 Web 應用程式註冊服務：

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

::: moniker range=">= aspnetcore-2.1"

<a name="setcompatibilityversion"></a>

### <a name="setcompatibilityversion-for-aspnet-core-mvc"></a>適用於 ASP.NET Core MVC 的 SetCompatibilityVersion 

`SetCompatibilityVersion` 方法可讓應用程式選擇加入或退出 ASP.NET MVC Core 2.1 以上版本所引入的可能重大行為變更。 這些可能的重大行為變更通常在於 MVC 子系統的運作方式，以及執行階段呼叫**您的程式碼**的方式。 透過選擇加入，您可以取得最新的行為和 ASP.NET Core 的長期行為。

下列程式碼會將相容性模式設定為 ASP.NET Core 2.1：

[!code-csharp[Main](startup/sampleCompatibility/Startup.cs?name=snippet1)]

建議您使用最新版本 (`CompatibilityVersion.Version_2_1`) 測試應用程式。 預計大部分的應用程式都不會使用最新版本進行重大行為變更。 

呼叫 `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` 的應用程式會受到保護，防止執行 ASP.NET Core 2.1 MVC 和更新版本的 2.x 版所引入的可能重大行為變更。 這項保護：

* 不適用於所有 2.1 和更新版本的變更，它的目標是 MVC 子系統中的可能重大 ASP.NET Core 執行階段行為變更。
* 不會擴充到下一個主要版本。

適用於 ASP.NET Core 2.1 和更新版本的 2.x 應用程式且**不會**呼叫 `SetCompatibilityVersion` 的預設相容性為 2.0 相容性。 也就是說，不呼叫 `SetCompatibilityVersion` 等同於呼叫 `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`。

下列程式碼會將相容性模式設定為 ASP.NET Core 2.1，但下列行為除外：

* [AllowCombiningAuthorizeFilters](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)
* [InputFormatterExceptionPolicy](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)

[!code-csharp[Main](startup/sampleCompatibility/Startup2.cs?name=snippet1)]

對於出現重大行為變更的應用程式，使用適當的相容性參數：

* 可讓您使用最新版本，並選擇退出特定的重大行為變更。
* 讓您有時間來更新您的應用程式，使其適用於最新的變更。

[MvcOptions](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) 類別來源註解充分說明變更的項目，以及所做變更對於大部分使用者來說都得到改善的原因。

在未來某個日期將會推出 [ASP.NET Core 3.0 版](https://github.com/aspnet/Home/wiki/Roadmap)。 3.0 版將移除相容性參數支援的舊行為。 我們覺得這些是有利於幾乎所有使用者的正向變更。 現在引進這些變更，大部分的應用程式皆可獲益，而其他人則有時間來更新其應用程式。

::: moniker-end

## <a name="services-available-in-startup"></a>Startup 中可用的服務

Web 主機提供一些可用於 `Startup` 類別建構函式的服務。 應用程式會透過 `ConfigureServices` 新增其他服務。 然後，主機和應用程式服務都可以在 `Configure` 和整個應用程式中使用。

## <a name="the-configure-method"></a>Configure 方法

[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) 方法用來指定應用程式如何回應 HTTP 要求。 藉由將[中介軟體](xref:fundamentals/middleware/index)元件新增至 [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) 執行個體，即可設定要求管線。 `IApplicationBuilder` 可用於 `Configure` 方法，但它未註冊在服務容器中。 裝載會建立 `IApplicationBuilder`，並將它直接傳遞至 `Configure` ([參考來源](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192))。

[ASP.NET Core 範本](/dotnet/core/tools/dotnet-new)將管線設定為支援開發人員例外狀況頁面、[BrowserLink](http://vswebessentials.com/features/browserlink)、錯誤頁面、靜態檔案，以及 ASP.NET MVC：

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

每個 `Use` 擴充方法會將中介軟體元件新增至要求管線。 比方說，`UseMvc` 擴充方法會將[路由中介軟體](xref:fundamentals/routing)新增至要求管線，並將 [MVC](xref:mvc/overview) 設定為預設處理常式。

要求管線中的每個中介軟體元件負責叫用管線中的下一個元件，或於適當時，對鏈結執行最少運算。 如果沿著中介軟體鏈結不會進行最少運算，每個中介軟體在將要求傳送至用戶端之前，會有第二次機會來處理該要求。

`IHostingEnvironment` 和 `ILoggerFactory` 等其他服務也可以指定在方法簽章中。 如果已指定，其他服務在可用時即會插入。

如需 `IApplicationBuilder` 使用方式和中介軟體處理順序的詳細資訊，請參閱[中介軟體](xref:fundamentals/middleware/index)。

## <a name="convenience-methods"></a>便利的方法

您可以使用 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) 和 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) 這些便利的方法來取代指定 `Startup` 類別。 多次呼叫 `ConfigureServices` 會彼此附加。 多個呼叫 `Configure` 則使用最後一個方法呼叫。

[!code-csharp[](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="startup-filters"></a>啟動篩選條件

使用 [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) 在應用程式的 [Configure](#the-configure-method) 中介軟體管線的開頭或結尾設定中介軟體。 `IStartupFilter` 有助於確保某個中介軟體會在應用程式要求處理管線的開頭或結尾，於程式庫所新增的中介軟體之前或之後執行。

`IStartupFilter` 會實作 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure) 這個單一方法，以接收並傳回 `Action<IApplicationBuilder>`。 [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) 會定義類別來設定應用程式的要求管線。 如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)。

每個 `IStartupFilter` 會在要求管線中實作一或多個中介軟體。 篩選條件將依照它們新增至服務容器的順序叫用。 篩選條件可能會在控制權傳給下一個篩選條件之前或之後新增中介軟體，因此它們會附加至應用程式管線的開頭或結尾。

[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([如何下載](xref:tutorials/index#how-to-download-a-sample)) 示範如何使用 `IStartupFilter` 註冊中介軟體。 範例應用程式包含一個中介軟體，用來從查詢字串參數設定選項值：

[!code-csharp[](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` 設定在 `RequestSetOptionsStartupFilter` 類別中：

[!code-csharp[](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` 註冊在 `ConfigureServices` 的服務容器中：

[!code-csharp[](startup/sample/Startup.cs?name=snippet1&highlight=3)]

提供 `option` 的查詢字串參數時，中介軟體會在 MVC 中介軟體呈現回應之前，先處理值指派：

![顯示呈現的 Index 頁面的瀏覽器視窗。 根據要求頁面時選項的查詢字串參數和值設定為 'From Middleware'，選項的值會呈現為 'From Middleware'。](startup/_static/index.png)

中介軟體執行順序是依照 `IStartupFilter` 註冊順序來設定：

* 多個 `IStartupFilter` 實作可能會與相同的物件互動。 如果順序很重要，請排列其 `IStartupFilter` 服務註冊順序，以符合其中介軟體執行應該依照的順序。
* 程式庫可能使用一或多個 `IStartupFilter` 實作 (其在使用 `IStartupFilter` 註冊的其他應用程式中介軟體之前或之後執行) 來新增中介軟體。 若要在程式庫的 `IStartupFilter` 所新增的中介軟體之前叫用 `IStartupFilter` 中介軟體，請在程式庫新增至服務容器之前放置服務註冊。 若要在之後叫用它，請在新增程式庫之後放置服務註冊。

## <a name="adding-configuration-at-startup-from-an-external-assembly"></a>在啟動時從外部組件新增組態

實作 [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) 即可在啟動時，從應用程式非 `Startup` 類別的外部組件，對應用程式新增強功能。 如需詳細資訊，請參閱[從外部組件增強應用程式](xref:fundamentals/configuration/platform-specific-configuration)。

## <a name="additional-resources"></a>其他資源

* [裝載](xref:fundamentals/host/index)
* [使用多重環境](xref:fundamentals/environments)
* [中介軟體](xref:fundamentals/middleware/index)
* [記錄](xref:fundamentals/logging/index)
* [組態](xref:fundamentals/configuration/index)
* [StartupLoader 類別：FindStartupType 方法 (參考來源)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
