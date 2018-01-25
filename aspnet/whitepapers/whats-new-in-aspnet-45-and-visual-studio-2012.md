---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: "新功能 ASP.NET 4.5 和 Visual Studio 2012 |Microsoft 文件"
author: rick-anderson
description: "本文件說明新功能和 ASP.NET 4.5 中引進的增強功能。 它也會描述針對 web 開發所進行的增強功能..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/29/2012
ms.topic: article
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 4487eb7436c0b6241505f41621a7f31b89c38b28
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>什麼是 ASP.NET 4.5 和 Visual Studio 2012 的新功能
====================
> 本文件說明新功能和 ASP.NET 4.5 中引進的增強功能。 它也會描述正在進行 Visual Studio 2012 中 web 程式開發的增強功能。 這份文件最初發行於 2012 年 2 月 29 日。


- [ASP.NET Core 執行階段和架構](#_Toc318097372)

    - [以非同步方式讀取和寫入 HTTP 要求和回應](#_Toc318097373)
    - [HttpRequest 處理的增強功能](#_Toc318097374)
    - [非同步清除回應](#_Toc318097375)
    - [支援*await*和*工作*為基礎的非同步模組和處理常式](#_Toc318097376)
    - [非同步 HTTP 模組](#_Toc318097377)
    - [非同步 HTTP 處理常式](#_Toc318097378)
    - [ASP.NET 要求驗證的新功能](#_Toc318097379)
    - [延遲 （「 延遲 」） 的要求驗證](#_Toc318097380)
    - [未驗證要求的支援](#_Toc318097381)
    - [AntiXSS 程式庫](#_Toc318097382)
    - [支援的 Websocket 通訊協定](#_Toc318097383)
    - [統合和縮製](#_Toc318097384)
    - [適用於虛擬主機的效能改進](#_Toc_perf)

        - [關鍵效能因素](#_Toc_perf_1)
        - [新的效能功能的需求](#_Toc_perf_2)
        - [共用通用的組件](#_Toc_perf_3)
        - [使用多核心 JIT 編譯的更快速啟動](#_Toc_perf_4)
        - [調整以最佳化記憶體回收](#_Toc_perf_5)
        - [預先擷取 web 應用程式](#_Toc_perf_6)
- [ASP.NET Web Form](#_Toc318097385)

    - [強型別資料控制項](#_Toc318097386)
    - [模型繫結](#_Toc318097387)

        - [選取資料](#_Toc318097388)
        - [值提供者](#_Toc318097389)
        - [篩選控制項中的值](#_Toc318097390)
    - [HTML 編碼的資料繫結運算式](#_Toc318097391)
    - [不顯眼的驗證](#_Toc318097392)
    - [HTML5 更新](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [ASP.NET Web Pages 2](#_Toc318097395)
- [Visual Studio 2012 發行候選版本](#_Toc318097396)

    - [Visual Studio 2010 和 Visual Studio 2012 發行候選版本 （專案相容性） 之間共用的專案](#project-compatibility)
    - [ASP.NET 4.5 的網站範本中的組態變更](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [在 IIS 7 進行 ASP.NET 路由中的原生支援](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [HTML 編輯器](#_Toc318097397)

        - [智慧工作提示](#_Toc318097398)
        - [WAI ARIA 支援](#_Toc318097399)
        - [新的 HTML5 程式碼片段](#_Toc318097400)
        - [擷取至使用者控制項](#_Toc318097401)
        - [屬性中的程式碼區塊的 IntelliSense](#_Toc318097402)
        - [當您重新命名的開頭或結尾標記時，比對標記的自動重新命名](#_Toc318097403)
        - [事件處理常式產生](#_Toc318097404)
        - [智慧型縮排](#_Toc318097405)
        - [自動減少陳述式完成](#_Toc318097406)
    - [JavaScript Editor](#_Toc318097407)

        - [程式碼大綱](#_Toc318097408)
        - [括號對稱](#_Toc318097409)
        - [移至定義](#_Toc318097410)
        - [ECMAScript5 支援](#_Toc318097411)
        - [DOM IntelliSense](#_Toc318097412)
        - [VSDOC 簽章的多載](#_Toc318097413)
        - [隱含參考](#_Toc318097414)
    - [CSS 編輯器](#_Toc318097415)

        - [自動減少陳述式完成](#_Toc318097416)
        - [階層式縮排。](#_Toc318097417)
        - [CSS 缺失支援](#_Toc318097418)
        - [廠商特定的結構描述 (-moz--，webkit)](#_Toc318097419)
        - [註解和取消的支援](#_Toc318097420)
        - [色彩選擇器](#_Toc318097421)
        - [程式碼片段](#_Toc318097422)
        - [自訂區域](#_Toc318097423)
    - [Page Inspector](#_Toc318097424)
    - [發行](#_Toc318097425)

        - [發行設定檔](#_Toc318097426)
        - [ASP.NET 先行編譯及合併](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [Disclaimer](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>ASP.NET Core 執行階段和架構

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>以非同步方式讀取和寫入 HTTP 要求和回應

ASP.NET 4 導入了能夠讀取資料流使用的 HTTP 要求實體*HttpRequest.GetBufferlessInputStream*方法。 這個方法會提供資料流存取要求的實體。 不過，它以同步方式執行，其中繫結起來，執行緒要求的持續時間。

ASP.NET 4.5 支援 HTTP 要求實體，以非同步方式上的資料流中讀取和非同步排清的能力。 ASP.NET 4.5 還提供雙重緩衝 HTTP 要求實體，可提供更容易整合具有下游的 HTTP 處理常式，例如.aspx 網頁處理常式和 ASP.NET MVC 控制站的能力。

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>HttpRequest 處理的增強功能

從 ASP.NET 4.5 所傳回的資料流參考*HttpRequest.GetBufferlessInputStream*支援同步和非同步讀取的方法。 *資料流*從傳回的物件*GetBufferlessInputStream*現在實作的 BeginRead 和 EndRead 方法。 非同步*資料流*方法可讓您以非同步方式讀取要求實體中的區塊，而 ASP.NET 釋放目前執行緒的非同步讀取迴圈的每個反覆項目之間。

ASP.NET 4.5 也加入附屬方法讀取要求實體的經緩衝處理的方式： *HttpRequest.GetBufferedInputStream*。 這個新的多載的運作方式類似*GetBufferlessInputStream*，支援同步和非同步讀取。 不過，因為它所讀取*GetBufferedInputStream*也將實體位元組複製到 ASP.NET 內部緩衝區，如此下游的模組和處理常式仍然可以存取要求的實體。 例如，如果某些上游管線中的程式碼已讀取要求實體使用*GetBufferedInputStream*，您仍然可以使用*HttpRequest.Form*或*HttpRequest.Files*. 這可讓您執行的非同步處理的要求 （例如，資料流處理大型檔案上的傳至資料庫），但仍執行的.aspx 網頁和 ASP.NET MVC 控制器之後。

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>非同步清除回應

傳送回應至 HTTP 用戶端可能需要相當長的時間，當用戶端很遠，或具有低頻寬連線。 通常 ASP.NET 應用程式所建立時緩衝處理回應位元組。 ASP.NET 然後會執行單一傳送作業的要求處理結尾處累加的緩衝區。

如果已緩衝的回應是大型 （例如，用戶端的大型檔案資料流處理），必須定期呼叫*HttpResponse.Flush*緩衝的輸出傳送至用戶端，並保留控制下的記憶體使用量。 不過，因為*排清*是同步的呼叫，並反覆地呼叫*排清*仍可能長時間執行要求期間使用執行緒。

ASP.NET 4.5 新增支援，如使用以非同步方式執行排清*BeginFlush*和*EndFlush*方法*HttpResponse*類別。 您可以使用這些方法，建立非同步的模組和累加地傳送資料到用戶端而不會佔用作業系統執行緒的非同步處理常式。 在中間*BeginFlush*和*EndFlush*呼叫，ASP.NET 釋放目前的執行緒。 這會大幅地減少以支援長時間執行的 HTTP 下載所需的作用中執行緒的總數。

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>支援*await*和*工作*為基礎的非同步模組和處理常式

.NET Framework 4 引進稱為 「 非同步程式設計概念*工作*。 工作由*工作*類型和相關的類型中*System.Threading.Tasks*命名空間。 .NET Framework 4.5 與編譯器搭配使用的增強功能建置這*工作*簡單的物件。 在.NET Framework 4.5，編譯器支援兩個新的關鍵字： *await*和*非同步*。 *Await*關鍵字是用以表示某段程式碼應該以非同步方式等候一段程式碼的語法縮寫。 *非同步*關鍵字表示提示可讓您將方法標記為以工作為基礎的非同步方法。

組合*await*，*非同步*，而*工作*物件可讓您在.NET 4.5 中撰寫非同步程式碼更容易。 ASP.NET 4.5 支援這些簡單化新應用程式開發介面可讓您撰寫非同步 HTTP 模組，並使用新編譯器增強功能的非同步 HTTP 處理常式。

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>非同步 HTTP 模組

假設您想要執行非同步工作傳回方法內*工作*物件。 下列程式碼範例會定義非同步方法，讓下載 Microsoft 首頁上的非同步呼叫。 請注意，使用*非同步*方法簽章中的關鍵字和*await*呼叫*DownloadStringTaskAsync*。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

這就是您所撰寫，.NET Framework 將會自動處理在等候下載完成，以及下載完成之後，自動還原呼叫堆疊時回溯呼叫堆疊。

現在，假設您想要使用非同步的 ASP.NET HTTP 模組中的這個非同步方法。 ASP.NET 4.5 包括 helper 方法 (*EventHandlerTaskAsyncHelper*) 和新的委派型別 (*TaskEventHandler*)，您可以使用與舊整合以工作為基礎的非同步方法將 ASP.NET HTTP 管線所公開的非同步程式設計模型。 這個範例會示範如何：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>非同步 HTTP 處理常式

在 ASP.NET 中撰寫非同步處理常式的傳統方法為實作*IHttpAsyncHandler*介面。 ASP.NET 4.5 引進了*HttpTaskAsyncHandler*非同步基底類型可以衍生自，使其更容易撰寫非同步處理常式。

*HttpTaskAsyncHandler*類型是抽象的會要求您覆寫*ProcessRequestAsync*方法。 ASP.NET 會在內部處理的整合傳回簽章 (*工作*物件) 的*ProcessRequestAsync*與較舊的非同步程式設計模型使用的 ASP.NET 管線。

下列範例示範如何使用*工作*和*await*為非同步的 HTTP 處理常式實作的一部分：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>ASP.NET 要求驗證的新功能

根據預設，ASP.NET 會執行要求驗證，它會檢查標記或指令碼中的欄位、 標頭、 cookie 和等等的要求。 如果偵測到任何時，ASP.NET 會擲回例外狀況。 這可以做為第一行防禦機制以防止潛在的跨網站指令碼的攻擊。

ASP.NET 4.5 輕鬆地選擇性地讀取未經驗證的要求資料。 ASP.NET 4.5 也整合熱門 AntiXSS 程式庫，原來外部程式庫。

開發人員經常能夠選擇性地關閉要求驗證其應用程式的要求。 比方說，如果您的應用程式論壇軟體，您可能想要允許使用者提交的 HTML 格式論壇文章和註解，但是仍然確定要求驗證會檢查所有其他項目。

ASP.NET 4.5 引進了兩項功能可簡化您選擇性地使用未經驗證的輸入： 延遲 （「 延遲 」） 的要求驗證和未經驗證的要求資料的存取權。

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>延遲 （「 延遲 」） 的要求驗證

在 ASP.NET 4.5，根據預設所有要求資料受都限於要求驗證。 不過，您可以設定應用程式，以延遲要求驗證，直到您實際存取要求的資料為止。 （這有時候稱為延遲要求驗證，根據等消極式載入某些資料案例中的詞彙。）您可以設定應用程式的 Web.config 檔案中使用延後的驗證，藉由設定*requestValidationMode*屬性中的 4.5 *httpRUntime*項目，如下列範例所示：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

當要求的驗證模式設定為 4.5 時，僅針對特定要求值，且只有在程式碼會存取該值時，才觸發要求驗證。 比方說，如果您的程式碼取得 Request.Form["forum 值\_張貼"]，僅針對表單集合中的元素叫用要求驗證。 沒有任何其他元素中*表單*集合會進行驗證。 在舊版 ASP.NET 中，是在整個要求集合觸發要求驗證已存取集合中的任何項目時。 新的行為可讓您更輕鬆地查看不同片段的要求資料，而不觸發要求驗證，其他部分的不同應用程式元件。

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>未驗證要求的支援

延後的要求驗證單獨做無法解決問題的選擇性地略過要求驗證。 呼叫 Request.Form["forum\_張貼"] 觸發程序仍要求驗證，針對該特定要求的值。 不過，您可能想要存取此欄位，而不觸發驗證，因為您想要讓該欄位中的標記。

若要允許這樣做，ASP.NET 4.5 現在支援未經驗證之的資料的存取權要求。 包含新的 ASP.NET 4.5 *Unvalidated*集合屬性中的*HttpRequest*類別。 這個集合會提供類似的存取權的所有要求資料的一般值*表單*， *QueryString*， *Cookie*，和*Url*。

使用論壇的範例，若要能夠讀取未經驗證的要求資料，您必須先設定應用程式使用新的要求驗證模式：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

然後您可以使用*HttpRequest.Unvalidated*讀取未驗證的表單值的屬性：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> 安全性-*請小心使用未經驗證的要求資料 ！* ASP.NET 4.5 中已加入的未經驗證的要求內容和以方便您存取非常特定未經驗證的要求資料的集合。 不過，您仍必須執行未經處理的要求資料，以確保危險的文字不會呈現使用者自訂的驗證。


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>AntiXSS 程式庫

由於 Microsoft AntiXSS 文件庫的普及，ASP.NET 4.5 現已加入從 4.0 版，該程式庫的核心編碼常式。

編碼常式由實作*AntiXssEncoder*在新的型別*System.Web.Security.AntiXss*命名空間。 您可以使用*AntiXssEncoder*直接藉由呼叫的任何靜態類型中實作的編碼方法的型別。 不過，使用新的反 XSS 常式的最簡單方法是設定 ASP.NET 應用程式使用*AntiXssEncoder*預設類別。 若要這樣做，請加入 Web.config 檔案中的下列屬性：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

當*encoderType*屬性設定為使用*AntiXssEncoder*所有型別，輸出的編碼方式在 ASP.NET 自動使用新的編碼常式。

這些是已納入 ASP.NET 4.5 外部 AntiXSS 文件庫的部分：

- *HtmlEncode*， *HtmlFormUrlEncode*，和*HtmlAttributeEncode*
- *XmlAttributeEncode*和*XmlEncode*
- *進行 urlencode 處理*和*UrlPathEncode* （新增）
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>支援的 Websocket 通訊協定

Websocket 通訊協定會定義如何建立透過 HTTP 的用戶端與伺服器之間的安全、 即時雙向通訊的標準為基礎網路通訊協定。 Microsoft 努力與 IETF 和 W3C 標準內文，以協助定義的通訊協定。 Websocket 通訊協定與投資大量資源的用戶端及行動作業系統上支援 Websocket 通訊協定的 Microsoft 支援的任何用戶端 （不只是瀏覽器使用）。

Websocket 通訊協定可讓您更容易建立用戶端與伺服器之間的長時間執行資料傳輸。 例如，寫入交談應用程式就能輕鬆因為您可以建立用戶端與伺服器之間，則為 true 的長時間執行連接。 您不必訴諸於定期輪詢或 HTTP 長期輪詢模擬通訊端的行為類似的因應措施。

ASP.NET 4.5 和 IIS 8 包含低階 WebSockets 的支援，讓 ASP.NET 開發人員使用受管理的應用程式開發介面進行以非同步方式讀取和寫入 WebSockets 物件上的字串和二進位資料。 針對 ASP.NET 4.5 沒有新*System.Web.WebSockets*包含型別，Websocket 通訊協定所使用的命名空間。

瀏覽器用戶端藉由建立 DOM 建立 Websocket 連接*WebSocket* ASP.NET 應用程式中，如下列範例所示的 URL 所指向的物件：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

您可以在 ASP.NET 中使用任何種類的模組或處理常式建立 WebSockets 端點。 在上述範例中，已使用.ashx 檔案，因為.ashx 檔案快速建立處理常式。

根據 Websocket 通訊協定，ASP.NET 應用程式會接受用戶端的 Websocket 要求，藉由指示要求應該升級從 HTTP GET 要求，為 Websocket 要求。 以下為範例：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

*AcceptWebSocketRequest*方法可接受函式委派，因為 ASP.NET 回溯目前 HTTP 要求，然後將控制項傳送至函式委派。 在概念上類似於您如何使用這種方法是*System.Threading.Thread*，您用來定義在背景中執行工作的執行緒啟動委派。

ASP.NET 和用戶端已順利完成 WebSockets 交握之後，ASP.NET 會呼叫您的委派，而且 WebSockets 應用程式開始執行。 下列程式碼範例示範使用內建的 Websocket 支援在 ASP.NET 中的簡單的回應應用程式：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

為.NET 4.5 中的支援*await*關鍵字和以工作為基礎的非同步作業適合自然撰寫 WebSockets 應用程式。 程式碼範例示範 Websocket 要求內 ASP.NET 完全以非同步方式執行。 應用程式會以非同步方式等候藉由呼叫從用戶端傳送訊息*await 通訊端。ReceiveAsync*。 同樣地，將非同步訊息用戶端藉由呼叫*await 通訊端。SendAsync*。

在瀏覽器中，應用程式接收透過 WebSockets 訊息*onmessage*函式。 若要從瀏覽器傳送訊息，請呼叫*傳送*方法*WebSocket* DOM 型別，如本範例所示：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

未來，我們可能會發行更新至這項功能，抽象離開部分低階的編碼也就需要在此版本中 websockets 應用程式。

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>統合及縮製

結合在一起，可讓您可以視為單一檔案的配套中結合個別的 JavaScript 和 CSS 檔案。 縮製總是 JavaScript 和 CSS 的檔案，藉由移除空白字元，則不需要其他字元。 這些功能是由 Web Form、 ASP.NET MVC 中，以及網頁所使用。

組合是使用組合類別或其中一個它的子類別、 組合和 StyleBundle 所建立的。 設定後之組合的執行個體，組合所提供的內送要求只將它加入全域 BundleCollection 執行個體。 在預設範本，套件組合的組態執行 BundleConfig 檔案中。 這個預設組態建立的所有核心指令碼和範本所使用的 css 檔案的組合。

從檢視內組合會使用幾種可能的 helper 方法的其中一個參考。 為了支援呈現不同的標記在偵錯與發行模式中的組合，為組合和 StyleBundle 類別有 helper 方法，呈現。 在偵錯模式中轉譯將會產生組合中的每個資源的標記。 在發行模式中轉譯將會產生組合的單一標記項目。 切換之間偵錯和發行模式可藉由修改 compilation 項目，在 web.config 中的偵錯屬性，如下所示：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

此外，啟用或停用最佳化可以直接透過 BundleTable.EnableOptimizations 屬性來設定。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

當檔案會集結時，它們會先依字母順序排序 (中顯示的方式**方案總管 中**)。 然後組織方式，讓已知的文件庫，以及其自訂延伸模組 （例如 jQuery、 MooTools 和 Dojo） 載入第一次。 例如，如上所示的指令碼資料夾組合的最終次序會是：

1. jquery-1.6.2.js
2. jquery-ui.js
3. jquery.tools.js
4. a.js

CSS 檔案是也依字母順序排序，然後重新組織，以便 reset.css 和 normalize.css 前面的任何其他檔案。 最終排序結合在一起的如上所示的 [Styles] 資料夾會是這樣：

1. reset.css
2. content.css
3. forms.css
4. globals.css
5. menu.css
6. styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>適用於虛擬主機的效能改進

.NET Framework 4.5 和 Windows 8 引進可協助您達成大幅提升效能的 web 伺服器工作負載的功能。 這包括減少 （最多 35%) 在這兩個啟動時間和記憶體耗用量的 web 主控使用 ASP.NET 的網站。

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>關鍵效能因素

在理想情況下，應使用的所有網站並在記憶體中以確保快速的下一個要求的回應，每當到達。 可能會影響網站的回應性的因素包括：

- 回收應用程式集區之後，重新啟動站台所需的時間。 這是不再記憶體的站台的組件時啟動站台 web 伺服器處理序所花費的時間。 （平台組件是仍在記憶體中，因為它們由其他站台）。這種情況稱為 「 冷網站，暖架構啟動 」 或只 「 冷網站啟動。 」
- 站台所佔據的記憶體數量。 這個詞彙是 「 每個網站的記憶體耗用量 」 或 「 非共用的工作集 」。

新的效能改進專注於這兩個因素。

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>新的效能功能的需求

新功能的需求可以分成下列類別：

- 執行.NET Framework 4 的增強功能。
- 改良功能，需要.NET Framework 4.5，但是可以在任何版本的 Windows 上執行。
- 只能搭配 Windows 8 上執行的.NET Framework 4.5 可用的增強功能。

效能會隨著的改進，您便能夠啟用每個層級。

某些.NET Framework 4.5 功能改進利用更廣泛適用於其他情況下的效能功能。

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>共用通用的組件

**需求**:.NET Framework 4 和 Visual Studio 11 開發人員預覽 SDK

不同的站台伺服器上通常會使用相同的協助程式組件 （例如，組件從入門套件或範例應用程式）。 每個站台會有自己的這些組件的複本，它的 Bin 目錄中。 即使組件的物件程式碼相同，它們是實體上不同的組件，因此每個組件必須個別讀取站台冷啟動期間，且分別保存在記憶體中。

實習的新功能可解決此效率不彰，並減少 RAM 需求和載入時間。 實習可讓 Windows 在檔案系統中，讓每個組件的單一複本和個別站台 Bin 資料夾中的組件會取代單一複本的符號連結。 如果個別站台需要不同版本的組件，符號連結取代為新版本的組件，並會影響這個站台。

共用組件使用的符號連結需要新的工具，名為 aspnet\_intern.exe，可讓您建立和管理組件已經保留的存放區。 它提供在 Visual Studio 11 開發人員預覽 SDK 的一部分。 (不過，它適用於具有只有安裝.NET Framework 4，假設您已安裝最新的系統[更新](https://support.microsoft.com/kb/2468871)。)

若要確保所有合格的組件有實習，您可以執行 aspnet\_intern.exe 定期 （例如每週一次當做排程工作）。 一般用法如下所示：

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

若要查看所有選項，執行不含引數的工具。

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>使用多核心 JIT 編譯的更快速啟動

**需求**:.NET Framework 4.5

為冷網站啟動時，組件沒有要讀取的磁碟，不僅站台必須是 JIT 編譯。 針對複雜的站台，這可以加入明顯的延遲。 .NET Framework 4.5 中新的一般用途技術可減少這些延遲分散到可用的處理器核心分配 JIT 編譯。 它會盡可能，並儘可能及早使用期間蒐集資訊先前啟動的站台。 這項功能所實作[System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx)方法。

JIT 編譯使用多個核心預設會啟用在 ASP.NET 中，因此您不需要執行任何動作來利用這項功能。 如果您想要停用此功能，請在 Web.config 檔案中進行下列設定：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>調整以最佳化記憶體回收

**需求**:.NET Framework 4.5

當站台執行時，其使用記憶體回收行程 (GC) 可以是堆積的其記憶體耗用量的重要因素。 任何記憶體回收行程，像是.NET Framework GC 讓 （頻率和精確度的集合） 的 CPU 時間和記憶體耗用量 （新的、 已釋放，或可以釋出的物件都會使用的額外空間） 之間的權衡取捨。 為了舊版本中，我們已提供指引，有關如何設定來達成的正確平衡 GC (例如，請參閱[ASP.NET 2.0/3.5 共用裝載組態](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration))。

.NET Framework 4.5，而不是多個獨立設定，工作負載定義的組態設定為可用，可讓所有的先前建議的 GC 設定，以及新的微調，以提供每個網站的其他效能工作集。

若要啟用 GC 記憶體微調，請先 Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config 檔案中加入下列設定：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(如果您是熟悉 aspnet.config 來變更先前的指引，請注意這項設定會取代舊的設定 — 比方說，不是需要設定 gcServer gcConcurrent、 等等。您不必移除舊的設定。）

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>預先擷取 web 應用程式

**需求**： 在 Windows 8 上執行的.NET Framework 4.5

Windows 具有數個版本，包含技術，又稱為[預先提取器](http://en.wikipedia.org/wiki/Prefetcher)，降低磁碟讀取的應用程式啟動。 因為冷啟動用戶端應用程式的主要問題，這項技術尚未包含在 Windows Server 中，包括 server 所必要的元件。 預先提取是立即可用的最新版本的 Windows Server，它可以在此最佳化在個別網站中的啟動。

適用於 Windows Server 預設不會啟用預先提取器。 若要啟用並設定預先提取器適用之高密度 web 主機，執行以下幾組命令在命令列：

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

然後，若要預先提取器整合 ASP.NET 應用程式中，加入下列 Web.config 檔案：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>ASP.NET Web Form

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>強型別的資料控制項

在 ASP.NET 4.5 Web Form 會包含一些改善使用資料。 改進的第一個是強型別的資料控制項。 在舊版的 ASP.NET Web Form 控制項，為您顯示資料繫結值，使用*Eval*和資料繫結運算式：

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

在您使用雙向資料繫結，*繫結*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

在執行階段，這些呼叫會使用反映以讀取的指定成員值，並在標記中顯示結果。 這種方法讓您更容易針對任意、 unshaped 資料的資料繫結。

不過，像這樣的資料繫結運算式不支援 IntelliSense 等功能的成員名稱、 瀏覽 （例如移至定義） 或在編譯時間檢查這些名稱。

若要解決此問題，ASP.NET 4.5，請加入的控制項繫結至資料的資料型別宣告的功能。 您使用新*ItemType*屬性。 當您設定這個屬性時，兩個新的型別的變數就可以使用資料繫結運算式的範圍：*項目*和*BindItem*。 因為變數強型別，您會取得 Visual Studio 程式開發體驗完整的優點。


針對雙向資料繫結運算式，使用*BindItem*變數：

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


ASP.NET Web Form framework 中大部分支援資料繫結的控制項已更新為支援*ItemType*屬性。

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>模型繫結

模型繫結延伸 ASP.NET Web Form 控制項中使用程式碼為重心資料存取的資料繫結。 其中包含從概念*ObjectDataSource*控制項與 ASP.NET MVC 中的模型繫結。

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>選取資料

若要設定要用來選取資料的模型繫結資料控制項，您將控制項的*SelectMethod*網頁的程式碼中的方法名稱的屬性。 資料控制項在頁面生命週期中的適當時間呼叫的方法，並自動繫結傳回的資料。 沒有不需要明確地呼叫*DataBind*方法。

在下列範例中， *GridView*控制項設定為使用名為的方法*GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

您建立*GetCategories*網頁的程式碼中的方法。 簡單的選取作業的方法不需要任何參數，而且應該傳回*IEnumerable*或*IQueryable*物件。 如果新*ItemType*屬性設定 (這可讓強型別資料繫結運算式，如底下所說明[強型別資料控制項](#_Toc318097386)稍早)，這些介面的泛型版本應該傳回 — *IEnumerable&lt;T&gt;* 或*IQueryable&lt;T&gt;*，與*T*比對的型別參數*ItemType*屬性 (例如， *IQueryable&lt;類別&gt;*)。

下列範例顯示的程式碼*GetCategories*方法。 此範例使用 Northwind 範例資料庫使用 Entity Framework Code First 模型。 程式碼可確保查詢會傳回詳細資料，藉由每個類別目錄的相關產品*Include*方法。 (這可確保*TemplateField*標記中的項目顯示每個類別目錄中的產品計數，而不需要[n + 1 選取](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem)。)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

當頁面執行，則*GridView*控制呼叫*GetCategories*方法自動呈現傳回的資料，使用已設定的欄位：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

因為選取的方法會傳回*IQueryable*物件*GridView*控制項可以進一步操作查詢，然後再執行它。 例如， *GridView*控制可以加入查詢運算式的排序和分頁所傳回*IQueryable*物件之前執行，讓這些作業都是透過基礎LINQ 提供者。 在此情況下，Entity Framework 可確保這些作業將會在資料庫中。

下列範例所示*GridView*修改成允許排序和分頁的控制項：

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

現在當執行網頁時，控制項可以確定會顯示資料的目前頁面，依所選資料行：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

若要篩選傳回的資料，參數必須加入至 select 方法。 模型繫結，在執行階段，將會填入這些參數，您可以使用它們來傳回資料之前修改查詢。

例如，假設您想要的查詢字串中輸入關鍵字來讓使用者篩選產品。 您可以將參數加入至方法及更新程式碼以使用參數值：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

這個程式碼包含*其中*運算式，如果提供值給*關鍵字*，然後傳回查詢結果。

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>值提供者

前一個範例不是特定之位置的值*關鍵字*參數來自。 若要指出這項資訊，您可以使用 參數屬性。 對於此範例中，您可以使用*QueryStringAttribute*中類別*System.Web.ModelBinding*命名空間：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

這會指示模型繫結至繫結的查詢字串中的值再試一次*關鍵字*在執行階段參數。 （這可能需要執行類型轉換，雖然它不在此情況下。）如果無法提供值的類型是不可為 null，則會擲回例外狀況。

這些方法的值的來源指值提供者，以及參數屬性，以指出要使用哪些值提供者指值提供者屬性。 Web Form 會在 Web Form 應用程式，例如查詢字串、 cookies、 表單值、 控制項、 檢視狀態、 工作階段狀態，以及設定檔屬性包含值提供者及所有的一般使用者輸入的來源對應屬性。 您也可以撰寫自訂值提供者。

根據預設，參數名稱用於做為索引鍵的值提供者集合中尋找的值。 在範例中，程式碼會尋找名為關鍵字的查詢字串值 (例如，~ / default.aspx?keyword=chef)。 您可以將它做為引數傳遞給參數屬性來指定自訂的索引鍵。 例如，若要使用名為 q 的查詢字串變數的值，您無法這樣做：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

如果此方法是在頁面的程式碼中，使用者可以藉由傳遞使用的查詢字串關鍵字篩選的結果：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

模型繫結會完成您原本必須以手動方式編碼的許多工作： 讀取的值、 檢查 null 值、 嘗試將它轉換成適當型別、 檢查轉換是否成功，以及最後，使用中的值查詢。 模型繫結的結果，最少的程式碼和重複使用的功能整個應用程式的能力。

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>篩選控制項中的值

假設您想要擴充範例以讓使用者從下拉式清單中選擇篩選值。 標記中加入下列下拉式清單，然後將它設定為從另一個使用的方法取得其資料*SelectMethod*屬性：

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

通常您也會將*EmptyDataTemplate*元素*GridView*控制，讓控制項將會顯示一則訊息，如果找不到任何相符的產品：

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

在網頁程式碼中，加入新的選取下拉式清單的方法：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

最後，更新*GetProducts*選取包含從下拉式清單選取類別目錄識別碼的新參數的方法：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

現在當執行網頁時，使用者可以從下拉式清單中，選取類別和*GridView*控制項是自動重新繫結以顯示篩選過的資料。 這可能是因為模型繫結會追蹤選取的方法參數的值，並偵測回傳之後是否已變更任何參數值。 如果是的話，則模型繫結會強制相關聯的資料控制項重新繫結至資料。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>HTML 編碼的資料繫結運算式

您可以立即進行 HTML 編碼的資料繫結運算式的結果。 加入冒號 （:） 的結尾&lt;%# 前置詞標示的資料繫結運算式：

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>不顯眼的驗證

您現在可以設定要使用不顯眼的 JavaScript 用戶端驗證邏輯的內建的驗證程式控制項。 這會大幅減少呈現內嵌在網頁標記中的 JavaScript，並減少整體的頁面大小。 您可以設定不顯眼的 JavaScript 驗證程式控制項的任何一種：

- 全域藉由新增下列設定加入 *&lt;appSettings&gt;*  Web.config 檔案中的項目： 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- 全域藉由設定靜態*System.Web.UI.ValidationSettings.UnobtrusiveValidationMode*屬性*UnobtrusiveValidationMode.WebForms* (通常在*應用程式\_啟動*Global.asax 檔中的方法)。
- 個別的頁面，藉由設定新*UnobtrusiveValidationMode*屬性*頁面*類別*UnobtrusiveValidationMode.WebForms*。

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>HTML5 更新

某些已改良 Web form 伺服器控制項，以充分利用新的 HTML5 功能：

- *TextMode*屬性*文字方塊*控制項已更新以支援新 HTML5 輸入的類型，例如*電子郵件*， *datetime*，和等等。
- *檔案上傳*控制現在支援多個檔案上傳支援這項功能 HTML5 的瀏覽器中。
- 驗證程式控制現在支援驗證 HTML5 的輸入項目。
- 新的 HTML5 項目具有屬性代表 URL 現在支援 runat ="server"。 如此一來，您可以使用 ASP.NET 慣例在 URL 路徑，例如 ~ 來表示應用程式根目錄運算子 (例如，&lt;視訊 runat ="server"src="~/myVideo.wmv"/&gt;)。
- *UpdatePanel*控制項問題已獲得修正，以支援張貼 HTML5 輸入的欄位。

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 Beta 已隨附於 Visual Studio 11 Beta。 ASP.NET MVC 是運用 「 模型檢視控制器 (MVC) 模式開發高度可測試且可維護的 Web 應用程式的架構。 ASP.NET MVC 4 可讓您更輕鬆地建立行動裝置的 Web 應用程式，並包含 ASP.NET Web API，可協助您建立可以連線到任何裝置的 HTTP 服務。 如需詳細資訊，請參閱[ASP.NET MVC 4 版本資訊](mvc4-release-notes.md)。

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>ASP.NET Web Pages 2

新功能包括下列各項：

- 新增和更新的網站範本。
- 加入伺服器端和用戶端驗證使用*驗證*協助程式。
- 若要註冊指令碼使用的資產管理員功能。
- 啟用從 Facebook 和其他站台使用 OAuth 和 OpenID 登入。
- 新增對應使用*對應*協助程式。
- 執行網頁應用程式的並存。
- 行動裝置的轉譯頁面。

如需有關這些功能和整頁的程式碼範例的詳細資訊，請參閱[Web Pages 2 beta 頂端功能](https://go.microsoft.com/fwlink/?LinkID=227824)。

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta

本節提供針對 web 程式開發，Visual Web Developer 11 beta 版，並且 Visual Studio 2012 發行候選版本的改進功能的相關資訊。

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Visual Studio 2010 和 Visual Studio 2012 發行候選版本 （專案相容性） 之間共用的專案

Visual Studio 2012 發行候選版本，直到在較新版的 Visual Studio 中開啟現有專案啟動轉換精靈。 這強制新的格式不相容的內容 （資產） 的專案及方案的升級。 因此，在轉換後您無法開啟專案在舊版的 Visual Studio 中。

許多客戶有告訴我們這不是正確的方法。 在 Visual Studio 11 Beta，我們現在支援共用專案和方案具有 Visual Studio 2010 SP1。 這表示，如果您在 Visual Studio 2012 發行候選版本中開啟 2010年專案，您將仍然能夠在 Visual Studio 2010 SP1 中開啟專案。

> [!NOTE]
> 幾個類型的專案無法在 Visual Studio 2010 SP1 和 Visual Studio 2012 發行候選版本之間共用。 包括一些較舊的專案 （例如 ASP.NET MVC 2 專案） 或專案 （例如安裝專案） 的特殊用途。

當您在 Visual Studio 11 Beta 中的第一次開啟 Visual Studio 2010 SP1 Web 專案時，專案檔會加入下列屬性：

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

升級專案檔的程序會使用 FileUpgradeFlags、 UpgradeBackupLocation 和 OldToolsVersion。 它們不會有影響使用 Visual Studio 2010 中的專案。

VisualStudioVersion 是 MSBuild 4.5，指出目前專案的 Visual Studio 版本所使用的新屬性。 因為這個屬性不存在於在 MSBuild 4.0 中 （Visual Studio 2010 SP1 使用的 MSBuild 版本），我們會將插入專案檔的預設值。

VSToolsPath 屬性用來決定要從表示 MSBuildExtensionsPath32 設定的路徑匯入正確的.targets 檔案中。

也有一些與匯入項目相關的變更。 為了支援這兩個版本的 Visual Studio 之間的相容性需要進行這些變更。

> [!NOTE]
> 如果專案 Visual Studio 2010 SP1 和 Visual Studio 11 Beta 之間兩個不同的電腦上共用，而且專案的應用程式中包含本機資料庫\_資料資料夾中，您必須確定 SQL server 資料庫所用的版本安裝在兩部電腦上。

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>ASP.NET 4.5 的網站範本中的組態變更

預設值來進行下列變更，就有*Web.config*網站使用網站範本在 Visual Studio 2012 發行候選版本中建立的檔案：

- 在`<httpRuntime>`項目，`encoderType`屬性現在會依預設設定為使用已加入至 ASP.NET AntiXSS 類型。 如需詳細資訊，請參閱[AntiXSS 文件庫](#_Toc318097382)。
- 此外，在`<httpRuntime>`項目，`requestValidationMode`屬性設為"4.5"。 這表示根據預設，設定要求驗證會使用延後 （「 延遲 」） 的驗證。 如需詳細資訊，請參閱[新 ASP.NET 要求驗證功能](#_Toc318097379)。
- `<modules>`元素`<system.webServer>`區段不包含`runAllManagedModulesForAllRequests`屬性。 （預設值為 false）。這表示如果您使用 IIS 7，尚未更新為 SP1 版本，您可能會有問題中新的站台的路由。 如需詳細資訊，請參閱[在 IIS 7 進行 ASP.NET 路由中的原生支援](#Native_Support_In_IIS7_For_ASPNET_Routine)。

這些變更不會影響現有的應用程式。 不過，它們可能代表現有的網站和您建立使用新範本的 ASP.NET 4.5 的新網站之間的行為差異。

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>在 IIS 7 進行 ASP.NET 路由中的原生支援

這不是 ASP.NET 的變更，但如果會影響您使用 IIS 7 未套用 SP1 更新版本的新網站專案範本中的變更。

在 ASP.NET 中，您可以將下列組態設定加入應用程式，才能支援路由：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

當**runAllManagedModulesForAllRequests**是 true，URL，例如`http://mysite/myapp/home`前往 ASP.NET，即使有沒有*.aspx*， *.mvc*，或類似的副檔名URL。

已對 IIS 7 的更新可讓**runAllManagedModulesForAllRequests**不必要的設定，並支援 ASP.NET 路由原生。 (如需更新的資訊，請參閱 Microsoft 支援文章[可更新功能可讓某些 IIS 7.0 或 IIS 7.5 處理常式來處理要求的 Url 不以句號結束](https://support.microsoft.com/kb/980368)。)

如果您的網站在 IIS 7 上執行，而且如果 IIS 已經更新，您不需要設定**runAllManagedModulesForAllRequests**為 true。 事實上，將它設定為 true 不建議，因為它會加入不必要處理負荷的要求。 這項設定為 true 時，所有要求，包括用於*.htm*， *.jpg*，和其他靜態檔案，也會經過 ASP.NET 要求管線。

如果您建立新的 ASP.NET 4.5 網站，使用 Visual Studio 2012 RC 中所提供的範本，為網站組態不包含**runAllManagedModulesForAllRequests**設定。 這表示根據預設設定為 false。

如果您之後執行的網站在 Windows 7 上未安裝 SP1，IIS 7 將不會包含所需的更新。 因此，路由將無法運作，您會看到錯誤。 如果您有問題，路由無法運作，您可以執行下列其中一個：

- 更新 Windows 7 SP1，會將更新新增至 IIS 7。
- 安裝先前所列的 Microsoft 支援文章所述的更新。
- 設定**runAllManagedModulesForAllRequests**設為 true，該網站的 Web.config 檔案中。 請注意，這會造成額外負擔的要求。

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>HTML 編輯器

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>智慧工作提示

在設計檢視中，通常為伺服器控制項的複雜屬性有關聯的對話方塊和精靈，可讓您輕鬆設定它們。 例如，您可以使用特殊的對話方塊加入至資料來源*中繼器*控制或將資料行加入*GridView*控制項。

不過，這種類型的複雜屬性 UI 說明尚未在來源檢視中使用。 因此，Visual Studio 11 導入智慧工作提示來源檢視。 智慧工作都在 C# 和 Visual Basic 編輯器中的常用功能的內容感知快速鍵。

ASP.NET Web Form 控制項的智慧工作提示時顯示在上伺服器標記為小型字符插入點位於項目內：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

當您按一下圖像，或按 CTRL +，則會展開智慧工作。 （點），如同在程式碼編輯器。 然後，它會顯示類似於在設計檢視中的智慧工作的捷徑。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

例如，在上圖中的智慧工作會示範 GridView 工作選項。 如果您選擇編輯資料行時，會顯示下列對話方塊：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

對話方塊會設定相同的屬性填入您可以設定在設計檢視中。 當您按一下 [確定] 時，控制項的標記會更新以新的設定：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>WAI ARIA 支援

撰寫可存取的網站變得越來越重要。 [WAI ARIA 協助工具標準](http://www.w3.org/WAI/intro/aria)定義開發人員撰寫可存取的網站的方式。 在 Visual Studio 現在完全支援這項標準。

例如，*角色*屬性現在會有完整的 IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

WAI ARIA 標準也導入了前面會加上的屬性*aria-* ，可讓您加入的 HTML5 文件的語意。 Visual Studio 也完全支援這些*aria-*屬性：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>新的 HTML5 程式碼片段

為了讓您更快速且更容易撰寫常用的 HTML5 標記，Visual Studio 會包含程式碼片段的數目。 範例是視訊的程式碼片段：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

要叫用程式碼片段，按下 Tab 鍵會在 IntelliSense 中選取的項目時，兩次：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

這會產生程式碼片段，您可以自訂。

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>擷取至使用者控制項

在大型網頁，它可以是不錯的想法，即可將個別項目移至使用者控制項。 這種形式的重構協助提升網頁的可讀性，並可簡化頁面結構。

若要進行簡化，當您編輯原始碼檢視中的 Web Form 網頁時，您可以立即在頁面中選取文字、 以滑鼠右鍵按一下，，然後選擇 擷取至使用者控制項：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>屬性中的程式碼區塊的 IntelliSense

Visual Studio 的伺服器端程式碼區塊中的任何頁面或控制項一律提供 IntelliSense。 現在 Visual Studio 會包含程式碼區塊，以及 HTML 屬性中的 IntelliSense。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

這可讓您更輕鬆地建立資料繫結運算式：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>當您重新命名的開頭或結尾標記時，比對標記的自動重新命名

如果您重新命名的 HTML 項目 (例如，您將變更*div*標記為*標頭*標記)，則對應的開啟或關閉標記也會變更即時。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

這有助於避免您忘了用來變更結尾標記，或變更錯誤的錯誤。

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>事件處理常式產生

Visual Studio 現在會包含可協助您撰寫事件處理常式，並手動將其繫結的來源檢視中的功能。 如果您編輯原始碼檢視中的事件名稱，IntelliSense 會顯示&lt;建立新的事件&gt;，這將會建立事件處理常式中網頁的程式碼，具有正確的簽章：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

根據預設，此事件處理常式將事件處理方法的名稱使用控制項的 ID:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

（在此情況下，在 C# 中)，產生的事件處理常式看起來像這樣：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>智慧型縮排

當您按下 Enter 空白 HTML 項目內時，編輯器會將插入點放在正確的位置：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

如果您按下 Enter，在這個位置，結尾標記會向下移動，並縮排到符合的開頭標記。 插入點也會縮排：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>自動減少陳述式完成

在 Visual Studio 現在篩選，根據您輸入的內容，使其顯示只有相關的選項中的 IntelliSense 清單：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense 也篩選依據的 IntelliSense 清單中的個別字首大寫。 例如，如果您輸入 「 dl"，則 dl 和 asp: DataList 將會顯示：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

此功能讓您更快取得的已知的項目陳述式完成。

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>JavaScript 編輯器

Visual Studio 2012 發行候選版本中的 JavaScript 編輯器是全新的它可大幅提升使用 Visual Studio 中的 JavaScript 的體驗。

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>程式碼大綱

大綱區域現在會自動建立的所有函式，讓您可以摺疊的檔案不是與您目前的焦點相關的組件。

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>括號對稱

當您將插入點放在左或右大括號上時，編輯器會反白顯示比對其中一個。

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>移至定義

移至定義命令可讓您跳到函式或變數的來源。

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>ECMAScript5 支援

編輯器支援 ECMAScript5，說明 JavaScript 語言的標準的最新版本的新語法和應用程式開發介面。

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>DOM IntelliSense

已改進 IntelliSense 的 DOM 應用程式開發介面，支援許多新 HTML5 應用程式開發介面包括*querySelector*，DOM 儲存體，跨文件訊息，和*畫布*。 單一的簡單 JavaScript 檔案，而非原生型別程式庫定義，現在是驅動 DOM IntelliSense。 這可讓您輕鬆地擴充或取代。

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>VSDOC 簽章的多載

詳細的 IntelliSense 註解可以現在為宣告不同的 JavaScript 函式多載使用新*&lt;簽章&gt;*項目，如這個範例所示：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>隱含參考

現在您可以將 JavaScript 檔案加入中央的清單會以隱含方式包含的檔案清單中任何特定的 JavaScript 檔案或區塊的參考，表示您可以使用 IntelliSense，為其內容。 比方說，您可以將 jQuery 檔案新增至中央檔案的清單，且您可以 IntelliSense jQuery 函式中的檔案，任何 JavaScript 區塊是否已明確參考 (使用 / /&lt;參考 /&gt;) 與否。

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>CSS 編輯器

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>自動減少陳述式完成

用於根據 CSS 屬性的 CSS 現在篩選與所選取的結構描述支援的值的 IntelliSense 清單。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense 也支援標題大小寫搜尋：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>階層式縮排

CSS 編輯器使用縮排顯示階層式的規則，讓您有階層式的規則邏輯的組織方式的概觀。 在下列範例中，#list 選取器是階層式清單的子系，因此縮排。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

下列範例會示範更複雜的繼承：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

規則的縮排是由其父系規則所決定。 依預設，會啟用階層式縮排，但您可以停用 [選項] 對話方塊 （工具，從功能表列的選項）：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>CSS 缺失支援

數百個真實世界的 CSS 檔案的分析顯示 CSS hack 是很常見，Visual Studio 現在支援最廣泛使用的項目。 這項支援包括 IntelliSense 和驗證的星號 (\*) 和底線 (\_) 駭客屬性：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

一般的選取器 hack 也支援階層式縮排維持即使它們會套用。 前面加上與選取器是用於目標 Internet Explorer 7 一般的選取器 hack  *\*： 第一個子系 + html*。 使用該規則將會維護階層式縮排：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>廠商特定的結構描述 (-moz-、-webkit)

CSS3 導入了許多屬性已實作不同的瀏覽器在不同的時間。 先前，這會強制特定瀏覽器的程式碼開發人員使用廠商特有的語法。 這些瀏覽器特有的屬性現在會包含在 IntelliSense 中。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>註解和取消的支援

您現在可以註解，並取消註解使用相同的快速鍵 （Ctrl + K、 註解的 C 和 Ctrl + K、 您取消註解） 程式碼編輯器中使用的 CSS 規則。

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>色彩選擇器

在舊版的 Visual Studio 中，IntelliSense 的色彩相關屬性所組成的命名的色彩值的下拉式清單。 該清單已取代的功能完整的色彩選擇器。

當您輸入色彩值時，色彩選擇器會自動顯示，並顯示先前使用後面接著預設色彩調色盤的色彩清單。 您可以選取色彩，使用滑鼠或鍵盤。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

清單可以展開到完整的色彩選擇器。 選擇器可讓您控制所移動的不透明度滑桿時，自動將任何色彩轉換成 RGBA alpha 色板：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>程式碼片段

CSS 編輯器中的程式碼片段讓您更輕鬆且快速地建立跨瀏覽器樣式。 需要特定的瀏覽器設定的許多 CSS3 屬性現在已回復到程式碼片段。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

CSS 程式碼片段支援進階的案例 （例如 CSS3 媒體查詢），輸入在符號 (@)，其中顯示 IntelliSense 清單。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

當您選取@media值並按下 Tab 鍵，CSS 編輯器會插入下列程式碼片段：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

如同程式碼的程式碼片段，您可以建立您自己的 CSS 程式碼片段。

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>自訂區域

名為程式碼區域，已有程式碼編輯器中，現在均可供編輯 CSS。 這可讓您輕鬆地群組相關的樣式區塊。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

區域摺疊時顯示區域的名稱：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Page Inspector

Page Inspector 是一種工具，呈現網頁 （HTML、 Web Form、 ASP.NET MVC 或 Web 網頁） 在 Visual Studio IDE，並可讓您檢查原始程式碼與產生的輸出。 適用於 ASP.NET 網頁，Page Inspector 可讓您判斷哪些伺服器端程式碼所產生的瀏覽器中呈現的 HTML 標記。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

如需有關 Page Inspector 的詳細資訊，請參閱下列教學課程：

- 使用 Page Inspector 中[ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- 使用 Page Inspector 中[ASP.NET Web Form](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>發佈

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>發行設定檔

在 Visual Studio 2010、 發行 Web 應用程式專案的資訊不會儲存在版本控制，而且不是與他人共用。 在 Visual Studio 2012 發行候選版本，將發行設定檔的格式已經變更。 已在進行團隊的成品，所以現在可以輕鬆地利用從 MSBuild 的組建。 組建組態資訊是在 [發行] 對話方塊中，讓您可以輕鬆切換發行前的組建組態。

發行設定檔會儲存在 PublishProfiles 資料夾。 資料夾的位置取決於您使用何種程式語言：

- C#: Properties\PublishProfiles
- 我 Project\PublishProfiles Visual Basic:

每個設定檔是 MSBuild 檔案。 在發佈期間，此檔案匯入專案的 MSBuild 檔案。 在 Visual Studio 2010 中，如果您想要變更的發行或封裝的程序，您必須在名為的檔案中放入您的自訂**ProjectName**。 wpp.targets。 仍然支援，但現在，您可以將您的自訂放在本身的發行設定檔中。 這樣一來，將只針對該設定檔使用自訂項目。

您可以現在也利用會發行從 MSBuild 的設定檔。 當您建置專案時，若要這樣做，請使用下列命令：

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

Project.csproj 值是專案的路徑和設定檔名稱是要發行設定檔的名稱。 或者，而不是傳遞的設定檔名稱*PublishProfile*屬性，您可以傳入的完整路徑的發行設定檔。

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>ASP.NET 先行編譯及合併

針對 Web 應用程式專案，Visual Studio 2012 發行候選版本會加入可讓您先行編譯，以及合併站台的內容，當您發行或封裝專案的 封裝/發行 Web 屬性頁面上的選項。 若要查看這些選項，以滑鼠右鍵按一下方案總管 中的專案，選擇 內容，然後選擇 封裝/發行 Web 屬性頁。 下圖顯示 Precompile 此應用程式，才能發行選項。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

選取此選項時，Visual Studio 會先行編譯的應用程式，每當您發行或封裝的 web 應用程式。 如果您想要控制如何先行編譯網站時，或如何合併組件，請按一下 [進階] 按鈕來設定這些選項。

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

在 Visual Studio 中的測試 web 專案的預設 web 伺服器現在為 IIS Express。 Visual Studio 程式開發伺服器仍在開發期間，本機 web 伺服器的選項，但 IIS Express 現在是建議的伺服器。 在 Visual Studio 11 Beta 中使用 IIS Express 的經驗是非常類似於在 Visual Studio 2010 SP1 中使用它。

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>免責聲明

這是一份初稿，內容在本文所述的軟體於正式商業發行前都可能有所更動。

本文件中的資訊表示直到文件發行日前 Microsoft Corporation 針對問題的看法。 Microsoft 必須因應不斷變化的市場狀況，因此本文件不代表 Microsoft 的保證，且 Microsoft 不保證這些資訊在文件發行後的正確性。

本技術白皮書僅供參考。 MICROSOFT 對本文件中的資訊不提供任何明示、暗示或法定擔保。

承諾遵守所有適用的著作權法是使用者的責任。 著作權法沒有針對某種權利加以限制，但在未獲得 Microsoft Corporation 書面同意的情況下，本文件的任何部分不得複製、以檢索系統存放或擷取、以任何形式或方法傳送 (電子、機械、影像複製、錄音或其他任何方法)、或基於任何其他不良意圖。

本文件所提及的主要事務，Microsoft 得擁有專利、專利應用程式、商標、著作權或其他智慧財產權。 除了 Microsoft 於授權合約書中書面提供的之外，本文件所述內容並未賦予您這些專利、商標、著作權、或其他智慧財產的任何授權或使用權利。

除非特別註明，否則本文件中所述，用來舉例之公司、組織、產品、網域名稱、電子郵件地址、標誌、人物、場所和事件皆為虛構，沒有意圖或不應該推斷為與任何真實存在的公司、組織、產品、網域名稱、電子郵件地址、標誌、人物、場所或事件有所關聯。

(C) 2012 Microsoft Corporation. 著作權所有，並保留一切權利。

Microsoft 和 Windows 是 Microsoft Corporation 在美國及/或其他國家/地區的註冊商標或商標。

本文件中所提實際公司和產品，可能為各所有人所有之商標。
