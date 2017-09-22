---
title: "在 ASP.NET Core 應用程式啟動"
author: ardalis
description: "說明在 ASP.NET Core 啟動類別。"
keywords: "ASP.NET Core，啟動時，設定方法、 ConfigureServices 方法"
ms.author: tdykstra
manager: wpickett
ms.date: 02/29/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: 009df1416c822018d6e88912cc77e525c7349c34
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="application-startup-in-aspnet-core"></a>在 ASP.NET Core 應用程式啟動

由[Steve Smith](https://ardalis.com/)和[Tom Dykstra](https://github.com/tdykstra/)

`Startup`類別來設定服務和應用程式的要求管線。 

## <a name="the-startup-class"></a>啟動類別

ASP.NET Core 應用程式需要`Startup`類別。 依照慣例，`Startup`類別的名稱為 「 啟動 」。 指定啟動類別名稱在`Main`程式的[WebHostBuilderExtensions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions) [ `UseStartup<TStartup>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)方法。 請參閱[主控](xref:fundamentals/hosting)若要深入了解`WebHostBuilder`，執行之前`Startup`。

您可以定義個別`Startup`不同環境中，並在執行階段會選取一個適當的類別。 如果您指定`startupAssembly`中[WebHost 設定](https://docs.microsoft.com/aspnet/core/fundamentals/hosting?tabs=aspnetcore2x#configuring-a-host)或裝載的選項將會載入該啟動組件，並搜尋`Startup`或`Startup[Environment]`型別。 類別目前的環境來排列優先順序的名稱後置詞相符項目，因此如果在執行應用程式*開發*環境中，並同時包含`Startup`和`StartupDevelopment`類別`StartupDevelopment`類別將為使用。 請參閱[FindStartupType](https://github.com/aspnet/Hosting/blob/rel/1.1.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs)中`StartupLoader`和[使用多個環境](environments.md#startup-conventions)。

或者，您可以定義固定`Startup`不論環境將使用藉由呼叫類別`UseStartup<TStartup>`。 這是建議的處理方式。

`Startup`類別建構函式可以接受經由所提供的相依性[相依性插入](xref:fundamentals/dependency-injection)。 常見的方法是使用`IHostingEnvironment`設定[組態](xref:fundamentals/configuration)來源。

`Startup`類別必須包含`Configure`方法，而且可以選擇包含`ConfigureServices`方法，這兩種應用程式啟動時呼叫。 類別也可以包含[這些方法的環境特定版本](xref:fundamentals/environments#startup-conventions)。 `ConfigureServices`如果有的話，會呼叫之前`Configure`。

深入了解[例外狀況處理應用程式啟動期間](xref:fundamentals/error-handling#startup-exception-handling)。

## <a name="the-configureservices-method"></a>ConfigureServices 方法

[ConfigureServices](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.startupbase#Microsoft_AspNetCore_Hosting_StartupBase_ConfigureServices_Microsoft_Extensions_DependencyInjection_IServiceCollection_)方法是選擇項，但如果使用，它就稱為之前`Configure`web 主機的方法。 Web 主機可能會設定某些服務之前``Startup``呼叫的方法 (請參閱[裝載](xref:fundamentals/hosting))。 依照慣例，[組態選項](xref:fundamentals/configuration)中這個方法所設定。

對於需要大量的安裝程式的功能有`Add[Service]`的擴充方法[IServiceCollection](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.iservicecollection)。 這個範例來自預設的網站範本會設定應用程式使用 Entity Framework、 識別和 MVC 的服務：

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

將服務加入至服務容器可透過應用程式中使用[相依性插入](xref:fundamentals/dependency-injection)。

## <a name="services-available-in-startup"></a>用於啟動服務

ASP.NET Core 相依性插入的應用程式啟動時提供服務。 您可以要求這些服務包括做為參數的適當的介面上您`Startup`類別的建構函式或其`Configure`方法。 `ConfigureServices`方法只接受`IServiceCollection`參數 （但任何的已註冊的服務可以擷取從這個集合中，因此不需要額外的參數）。

下面是一些通常會要求的服務`Startup`方法：

* 建構函式： `IHostingEnvironment`，`ILogger<Startup>`
* 在`ConfigureServices`:`IServiceCollection`
* 在`Configure`: `IApplicationBuilder`， `IHostingEnvironment`，`ILoggerFactory`

新增的任何服務``WebHostBuilder````ConfigureServices``方法可要求的``Startup``類別建構函式或其``Configure``方法。 使用`WebHostBuilder`提供任何服務需要期間`Startup`方法。

## <a name="the-configure-method"></a>Configure 方法

`Configure`方法用來指定 ASP.NET 應用程式將如何回應 HTTP 要求。 透過加入設定要求管線[中介軟體](middleware.md)元件`IApplicationBuilder`相依性插入所提供的執行個體。

在下列範例從預設的網站範本中，數個擴充方法可用來設定管線，可以支援[BrowserLink](http://vswebessentials.com/features/browserlink)，錯誤頁面、 靜態檔案，ASP.NET MVC 和身分識別。

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

每個`Use`擴充方法新增[中介軟體](xref:fundamentals/middleware)要求管線元件。 比方說，`UseMvc`擴充方法新增[路由](routing.md)要求管線中的介軟體，並設定[MVC](xref:mvc/overview)為預設處理常式。

如需有關如何使用`IApplicationBuilder`，請參閱[中介軟體](xref:fundamentals/middleware)。

其他服務，例如`IHostingEnvironment`和`ILoggerFactory`也可以在方法簽章中指定這些服務將無法在此情況下[插入](dependency-injection.md)在有提供。 

## <a name="additional-resources"></a>其他資源

* [使用多個環境](xref:fundamentals/environments)
* [中介軟體](xref:fundamentals/middleware)
* [記錄](xref:fundamentals/logging)
* [組態](xref:fundamentals/configuration)
