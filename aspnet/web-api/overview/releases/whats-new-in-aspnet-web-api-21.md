---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: ASP.NET Web API 2.1 中最新消息 |Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 01/20/2014
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 7952614456b1de24e4c618b9e7ba8448b2a01741
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838399"
---
<a name="whats-new-in-aspnet-web-api-21"></a>ASP.NET Web API 2.1 中最新消息
====================
by [Microsoft](https://github.com/microsoft)

本主題會描述什麼是 ASP.NET Web API 2.1 的新功能。

- [下載](#download)
- [文件](#documentation)
- [ASP.NET Web API 2.1 中的新功能](#new-features)

    - [全域錯誤處理](#global-error)
    - [屬性路由的增強功能](#attribute-routing)
    - [說明頁面改善](#help-page)
    - [IgnoreRoute 支援](#ignoreroute)
    - [BSON 的媒體類型格式器](#bson)
    - [非同步篩選條件有更好的支援](#async-filters)
    - [查詢剖析為格式化的媒體櫃用戶端](#query-parsing)
- [已知的問題和重大變更](#known-issues)
- [Bug 修正](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>下載

執行階段功能會以 NuGet 套件，NuGet gallery 上發行。 所有執行階段套件遵循[語意版本設定](http://semver.org/)規格。 最新的 ASP.NET Web API 2.1 RTM 套件有下列版本:"5.1.2"。 您可以安裝或更新這些套件，透過[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/)。 此版也包含在 NuGet 上的對應當地語系化的套件。

您可以安裝或使用 NuGet 套件管理員主控台更新到發行的 NuGet 套件：

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>文件

教學課程和 ASP.NET Web API 2.1 RTM 的其他資訊都是從 ASP.NET 網站 ([https://www.asp.net/web-api](../../index.md))。

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>ASP.NET Web API 2.1 中的新功能

<a id="global-error"></a>
### <a name="global-error-handling"></a>全域錯誤處理

現在可以透過一個中央的機制，記錄所有未處理的例外狀況，並可自訂的未處理例外狀況的行為。

架構支援多個例外狀況記錄器，其所有的未處理的例外狀況和其發生的例如當時正在處理的要求內容的相關資訊，請參閱。

例如，下列程式碼會使用 System.Diagnostics.TraceSource 記錄所有未處理的例外狀況：

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

您也可以取代預設的例外狀況處理常式，以便您可以完全自訂的未處理的例外狀況時傳送的 HTTP 回應訊息就會發生。

我們提供了[範例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt)會記錄所有未處理的例外狀況，透過受歡迎的 ELMAH 架構。

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>屬性路由的增強功能

屬性路由現在支援條件約束，啟用版本控制和標頭為基礎的路由選取。 此外，已可透過自訂屬性路由的許多層面**IDirectRouteFactory**介面並**RouteFactoryAttribute**類別。 路由前置字元現在是透過可延伸**IRoutePrefix**介面並**RoutePrefixAttribute**類別。

我們提供了[範例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt)來動態篩選控制器，' api 版本 ' HTTP 標頭所使用的條件約束。

<a id="help-page"></a>
### <a name="help-page-improvements"></a>說明頁面改善

Web API 2.1 包含下列增強功能[API 說明頁面](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- 文件的個別屬性的參數或傳回類型的動作。
- 文件的資料模型註釋。

[說明] 頁的 UI 設計也已更新，成配合這些變更。

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>IgnoreRoute 支援

Web API 2.1 支援略過在一組透過 Web API 路由的 URL 模式**IgnoreRoute**擴充方法**HttpRouteCollection**。 這些方法會導致 Web API，可忽略符合指定的範本，任何 Url，並允許將額外的處理視主機。

下列範例會略過開頭的 Uri&quot;內容&quot;區段：

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>BSON 的媒體類型格式器

Web API 現在支援[BSON](http://bsonspec.org/)電傳格式，在用戶端和伺服器上。

若要在伺服器端啟用 BSON，新增**BsonMediaTypeFormatter**格式器集合：

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

以下是.NET 用戶端如何使用 BSON 格式：

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

我們提供了[範例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt)示範用戶端和伺服器端。

如需詳細資訊，請參閱[Web API 2.1 中的 BSON 支援](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>非同步篩選條件有更好的支援

Web API 現在支援簡單的方法，來建立篩選器，以非同步方式執行。 這個功能非常實用是您的篩選器必須執行非同步動作，例如資料庫的存取。 以前，若要建立非同步篩選條件，您必須實作篩選條件介面，因為篩選器的基底類別只會公開同步的方法。 現在可以覆寫虛擬`On*Async`方法篩選條件的基底類別。

例如: 

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

**AuthorizationFilterAttribute**， **Actionfilterattribut**，並**ExceptionFilterAttribute**類別全都支援 Web API 2.1 中的非同步。

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>查詢剖析為格式化的媒體櫃用戶端

先前**System.Net.Http.Formatting**支援剖析及更新的伺服器端程式碼中，URI 查詢，但對應的可攜式程式庫缺少這項功能。 在 Web API 2.1 中，用戶端應用程式現在可以輕鬆剖析並更新查詢字串。

下列範例示範如何剖析、 修改和產生 URI 查詢。 （範例顯示簡單的主控台應用程式）。

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的問題和重大變更

本章節描述的已知的問題和 ASP.NET Web API 2.1 RTM 中的重大變更。

### <a name="attribute-routing"></a>屬性路由

模稜兩可的屬性路由的相符項目現在會報告錯誤，而不是選擇第一個相符項目。

屬性路由會嚴禁 *{controller}* 參數，透過 *{action}* 路由參數放在動作上。 這些參數，很可能會導致模稜兩可。

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>到 5.1 封裝在中使用結果 5.0 封裝不存在專案中之專案的 scaffolding MVC/Web API

ASP.NET Web API 2.1 rtm 更新 NuGet 套件不會更新 Visual Studio 工具，例如 ASP.NET scaffolding 或 ASP.NET Web 應用程式專案範本。 他們使用舊版 ASP.NET 執行階段套件 (version=5.0.0.0)。 如此一來，ASP.NET scaffolding 會安裝必要的套件，先前的版本 (version=5.0.0.0)，如果它們還不在您的專案中。 不過，在 Visual Studio 2013 RTM 或更新 1 中的 ASP.NET 樣板不能覆寫您在專案中最新的套件。

如果您使用 ASP.NET scaffolding Web API 2.1 或 ASP.NET MVC 5.1 更新封裝之後，請確定 Web API 和 MVC 的版本不一致。

### <a name="type-renames"></a>型別重新命名

一些用於屬性路由的擴充性的類型已重新命名從 RC 為 2.1 RTM。

| 舊的型別名稱 (2.1 RC) | 新的型別名稱 (2.1 RTM) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>例外狀況篩選條件不解除包裝彙總中非同步動作擲回的例外狀況

先前，如果非同步動作擲回**AggregateException**，例外狀況篩選條件會解除包裝為例外狀況，並**OnException**會取得基底例外狀況。 在 2.1 中，例外狀況篩選條件不會不解除包裝，並**OnException**取得原始**AggregateException**。

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Bug 修正

此版本也包含數個 bug 修正。 您可以找到完整的清單：

- [5.1.0 套件](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 封裝](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

5.1.2 封裝包含 IntelliSense 的更新，但任何錯誤修正。
