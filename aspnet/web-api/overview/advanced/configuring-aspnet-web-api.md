---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: "設定 ASP.NET Web API 2 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f9b471fe2afdce278869a2e4d9b693a78030324b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="configuring-aspnet-web-api-2"></a>設定 ASP.NET Web API 2
====================
由[Mike Wasson](https://github.com/MikeWasson)

本主題描述如何設定 ASP.NET Web API。

- [組態設定](#settings)
- [設定與 ASP.NET 裝載的 Web 應用程式開發介面](#webhost)
- [設定與 OWIN 自我主控的 Web 應用程式開發介面](#selfhost)
- [全域 Web API 服務](#services)
- [每個控制站設定](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>組態設定

中所定義的 web API 組態所[HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx)類別。

| 成員 | 描述 |
| --- | --- |
| **DependencyResolver** | 可讓相依性插入控制站。 請參閱[使用 Web 應用程式開發介面的相依性解析程式](dependency-injection.md)。 |
| **篩選** | 動作篩選條件。 |
| **格式器** | [媒體類型格式器](../formats-and-model-binding/media-formatters.md)。 |
| **IncludeErrorDetailPolicy** | 指定伺服器在 HTTP 回應訊息中是否包含錯誤詳細資料，例如例外狀況訊息和堆疊追蹤。 請參閱[IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108))。 |
| **Initializer** | 執行最後的初始化函式**HttpConfiguration**。 |
| **MessageHandlers** | [HTTP 訊息處理常式](http-message-handlers.md)。 |
| **ParameterBindingRules** | 用於控制器動作上的繫結參數的規則集合。 |
| **屬性** | 一般屬性包。 |
| **路由** | 路由的集合。 請參閱[中 ASP.NET Web API 的路由](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)。 |
| **服務** | 服務集合。 請參閱[服務](#services)。 |


## <a name="prerequisites"></a>必要條件

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community、 Professional 或 Enterprise Edition。

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>設定與 ASP.NET 裝載的 Web 應用程式開發介面

ASP.NET 應用程式中，設定 Web 應用程式開發介面呼叫[GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx)中**應用程式\_啟動**方法。 **設定**方法會採用單一參數的型別與委派**HttpConfiguration**。 執行所有的委派在您使用設定。

以下是使用在匿名委派的範例：

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

在 Visual Studio 2017，「 ASP.NET Web 應用程式 」 專案範本會自動設定的組態程式碼中，如果您選取 「 Web 應用程式開發介面 」 中**新增 ASP.NET 專案**對話方塊。

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

專案範本建立名為在應用程式內 WebApiConfig.cs\_開始資料夾。 這個程式碼檔會定義應該放置 Web API 組態程式碼的委派。

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

專案範本也會加入程式碼呼叫的委派**應用程式\_啟動**。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>設定與 OWIN 自我主控的 Web 應用程式開發介面

如果您是自我裝載與 OWIN，建立新**HttpConfiguration**執行個體。 這個執行個體上，執行任何設定，然後將傳遞至執行個體**Owin.UseWebApi**擴充方法。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

本教學課程[Self-Host ASP.NET Web API 2 使用 OWIN](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)顯示完成的步驟。

<a id="services"></a>
## <a name="global-web-api-services"></a>全域 Web API 服務

**HttpConfiguration.Services**集合包含一組通用 Web API 用來執行各種工作，例如控制器選取範圍和內容交涉的服務。

> [!NOTE]
> **服務**集合不是用於服務探索或相依性插入的一般用途機制。 它只會儲存至 Web API 架構已知的服務類型。


**服務**集合已初始化與一組預設的服務，而且您可以提供您自己的自訂實作。 有些服務支援多個執行個體，而其他人可以有只有一個執行個體。 (不過，您也可以提供在控制器層級的服務，請參閱[每個控制站設定](#percontrollerconfig)。

單一執行個體的服務


| 服務 | 描述 |
| --- | --- |
| **IActionValueBinder** | 取得參數繫結。 |
| **IApiExplorer** | 取得描述元的應用程式所公開的 Api。 請參閱[建立 Web API 說明頁面](../getting-started-with-aspnet-web-api/creating-api-help-pages.md)。 |
| **IAssembliesResolver** | 取得應用程式的組件清單。 請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。 |
| **IBodyModelValidator** | 驗證媒體類型格式器所讀取的要求主體的模型。 |
| **IContentNegotiator** | 執行內容交涉。 |
| **IDocumentationProvider** | 提供 Api 文的件。 預設值是**null**。 請參閱[建立 Web API 說明頁面](../getting-started-with-aspnet-web-api/creating-api-help-pages.md)。 |
| **IHostBufferPolicySelector** | 表示主機是否應緩衝 HTTP 訊息實體內文。 |
| **IHttpActionInvoker** | 叫用控制器動作。 請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。 |
| **IHttpActionSelector** | 選取控制器動作。 請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。 |
| **IHttpControllerActivator** | 啟動控制站。 請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。 |
| **IHttpControllerSelector** | 選取控制器。 請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。 |
| **IHttpControllerTypeResolver** | 提供一份應用程式中的 Web API 控制器類型。 請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。 |
| **ITraceManager** | 初始化追蹤架構。 請參閱[中 ASP.NET Web API 追蹤](../testing-and-debugging/tracing-in-aspnet-web-api.md)。 |
| **ITraceWriter** | 提供追蹤寫入器。 預設為 「 無操作 」 追蹤寫入器。 請參閱[中 ASP.NET Web API 追蹤](../testing-and-debugging/tracing-in-aspnet-web-api.md)。 |
| **IModelValidatorCache** | 提供模型驗證程式的快取。 |

多個執行個體的服務


| 服務 | 描述 |
| --- | --- |
| **IFilterProvider** | 傳回控制器動作的篩選清單。 |
| **ModelBinderProvider** | 傳回指定型別的模型繫結。 |
| **ModelMetadataProvider** | 提供模型中繼資料。 |
| **ModelValidatorProvider** | 提供模型驗證程式。 |
| **ValueProviderFactory** | 建立值提供者。 如需詳細資訊，請參閱的 Mike 拖延部落格文章[如何在 WebAPI 中建立的自訂值提供者](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |。

若要加入多個執行個體服務的自訂實作，請呼叫**新增**或**插入**上**服務**集合：

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

若要取代的自訂實作中的單一執行個體服務，請呼叫**取代**上**服務**集合：

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>每個控制站設定

您可以覆寫每個控制站為基礎的下列設定：

- 媒體類型格式器
- 參數繫結規則
- 服務

若要這樣做，請定義自訂屬性來實作**IControllerConfiguration**介面。 然後將屬性套用至控制器。

下列範例以自訂格式子取代預設媒體類型格式器。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

**IControllerConfiguration.Initialize**方法會採用兩個參數：

- **HttpControllerSettings**物件
- **HttpControllerDescriptor**物件

**HttpControllerDescriptor**包含控制站，您可以檢查僅供參考之用 （例如，若要在區別兩個控制站） 的描述。

使用**HttpControllerSettings**物件來設定控制器。 此物件包含您可以覆寫每個控制站為基礎的組態參數的子集。 請不要變更任何設定預設為全域**HttpConfiguration**物件。
