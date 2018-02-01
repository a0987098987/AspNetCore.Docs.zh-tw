---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: "ASP.NET Web API 2.1 中最新消息 |Microsoft 文件"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: cc5dc111d88cc7dae6a4a93203317fa0769d5427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-21"></a>ASP.NET Web API 2.1 中最新消息
====================
由[Microsoft](https://github.com/microsoft)

本主題說明 ASP.NET Web API 2.1 的新功能。

- [下載](#download)
- [文件 (英文)](#documentation)
- [ASP.NET Web API 2.1 的新功能](#new-features)

    - [全域錯誤處理](#global-error)
    - [屬性路由的增強功能](#attribute-routing)
    - [說明頁面增強功能](#help-page)
    - [IgnoreRoute 支援](#ignoreroute)
    - [BSON 的媒體類型格式器](#bson)
    - [非同步篩選器更佳的支援](#async-filters)
    - [格式化文件庫的用戶端所剖析的查詢](#query-parsing)
- [已知的問題和重大變更](#known-issues)
- [Bug 修正](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>下載

執行階段功能會以 NuGet 封裝在 NuGet gallery 上發行。 所有執行階段封裝遵循[語意版本設定](http://semver.org/)規格。 最新的 ASP.NET Web API 2.1 RTM 封裝有下列版本:"5.1.2"。 您可以安裝或更新這些套件透過[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/)。 此版也包含對應當地語系化的 NuGet 套件。

您可以安裝或使用 NuGet 封裝管理員主控台更新到發行的 NuGet 封裝：

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>文件

教學課程和 ASP.NET Web API 2.1 RTM 的其他資訊都是從 ASP.NET 網站 ([https://www.asp.net/web-api](../../index.md))。

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>ASP.NET Web API 2.1 的新功能

<a id="global-error"></a>
### <a name="global-error-handling"></a>全域錯誤處理

所有未處理的例外狀況現在會記錄透過一個中央的機制，並可自訂的未處理例外狀況行為。

架構支援多個例外狀況記錄器，其所有未處理的例外狀況和發生，例如時正在處理的要求內容的相關資訊，請參閱。

例如，下列程式碼會使用 System.Diagnostics.TraceSource 記錄所有未處理的例外狀況：

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

您也可以取代預設的例外狀況處理常式，可讓您完全可以自訂處理的例外狀況時，會傳送 HTTP 回應訊息，就會發生。

我們已提供[範例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt)記錄透過受歡迎的 ELMAH framework 的所有未處理例外狀況。

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>屬性路由的增強功能

屬性現在路由支援條件約束，啟用版本控制和標頭為基礎的路由選取。 此外，已可透過自訂屬性路由的許多層面**IDirectRouteFactory**介面和**RouteFactoryAttribute**類別。 路由前置字元現在是可透過延伸**IRoutePrefix**介面和**RoutePrefixAttribute**類別。

我們已提供[範例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt)來動態篩選控制站 'api-version' HTTP 標頭所使用的條件約束。

<a id="help-page"></a>
### <a name="help-page-improvements"></a>說明頁面增強功能

Web API 2.1 包含下列增強功能[API 說明頁面](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- 文件的個別屬性的參數或傳回類型的動作。
- 文件集的資料模型的註解。

說明網頁的 UI 設計也已更新，以容納這些變更。

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>IgnoreRoute 支援

Web 應用程式開發介面 2.1 支援忽略中透過一組 Web API 路由的 URL 模式**IgnoreRoute**的擴充方法**HttpRouteCollection**。 這些方法會導致 Web API，略過任何符合指定的範本的 Url，而且允許套用額外的處理視主機。

下列範例會忽略開頭的 Uri&quot;內容&quot;區段：

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>BSON 的媒體類型格式器

Web API 現在支援[BSON](http://bsonspec.org/) wire 格式，在用戶端和伺服器上。

將加入伺服器端上，讓 BSON **BsonMediaTypeFormatter**格式器集合：

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

以下是如何在.NET 用戶端可以取用 BSON 格式：

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

我們已提供[範例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt)，它會顯示用戶端和伺服器端。

如需詳細資訊，請參閱[BSON 支援 Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>非同步篩選器更佳的支援

Web API 現在支援用來建立篩選器，以非同步方式執行。 此功能十分有用是您的篩選條件必須執行非同步動作，例如資料庫的存取。 以前，若要建立非同步篩選器，您必須實作篩選介面自行，因為篩選器的基底類別只會公開同步方法。 現在可以覆寫虛擬`On*Async`方法篩選器的基底類別。

例如: 

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

**AuthorizationFilterAttribute**， **Actionfilterattribut**，和**ExceptionFilterAttribute**類別所有 Web 應用程式開發介面 2.1 中來都支援非同步。

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>格式化文件庫的用戶端所剖析的查詢

先前， **System.Net.Http.Formatting**支援剖析及更新的伺服器端程式碼中，URI 查詢，但對等的可攜式程式庫遺失此功能。 在 Web 應用程式開發介面 2.1 中，用戶端應用程式現在可以輕鬆地剖析，並更新查詢字串。

下列範例顯示如何剖析、 修改及產生 URI 查詢。 （範例將說明簡單的主控台應用程式）。

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的問題和重大變更

本章節描述已知的問題和 ASP.NET Web API 2.1 RTM 中的重大變更。

### <a name="attribute-routing"></a>路由屬性

屬性路由相符項目中的模稜兩可現在會報告錯誤，而不是選擇第一個相符項目。

屬性路由嚴禁*{控制器}*參數，並從使用*{action}*路由參數放在 [動作]。 這些參數會非常有可能會導致模稜兩可。

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Scaffolding MVC/Web 應用程式開發介面到 5.1 封裝在中使用結果 5.0 封裝還不存在專案中的專案

ASP.NET Web API 2.1 rtm 更新 NuGet 封裝不會更新 Visual Studio 工具，例如 ASP.NET scaffolding 或 ASP.NET Web 應用程式專案範本。 他們使用的舊版 ASP.NET 執行階段封裝 (5.0.0.0)。 如此一來，ASP.NET scaffolding 會安裝必要的封裝前, 一版 (5.0.0.0)，如果還沒有出現在您的專案。 不過，在 Visual Studio 2013 RTM 或更新 1 中的 ASP.NET scaffolding 不會覆寫您的專案中最新的封裝。

如果您使用 ASP.NET scaffolding Web API 2.1 或 ASP.NET MVC 5.1 更新封裝之後，請確定 Web API 和 MVC 的版本不一致。

### <a name="type-renames"></a>型別重新命名

某些屬性的路由擴充性所使用的類型已重新命名從 RC 2.1 RTM。

| 舊的型別名稱 (2.1 RC) | 新的類型名稱 (2.1 RTM) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>例外狀況篩選條件不解除包裝在非同步動作中擲回的例外狀況彙總

過去，如果擲回的非同步動作**AggregateException**，例外狀況篩選條件會解除包裝的例外狀況和**OnException**得到基底例外狀況。 在 2.1 中，例外狀況篩選條件不會不解除包裝，和**OnException**取得原始**AggregateException**。

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Bug 修正

此版本也包含數個 bug 修正。 您可以找到完整的清單：

- [5.1.0 封裝](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 封裝](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

5.1.2 封裝包含 IntelliSense 的更新，但沒有 bug 修正。
