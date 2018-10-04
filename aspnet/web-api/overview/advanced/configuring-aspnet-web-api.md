---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: 設定 ASP.NET Web API 2 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/31/2014
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 57066b8ce3254caf59cf927d16d96f8bc22a8acd
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795247"
---
<a name="configuring-aspnet-web-api-2"></a>設定 ASP.NET Web API 2
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

本主題描述如何設定 ASP.NET Web API。

- [組態設定](#settings)
- [設定與 ASP.NET 裝載的 Web API](#webhost)
- [設定 Web API 與 OWIN 自我裝載](#selfhost)
- [全域的 Web API 服務](#services)
- [每個控制站設定](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>組態設定

Web API 組態設定中定義[HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx)類別。

| 成員 | 描述 |
| --- | --- |
| **DependencyResolver** | 可讓控制站的相依性插入。 請參閱[使用 Web API 的相依性解析程式](dependency-injection.md)。 |
| **篩選** | 動作篩選條件。 |
| **格式器** | [媒體類型格式器](../formats-and-model-binding/media-formatters.md)。 |
| **IncludeErrorDetailPolicy** | 指定伺服器是否應該在 HTTP 回應訊息中包含錯誤詳細資料，例如例外狀況訊息和堆疊追蹤。 請參閱[IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108))。 |
| **初始設定式** | 執行最後的初始化函式**HttpConfiguration**。 |
| **MessageHandlers** | [HTTP 訊息處理常式](http-message-handlers.md)。 |
| **ParameterBindingRules** | 用於控制器動作上的繫結參數的規則集合。 |
| **屬性** | 泛型屬性包。 |
| **路由** | 路由的集合。 請參閱[ASP.NET Web API 中的路由](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)。 |
| **服務** | 服務集合。 請參閱[Services](#services)。 |


## <a name="prerequisites"></a>必要條件

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community、 Professional 或 Enterprise edition。

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>設定與 ASP.NET 裝載的 Web API

在 ASP.NET 應用程式，請藉由呼叫中設定 Web API [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx)中**應用程式\_啟動**方法。 **設定**方法會接受具有單一參數型別的委派**HttpConfiguration**。 執行所有您在委派內的組態。

以下是使用匿名委派的範例：

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

在 Visual Studio 2017 中，「 ASP.NET Web 應用程式 」 專案範本會自動設定的組態程式碼中，如果您選取 Web API 」 中**新的 ASP.NET 專案**對話方塊。

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

專案範本會建立名為 「 應用程式的 WebApiConfig.cs 檔案\_開始資料夾。 這個程式碼檔會定義應放置您的 Web API 組態程式碼的委派。

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

專案範本也會將加入程式碼呼叫的委派**應用程式\_啟動**。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>設定 Web API 與 OWIN 自我裝載

如果您是使用 OWIN 自我裝載，建立新**HttpConfiguration**執行個體。 在此情況下，執行任何設定，然後將傳遞至執行個體**Owin.UseWebApi**擴充方法。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

本教學課程[使用 OWIN 自我裝載 ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)顯示完成的步驟。

<a id="services"></a>
## <a name="global-web-api-services"></a>全域的 Web API 服務

**HttpConfiguration.Services**集合包含一組 Web API 用來執行各種工作，例如控制器選取項目和內容交涉的全域服務。

> [!NOTE]
> **Services**集合不是服務探索或相依性插入的一般用途機制。 它只會儲存 Web API 架構已知的服務類型。


**Services**集合已初始化與一組預設的服務，而且您可以提供您自己的自訂實作。 某些服務會支援多個執行個體，而有些則可以有只有一個執行個體。 (不過，您也可以提供在控制器層級的服務，請參閱[每個控制站設定](#percontrollerconfig)。

單一執行個體的服務


| 服務 | 描述 |
| --- | --- |
| **IActionValueBinder** | 取得參數繫結。 |
| **IApiExplorer** | 取得描述元的應用程式所公開的 Api。 請參閱[建立 Web API 說明頁面](../getting-started-with-aspnet-web-api/creating-api-help-pages.md)。 |
| **IAssembliesResolver** | 取得應用程式的組件清單。 請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。 |
| **IBodyModelValidator** | 驗證的模型，會從要求主體讀取媒體類型格式器。 |
| **IContentNegotiator** | 執行內容交涉。 |
| **IDocumentationProvider** | 提供 Api 的文件。 預設值是**null**。 請參閱[建立 Web API 說明頁面](../getting-started-with-aspnet-web-api/creating-api-help-pages.md)。 |
| **IHostBufferPolicySelector** | 表示主機是否應緩衝 HTTP 訊息實體內文。 |
| **IHttpActionInvoker** | 叫用控制器動作。 請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。 |
| **IHttpActionSelector** | 選取控制器動作。 請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。 |
| **IHttpControllerActivator** | 啟動控制站。 請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。 |
| **IHttpControllerSelector** | 選取控制器。 請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。 |
| **IHttpControllerTypeResolver** | 提供一份應用程式中的 Web API 控制器類型。 請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。 |
| **ITraceManager** | 初始化追蹤架構。 請參閱[ASP.NET Web API 中追蹤](../testing-and-debugging/tracing-in-aspnet-web-api.md)。 |
| **ITraceWriter** | 提供的追蹤寫入器。 預設為 「 無操作 」 追蹤寫入器。 請參閱[ASP.NET Web API 中追蹤](../testing-and-debugging/tracing-in-aspnet-web-api.md)。 |
| **IModelValidatorCache** | 提供模型驗證程式的快取。 |

多個執行個體的服務


|                 服務                 |                                                                                                              描述                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <strong>IFilterProvider</strong>     |                                                                                           傳回控制器動作篩選條件的清單。                                                                                           |
|  <strong>ModelBinderProvider</strong>   |                                                                                                傳回指定型別的模型繫結。                                                                                                |
| <strong>ModelMetadataProvider</strong>  |                                                                                                     提供模型中繼資料。                                                                                                     |
| <strong>ModelValidatorProvider</strong> |                                                                                                   提供模型驗證程式。                                                                                                    |
|  <strong>ValueProviderFactory</strong>  | 建立值提供者。 如需詳細資訊，請參閱 Mike Stall 的部落格文章[如何在 WebAPI 中建立的自訂值提供者](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |

若要加入多個執行個體服務的自訂實作，請呼叫**新增**或是**插入**上**Services**集合：

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

若要使用的自訂實作取代單一執行個體服務，請呼叫**取代**上**服務**集合：

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>每個控制站設定

您可以覆寫每個控制器為基礎的下列設定：

- 媒體類型格式器
- 參數繫結規則
- 服務

若要這樣做，請定義自訂屬性，以實作**IControllerConfiguration**介面。 然後將屬性套用至控制器中。

下列範例會以自訂格式器取代預設媒體類型格式器。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

**IControllerConfiguration.Initialize**方法採用兩個參數：

- **HttpControllerSettings**物件
- **HttpControllerDescriptor**物件

**HttpControllerDescriptor**包含控制器，您可以檢查僅供參考之用 （例如，若要區別兩個控制站） 的描述。

使用**HttpControllerSettings**設定控制站的物件。 此物件包含您可以覆寫每個控制器為基礎的組態參數的子集。 您不會變更任何設定預設為全域**HttpConfiguration**物件。
