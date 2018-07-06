---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: 不執行在 ASP.NET 中，以及該做什麼 |Microsoft Docs
author: tfitzmac
description: 本主題會描述人員進行 ASP.NET web 專案中的數個常見的錯誤。 它提供您該如何避免這些通用的建議...
ms.author: aspnetcontent
ms.date: 05/08/2014
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 9e9b192126995ac8a461b15bce69b60d57ca9ba1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806014"
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>不執行在 ASP.NET 中，以及該做什麼
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本主題會描述人員進行 ASP.NET web 專案中的數個常見的錯誤。 它提供建議您應該如何避免這些常見的錯誤。 它根據[簡報](http://vimeo.com/68390507)依**Damian Edwards**在 Norwegian Developers Conference。


## <a name="disclaimer"></a>免責聲明

本主題不是做為完整的指南以確保您的應用程式既安全又有效率。 您仍然要遵循的安全性和效能不會在本主題中所述的最佳作法。 它只會建議如何避免常見的錯誤與.NET 類別和程序。

## <a name="overview"></a>總覽

此主題包括下列章節：

- [標準相容性](#standards)

    - [控制項配接器](#adapters)
    - [控制項的樣式屬性](#styleprop)
    - [頁面和控制項的回呼](#callback)
    - [偵測瀏覽器功能](#browsercap)
- [安全性](#security)

    - [要求驗證](#validation)
    - [Cookieless 表單驗證和工作階段](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [中度信任](#medium)
    - [&lt;appSettings&gt;](#appsettings)
    - [UrlPathEncode](#urlpathencode)
- [可靠性和效能](#performance)

    - [PreSendRequestHeaders 和 PreSendRequestContent](#presend)
    - [Web form 的非同步頁面事件](#asyncevents)
    - [Fire-and-Forget Work](#fire)
    - [要求實體內容](#requestentity)
    - [Response.Redirect 和 Response.End](#redirect)
    - [EnableViewState 和 ViewStateMode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [長時間執行的要求 (> 110 秒)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>標準相容性

<a id="adapters"></a>

### <a name="control-adapters"></a>控制項配接器

建議： 可讓您停止使用調適性的轉譯控制項配接器，並改為使用 CSS 媒體查詢和符合標準的 HTML。

要呈現已自訂為不同的裝置和環境的展示程式碼的.NET 2.0 中引進的控制項配接器。 現在，此調整呈現即可透過 CSS 與 HTML。 您應該停止使用控制項配接器，並將任何現有的配接器轉換為 CSS 與 HTML。

如需詳細資訊，請參閱 <<c0> [ 媒體查詢](http://www.w3.org/TR/css3-mediaqueries/)並[How To： 將行動頁面新增至您的 ASP.NET Web Form / MVC 應用程式](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md)。

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>控制項的樣式屬性

建議： 停止在控制項標記中，設定樣式值，並改為設定 CSS 樣式表中的 格式化的值。

Web 伺服器控制項包含數十個可用設定的內嵌樣式屬性的屬性。 比方說，ForeColor 屬性設定控制項的文字的色彩。 您可以達成此相同的效果更有效率地透過 CSS 樣式表。 樣式表可讓您集中管理樣式值，並避免設定這些值在您的應用程式。

下列範例顯示 CSS 類別集文字設為紅色。

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

下一個範例示範如何以動態方式套用的 CSS 類別。

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>頁面和控制項的回呼

建議： 可讓您停止使用頁面和控制項的回呼，並改為使用下列任一項： AJAX、 UpdatePanel、 MVC 動作方法、 Web API 或 SignalR。

在舊版 ASP.NET 中，頁面和控制項的回呼方法會啟用您更新網頁的一部分，而不重新整理整個頁面。 您現在可以完成部分頁面更新，透過[AJAX](../../../ajax/index.md)， [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx)， [MVC](../../../mvc/index.md)， [Web API](../../../web-api/index.md)或[SignalR](../../../signalr/index.md). 您應該停止使用回呼方法，因為它們會導致問題，使用易記的 Url 和路由。 根據預設，控制項不會啟用回呼方法，但如果您啟用這項功能在控制項中的，您應該停用它。

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>偵測瀏覽器功能

建議： 可讓您停止使用靜態的瀏覽器功能偵測，並改為使用動態功能偵測。

在舊版 ASP.NET 中，每個瀏覽器支援的功能已儲存在 XML 檔案。 透過靜態查閱支援的偵測功能不是最好的方法。 現在，您可以動態地偵測瀏覽器支援的功能使用的功能偵測架構，例如[Modernizr](http://modernizr.com/)。 功能偵測來嘗試使用的方法或屬性，然後檢查以查看是否瀏覽器就會產生所要的結果判斷支援。 根據預設，Modernizr 包含在 Web 應用程式範本。

<a id="security"></a>

## <a name="security"></a>安全性

<a id="validation"></a>

### <a name="request-validation"></a>要求驗證

建議： 驗證使用者輸入，並將來自使用者的輸出編碼。

要求驗證是一項功能的 ASP.NET 會檢查每個要求，並停止要求，如果找到察覺到的威脅。 不相依於要求驗證來保護您的應用程式防止跨網站指令碼攻擊。 相反地，驗證來自使用者的所有輸入和輸出編碼。 在某些限制的情況下，您可以使用規則運算式來驗證輸入，但在更複雜的情況下，您應該驗證使用者輸入，使用.NET 類別，以決定如果值符合允許的值。

下列範例示範如何使用 Uri 類別中的靜態方法，以判斷使用者所提供的 Uri 是否有效。

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

不過，若要充分驗證 Uri，您也應該檢查以確定它會指定`http`或`https`。 下列範例會使用執行個體方法，驗證 Uri 有效。

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

轉譯為 HTML 的使用者輸入，或在 SQL 查詢中包括使用者輸入，再將編碼的值，以確保惡意程式碼就不會包含。

您可以 HTML 編碼標記中的值&lt;%: %&gt;語法，如下所示。

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

或者，您可以在 Razor 語法中，可以 HTML 編碼與 @，如下所示。

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

下一個範例顯示如何為 HTML 編碼程式碼後置中的值。

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

若要安全地將編碼的 SQL 命令的值，請使用命令參數如下[SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx)。 <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Cookieless 表單驗證和工作階段

建議： 需要 cookie。

查詢字串中傳遞驗證資訊並不安全。 因此，當您的應用程式包含驗證時，才需要 cookie。 如果您的 cookie 儲存機密資訊，請考慮 cookie 需要 SSL。

下列範例示範如何在 Web.config 檔案中指定表單驗證，需要透過 SSL 傳送的 cookie。

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

建議： 永遠不會設定為 false。

根據預設，設定 EnbableViewStateMac 設為 true。 即使您的應用程式不使用檢視狀態，不將 EnableViewStateMac 設定為 false。 將此值設定為 false 會讓應用程式容易遭受跨網站指令碼。

從 ASP.NET 4.5.2 開始，執行階段會強制**EnableViewStateMac = true**。 即使您將它設定為 false，執行階段就會忽略此值，並會繼續進行設定的值，設為 true。 如需詳細資訊，請參閱 < [ASP.NET 4.5.2 和 EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx)。

下列範例示範如何將 EnableViewStateMac 設定為 true。 您不需要實際此值設為 true，因為它是預設值為 true。 不過，如果您已將它設定為 false 任何頁面上您的應用程式中，您必須立即更正此值。

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>中度信任

建議： 不相依於中度信任 （或任何其他的信任層級） 做為安全性界限。

在部分信任不會適當地保護您的應用程式，和不應使用。 相反地，使用完全信任 」，並隔離個別的應用程式集區中不受信任的應用程式。 此外，執行下的唯一識別每個應用程式集區。 如需詳細資訊，請參閱 < [ASP.NET 部分信任的資訊並不保證應用程式隔離](https://support.microsoft.com/kb/2698981)。

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

建議： 請勿停用中的安全性設定&lt;appSettings&gt;項目。

AppSettings 項目包含許多值所需的安全性更新。 您不應該變更或停用這些值。 如果部署更新時，您必須停用這些值，請立即重新啟用完成部署之後。

如需詳細資訊，請參閱 < [ASP.NET appSettings 項目](https://msdn.microsoft.com/library/hh975440.aspx)。

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

建議： 使用[UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx)改。

UrlPathEncode 方法已新增至.NET Framework，才能解決非常特定的瀏覽器相容性問題。 它不會適當地編碼 URL，並不會保護您的應用程式從跨網站指令碼。 您應該永遠不會使用您的應用程式中。 請改用[UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx)。

下列範例示範如何為超連結控制項的查詢字串參數傳遞編碼的 URL。

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>可靠性和效能

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders 和 PreSendRequestContent

建議： 不要在使用這些事件的受管理模組。 相反地，撰寫原生 IIS 模組來執行必要的工作。 請參閱[建立原生程式碼 HTTP 模組](https://msdn.microsoft.com/library/ms693629.aspx)。

您可以使用[PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx)並[PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx)使用原生 IIS 模組的事件。
> [!WARNING]
> 請勿使用`PreSendRequestHeaders`並`PreSendRequestContent`實作的受管理模組使用`IHttpModule`。 設定這些屬性會導致發生問題的非同步要求。 應用程式要求路由 (ARR) 和 websockets 的組合可能會導致存取違規例外狀況會導致損毀的 w3wp。 比方說，iiscore ！W3_CONTEXT_BASE::GetIsLastNotification + 68 iiscore.dll 中的造成存取違規例外 (0xC0000005)。

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Web form 的非同步頁面事件

建議： 在 Web Form 中避免撰寫非同步 void 的方法，適用於網頁生命週期事件，並改用[Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx)非同步程式碼。

當您將標記的頁面事件**非同步**並**void**，您無法決定完成非同步的程式碼時。 請改用 Page.RegisterAsyncTask 執行非同步程式碼可讓您追蹤其完成方式。

下列範例顯示的按鈕中，按一下 處理常式，其中包含非同步程式碼。 此範例包含以非同步方式讀取的字串值所提供只做為非同步工作的簡化範例，而不是建議的作法。

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

如果您使用的非同步工作，設定為 4.5 Http 執行階段目標架構，在 Web.config 檔案中。 .NET 4.5 中已新增設定的目標 framework 4.5 的開啟新的同步處理內容上的。 此值設定是在 Visual Studio 2012 中的新專案中的預設值，但如果不是設定您正在使用現有的專案。

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>火災和忘記工作

建議： 當處理 asp.net 要求，避免啟動火災和忘記工作 （這類呼叫 ThreadPool.QueueUserWorkItem 方法或建立一個計時器，重複呼叫委派）。

如果您的應用程式有在 ASP.NET 中執行的單一事件-fire-and-forget 工作，您的應用程式可以取得不同步。在任何時間，應用程式定義域可以終結這表示您持續不斷的過程可能不再符合應用程式的目前狀態。

您應該移動這種類型的工作，在 ASP.NET 之外。 您可以在 Azure 中使用 Web 工作、 Windows 服務或背景工作角色來執行進行中的工作，並從另一個處理序執行該程式碼。

如果您必須執行這項工作在 ASP.NET 中的，您可以新增 Nuget 套件，稱之為[WebBackgrounder](http://www.nuget.org/packages/webbackgrounder)執行程式碼。

<a id="requestentity"></a>

### <a name="request-entity-body"></a>要求實體內容

建議： 請避免讀取 Request.Form 或 Request.InputStream 之前的處理常式執行事件。

您應該讀取 Request.Form 或 Request.InputStream 最舊期間的處理常式執行事件。 在 MVC 中，控制器是處理常式的執行事件時，則在動作方法執行。 在 Web Form 頁面是處理常式，Page.Init 事件引發時執行事件。 如果您的執行事件之前讀取要求實體內容，您會干擾處理要求。

如果您要讀取要求實體內容的執行事件之前，請使用[Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx)或是[Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx)。 當您使用 GetBufferlessInputStream 時，您會從要求取得未經處理的資料流，並負責提供處理整個要求。 在呼叫之後 GetBufferlessInputStream，Request.Form 和 Request.InputStream 無法使用。 因為它們未填入 asp.net 當您使用 GetBufferedInputStream 時，您會從要求取得一份資料流。 Request.Form Request.InputStream 等要求的更新版本中仍然可用因為 ASP.NET 會填入其他複本。

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Response.Redirect 和 Response.End

建議： 需要注意的執行緒之後呼叫的處理方式的差異[Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx)。

[Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx)方法會呼叫 Response.End 方法。 在同步處理中，呼叫 Request.Redirect，將會導致目前的執行緒立即中止。 不過，在非同步處理序，呼叫 Response.Redirect 不會中止目前的執行緒，因此執行程式碼會繼續要求。 在非同步處理序，您必須從要停止執行程式碼的方法傳回工作。

在 MVC 專案中，您不應該呼叫 Response.Redirect。 相反地，傳回 RedirectResult。

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState 和 ViewStateMode

建議： 使用 ViewStateMode，而不是 EnableViewState，以提供更精確地控制哪些控制項使用檢視狀態。

當您設定為 false，Page 指示詞中的 EnableViewState 時，檢視狀態已停用頁面內的所有控制項，且無法啟用。 如果您想要啟用在網頁中某些控制項的檢視狀態，設定為停用 ViewStateMode 頁面。

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

然後，設定為 已啟用 ViewStateMode 實際上需要檢視狀態的控制項上。

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

藉由啟用需要它的控制項檢視狀態，您可以讓網頁的壓縮檢視狀態的大小。

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

建議： 請使用通用的提供者。

已被取代目前的專案範本，SqlMembershipProvider [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers)，這是以 NuGet 套件形式提供。 如果您使用 SqlMembershipProvider 使用較早版本的範本建置的專案中，您應該切換至 Universal Providers。 通用的提供者使用 Entity Framework 所支援的所有資料庫。

如需詳細資訊，請參閱 < [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx)。

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>長時間執行的要求 (> 110 秒)

建議： 使用[WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx)或是[SignalR](../../../signalr/index.md)連線的用戶端和使用非同步 I/O 作業。

長時間執行的要求可能會在您的 web 應用程式中造成無法預期的結果和效能不佳。 要求的預設逾時設定為 110 秒。 如果您使用工作階段狀態與長時間執行要求，ASP.NET 會 110 秒後發行的工作階段物件上的鎖定。 不過，您的應用程式可能正在工作階段物件上的作業時釋放鎖定，以及作業可能無法順利完成。 如果第一個要求正在執行時，會封鎖來自使用者的第二個要求，第二個要求可能需要存取工作階段物件不一致的狀態。

如果您的應用程式包含封鎖 （或非同步） I/O 作業，應用程式將會沒有回應。

為了改善效能，請在 .NET Framework 中使用非同步 I/O 作業。 此外，使用 WebSockets 或 SignalR 來連接到伺服器的用戶端。 這些功能被設計來有效率地處理長時間執行要求。
