---
title: "在 ASP.NET Core 應用程式啟動"
author: ardalis
description: "探索啟動類別中 ASP.NET Core 如何設定服務和應用程式的要求管線。"
ms.author: tdykstra
manager: wpickett
ms.custom: mvc
ms.date: 12/08/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: 8adb96c7261a2e7b1556f0daddcf6f135862b53a
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2017
---
# <a name="application-startup-in-aspnet-core"></a>在 ASP.NET Core 應用程式啟動

由[Steve Smith](https://ardalis.com)， [Tom Dykstra](https://github.com/tdykstra)，和[Luke Latham](https://github.com/guardrex)

`Startup`類別來設定服務和應用程式的要求管線。

## <a name="the-startup-class"></a>啟動類別

ASP.NET Core 應用程式使用`Startup`類別，名為`Startup`依慣例。 `Startup`類別：

* 可以選擇性地包含[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)方法來設定應用程式的服務。
* 必須包含[設定](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)方法來建立應用程式的要求處理管線。

`ConfigureServices`和`Configure`應用程式啟動時，執行階段會呼叫：

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

指定`Startup`類別[WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)方法：

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

`Startup`類別建構函式接受主機所定義的相依性。 常見用法[相依性插入](xref:fundamentals/dependency-injection)到`Startup`類別是要插入[IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment)由環境中設定服務：

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

插入的替代方式`IHostingStartup`是使用以慣例為基礎的方法。 應用程式可以定義個別`Startup`不同環境的類別 (例如， `StartupDevelopment`)，並在執行階段選取適當的啟動類別。 類別的名稱尾碼符合目前的環境已設定優先權。 如果應用程式會在開發環境中執行，並同時包含`Startup`類別和`StartupDevelopment`類別`StartupDevelopment`類別使用。 如需詳細資訊，請參閱[使用多個環境](xref:fundamentals/environments#startup-conventions)。

若要深入了解`WebHostBuilder`，請參閱[主控](xref:fundamentals/hosting)主題。 如需在啟動期間處理的錯誤，請參閱[啟動例外狀況處理](xref:fundamentals/error-handling#startup-exception-handling)。

## <a name="the-configureservices-method"></a>ConfigureServices 方法

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)方法是：

* 選擇項。
* 由 web 主機之前呼叫`Configure`方法來設定應用程式的服務。
* 其中[組態選項](xref:fundamentals/configuration/index)慣例所設定。

將服務加入至服務容器可讓它們在應用程式和`Configure`方法。 服務是透過解析[相依性插入](xref:fundamentals/dependency-injection)或從[IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices)。

Web 主機可能會設定某些服務之前`Startup`呼叫的方法。 在詳細資料可用[主控](xref:fundamentals/hosting)主題。 

對於需要大量的安裝程式的功能，有`Add[Service]`的擴充方法[IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection)。 典型的 web 應用程式註冊 Entity Framework、 識別和 MVC 的服務：

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a>用於啟動服務

Web 主機提供某些服務，可用於`Startup`類別建構函式。 應用程式將透過其他服務`ConfigureServices`。 主機和應用程式中可用服務然後`Configure`和整個應用程式。

## <a name="the-configure-method"></a>Configure 方法

[設定](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)方法用來指定應用程式如何回應 HTTP 要求。 透過加入設定要求管線[中介軟體](xref:fundamentals/middleware)元件[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder)執行個體。 `IApplicationBuilder`若要使用`Configure`方法，但它未登錄在服務容器。 建立裝載`IApplicationBuilder`，並將它直接`Configure`([參考來源](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192))。

[ASP.NET Core 範本](/dotnet/core/tools/dotnet-new)設定管線，開發人員例外狀況 頁面上，支援[BrowserLink](http://vswebessentials.com/features/browserlink)，錯誤頁面、 靜態檔案，以及 ASP.NET MVC:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

每個`Use`擴充方法新增中介軟體元件，以要求管線。 比方說，`UseMvc`擴充方法新增[路由的中介軟體](xref:fundamentals/routing)要求管線，並設定[MVC](xref:mvc/overview)為預設處理常式。

其他服務，例如`IHostingEnvironment`和`ILoggerFactory`，也可以指定方法簽章中。 指定時，如果已經有會插入的其他服務。

如需有關如何使用`IApplicationBuilder`，請參閱[中介軟體](xref:fundamentals/middleware)。

## <a name="convenience-methods"></a>便利的方法

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices)和[設定](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure)使用便利的方法來取代指定`Startup`類別。 多個呼叫`ConfigureServices`附加至另一個。 多個呼叫`Configure`使用最後一個方法呼叫。

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=16,20)]

## <a name="startup-filters"></a>啟動篩選

使用[IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter)開頭或結尾的應用程式設定中介軟體[設定](#the-configure-method)中介軟體管線。 `IStartupFilter`適合用於確保中介軟體執行之前或之後新增的開頭或結尾的應用程式要求處理管線的程式庫的中介軟體。

`IStartupFilter`實作單一方法[設定](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure)，會接收並傳回`Action<IApplicationBuilder>`。 [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder)定義來設定應用程式的要求管線的類別。 如需詳細資訊，請參閱[建立中介軟體管線與 IApplicationBuilder](xref:fundamentals/middleware#creating-a-middleware-pipeline-with-iapplicationbuilder)。

每個`IStartupFilter`實作一或多個 middlewares 要求管線中。 篩選條件會加入至服務容器的順序叫用。 篩選可能增加中的介軟體之前或之後將控制權傳遞至下一個篩選，因此它們附加至的開頭或結尾的應用程式管線。

[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/)([如何下載](xref:tutorials/index#how-to-download-a-sample)) 示範如何註冊與中的介軟體`IStartupFilter`。 範例應用程式包含中介軟體，設定選項值從查詢字串參數：

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware`中設定`RequestSetOptionsStartupFilter`類別：

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter`註冊的服務容器中`ConfigureServices`:

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

當查詢字串參數為`option`提供中介軟體的 MVC 中介軟體呈現回應之前，先進行處理的值指派：

![顯示轉譯的索引頁的瀏覽器視窗。 選項的值會呈現 '位於 來自中介軟體' 根據要求查詢字串參數和值的選項設定為 '位於 來自中介軟體' 頁面。](startup/_static/index.png)

中介軟體執行順序依以下順序設定`IStartupFilter`註冊：

* 多個`IStartupFilter`實作可能會與相同的物件互動。 如果順序很重要，其`IStartupFilter`服務註冊，以符合其 middlewares 應該執行的順序。
* 程式庫可能新增中介軟體的一或多個`IStartupFilter`之前或之後註冊其他應用程式中介軟體執行的實作`IStartupFilter`。 要叫用`IStartupFilter`之前新增的媒體櫃的中介軟體的中介軟體`IStartupFilter`，程式庫加入至服務容器之前位置的服務登錄。 若要之後叫用它，加入程式庫之後的位置的服務登錄。

## <a name="additional-resources"></a>其他資源

* [裝載](xref:fundamentals/hosting)
* [使用多個環境](xref:fundamentals/environments)
* [中介軟體](xref:fundamentals/middleware)
* [記錄](xref:fundamentals/logging/index)
* [組態](xref:fundamentals/configuration/index)
* [StartupLoader 類別： FindStartupType 方法 （參考來源）](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116))
