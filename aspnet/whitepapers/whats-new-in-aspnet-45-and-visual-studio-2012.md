---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: 什麼是 ASP.NET 4.5 和 Visual Studio 2012 的新功能 |Microsoft Docs
author: rick-anderson
description: 本文件說明新功能和 ASP.NET 4.5 中引進的增強功能。 此外，本文也將說明進行網頁程式開發的增強功能...
ms.author: aspnetcontent
ms.date: 02/29/2012
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: cfe9b1a7f05b43d5eb638c8fa7cb581d1edac9d5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835809"
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>在 ASP.NET 4.5 和 Visual Studio 2012 中最新消息
====================
> 本文件說明新功能和 ASP.NET 4.5 中引進的增強功能。 它也會說明適用於 Visual Studio 2012 中的 web 開發所做的改進。 這份文件最初發行於 2012 年 2 月 29 日。


- [ASP.NET Core 執行階段和架構](#_Toc318097372)

    - [以非同步方式讀取和寫入 HTTP 要求和回應](#_Toc318097373)
    - [HttpRequest 處理的增強功能](#_Toc318097374)
    - [以非同步方式排清回應](#_Toc318097375)
    - [支援*await*並*工作*-架構非同步模組和處理常式](#_Toc318097376)
    - [非同步 HTTP 模組](#_Toc318097377)
    - [非同步 HTTP 處理常式](#_Toc318097378)
    - [新的 ASP.NET 要求驗證功能](#_Toc318097379)
    - [延後 （「 延遲 」） 的要求驗證](#_Toc318097380)
    - [支援未經驗證的要求](#_Toc318097381)
    - [AntiXSS 程式庫](#_Toc318097382)
    - [支援 WebSockets 通訊協定](#_Toc318097383)
    - [統合和縮製](#_Toc318097384)
    - [適用於虛擬主機的效能改進](#_Toc_perf)

        - [關鍵效能因素](#_Toc_perf_1)
        - [如需新效能功能的需求](#_Toc_perf_2)
        - [共用通用的組件](#_Toc_perf_3)
        - [使用多核心 JIT 編譯的啟動速度更快](#_Toc_perf_4)
        - [調整記憶體回收的記憶體最佳化](#_Toc_perf_5)
        - [預先擷取 web 應用程式](#_Toc_perf_6)
- [ASP.NET Web Form](#_Toc318097385)

    - [強型別資料控制項](#_Toc318097386)
    - [模型繫結](#_Toc318097387)

        - [選取資料](#_Toc318097388)
        - [值提供者](#_Toc318097389)
        - [依控制項的值進行篩選](#_Toc318097390)
    - [HTML 編碼的資料繫結運算式](#_Toc318097391)
    - [低調驗證](#_Toc318097392)
    - [HTML5 更新](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [ASP.NET Web Pages 2](#_Toc318097395)
- [Visual Studio 2012 Release Candidate](#_Toc318097396)

    - [Visual Studio 2010 和 Visual Studio 2012 Release Candidate （專案相容性） 之間共用的專案](#project-compatibility)
    - [ASP.NET 4.5 網站範本中的組態變更](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [在 IIS 7 ASP.NET 路由中的原生支援](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [HTML 編輯器](#_Toc318097397)

        - [智慧工作](#_Toc318097398)
        - [等待 ARIA 支援](#_Toc318097399)
        - [新的 HTML5 片段](#_Toc318097400)
        - [擷取至使用者控制項](#_Toc318097401)
        - [在屬性中的程式碼區塊的 IntelliSense](#_Toc318097402)
        - [當您重新命名開頭或結尾標記相符的標記的自動重新命名](#_Toc318097403)
        - [產生事件處理常式](#_Toc318097404)
        - [智慧縮排](#_Toc318097405)
        - [自動減少陳述式完成](#_Toc318097406)
    - [JavaScript 編輯器](#_Toc318097407)

        - [程式碼大綱](#_Toc318097408)
        - [括號對稱](#_Toc318097409)
        - [移至定義](#_Toc318097410)
        - [支援 ECMAScript5](#_Toc318097411)
        - [DOM 的 IntelliSense](#_Toc318097412)
        - [VSDOC 簽章的多載](#_Toc318097413)
        - [隱含的參考](#_Toc318097414)
    - [CSS 編輯器](#_Toc318097415)

        - [自動減少陳述式完成](#_Toc318097416)
        - [階層式縮排。](#_Toc318097417)
        - [CSS 區隔設計支援](#_Toc318097418)
        - [廠商特定的結構描述 (-moz--，webkit)](#_Toc318097419)
        - [註解和取消註解的支援](#_Toc318097420)
        - [色彩選擇器](#_Toc318097421)
        - [程式碼片段](#_Toc318097422)
        - [自訂區域](#_Toc318097423)
    - [Page Inspector](#_Toc318097424)
    - [發行](#_Toc318097425)

        - [發行設定檔](#_Toc318097426)
        - [ASP.NET 先行編譯和合併](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [免責聲明](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>ASP.NET Core 執行階段和架構

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>以非同步方式讀取和寫入 HTTP 要求和回應

ASP.NET 4 引進了能夠讀取 HTTP 要求實體做為資料流，使用*HttpRequest.GetBufferlessInputStream*方法。 這個方法會提供資料流存取要求的實體。 不過，它以同步方式執行，其中繫結要求的持續期間的執行緒。

ASP.NET 4.5 支援能夠讀取資料流上 HTTP 要求實體中，以非同步方式，以及能夠以非同步方式排清。 ASP.NET 4.5 也提供雙重緩衝 HTTP 要求實體，可提供您更輕鬆整合使用下游的 HTTP 處理常式，例如.aspx 頁面處理常式和 ASP.NET MVC 控制站的能力。

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>HttpRequest 處理的增強功能

從 ASP.NET 4.5 所傳回的 Stream 參考*HttpRequest.GetBufferlessInputStream*支援同步和非同步讀取的方法。 *Stream*從傳回的物件*GetBufferlessInputStream*現在實作將 BeginRead 與 EndRead 方法。 非同步*Stream*方法可讓您以非同步方式讀取要求實體中的區塊，而 ASP.NET 會釋放目前執行緒的非同步讀取迴圈的每個反覆項目之間。

ASP.NET 4.5 也已新增方法以經過緩衝處理的方式讀取要求的實體： *HttpRequest.GetBufferedInputStream*。 這個新的多載的運作方式類似*GetBufferlessInputStream*，支援同步和非同步讀取。 不過，它會讀取，如*GetBufferedInputStream*也將實體位元組複製到 ASP.NET 內部緩衝區，以便下游的模組和處理常式仍然可以存取要求的實體。 例如，如果某些上游管線中的程式碼已讀取要求實體使用*GetBufferedInputStream*，您仍然可以使用*HttpRequest.Form*或*HttpRequest.Files*. 這可讓您執行的非同步處理的要求 （例如，資料流處理大型檔案上的傳至資料庫），但仍執行的.aspx 頁面和 ASP.NET MVC 控制站之後。

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>以非同步方式排清回應

傳送至 HTTP 用戶端的回應，可能需要相當長的時間，當用戶端位於遠處或低頻寬連線較。 在建立應用程式中，通常 ASP.NET 會緩衝處理回應位元組。 然後，ASP.NET 會執行單一傳送作業的每一端的要求處理應計的緩衝區。

如果已緩衝的回應很大 （例如，串流處理到用戶端的大型檔案） 的必須定期呼叫*HttpResponse.Flush*緩衝的輸出傳送至用戶端，而且將保留在控制下的記憶體使用量。 不過，因為*排清*是同步的呼叫，並反覆地呼叫*排清*仍會在執行緒耗用可能長時間執行要求的持續時間。

ASP.NET 4.5 新增了對支援使用以非同步方式執行排清*BeginFlush*並*EndFlush*方法*HttpResponse*類別。 您可以使用這些方法，建立非同步模組和非同步處理常式，但不會佔用作業系統執行緒，以累加方式將資料傳送至用戶端。 在中間*BeginFlush*並*EndFlush*呼叫，ASP.NET 會釋放目前的執行緒。 這可以大幅減少以支援長時間執行 HTTP 下載所需的作用中執行緒的總數。

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>支援*await*並*工作*-架構非同步模組和處理常式

.NET Framework 4 引進稱為 「 非同步程式設計概念*任務*。 工作由*任務*型別和中的相關型別*System.Threading.Tasks*命名空間。 .NET Framework 4.5 中建立這項以便於使用的編譯器強化*任務*簡單的物件。 在.NET Framework 4.5 中，編譯器都支援兩個新的關鍵字： *await*並*非同步*。 *Await*關鍵字是速記語法來表示，某段程式碼應該以非同步方式等候一段程式碼。 *非同步*關鍵字各表示可用來將方法標記為以工作為基礎的非同步方法的提示。

組合*await*，*非同步*，而*工作*物件可讓您在.NET 4.5 中撰寫非同步程式碼更容易。 ASP.NET 4.5 支援這些新的 Api，可讓您撰寫非同步 HTTP 模組，並使用新的編譯器強化的非同步 HTTP 處理常式與簡單化。

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>非同步 HTTP 模組

假設您想要執行非同步工作傳回方法內*任務*物件。 下列程式碼範例會定義非同步方法，可讓下載 Microsoft 首頁上的非同步呼叫。 請注意，使用*非同步*方法簽章中的關鍵字而*await*呼叫*DownloadStringTaskAsync*。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

這就是您必須撰寫 —.NET Framework 會自動處理時等待，才能完成下載，以及在下載完成後，自動還原在呼叫堆疊回溯呼叫堆疊。

現在，假設您想要使用非同步 ASP.NET HTTP 模組中的這個非同步方法。 ASP.NET 4.5 包含協助程式方法 (*EventHandlerTaskAsyncHelper*) 和新的委派型別 (*TaskEventHandler*)，您可以使用與舊版整合以工作為基礎的非同步方法ASP.NET HTTP 管線所公開的非同步程式設計模型。 此範例示範如何：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>非同步 HTTP 處理常式

在 ASP.NET 中撰寫非同步處理常式的傳統方法是實作*IHttpAsyncHandler*介面。 ASP.NET 4.5 引入*HttpTaskAsyncHandler*非同步基底型別，您可以從中衍生且可更輕鬆地撰寫非同步處理常式。

*HttpTaskAsyncHandler*型別是抽象的而且會要求您覆寫*ProcessRequestAsync*方法。 ASP.NET 在內部會負責將傳回的簽章的整合 (*任務*物件) 的*ProcessRequestAsync*與較舊的非同步程式設計模型使用 ASP.NET 管線。

下列範例示範如何使用*任務*並*await*非同步 HTTP 處理常式實作的一部分：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>新的 ASP.NET 要求驗證功能

根據預設，ASP.NET 會執行要求驗證，它會檢查要求，若要尋找的標記或指令碼中的欄位、 標頭、 cookie 和等等。 如果偵測到任何時，ASP.NET 會擲回例外狀況。 這可以做為第一道防禦潛在的跨網站指令碼的攻擊。

ASP.NET 4.5 可讓您更容易選擇性讀取未經驗證的要求資料。 ASP.NET 4.5 也整合了熱門 AntiXSS 程式庫，原來的外部程式庫。

開發人員必須能夠選擇性地關閉他們的應用程式的要求驗證的常見問題集。 比方說，如果您的應用程式論壇軟體，您可能想要允許使用者提交 HTML 格式的論壇文章和註解，但是仍然確定要求驗證會檢查所有其他項目。

ASP.NET 4.5 引進了兩項功能可簡化您選擇性地使用未經驗證的輸入： 延後 （「 延遲 」） 的要求驗證和未經驗證的要求資料的存取權。

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>延後 （「 延遲 」） 的要求驗證

在 ASP.NET 4.5 中，依預設所有要求資料受都限於要求驗證。 不過，您可以設定要延後要求驗證，直到您實際存取要求資料的應用程式。 （這有時候稱為延遲要求驗證，根據等詞彙消極式載入特定的資料案例。）您可以設定應用程式的 Web.config 檔案中使用延後的驗證，藉由設定*requestValidationMode*屬性中的 4.5 *httpRUntime*項目，如下列範例所示：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

當要求的驗證模式設定為 4.5 時，只會針對特定要求的值，且只有在您的程式碼會存取該值時，才將會觸發要求驗證。 例如，如果您的程式碼取得的值 Request.Form["forum\_張貼"]，只會針對該表單集合中的項目叫用要求驗證。 沒有任何其他元素在*表單*集合會進行驗證。 在舊版 ASP.NET 中，已在整個要求集合觸發要求驗證時存取集合中的任何項目。 新的行為更容易查看要求資料的不同片段，而不觸發要求驗證，在其他片段上的不同應用程式元件。

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>支援未經驗證的要求

延後的要求驗證單獨做無法解決問題，選擇性地略過要求驗證。 呼叫 Request.Form["forum\_張貼 「] 觸發程序仍要求驗證，針對該特定的要求值。 不過，您可能要存取這個欄位，而不觸發驗證，因為您想要允許在該欄位中的標記。

若要允許這種情況，ASP.NET 4.5 現在支援未經驗證的存取要求資料。 ASP.NET 4.5 包含新*Unvalidated*中的集合屬性*HttpRequest*類別。 這個集合提供存取所有常見的值，要求資料，例如*表單*， *QueryString*， *Cookie*，以及*Url*。

使用論壇範例中，若要能夠讀取未經驗證的要求資料，您必須先設定應用程式，以使用新的要求驗證模式：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

然後您可以使用*HttpRequest.Unvalidated*讀取未驗證的表單值的屬性：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> 安全性-*請小心使用未經驗證的要求資料 ！* ASP.NET 4.5 中已加入的未經驗證的要求屬性和集合，以讓您更輕鬆地存取非常特定的未驗證的要求資料。 不過，您仍必須執行未經處理的要求資料，以確保危險的文字不會呈現使用者自訂的驗證。


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>AntiXSS 程式庫

由於 Microsoft AntiXSS 程式庫的熱門程度，ASP.NET 4.5 現在會包含從 4.0 版，該程式庫的核心編碼常式。

編碼常式由實作*AntiXssEncoder*在新的型別*System.Web.Security.AntiXss*命名空間。 您可以使用*AntiXssEncoder*直接藉由呼叫的任何靜態型別中實作的編碼方法的型別。 不過，使用新的 ANTI-XSS 常式最簡單的方法是設定 ASP.NET 應用程式使用*AntiXssEncoder*預設類別。 若要這樣做，請在 Web.config 檔案中加入下列屬性：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

當*encoderType*屬性設定為使用*AntiXssEncoder*所有型別，輸出的編碼方式在 ASP.NET 中自動使用新的編碼常式。

以下是已併入 ASP.NET 4.5 的外部 AntiXSS 程式庫部分：

- *HtmlEncode*， *HtmlFormUrlEncode*，和*HtmlAttributeEncode*
- *XmlAttributeEncode*和*XmlEncode*
- *UrlEncode*並*UrlPathEncode* （新）
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>支援 WebSockets 通訊協定

WebSockets 通訊協定會定義如何建立透過 HTTP 的用戶端與伺服器之間的安全、 即時的雙向通訊的標準網路通訊協定。 Microsoft 已具有 IETF 和 W3C 標準內文，以協助定義通訊協定。 任何用戶端 （不只是瀏覽器），支援 WebSockets 通訊協定與 Microsoft 投入大量資源的用戶端和行動作業系統上支援 WebSockets 通訊協定。

WebSockets 通訊協定可讓您更容易建立用戶端與伺服器之間的長時間執行資料傳輸。 比方說，撰寫聊天應用程式就能輕鬆因為您可以建立用戶端與伺服器之間，則為 true 的長時間執行連線。 您不必訴諸於因應措施，例如定期輪詢或 HTTP 長時間輪詢來模擬通訊端的行為。

ASP.NET 4.5 和 IIS 8 包含低層級的 Websocket 支援，讓 ASP.NET 開發人員使用受管理的 Api 來以非同步方式讀取和寫入 WebSockets 物件上的 字串和二進位資料。 針對 ASP.NET 4.5，沒有新*System.Web.WebSockets*使用 WebSockets 通訊協定包含類型的命名空間。

瀏覽器用戶端建立 Websocket 連線，藉由建立 DOM *WebSocket*中 ASP.NET 應用程式，如下列範例所示的 URL 所指向的物件：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

您可以在 ASP.NET 中使用任何類型的模組或處理常式來建立 Websocket 端點。 在上述範例中，已使用.ashx 檔案，因為.ashx 檔案快速建立處理常式所示。

根據 Websocket 通訊協定，ASP.NET 應用程式會藉由指示要求應該升級從 HTTP GET 要求，為 Websocket 要求中接受用戶端的 Websocket 要求。 以下為範例：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

*AcceptWebSocketRequest*方法接受函數委派，因為 ASP.NET 會回溯目前 HTTP 要求，然後將控制權傳輸至函式委派。 在概念上類似於您如何使用這種方法是*System.Threading.Thread*，您可以在此定義的工作在背景中執行的執行緒開始委派。

ASP.NET 和用戶端已順利完成 Websocket 信號交換之後，ASP.NET 會呼叫您的委派和 WebSockets 的應用程式開始執行。 下列程式碼範例顯示一個簡單的回應應用程式，在 ASP.NET 中使用的內建的 Websocket 支援：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

在.NET 4.5 中的支援*await*關鍵字和以工作為基礎的非同步作業相當適合用來撰寫 WebSockets 的應用程式。 在程式碼範例示範的 Websocket 要求 ASP.NET 內完全以非同步方式執行。 在應用程式以非同步方式等候呼叫，從用戶端傳送的訊息*await 通訊端。ReceiveAsync*。 同樣地，傳送非同步訊息給用戶端藉由呼叫*await 通訊端。SendAsync*。

在瀏覽器中，應用程式會收到 Websocket 訊息*onmessage*函式。 若要從瀏覽器傳送訊息，請呼叫*傳送*方法*WebSocket* DOM 型別，在此範例中所示：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

在未來，我們可能會發行這項功能，抽象消失的低層級撰寫的程式碼也就一些所需在此版本中的 Websocket 應用程式的更新。

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>統合和縮製

統合可讓您將個別的 JavaScript 和 CSS 檔案合併成一個可以被視為單一檔案的配套。 縮製總是 JavaScript 和 CSS 檔案，藉由移除空白字元和其他不必要的字元。 這些功能是由 Web Form、 ASP.NET MVC 和 Web 網頁所使用。

套件組合會建立使用 Bundle 類別或其中一個子類別、 組合和 StyleBundle。 設定套件組合的執行個體之後, 組合是所提供的內送要求只將它新增至全域 BundleCollection 執行個體。 在預設範本中，套件組合設定被執行 BundleConfig 檔案中。 此預設設定建立的所有核心指令碼和範本所使用的 css 檔案的搭售方案。

從檢視內套組會使用幾個可能的 helper 方法的其中一個參考。 為了支援呈現不同的標記在偵錯與發行模式中的套件組合，組合和 StyleBundle 類別有 helper 方法會呈現。 在 偵錯模式中，轉譯會產生組合中的每個資源的標記。 在發行模式中轉譯會產生整個套件組合的單一標記項目。 切換之間偵錯和發行模式，可透過修改 compilation 項目，在 web.config 中的偵錯屬性，如下所示：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

此外，啟用或停用最佳化可以直接透過 BundleTable.EnableOptimizations 屬性來設定。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

當檔案會集結時，它們會先依字母順序排序 (中所顯示的方式**方案總管 中**)。 然後組織，讓已知的程式庫和其自訂的擴充功能 （例如 jQuery、 MooTools 和 Dojo） 會載入第一次。 比方說，會是最終的順序，統合的指令碼資料夾，如上所示：

1. jquery-1.6.2.js
2. jquery-ui.js
3. jquery.tools.js
4. a.js

CSS 檔案也會依字母順序排序，並再重新組織，以便 reset.css 和 normalize.css 前面的任何其他檔案。 最終排序的統合如上所示的 [Styles] 資料夾，將會是這樣：

1. reset.css
2. content.css
3. forms.css
4. globals.css
5. menu.css
6. styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>適用於虛擬主機的效能改進

.NET Framework 4.5 和 Windows 8 導入功能，可協助您達成大幅提升效能的 web 伺服器工作負載。 這包括減少 （最多 35%) 在這兩個啟動時間和 web 裝載使用 ASP.NET 的站台的記憶體使用量。

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>關鍵效能因素

在理想情況下，應使用的所有網站，而且在記憶體中以確保快速回應的下一個要求，每當其中。 可能會影響網站回應能力的因素包括：

- 應用程式集區回收之後重新啟動站台所需的時間。 這是啟動 web 伺服器處理序站台的站台的組件已不在記憶體中時所花費的時間。 （平台組件是仍在記憶體中，因為它們由其他站台）。這種情況稱為 「 冷的網站，framework 暖啟動 」 或只是 「 冷網站啟動。 」
- 站台會佔用的記憶體數量。 這個詞彙是 「 每個網站的記憶體耗用量 」 或 「 取消共用工作集 」。

新的效能改進專注於這兩個這些因素。

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>如需新效能功能的需求

新功能的需求可以分成下列類別：

- 在.NET Framework 4 執行的增強功能。
- 需要.NET Framework 4.5，但是可以在任何版本的 Windows 上執行的改進。
- 有只與 Windows 8 上執行的.NET Framework 4.5 的增強功能。

效能會隨著每個層級的改進措施，您就可以啟用。

某些.NET Framework 4.5 改善利用更廣泛套用至其他案例的效能功能。

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>共用通用的組件

**需求**:.NET Framework 4 和 Visual Studio 11 開發人員預覽 SDK

不同的站台伺服器上通常會使用相同的協助程式組件 （例如，組件從入門套件 」 或 「 範例應用程式）。 每個站台會有自己的這些組件的複本，它的 Bin 目錄中。 即使組件的物件程式碼完全相同，它們是實際分開的組件，因此每個組件具有個別讀取冷的站台啟動期間，並分開保留在記憶體中。

新的 interning 功能解決了此效率不彰，以及減少 RAM 需求和負載的時間。 暫留 」 可讓 Windows 檔案系統中保留一份每個組件，並在站台 Bin 資料夾中的個別組件會取代成單一複本的符號連結。 如果個別的網站需要不同版本的組件，符號連結取代了新版的組件，所以只有該站台會受到影響。

共用使用符號連結的組件需要新的工具，名為 aspnet\_intern.exe，可讓您建立及管理保留的組件的存放區。 它可在 Visual Studio 11 開發人員預覽 SDK 的一部分。 (不過，它會在具有只安裝.NET Framework 4，假設您已安裝最新的系統上運作[更新](https://support.microsoft.com/kb/2468871)。)

若要確定所有合格的組件已暫留，您可以執行 aspnet\_intern.exe 定期 （例如每週一次當做排程工作）。 典型的用法就是，如下所示：

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

若要查看所有選項，請使用任何引數執行工具。

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>使用多核心 JIT 編譯的啟動速度更快

**需求**:.NET Framework 4.5

冷的站台啟動，不只執行組件必須從磁碟讀取，但站台必須是 JIT 編譯。 針對複雜的站台，這可以新增明顯的延遲。 在.NET Framework 4.5 的新通用技術可減少這些延遲將分散在可用的處理器核心的 JIT 編譯。 它會盡可能並儘早使用期間所收集的資訊之前啟動站台。 藉由將此功能[System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx)方法。

JIT 編譯使用多個核心預設會啟用在 ASP.NET 中，因此您不需要採取任何動作來利用這項功能。 如果您想要停用此功能，請在 Web.config 檔案中進行下列設定：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>調整記憶體回收的記憶體最佳化

**需求**:.NET Framework 4.5

當站台執行時，其使用的記憶體回收行程 (GC) 堆積可以是它的記憶體耗用量的重要因素。 任何記憶體回收行程，例如.NET Framework GC 會讓 CPU 時間 （頻率和重要性的集合） 和記憶體耗用量 （用於新的、 已釋放，或可以釋放的物件的額外空格） 之間的權衡取捨。 舊版本中，我們提供指引有關如何設定以達到適當的平衡 GC (例如，請參閱[ASP.NET 2.0/3.5 共用裝載設定](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration))。

針對.NET Framework 4.5 中，而不是多個獨立設定，工作負載定義的組態設定，可讓所有先前建議的 GC 設定，以及新的微調，可提供額外的效能，每個網站工作集。

若要啟用調整的 GC 記憶體，請先 Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config 檔案中加入下列設定：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(如果您已熟悉 aspnet.config 的變更之前的指引，請注意，此設定會取代舊的設定 — 比方說，則不需要設定 Gcserver>、 gcConcurrent 等。您不必移除舊的設定。）

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>預先擷取 web 應用程式

**需求**: Windows 8 上執行的.NET Framework 4.5

Windows 具有數個版本，包含技術，稱為[預先擷取程式](http://en.wikipedia.org/wiki/Prefetcher)，降低磁碟讀取的應用程式啟動。 由於冷啟動是主要的用戶端應用程式的問題，這項技術尚未包含在 Windows Server 中，其中包含 server 所必要的元件。 預先提取是現在用於 Windows Server，它可以在此最佳化的個別網站推出的最新版本。

適用於 Windows Server 預設不會啟用預先擷取程式。 若要啟用並設定高密度 web 裝載的預先擷取程式，執行下列命令組在命令列：

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

然後，若要整合 ASP.NET 應用程式的預先擷取程式，加入下列 Web.config 檔案：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>ASP.NET Web Form

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>強型別的資料控制項

在 ASP.NET 4.5 Web Form 會包含一些改良，用於處理資料。 第一次的改進是強型別的資料控制項。 在舊版的 ASP.NET Web Form 控制項，為您顯示資料繫結值，使用*Eval*和資料繫結運算式：

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

在您使用雙向資料繫結，如*繫結*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

在執行階段，這些呼叫會使用反映來讀取指定成員的值，然後在標記中顯示結果。 這種方法可讓您更輕鬆地針對任意、 unshaped 資料的資料繫結。

不過，這類的資料繫結運算式不支援成員名稱、 瀏覽 （例如移至定義），或在編譯時間檢查這些名稱的功能，例如 IntelliSense。

若要解決此問題，ASP.NET 4.5 會新增的控制項繫結至資料的資料型別宣告的功能。 做法是使用新*ItemType*屬性。 當您設定這個屬性時，兩個新的具型別的的變數可在資料繫結運算式的範圍內：*項目*並*BindItem*。 因為變數強型別，您會取得之完整優勢的 Visual Studio 開發經驗。


如需雙向資料繫結運算式，使用*BindItem*變數：

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


ASP.NET Web Forms framework 中大部分支援資料繫結的控制項已更新為支援*ItemType*屬性。

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>模型繫結

模型繫結延伸 ASP.NET Web Form 控制項中使用程式碼為主的資料存取的資料繫結。 其中包含從概念*ObjectDataSource*控制項和 ASP.NET MVC 中的模型繫結。

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>選取資料

若要設定資料控制項使用模型繫結來選取資料，您將控制項的*SelectMethod*網頁的程式碼中的方法名稱的屬性。 資料控制項在網頁生命週期中的適當時間呼叫的方法，並自動將傳回的資料繫結。 不需要明確呼叫*DataBind*方法。

在下列範例中， *GridView*控制項設定為使用名為方法*GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

您建立*GetCategories*網頁的程式碼中的方法。 簡單的選取作業中，此方法不需要任何參數，並應該會傳回*IEnumerable*或是*IQueryable*物件。 如果新*ItemType*屬性設定 (可讓強型別資料繫結運算式，如底下所述[強型別資料控制項](#_Toc318097386)稍早)，這些介面的泛型版本應該傳回 — *IEnumerable&lt;T&gt;* 或是*IQueryable&lt;T&gt;*，使用*T*比對的型別參數*ItemType*屬性 (例如*IQueryable&lt;類別&gt;*)。

下列範例顯示的程式碼*GetCategories*方法。 此範例會使用 Northwind 範例資料庫中的 Entity Framework Code First 模型。 程式碼可確保查詢會傳回的每個類別藉由相關的產品詳細資料*Include*方法。 (這可確保*TemplateField*標記中的項目顯示每個類別中的產品計數，而不需要[n + 1 選取](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem)。)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

當頁面執行時，則*GridView*控制呼叫*GetCategories*方法自動並呈現傳回的資料，使用已設定的欄位：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

因為選取的方法會傳回*IQueryable*物件， *GridView*控制項可以進一步操作查詢，然後再執行它。 例如， *GridView*控制可以加入排序和分頁所傳回的查詢運算式*IQueryable*物件之前執行，使這些作業由基礎LINQ 提供者。 在此情況下，Entity Framework 可確保這些作業將會在資料庫中。

下列範例所示*GridView*修改成允許排序和分頁的控制項：

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

現在頁面執行時，控制項可以確定資料的目前頁面會顯示並依所選資料行：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

若要篩選傳回的資料，參數必須加入至 select 方法。 模型繫結，在執行階段，系統會填入這些參數，您可以使用它們來改變查詢之前傳回的資料。

例如，假設您想要的查詢字串中輸入關鍵字來讓使用者篩選產品。 您可以將參數加入至方法，並更新程式碼以使用參數值：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

這個程式碼包含*何處*運算式，如果提供的值是*關鍵字*，然後傳回查詢結果。

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>值提供者

前一個範例不是特定位置的值*關鍵字*參數來自。 若要指出這項資訊，您可以使用參數屬性。 針對此範例中，您可以使用*QueryStringAttribute*中的類別*System.Web.ModelBinding*命名空間：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

這會指示嘗試將值繫結至查詢字串中的模型繫結*關鍵字*在執行階段參數。 （這可能需要執行類型轉換，雖然它不會在此情況下。）如果無法提供的值，而且不可為 null 的型別，則會擲回例外狀況。

這些方法的值的來源指值提供者，並指出要使用哪些值提供者的參數屬性指值提供者屬性。 Web Form 會包含值提供者和所有一般來源，使用者輸入的對應屬性中的 Web Form 應用程式，例如查詢字串、 cookie、 表單值、 控制項、 檢視狀態、 工作階段狀態和設定檔屬性。 您也可以撰寫自訂值提供者。

根據預設，參數名稱用做為索引鍵來尋找值提供者集合中的值。 在範例中，程式碼會尋找名為關鍵字的查詢字串值 (例如，~ / default.aspx?keyword=chef)。 您可以指定自訂的索引鍵做為引數傳遞至參數屬性。 例如，若要使用名為 q 的查詢字串變數的值，您無法這樣做：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

如果這個方法是在頁面的程式碼中，使用者可以藉由傳遞關鍵字，使用查詢字串來篩選結果：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

模型繫結會完成許多工作，您原本必須以手動方式撰寫程式碼： 讀取的值、 檢查 null 值、 嘗試將它轉換成適當的型別、 檢查轉換是否成功，以及最後，使用中的值查詢。 模型繫結結果，最少的程式碼和重複使用的功能，在您的應用程式的能力。

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>依控制項的值進行篩選

假設您想要擴充範例，以讓使用者從下拉式清單中選擇篩選值。 將下列下拉式清單加入至標記，並設定它以取得其資料從另一個方法使用*SelectMethod*屬性：

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

通常您也可以加入*EmptyDataTemplate*項目*GridView*控制項，讓控制項將會顯示一則訊息，如果找不到任何相符的產品：

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

在頁面程式碼中，加入新的選取下拉式清單的方法：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

最後，更新*GetProducts*選取採用新的參數，其中包含從下拉式清單中選取的類別識別碼的方法：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

現在當頁面執行時，使用者可以從下拉式清單中，選取類別和*GridView*控制項是自動重新繫結以顯示篩選過的資料。 這可能是因為模型繫結會追蹤選取的方法參數的值，並偵測是否在回傳後，已變更任何參數值。 如果是這樣，模型繫結會強制重新繫結至資料相關聯的資料控制項。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>HTML 編碼的資料繫結運算式

您可以立即進行 HTML 編碼資料繫結運算式的結果。 結尾加上冒號 （:） &lt;%# 前置詞標示資料繫結運算式：

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>低調驗證

您現在可以設定要使用不顯眼的 JavaScript 用戶端驗證邏輯的內建的驗證程式控制項。 這會大幅減少的 JavaScript 內嵌在網頁標記中的轉譯，並減少整體的頁面大小。 您可以使用任何一種方式來設定不顯眼的 JavaScript 驗證程式控制項：

- 全域藉由新增下列設定加入*&lt;appSettings&gt;* Web.config 檔案中的項目： 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- 全域藉由設定靜態*System.Web.UI.ValidationSettings.UnobtrusiveValidationMode*屬性設*UnobtrusiveValidationMode.WebForms* (通常在*應用程式\_啟動*Global.asax 檔案中的方法)。
- 藉由設定新的頁面針對個別*UnobtrusiveValidationMode*屬性*頁*類別*UnobtrusiveValidationMode.WebForms*。

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>HTML5 更新

一些改善已對 Web Form 伺服器控制項，以充分利用的 HTML5 的新功能：

- *TextMode*屬性*TextBox*控制項已更新為支援新 HTML5 輸入的類型，例如*電子郵件*， *datetime*，及等等。
- *FileUpload*控制項現在支援多重檔案上傳的支援這個 HTML5 功能的瀏覽器。
- 驗證程式控制項現在支援驗證 HTML5 輸入項目。
- 新的 HTML5 項目具有表示屬性的 URL 現在支援 runat ="server"。 如此一來，您可以使用 ASP.NET 慣例在 URL 路徑，例如 ~ 來代表應用程式根目錄運算子 (例如&lt;視訊 runat ="server"src="~/myVideo.wmv"/&gt;)。
- *UpdatePanel*已修正以支援張貼的 HTML5 輸入的欄位的控制項。

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 beta 版現已隨附於 Visual Studio 11 beta 版。 ASP.NET MVC 是運用 「 模型-檢視-控制器 (MVC) 模式開發高度可測試且可維護的 Web 應用程式的架構。 ASP.NET MVC 4 可讓您更輕鬆建置行動 Web 應用程式，並包含 ASP.NET Web API，可協助您建置 HTTP 服務可以連線到任何裝置。 如需詳細資訊，請參閱 < [ASP.NET MVC 4 版本資訊](mvc4-release-notes.md)。

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>ASP.NET Web Pages 2

新功能包括下列各項：

- 新增和更新的網站範本。
- 新增伺服器端和用戶端驗證，使用*驗證*協助程式。
- 若要註冊指令碼使用資產管理員能力。
- 啟用登入和來自 Facebook 與使用 OAuth 和 OpenID 其他站台。
- 使用新增 mape*對應*協助程式。
- 執行網頁應用程式的並存。
- 行動裝置的轉譯頁面。

如需有關這些功能和整頁的程式碼範例的詳細資訊，請參閱 < [Web Pages 2 的 beta 版的最佳功能](https://go.microsoft.com/fwlink/?LinkID=227824)。

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 beta 版

本節提供在 Visual Web Developer 11 beta 版和 Visual Studio 2012 Release Candidate 中的 web 程式開發的增強功能相關資訊。

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Visual Studio 2010 和 Visual Studio 2012 Release Candidate （專案相容性） 之間共用的專案

Visual Studio 2012 Release Candidate，直到新版的 Visual Studio 中開啟現有的專案啟動轉換精靈。 這並不具有回溯相容性的新格式強制升級的專案和方案的內容 （資產）。 因此，轉換之後，您無法開啟專案在舊版的 Visual Studio 中。

許多客戶告訴我們這不是正確的方法。 在 Visual Studio 11 beta 版，我們現在支援共用的專案和解決方案，利用 Visual Studio 2010 SP1。 這表示，如果您在 Visual Studio 2012 發行候選版本中開啟 2010年專案，您仍然能夠在 Visual Studio 2010 SP1 中開啟專案。

> [!NOTE]
> Visual Studio 2010 SP1 和 Visual Studio 2012 Release Candidate 之間無法共用許多類型的專案。 這些包含一些較舊的專案 （例如 ASP.NET MVC 2 專案） 或專案 （例如安裝程式專案） 的特殊用途。

當您在 Visual Studio 11 Beta 中的第一次開啟 Visual Studio 2010 SP1 Web 專案時，專案檔會新增下列屬性：

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

升級專案檔的程序會使用 FileUpgradeFlags、 UpgradeBackupLocation 和 OldToolsVersion。 還不會影響到使用 Visual Studio 2010 中的專案。

VisualStudioVersion 是 MSBuild 4.5，指出目前專案的 Visual Studio 版本所使用的新屬性。 因為此屬性不存在於 MSBuild 4.0 （Visual Studio 2010 SP1 使用的 MSBuild 版本） 中，我們會將插入專案檔的預設值。

VSToolsPath 屬性用來判斷正確的.targets 檔案，才能從 MSBuildExtensionsPath32 設定所代表的路徑匯入。

另外還有一些匯入項目相關的變更。 為了支援這兩個版本的 Visual Studio 之間的相容性需要這些變更。

> [!NOTE]
> 如果專案 Visual Studio 2010 SP1 和 Visual Studio 11 beta 版之間兩個不同的電腦上共用，而且專案的應用程式中包含本機資料庫\_資料 資料夾中，您必須確定 SQL server 資料庫所使用的版本安裝在兩部電腦上。

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>ASP.NET 4.5 網站範本中的組態變更

已進行下列變更為預設值*Web.config*網站使用 Visual Studio 2012 Release Candidate 中的網站範本所建立的檔案：

- 在 `<httpRuntime>`項目，`encoderType`屬性現在已設定預設為使用 AntiXSS 類型已新增至 ASP.NET。 如需詳細資訊，請參閱 < [AntiXSS 程式庫](#_Toc318097382)。
- 此外，在`<httpRuntime>`項目，`requestValidationMode`屬性設為"4.5"。 這表示根據預設，設定要求驗證會使用延後 （「 延遲 」） 驗證。 如需詳細資訊，請參閱 <<c0> [ 新 ASP.NET 要求驗證功能](#_Toc318097379)。
- `<modules>`項目`<system.webServer>`一節不包含`runAllManagedModulesForAllRequests`屬性。 （預設值為 false）。這表示，如果您使用尚未更新為 SP1 的 IIS 7 的版本，您可能必須路由至新站台中的問題。 如需詳細資訊，請參閱 < [IIS 7 ASP.NET 路由中的原生支援](#Native_Support_In_IIS7_For_ASPNET_Routine)。

這些變更不會影響現有的應用程式。 不過，它們可能代表現有的網站和您建立使用新的範本的 ASP.NET 4.5 的新網站之間的行為差異。

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>在 IIS 7 ASP.NET 路由中的原生支援

這不是 ASP.NET 的變更，而可能影響您如果您正在 IIS 7 未套用 SP1 更新的版本的新網站專案範本中的變更。

在 ASP.NET 中，您可以將下列組態設定新增至應用程式，才能支援路由：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

時**runAllManagedModulesForAllRequests**是 true，之類的 URL`http://mysite/myapp/home`前往 ASP.NET 中，即使沒有任何 *.aspx*， *.mvc*，或在類似的延伸模組URL。

已對 IIS 7 的更新可讓**runAllManagedModulesForAllRequests**不必要的設定，並支援 ASP.NET 路由原生。 (如需更新的資訊，請參閱 Microsoft 支援服務文章[的更新功能，可讓特定 IIS 7.0 或 IIS 7.5 處理常式來處理要求的 Url 不以句號結束](https://support.microsoft.com/kb/980368)。)

如果您的網站在 IIS 7 上執行，而且如果 IIS 已經更新，您不需要設定**runAllManagedModulesForAllRequests**設為 true。 事實上，將它設定為 true 時不建議，因為它會增加不必要的處理負擔要求。 此設定為 true 時，所有的要求，包括 *.htm*， *.jpg*，和其他靜態檔案，也會經過 ASP.NET 要求管線。

如果您建立新的 ASP.NET 4.5 網站，使用 Visual Studio 2012 RC 中提供的範本時，網站組態不包含**runAllManagedModulesForAllRequests**設定。 這表示根據預設設定為 false。

如果您之後執行的網站在 Windows 7 上未安裝 SP1，IIS 7 不會包含所需的更新。 如此一來，路由將無法運作，且您會看到錯誤。 如果您有任何問題，路由不會無法運作，您可以執行下列其中一個：

- 更新 Windows 7 SP1，會將更新新增至 IIS 7。
- 在先前所列的 Microsoft 支援文章中，安裝所述的更新。
- 設定**runAllManagedModulesForAllRequests**設為 true，該網站的 Web.config 檔案中。 請注意，這會將一些額外負荷新增至要求。

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>HTML 編輯器

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>智慧工作

在 [設計] 檢視中，通常伺服器控制項的複雜屬性有相關的對話方塊和精靈，可讓您輕鬆地設定它們。 比方說，您可以使用特殊的對話方塊中，加入至資料來源*Repeater*控制或將資料行加入*GridView*控制項。

不過，這種類型的複雜屬性 UI 說明已在來源檢視中可用。 因此，Visual Studio 11 介紹智慧工作的來源檢視。 智慧工作是內容感知的捷徑，在 C# 和 Visual Basic 編輯器中的常用功能。

ASP.NET Web Form 控制項的智慧工作時，會顯示伺服器標記為小型的圖像 （glyph） 的插入點項目內：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

當您按一下圖像或按 CTRL +，則會展開 「 Smart Task 」。 （點），在程式碼編輯器中一樣。 然後，它會顯示類似於智慧工作，在 [設計] 檢視中的快速鍵。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

比方說，「 Smart Task 」 在上圖中顯示 [GridView 工作] 選項。 如果您選擇編輯資料行時，會顯示下列對話方塊：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

填寫對話方塊會設定相同的屬性。 您可以設定在設計檢視中。 當您按一下 [確定] 時，控制項的標記會更新以新的設定：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>等待 ARIA 支援

撰寫可存取的網站會變得越來越重要的作業。 [WAI ARIA 協助工具標準](http://www.w3.org/WAI/intro/aria)定義開發人員應該如何撰寫可存取的網站。 在 Visual Studio 現在完全支援這項標準。

例如，*角色*屬性現在會有完整的 IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

等待 ARIA 標準也導入了前面會加上的屬性*aria-* ，可讓您加入的 HTML5 文件中的語意。 Visual Studio 也完全支援這些*aria-* 屬性：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>新的 HTML5 片段

若要更快速且輕鬆地撰寫常用的 HTML5 標記，Visual Studio 會包含程式碼片段的數目。 以下是影片的程式碼片段範例：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

若要叫用程式碼片段，請按下 Tab 鍵會在 IntelliSense 中選取的項目時，兩次：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

這會產生您可以自訂的程式碼片段。

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>擷取至使用者控制項

在大型網頁中，它可以是個不錯的主意，將個別的項目移至使用者控制項。 這種形式的重構可協助提高網頁的可讀性，並可簡化頁面結構。

為了簡化起見，當您編輯原始碼檢視中的 Web Form 網頁時，您可以現在在頁面中選取文字，以滑鼠右鍵按一下它，，然後選擇 擷取至使用者控制項：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>在屬性中的程式碼區塊的 IntelliSense

Visual Studio 的伺服器端程式碼區塊中的任何頁面或控制項一律提供 IntelliSense。 現在 Visual Studio 包含程式碼區塊，以及 HTML 屬性中的 IntelliSense。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

這可讓您更輕鬆地建立資料繫結運算式：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>當您重新命名開頭或結尾標記相符的標記的自動重新命名

如果您重新命名的 HTML 項目 (比方說，您的變更*div*標記要*標頭*標記)，則相對應的開啟或關閉標記也會即時變更。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

這有助於避免您忘了用來變更結尾標記，或變更了錯誤的錯誤。

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>產生事件處理常式

Visual Studio 現在會包含可協助您撰寫事件處理常式，並以手動方式將它們繫結的來源檢視中的功能。 如果您要編輯原始碼檢視中的事件名稱，IntelliSense 會顯示&lt;建立新的事件&gt;，這會建立事件處理常式在頁面的程式碼中，具有正確的簽章：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

根據預設，事件處理常式會使用控制項的 ID，事件處理方法的名稱：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

（在此情況下，在 C# 中)，產生的事件處理常式看起來像這樣：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>智慧縮排

當您按 Enter，空的 HTML 項目內時，編輯器會將插入點放在正確的位置：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

如果您按下 Enter，在這個位置，結尾標記會向下移動，並縮排，以符合項目的開頭標記。 插入點也會縮排：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>自動減少陳述式完成

在 Visual Studio 目前篩選條件，根據您輸入的內容，以顯示只有相關的選項 [IntelliSense] 清單中：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense 篩選也根據標題大小寫的 IntelliSense 清單中的個別文字。 例如，如果您輸入"dl"，則 dl 和 asp: DataList 將會顯示：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

這項功能可讓更快取得的已知的項目陳述式完成。

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>JavaScript 編輯器

Visual Studio 2012 Release Candidate 中的 JavaScript 編輯器是第一次，它可大幅提升使用 Visual Studio 中的 JavaScript 的經驗。

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>程式碼大綱

大綱區域現在會自動建立的所有函式，可讓您摺疊的檔案不是與您目前的焦點相關的組件。

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>括號對稱

當您將插入點放在開頭或結尾大括號上時，編輯器會反白顯示相符的一個。

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>移至定義

[移至定義] 命令可讓您直接跳至函式或變數的來源。

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>支援 ECMAScript5

編輯器支援 ECMAScript5，說明 JavaScript 語言的標準的最新版本的新語法和 Api。

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>DOM 的 IntelliSense

已改善 IntelliSense DOM api，並支援許多新 HTML5 Api 包括*querySelector*，DOM 儲存、 跨文件訊息，並*畫布*。 單一的簡單 JavaScript 檔案，而不是由原生型別程式庫定義，現在驅動 DOM IntelliSense。 這可讓您輕鬆地擴充或取代。

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>VSDOC 簽章的多載

詳細的 IntelliSense 註解可以現在為宣告不同的 JavaScript 函式多載使用新*&lt;簽章&gt;* 項目，在此範例中所示：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>隱含的參考

您現在可以將 JavaScript 檔案，加入將會以隱含方式包含在檔案清單中任何特定的 JavaScript 檔案] 或 [封鎖的參考，這表示您會收到 IntelliSense，為其內容的集中清單。 比方說，您可以將 jQuery 檔案新增至中央檔案的清單，而得到 IntelliSense jQuery 函式中的檔案，任何 JavaScript 區塊是否已明確參考 (使用 / /&lt;參考 /&gt;) 與否。

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>CSS 編輯器

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>自動減少陳述式完成

IntelliSense 對 CSS 屬性為基礎的 CSS 現在篩選和選取的結構描述所支援的值清單。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense 也支援標題大小寫的搜尋：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>階層式縮排

CSS 編輯器會使用縮排顯示階層式的規則，讓您的階層式的規則以邏輯方式的組織方式概觀。 在下列範例中，#list 選取器是階層式清單的子系，因此縮排。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

下列範例會示範更複雜的繼承：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

規則的縮排是由其父代規則決定。 根據預設，啟用階層式縮排，但您可以停用 [選項] 對話方塊中 （工具，從功能表列的選項）：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>CSS 區隔設計支援

數百個真實世界的 CSS 檔案的分析顯示 CSS 區隔設計是很常見，，和 Visual Studio 現在支援最廣泛使用的項目。 這項支援包括 IntelliSense 和驗證的星號 (\*) 和底線 (\_) 屬性區隔設計：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

也支援一般的選取器駭客攻擊，如此即使套用維護階層式縮排。 目標 Internet Explorer 7 使用一般的選取器駭客是前面加上使用的選取器 *\*: nth-child(1 + html*。 使用該規則會維護階層式縮排：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>廠商特定的結構描述 (-moz-，-webkit)

CSS3 導入了許多由實作過不同瀏覽器在不同時間的屬性。 先前，這會強制開發人員可以針對特定瀏覽器的程式碼使用廠商特有的語法。 在 IntelliSense 中現在包含這些瀏覽器特定的屬性。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>註解和取消註解的支援

您現在可以註解，並使用相同的快速鍵 (Ctrl + K、 註解的 C 和 Ctrl + K、 您取消註解） 程式碼編輯器中使用的 CSS 規則取消註解。

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>色彩選擇器

在舊版的 Visual Studio 中，IntelliSense 的色彩相關的屬性包含具名的色彩值的下拉式清單。 該清單已取代的功能完整的色彩選擇器。

當您輸入的色彩值時，色彩選擇器會自動顯示，並提供先前使用後面接著預設色彩調色盤的色彩的清單。 您可以選取色彩，使用滑鼠或鍵盤。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

到完整的色彩選擇器，即可展開清單。 選擇器可讓您控制 alpha 色頻所移動的不透明度滑桿時，會自動將任何色彩轉換成 RGBA:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>程式碼片段

CSS 編輯器中的程式碼片段可讓您更輕鬆且快速地建立跨瀏覽器樣式。 需要特定的瀏覽器設定的許多 CSS3 屬性現在已回復到程式碼片段。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

CSS 程式碼片段支援進階的案例中 （例如 CSS3 媒體查詢） 輸入 at 符號 (@)，這會顯示 IntelliSense 清單。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

當您選取@media值並按下 Tab 鍵，CSS 編輯器會插入下列程式碼片段：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

如同程式碼的程式碼片段，您可以建立您自己的 CSS 程式碼片段。

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>自訂區域

名為程式碼區域，已在程式碼編輯器中，已可供編輯 CSS。 這可讓您輕鬆地為群組相關的樣式區塊。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

摺疊的區域時它會顯示區域的名稱：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Page Inspector

Page Inspector 是一種工具可呈現在 Visual Studio IDE 中的 web 頁面 （HTML、 Web Form、 ASP.NET MVC 或 Web 網頁），並可讓您檢查原始程式碼和產生的輸出。 適用於 ASP.NET 網頁，Page Inspector 可讓您判斷哪些伺服器端程式碼所產生的 HTML 標記呈現至瀏覽器。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

如需有關 Page Inspector 的詳細資訊，請參閱下列教學課程：

- 使用中的 Page Inspector [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- 使用中的 Page Inspector [ASP.NET Web Form](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>發佈

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>發行設定檔

在 Visual Studio 2010 中，針對 Web 應用程式專案的發佈資訊不會儲存在版本控制，而且不是針對與其他人共用。 Visual Studio 2012 發行候選版本中，發行設定檔的格式已經變更。 已在進行小組的成品，並就現在可以輕鬆地利用的 MSBuild 為基礎的組建。 組建組態資訊都位於 [發行] 對話方塊中，以便您可以輕鬆地切換之前發行的組建組態。

發行設定檔會儲存在 [PublishProfiles] 資料夾。 資料夾的位置取決於您使用何種程式設計語言：

- C#: Properties\PublishProfiles
- 我 Project\PublishProfiles Visual Basic:

每個設定檔是 MSBuild 檔案。 期間發行，此檔案會匯入專案的 MSBuild 檔案。 在 Visual Studio 2010 中，如果您想要變更的發行或封裝的程序，您必須將您的自訂項目放在名為**ProjectName**.wpp.targets。 仍支援這樣設定，但現在，您可以將您的自訂項目放在本身的發行設定檔中。 如此一來，您將只為該設定檔使用自訂項目。

您可以現在也利用會發行從 MSBuild 的設定檔。 若要這樣做，請使用下列命令，當您建置專案：

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

Project.csproj 值是路徑的專案，而且 ProfileName 是發行設定檔的名稱。 或者，而不是傳遞的設定檔名稱*PublishProfile*屬性，您可以傳入的完整路徑的發行設定檔。

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>ASP.NET 先行編譯和合併

Web 應用程式專案的 Visual Studio 2012 Release Candidate 將加入 封裝/發行 Web 屬性頁可讓您先行編譯，以及合併站台的內容，當您發行或封裝的專案上的選項。 若要查看這些選項，以滑鼠右鍵按一下方案總管 中的專案，選擇 內容，然後選擇 封裝/發行 Web 屬性頁。 下圖顯示預先編譯此應用程式，再發行選項。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

選取此選項時，Visual Studio 會預先編譯的應用程式，每當您發行或封裝的 web 應用程式。 如果您想要控制如何先行編譯網站時，或如何合併組件，請按一下 [進階] 按鈕來設定這些選項。

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

Visual Studio 中測試的 web 專案的預設 web 伺服器現在是 IIS Express。 Visual Studio 程式開發伺服器仍在開發期間，本機 web 伺服器的選項，但 IIS Express 現在是建議的伺服器。 使用 Visual Studio 11 Beta 中的 IIS Express 的經驗是非常類似於在 Visual Studio 2010 SP1 中使用它。

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>免責聲明

這是一份初稿，內容在本文所述的軟體於正式商業發行前都可能有所更動。

本文件中的資訊表示直到文件發行日前 Microsoft Corporation 針對問題的看法。 Microsoft 必須因應不斷變化的市場狀況，因此本文件不代表 Microsoft 的保證，且 Microsoft 不保證這些資訊在文件發行後的正確性。

本技術白皮書僅供參考。 MICROSOFT 對本文件中的資訊不提供任何明示、暗示或法定擔保。

承諾遵守所有適用的著作權法是使用者的責任。 著作權法沒有針對某種權利加以限制，但在未獲得 Microsoft Corporation 書面同意的情況下，本文件的任何部分不得複製、以檢索系統存放或擷取、以任何形式或方法傳送 (電子、機械、影像複製、錄音或其他任何方法)、或基於任何其他不良意圖。

本文件所提及的主要事務，Microsoft 得擁有專利、專利應用程式、商標、著作權或其他智慧財產權。 除了 Microsoft 於授權合約書中書面提供的之外，本文件所述內容並未賦予您這些專利、商標、著作權、或其他智慧財產的任何授權或使用權利。

除非另有說明，範例公司、 組織、 產品、 網域名稱、 電子郵件地址、 標誌、 人物、 地點及事件本文所述之屬虛構，以及與任何真實公司、 組織、 產品、 網域名稱、 電子郵件沒有關聯地址、 標誌、 人員、 位置或事件是純屬巧合。

(C) 2012 Microsoft Corporation. 著作權所有，並保留一切權利。

Microsoft 和 Windows 是 Microsoft Corporation 在美國及/或其他國家/地區的註冊商標或商標。

本文件中所提實際公司和產品，可能為各所有人所有之商標。
