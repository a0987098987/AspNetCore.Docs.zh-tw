---
uid: whitepapers/aspnet4/overview
title: ASP.NET 4 和 Visual Studio 2010 網頁程式開發概觀 |Microsoft Docs
author: rick-anderson
description: 本文件會包含在.net Framework 4 和 Visual Studio 2010 中的 asp.net 提供許多新功能的概觀。
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 5c7aa95b18bc0a97f42cc981476c110830286fa5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829039"
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>ASP.NET 4 和 Visual Studio 2010 網頁程式開發概觀
====================
> 本文件會包含在.net Framework 4 和 Visual Studio 2010 中的 asp.net 提供許多新功能的概觀。
> 
> [下載此白皮書](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


**內容**

**[Core Services](#0.2__Toc253429238 "_Toc253429238")**  
[Web.config 檔案重構](#0.2__Toc253429239 "_Toc253429239")  
[可延伸的輸出快取](#0.2__Toc253429240 "_Toc253429240")  
[自動啟動 Web 應用程式](#0.2__Toc253429241 "_Toc253429241")  
[永久重新導向頁面](#0.2__Toc253429242 "_Toc253429242")  
[壓縮工作階段狀態](#0.2__Toc253429243 "_Toc253429243")  
[展開的可允許的 Url 範圍](#0.2__Toc253429244 "_Toc253429244")  
[可延伸的要求驗證](#0.2__Toc253429245 "_Toc253429245")  
[物件快取物件和快取的擴充性](#0.2__Toc253429246 "_Toc253429246")  
[可延伸的 HTML、 URL 和 HTTP 標頭的編碼方式](#0.2__Toc253429247 "_Toc253429247")  
[在單一背景工作處理序中的個別應用程式的效能監視](#0.2__Toc253429248 "_Toc253429248")  
[Multi-Targeting](#0.2__Toc253429249 "_Toc253429249")

**[Ajax](#0.2__Toc253429250 "_Toc253429250")**  
[包括在 Web Form 和 MVC 的 jQuery](#0.2__Toc253429251 "_Toc253429251")  
[內容傳遞網路支援](#0.2__Toc253429252 "_Toc253429252")  
[ScriptManager 明確指令碼](#0.2__Toc253429253 "_Toc253429253")

**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**  
[設定使用 Page.MetaKeywords 和 Page.MetaDescription 屬性的中繼標籤](#0.2__Toc253429257 "_Toc253429257")  
[啟用個別的控制項檢視狀態](#0.2__Toc253429258 "_Toc253429258")  
[變更瀏覽器能力](#0.2__Toc253429259 "_Toc253429259")  
[ASP.NET 4 中的路由](#0.2__Toc253429260 "_Toc253429260")  
[設定用戶端識別碼](#0.2__Toc253429261 "_Toc253429261")  
[資料控制項中選取的保存資料列](#0.2__Toc253429262 "_Toc253429262")  
[ASP.NET 的 Chart 控制項](#0.2__Toc253429263 "_Toc253429263")  
[篩選與 QueryExtender 控制項的資料](#0.2__Toc253429264 "_Toc253429264")  
[Html 編碼的程式碼運算式](#0.2__Toc253429265 "_Toc253429265")  
[專案範本變更](#0.2__Toc253429266 "_Toc253429266")  
[CSS 改良](#0.2__Toc253429267 "_Toc253429267")  
[隱藏 div 周圍隱藏欄位的項目](#0.2__Toc253429268 "_Toc253429268")  
[樣板化控制項呈現的外部資料表](#0.2__Toc253429269 "_Toc253429269")  
[ListView 控制項的增強功能](#0.2__Toc253429270 "_Toc253429270")  
[CheckBoxList 和 RadioButtonList 控制項的增強功能](#0.2__Toc253429271 "_Toc253429271")  
[功能表控制項改良](#0.2__Toc253429272 "_Toc253429272")  
[精靈和 CreateUserWizard 控制項 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[區域支援](#0.2__Toc253429275 "_Toc253429275")  
[資料註解屬性驗證支援](#0.2__Toc253429276 "_Toc253429276")  
[樣板化 Helper](#0.2__Toc253429277 "_Toc253429277")

**[Dynamic Data](#0.2__Toc253429278 "_Toc253429278")**  
[對於現有的專案中啟用動態資料](#0.2__Toc253429279 "_Toc253429279")  
[宣告式 DynamicDataManager 控制項語法](#0.2__Toc253429280 "_Toc253429280")  
[實體範本](#0.2__Toc253429281 "_Toc253429281")  
[新的欄位範本 Url 和電子郵件地址](#0.2__Toc253429282 "_Toc253429282")  
[建立與 DynamicHyperLink 控制項的連結](#0.2__Toc253429283 "_Toc253429283")  
[支援的資料模型中的繼承](#0.2__Toc253429284 "_Toc253429284")  
[支援多對多關聯性 (Entity Framework)](#0.2__Toc253429285 "_Toc253429285")  
[新的屬性，以控制顯示，並支援列舉](#0.2__Toc253429286 "_Toc253429286")  
[增強的篩選器支援](#0.2__Toc253429287 "_Toc253429287")

**[Visual Studio 2010 Web 開發改善](#0.2__Toc253429288 "_Toc253429288")**  
[改良 CSS 相容性](#0.2__Toc253429289 "_Toc253429289")  
[HTML 和 JavaScript 程式碼片段](#0.2__Toc253429290 "_Toc253429290")  
[JavaScript IntelliSense 的增強功能](#0.2__Toc253429291 "_Toc253429291")

**[Web 與 Visual Studio 2010 的應用程式部署](#0.2__Toc253429292 "_Toc253429292")**  
[Web 封裝](#0.2__Toc253429293 "_Toc253429293")  
[Web.config Transformation](#0.2__Toc253429294 "_Toc253429294")  
[資料庫部署](#0.2__Toc253429295 "_Toc253429295")  
[單鍵發行為 Web 應用程式](#0.2__Toc253429296 "_Toc253429296")  
[Resources](#0.2__Toc253429297 "_Toc253429297")

**[Disclaimer](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>核心服務

ASP.NET 4 導入了一些改善核心 ASP.NET 服務，例如輸出快取和工作階段狀態儲存體的功能。

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>重構的 Web.config 檔案

`Web.config`檔案，其中包含組態，如已加入新的功能，例如 Ajax，移轉過去的幾個版本的.NET framework 的 Web 應用程式已經大幅變得的路由，以及與 IIS 7 整合。 這使得它難設定，或啟動新的 Web 應用程式，而不需要 Visual Studio 之類的工具。 在蒐羅 NET Framework 4 中，主要的組態項目都已移至`machine.config`檔案和應用程式現在會繼承這些設定。 這可讓`Web.config`為空白或包含只是下列幾行，後者會指定適用於 Visual Studio 應用程式的目標 framework 版本的 ASP.NET 4 應用程式中的檔案：

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>可延伸的輸出快取

自 ASP.NET 1.0 發行時，輸出快取已啟用開發人員在記憶體中儲存的頁面、 控制項和 HTTP 回應所產生的輸出。 在後續 Web 要求，ASP.NET 可以提供內容更快速地從記憶體，而不重新產生的輸出，從零開始擷取產生的輸出。 不過，此方法有一項限制，產生的內容一律儲存在記憶體中，而且發生高流量的伺服器，在輸出快取所耗用的記憶體可以從 Web 應用程式的其他部分的記憶體需求與競爭。

ASP.NET 4 中加入的擴充點的輸出快取，可讓您設定一或多個自訂輸出快取提供者。 輸出快取提供者可以使用任何儲存機制，來保存的 HTML 內容。 這讓您能夠建立自訂輸出快取提供者，針對不同的持續性機制，包括本機或遠端磁碟，雲端儲存體，以及分散式快取引擎。

您可以建立自訂輸出快取提供者做為衍生自新的類別*System.Web.Caching.OutputCacheProvider*型別。 接著，您可以設定中的提供者`Web.config`使用新的檔案*提供者*子區段*outputCache*項目，如下列範例所示：

[!code-xml[Main](overview/samples/sample2.xml)]

根據預設，ASP.NET 4 中，所有的 HTTP 回應，在轉譯頁面和控制項在前一個範例中，所示，使用記憶體中輸出快取中，其中*defaultProvider*屬性設為 AspNetInternalProvider。 您可以變更用於 Web 應用程式，藉由指定不同的提供者名稱的預設輸出快取提供者*defaultProvider*。

此外，您可以選取每個控制項，且每個要求不同的輸出快取提供者。 選擇不同的輸出快取提供者，針對不同的 Web 使用者控制項的最簡單方式是使用新因此以宣告方式執行*providerName*屬性中的控制項指示詞，如下列範例所示：

[!code-aspx[Main](overview/samples/sample3.aspx)]

指定不同的輸出快取提供者針對 HTTP 要求需要多一點的工作。 而不是以宣告方式指定提供者，您會覆寫新*GetOuputCacheProviderName*方法中的`Global.asax`以程式設計方式指定要用於特定的要求哪一個提供者的檔案。 下列範例顯示如何執行這項工作。

[!code-csharp[Main](overview/samples/sample4.cs)]

加入至 ASP.NET 4 的輸出快取提供者擴充性之後中,，您現在可以針對您的網站，以嘗試更積極與更聰明的輸出快取策略。 比方說，現在正是可以快取的 「 前 10 個 」 頁面的網站，以在記憶體中，在快取取得較低的流量，在磁碟的頁面時。 或者，您可以快取呈現的頁面上，每個不同的組合，但使用分散式快取，以便卸載來自前端 Web 伺服器的記憶體耗用量。

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>自動啟動 Web 應用程式

載入大量資料，或執行昂貴的初始設定處理提供服務的第一個要求之前，需要一些 Web 應用程式。 在舊版 ASP.NET 中，這些情況下您必須想出 「 喚醒 」 的 ASP.NET 應用程式，然後再執行期間的初始化程式碼的自訂方法*應用程式\_負載*方法中的`Global.asax`檔案。

新的擴充性功能，稱為*自動啟動*直接位址這種情況下可當 ASP.NET 4 執行的 Windows Server 2008 R2 上的 IIS 7.5。 自動啟動功能提供啟動應用程式集區，初始化 ASP.NET 應用程式，然後接受 HTTP 要求的控制的的方法。

> [!NOTE] 
> 
> 適用於 IIS 7.5 的 IIS 應用程式準備模組
> 
> IIS 小組已發行應用程式準備模組的第一個 beta 版測試版本 for IIS 7.5。 這可讓您更容易，比先前所述的應用程式正在準備。 而不需要撰寫自訂程式碼，您可以指定要執行的 Web 應用程式接受來自網路的要求資源的 Url。 此準備，就會發生在 IIS 服務的啟動過程 (如果您已設定為 IIS 應用程式集區*AlwaysRunning*) 和 IIS 工作者處理序回收時。 期間回收，舊的 IIS 工作者處理序會繼續執行要求，直到新產生的背景工作處理序完全準備就緒，以便應用程式體驗不中斷或其他問題，因為 unprimed 快取。 請注意，此模組的運作方式與任何版本的 ASP.NET 中，從 2.0 版開始。
> 
> 如需詳細資訊，請參閱 < [Application Warm-Up](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) IIS.net 網站上。 如需說明如何使用 「 準備 」 功能的逐步解說，請參閱 < [Getting Started with IIS 7.5 的應用程式準備模組](https://www.iis.net/learn/manage)IIS.net 網站上。


若要使用自動啟動功能，IIS 管理員，請設定為自動啟動使用中的下列設定的 IIS 7.5 中的應用程式集區`applicationHost.config`檔案：

[!code-xml[Main](overview/samples/sample5.xml)]

因為單一應用程式集區可以包含多個應用程式，您會指定個別的應用程式使用中的下列設定為自動啟動`applicationHost.config`檔案：

[!code-xml[Main](overview/samples/sample6.xml)]

IIS 7.5 的 IIS 7.5 伺服器會是冷啟動時，或個別的應用程式集區回收時，使用中的資訊`applicationHost.config`檔案，以判斷哪一個 Web 應用程式必須自動啟動。 每個應用程式標示為自動啟動，IIS7.5 傳送要求至 ASP.NET 4，以啟動應用程式狀態，此時應用程式暫時不接受 HTTP 要求。 處於此狀態時，ASP.NET 會具現化所定義的型別*serviceAutoStartProvider*屬性 （如上例所示），然後呼叫到它的公用進入點。

使用必要的項目點建立受管理的自動啟動類型藉由實作*IProcessHostPreloadClient*介面，如下列範例所示：

[!code-csharp[Main](overview/samples/sample7.cs)]

程式碼會在您初始化之後*預先載入*方法和方法傳回時，ASP.NET 應用程式已準備好處理要求。

搭配 IIS.5 和 ASP.NET 4 自動啟動，您現在已有妥善定義的方法執行耗費資源的應用程式初始化之前處理第一個 HTTP 要求。 例如，您可以使用新的自動啟動功能，來初始化應用程式，並接著通知負載平衡器的應用程式已初始化並準備好接受 HTTP 流量。

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>永久重新導向頁面

它是常見的做法，在經過一段時間，可能會導致累積的過時的連結，在搜尋引擎中移動頁面和其他內容周圍的 Web 應用程式。 在 ASP.NET 中，開發人員有傳統上由舊 Url 的要求，使用來*Response.Redirect*方法，以將要求轉送到新的 URL。 不過，*重新導向*方法發出 HTTP 302 已找到 （暫時重新導向） 回應，導致額外 HTTP 來回行程當使用者嘗試存取舊的 Url。

加入新的 ASP.NET 4 *RedirectPermanent* helper 方法，可讓您輕鬆地發出 HTTP 301 已永久移動回應，如下列範例所示：

[!code-csharp[Main](overview/samples/sample8.cs)]

搜尋引擎和辨識永久重新導向其他使用者代理程式會將儲存新的內容，這樣會消除不必要的來回行程由暫時重新導向的瀏覽器相關聯的 URL。

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>壓縮工作階段狀態

ASP.NET 提供兩個工作階段狀態儲存在 Web 伺服陣列的預設選項： 叫用跨處理序工作階段狀態伺服器中，工作階段狀態提供者和工作階段狀態提供者在 Microsoft SQL Server 資料庫中儲存資料。 由於這兩個選項需要將儲存 Web 應用程式的背景工作處理序以外的狀態資訊，工作階段狀態，就必須傳送到遠端存放裝置之前，序列化。 根據多少開發人員會將儲存在工作階段狀態的資訊，序列化的資料可以成長的大小相當大。

ASP.NET 4 引進新的壓縮選項，讓這兩種跨處理序工作階段狀態提供者。 當*compressionEnabled*下列範例所示的組態選項設定為 *，則為 true*，ASP.NET 會壓縮 （和解壓縮） 使用.NET Framework序列化工作階段狀態*System.io.compression.gzipstream 就*類別。

[!code-xml[Main](overview/samples/sample9.xml)]

加上以新屬性的簡單`Web.config`檔案中，使用多餘的 CPU 週期，在 Web 伺服器上的應用程式可以實現大幅縮減大小序列化工作階段狀態資料。

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>展開可允許的 Url 的範圍

ASP.NET 4 引進了新的選項，擴充應用程式 Url 的大小。 舊版 ASP.NET 的受限 URL 路徑的長度為 260 個字元，根據 NTFS 檔案路徑限制。 在 ASP.NET 4 中，您可以選擇以增加 （或減少） 為適用於您的應用程式，此限制使用兩個新*httpRuntime*組態屬性。 下列範例會示範這些新屬性。

[!code-xml[Main](overview/samples/sample10.xml)]

若要允許長或短的路徑 （不包含通訊協定、 伺服器名稱，以及查詢字串之 url 的部分），修改*[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* 屬性。 若要允許長或短的查詢字串，修改的值*[maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* 屬性。

ASP.NET 4 也可讓您設定的 URL 字元檢查所使用的字元。 當 ASP.NET 在 URL 的路徑部分中找到無效的字元時，它會拒絕要求，並發出 HTTP 400 錯誤。 在舊版 ASP.NET 中，URL 字元檢查已限制為一組固定的字元。 在 ASP.NET 4 中，您可以自訂的一組使用新的有效字元*requestPathInvalidChars*屬性*httpRuntime*組態項目，如下列範例所示：

[!code-xml[Main](overview/samples/sample11.xml)]

根據預設， <em>requestPathInvalidChars</em>屬性定義為無效的八個字元。 (在字串指派給<em>requestPathInvalidChars</em>預設<em>，</em>小於 (&lt;)、 大於 (&gt;)，和連字號 (&amp;) 字元編碼，因為`Web.config`檔案是 XML 檔案。)視需要您可以自訂的一組無效的字元。

> [!NOTE]
> 請注意，ASP.NET 4 一律會拒絕包含字元 0x00 到 0x1F ASCII 範圍中的 URL 路徑，因為這些是在 IETF 的 RFC 2396 定義的無效的 URL 字元 ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt))。 Windows Server 版本上執行 IIS 6 或更新版本中，http.sys 通訊協定的裝置驅動程式會自動拒絕 Url 具有這些字元。


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>可延伸的要求驗證

ASP.NET 要求驗證跨網站指令碼 (XSS) 攻擊中，搜尋傳入 HTTP 要求資料的常用的字串。 如果找不到可能的 XSS 字串，則要求驗證旗標的可疑的字串，並傳回錯誤。 只有在找到最常見的字串，用於 XSS 攻擊時，才，內建的要求驗證就會傳回錯誤。 先前嘗試以進行更積極的 XSS 驗證卻造成太多誤判。 不過，客戶可以更積極，或反之可能會想要刻意放寬 XSS 要求驗證檢查的特定頁面，或針對特定類型的要求。

在 ASP.NET 4 中，要求驗證功能已擴充，讓您可以使用自訂要求驗證邏輯。 若要擴充要求驗證，您可以建立衍生自新的類別*System.Web.Util.RequestValidator*類型，以及您設定應用程式 (在*httpRuntime*區段`Web.config`檔案) 使用自訂的型別。 下列範例示範如何設定自訂要求驗證類別：

[!code-xml[Main](overview/samples/sample12.xml)]

新*requestValidationType*屬性需要指定類別，可提供自訂要求驗證的標準.NET Framework 型別識別項字串。 針對每個要求，ASP.NET 會叫用自訂的型別，來處理內送的 HTTP 要求資料的每個片段。 傳入的 URL，所有的 HTTP 標頭 （cookie 和自訂標頭），實體是可供用來檢查由自訂要求驗證類別類似下列範例所示：

[!code-csharp[Main](overview/samples/sample13.cs)]

其中您不想檢查傳入的 HTTP 資料片段的情況下，要求驗證類別可切換回讓只是呼叫執行的 ASP.NET 預設要求驗證*基底。IsValidRequestString。*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>物件快取物件和快取的擴充性

ASP.NET 已自其第一個版本中，包含了功能強大的記憶體內部物件快取 (*System.Web.Caching.Cache*)。 您已這麼受歡迎，非 Web 應用程式中將它使用快取實作。 不過，還是有點不便 Windows Form 或 WPF 應用程式包含的參考`System.Web.dll`只是為了要能夠使用 ASP.NET 物件快取。

若要讓快取適用於所有的應用程式，.NET Framework 4 引進了新的組件、 新的命名空間、 某些基底類型和具體快取實作。 新`System.Runtime.Caching.dll`組件包含在新的快取 API *System.Runtime.Caching*命名空間。 命名空間包含類別的兩個核心集：

- 提供的基礎建置的自訂快取實作的任何類型的抽象型別。
- 具體記憶體內部物件快取實作 ( *System.Runtime.Caching.MemoryCache*類別)。

新*MemoryCache*類別根據密切 ASP.NET 快取，以及它與 ASP.NET 共用許多內部快取引擎邏輯。 雖然快取的公用 Api，在*System.Runtime.Caching*如果您曾經使用過 ASP.NET 已更新為支援的自訂快取，開發*快取*物件，您會發現在熟悉的概念新的 Api。

新的深入討論*MemoryCache*類別和支援的基底的 Api 需要整份文件。 不過，下列範例可讓您了解新的快取 API 的運作方式。 此範例針對所撰寫的 Windows Forms 應用程式，而不需要任何相依性上`System.Web.dll`。

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>可延伸的 HTML、 URL 和 HTTP 標頭的編碼方式

在 ASP.NET 4 中，您可以建立自訂編碼常式，如下列的一般文字編碼工作：

- HTML 編碼。
- URL 編碼。
- HTML 屬性編碼。
- 輸出 HTTP 標頭的編碼方式。

您可以建立自訂編碼器，藉由衍生自新*System.Web.Util.HttpEncoder*類型，然後設定 ASP.NET 使用中的自訂型別*httpRuntime*一節`Web.config`檔案，做為下列範例所示：

[!code-xml[Main](overview/samples/sample15.xml)]

設定自訂編碼器之後，ASP.NET 會自動呼叫的自訂編碼實作公用方法的編碼方式時*System.Web.HttpUtility*或*System.Web.HttpServerUtility*類別則稱為。 這可讓建立自訂編碼器，會實作積極的字元編碼，而 Web 開發小組的其餘部分會繼續使用公用 Api 的編碼方式的 ASP.NET Web 開發小組的一個部分。 藉由集中設定中的自訂編碼器*httpRuntime*項目，您可保證所有文字編碼的呼叫，從公用 Api 的編碼方式的 ASP.NET 會路由都傳送到自訂編碼器。

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>在單一背景工作處理序中的個別應用程式的效能監視

為了增加可以在單一伺服器裝載的網站數目，許多主機服務提供者會在單一背景工作處理序中執行多個 ASP.NET 應用程式。 不過，如果多個應用程式使用單一共用的背景工作處理序，很難識別個別應用程式發生問題的伺服器系統管理員。

ASP.NET 4 會運用 CLR 所導入新資源監視功能。 若要啟用這項功能，您可以新增下列 XML 組態程式碼片段`aspnet.config`組態檔。

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> 請注意`aspnet.config`檔案位於.NET Framework 安裝所在的目錄。 它不是`Web.config`檔案。


當*Appdomainresourcemonitoring>* 已啟用功能、 兩個新的效能計數器位於 [ASP.NET 應用程式] 的效能分類：*受控處理器時間百分比*和*Managed 記憶體使用*。 這兩個這些效能計數器來追蹤估計的 CPU 時間和受管理的記憶體使用率的個別的 ASP.NET 應用程式使用新的 CLR 應用程式定義域資源管理功能。 如此一來，ASP.NET 4 中，以系統管理員現在可以在單一背景工作處理序中執行的個別應用程式的資源耗用量的更細微檢視。

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>多目標

您可以建立以特定版本的.NET framework 為目標的應用程式。 在 ASP.NET 4 中中的新屬性*編譯*項目`Web.config`檔案可讓您以.NET Framework 4 及更新版本為目標。 如果您明確的目標.NET Framework 4，，和包含選擇性的項目中`Web.config`檔案的項目如*system.codedom*，這些項目必須是正確的.NET Framework 4。 (如果未明確目標.NET Framework 4，目標架構會推斷中的項目和缺少`Web.config`檔案。)

下列範例示範使用*targetFramework*屬性中*編譯*項目`Web.config`檔案。

[!code-xml[Main](overview/samples/sample17.xml)]

請注意下列有關特定版本的.NET framework 為目標：

- 在.NET Framework 4 應用程式集區中，ASP.NET 建置系統會假設為.NET Framework 4 做為目標如果`Web.config`檔案不包含*targetFramework*屬性或`Web.config`遺漏的檔案。 （您可能需要對您的應用程式，讓它在.NET Framework 4 下執行的程式碼的變更）。
- 若您納入*targetFramework*屬性，而如果*system.codeDom*項目定義在`Web.config`檔案，此檔案必須包含正確的項目，適用於.NET Framework 4。
- 如果您使用*aspnet\_編譯器*命令來先行編譯您的應用程式 （例如在建置環境），您必須使用正確的版本*aspnet\_編譯器*目標 framework 的命令。 使用.NET Framework 2.0 （%windir%\microsoft.net\framework\v2.0.50727) 的出貨的編譯器編譯的.NET Framework 3.5 和更早版本。 使用.NET Framework 4 的附隨的編譯器來編譯應用程式建立使用該架構或更新版本。
- 在執行階段，編譯器會使用最新的 framework 組件安裝在電腦上 (因此在 GAC 中)。 如果 framework 稍後進行更新 （例如，假設版本 4.1 安裝），您可以使用功能較新的 framework 版本中，即使*targetFramework*屬性以較低的版本為目標（例如 4.0)。 (不過，在設計階段於 Visual Studio 2010 或當您使用*aspnet\_編譯器*命令時，使用較新的 framework 的功能將會導致編譯器錯誤)。

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>Ajax

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>包括在 Web Form 和 MVC 的 jQuery

Web Form 和 MVC 的 Visual Studio 範本包含開放原始碼 jQuery 程式庫。 當您建立新的網站或專案時，會建立包含下列 3 個檔案的指令碼資料夾：

- jQuery-1.4.1.js – 人類看得懂，unminified jQuery 程式庫版本。
- -jQuery 14.1.min.js 的 – 縮短的 jQuery 程式庫版本。
- jQuery-1.4.1-vsdoc.js – jQuery 程式庫的 Intellisense 文件檔案。

開發應用程式時包含 jQuery unminified 的版本。 包含的實際執行應用程式的 jQuery 縮短的版本。

例如，下列的 Web Form 頁面會說明如何使用 jQuery，以及在若要變更 ASP.NET TextBox 控制項設為黃色，當他們有焦點時的背景色彩。

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>內容傳遞網路支援

Microsoft Ajax 內容傳遞網路 (CDN) 可讓您輕鬆地將 ASP.NET Ajax 和 jQuery 指令碼新增至您的 Web 應用程式。 例如，您可以在其中開始使用 jQuery 程式庫新增`<script>`您指向 Ajax.microsoft.com，像這樣的頁面的標記：

[!code-html[Main](overview/samples/sample19.html)]

利用 Microsoft Ajax CDN，您可以大幅改善 Ajax 應用程式的效能。 Microsoft Ajax CDN 的內容會在位於世界各地的伺服器上快取。 此外，Microsoft Ajax CDN 可讓瀏覽器針對位於不同網域中的網站重複使用快取的 JavaScript 檔案。

Microsoft Ajax 內容傳遞網路支援 SSL (HTTPS)，萬一您需要提供使用 Secure Sockets Layer 的網頁。

無法使用 CDN 時，請實作後援。 測試此後援。

若要深入了解 Microsoft Ajax CDN，請瀏覽下列網站：

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

ASP.NET ScriptManager 支援 Microsoft Ajax CDN。 只要設定一個屬性，加入 EnableCdn 屬性中，您可以從 CDN 擷取所有 ASP.NET framework JavaScript 檔案：

[!code-aspx[Main](overview/samples/sample20.aspx)]

您將設定加入 EnableCdn 屬性值為 true 之後，ASP.NET 架構會擷取所有 ASP.NET framework JavaScript 檔案從 CDN 包括用於驗證和 UpdatePanel 的所有 JavaScript 檔案。 設定這個屬性會對您的 web 應用程式的效能造成重大的影響。

您可以使用 WebResource 屬性，為您自己的 JavaScript 檔案設定的 CDN 路徑。 新的 CdnPath 屬性會指定 CDN 加入 EnableCdn 屬性設定為值 true 時所使用的路徑：

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>ScriptManager 明確指令碼

在過去，如果您使用 ASP.NET ScriptManger 然後您必須將整個的整合型 ASP.NET Ajax 程式庫。 利用新的 ScriptManager.AjaxFrameworkMode 屬性，您可以控制完全載入的 ASP.NET Ajax 程式庫的元件，並載入 ASP.NET Ajax 程式庫所需的元件。

ScriptManager.AjaxFrameworkMode 屬性可以設定為下列值：

- 已啟用-指定 ScriptManager 控制項，會自動包含 MicrosoftAjax.js 指令碼檔案，也就是結合的指令碼檔案的每個核心架構指令碼 （舊版行為）。
- 停用-指定已停用所有的 Microsoft Ajax 指令碼功能和，ScriptManager 控制項未參考任何指令碼自動。
- 明確-指定您要明確地加入至您的頁面所需要的個別架構核心指令碼檔案的指令碼參考，並將每個指令碼檔所需的相依性的參考。

比方說，如果您將加入 AjaxFrameworkMode 屬性設定為明確的值然後您可以指定特定 ASP.NET Ajax 元件指令碼，您需要：

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Web Form

Web Form ASP.NET 1.0 發行以來已在 ASP.NET 中的核心功能。 針對 ASP.NET 4，包括下列這方面已許多增強功能：

- 若要設定的能力*meta*標記。
- 進一步控制檢視狀態的詳細資訊。
- 使用瀏覽器功能的簡單方法。
- 支援使用 ASP.NET Web form 的路由。
- 更充分掌控產生識別碼。
- 保存資料控制項中選取的資料列的能力。
- 更充分掌控中轉譯的 HTML *FormView*並*ListView*控制項。
- 篩選資料來源控制項的支援。

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>設定使用 Page.MetaKeywords 和 Page.MetaDescription 屬性的中繼標籤

ASP.NET 4 中加入兩個屬性，以*網頁*類別*MetaKeywords*並*MetaDescription*。 這兩個屬性，代表對應*meta*標記儲存在您的網頁，如下列範例所示：

[!code-aspx[Main](overview/samples/sample23.aspx)]

這兩個屬性的運作方式相同方式頁面*Title*屬性。 它們會遵循下列規則：

1. 如果有任何*中繼*標記中*標頭*符合屬性名稱的項目 (也就是名稱 = 「 關鍵字 」 *Page.MetaKeywords*和名稱 ="description"，如*Page.MetaDescription*，尚未設定這些屬性的意義)，則*meta*呈現時，將頁面加入標記。
2. 如果已有*meta*具有這些名稱的標記，這些屬性會做為取得和設定的現有標籤內容的方法。

您可以在執行階段，可讓您從資料庫或其他來源取得內容，並可讓您設定以動態方式來描述項目的標記來設定這些屬性是特定的頁面。

您也可以設定*關鍵字*並*描述*中的屬性 *@ Page*指示詞頂端的 Web Form 網頁的標記，如下列範例所示：

[!code-aspx[Main](overview/samples/sample24.aspx)]

這會覆寫*meta*標記內容 （如果有的話） 中的頁面已經宣告。

描述的內容*meta*標記用於改善搜尋列出 Google 中的預覽。 (如需詳細資訊，請參閱 <<c0> [ 改善程式碼片段與中繼描述改造](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html)Google 網站管理員中心部落格上。)Google 和 Windows Live Search 不會使用關鍵字的連接內容的任何項目，但其他搜尋引擎可能。 如需詳細資訊，請參閱 < [Meta 關鍵字建議](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php)搜尋引擎指南網站上。

這些新的屬性是一個簡單的功能，但它們會儲存您的需求，將這些手動新增或撰寫您自己的程式碼，以建立*meta*標記。

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>啟用個別的控制項檢視狀態

根據預設，檢視狀態被啟用頁面上，即使您不需要應用程式，每個頁面上的控制項可能儲存檢視狀態的結果。 檢視狀態資料包含在標記中，會產生頁面，並增加傳送到用戶端的頁面和回傳它所需的時間量。 儲存更多超出所需的檢視狀態，可能會導致效能大幅降低。 在舊版 ASP.NET 中，開發人員可以停用個別的控制項檢視狀態，以減少頁面大小，但必須進行明確的個別控制項。 在 ASP.NET 4 中，Web 伺服器控制項包括*ViewStateMode*屬性，可讓您依預設停用檢視狀態，然後再將它啟用只針對需要在頁面中的控制項。

*ViewStateMode*屬性會有三個值的列舉：*已啟用*，*已停用*，以及*繼承*。 *已啟用*啟用檢視狀態，該控制項，而設定為任何子控制項*繼承*或者，有任何設定。 *已停用*停用檢視狀態，並*繼承*指定控制項使用*ViewStateMode*設定從父控制項。

下列範例示範如何*ViewStateMode*屬性雖然有效。 標記和程式碼中的下列網頁的控制項包含值*ViewStateMode*屬性：

[!code-aspx[Main](overview/samples/sample25.aspx)]

如您所見，程式碼會停用 PlaceHolder1 控制項的檢視狀態。 子 label1 控制項繼承這個屬性值 (*繼承*的預設值*ViewStateMode* 。 控制項)，因此可以節省任何檢視狀態。 在 PlaceHolder2 控制項中， *ViewStateMode*設為*已啟用*，因此 label2 繼承這個屬性，並儲存檢視狀態。 第一次載入頁面時，*文字*屬性*標籤*控制項設定為"[DynamicValue]"的字串。

這些設定的效果是當頁面載入第一次，在瀏覽器中顯示下列輸出：

已停用 `: [DynamicValue]`

已啟用：`[DynamicValue]`

之後回傳，不過，會顯示下列輸出：

已停用 `: [DeclaredValue]`

已啟用：`[DynamicValue]`

Label1 控制項 (其*ViewStateMode*值設定為*停用*) 不會保留它已設定為在程式碼中的值。 不過，控制 label2 (其*ViewStateMode*值設定為*已啟用*) 已保留其狀態。

您也可以設定*ViewStateMode*中 *@ Page*指示詞，如下列範例所示：

[!code-aspx[Main](overview/samples/sample26.aspx)]

*網頁*類別是只是另一個控制項，它會作為網頁中的所有其他控制項的父控制項。 預設值*ViewStateMode*是*已啟用*的執行個體*頁面*。 因為控制項預設為*繼承*，控制項就會繼承*已啟用*屬性值，除非您設定*ViewStateMode*頁面或控制項的層級。

值*ViewStateMode*屬性會決定是否檢視狀態會維護才*EnableViewState*屬性設定為*true*。 如果*EnableViewState*屬性設定為*false*，將無法保持檢視狀態即使*ViewStateMode*設定為*已啟用*。

適合用於這項功能是使用*ContentPlaceHolder*中，您可以設定的主版頁面的控制項*ViewStateMode*來*已停用*主要頁面上，然後再啟用它為個別*ContentPlaceHolder*又包含需要控制項的控制項檢視狀態。

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>變更瀏覽器功能

ASP.NET 會判斷使用者正在使用利用稱為瀏覽您的網站的瀏覽器的功能*瀏覽器功能*。 瀏覽器功能由*HttpBrowserCapabilities*物件 (由*Request.Browser*屬性)。 例如，您可以使用*HttpBrowserCapabilities*來判斷型別和目前的瀏覽器版本是否支援特定版本的 JavaScript 物件。 或者，您可以使用*HttpBrowserCapabilities*物件，以判斷要求是否來自行動裝置。

*HttpBrowserCapabilities*物件由一組的瀏覽器定義檔案。 這些檔案包含特定的瀏覽器功能資訊。 在 ASP.NET 4 中，已更新這些瀏覽器定義檔案包含近期推出的瀏覽器和 Google Chrome、 研究等影片 BlackBerry smartphone 和 Apple iPhone 中的裝置的相關資訊。

下列清單會顯示新的瀏覽器定義檔案：

- *blackberry.browser*
- *chrome.browser*
- *Default.browser*
- *firefox.browser*
- *gateway.browser*
- *generic.browser*
- *ie.browser*
- *iemobile.browser*
- *iphone.browser*
- *opera.browser*
- *safari.browser*

#### <a name="using-browser-capabilities-providers"></a>使用瀏覽器功能的提供者

在 ASP.NET 3.5 版 Service Pack 1，您可以定義可透過下列方式在瀏覽器的功能：

- 在電腦層級中，您需要建立或更新`.browser`下列資料夾中的 XML 檔案：

- [!code-console[Main](overview/samples/sample27.cmd)]

- 您定義的瀏覽器功能之後，您執行下列命令從 Visual Studio 命令提示字元以重建瀏覽器的功能組件，並將它新增至 GAC:

- [!code-console[Main](overview/samples/sample28.cmd)]

- 在您建立個別的應用程式，如`.browser`應用程式的檔案`App_Browsers`資料夾。

這些方法需要您變更 XML 檔案和電腦層級變更，您必須重新啟動應用程式之後執行 aspnet\_regbrowsers.exe 程序。

ASP.NET 4 包含功能，稱為*瀏覽器功能的提供者*。 顧名思義，這可讓您建置接著可讓您的提供者會使用您自己的程式碼來判斷瀏覽器功能。

在實務上，開發人員通常不會定義自訂的瀏覽器功能。 瀏覽器檔案是難更新程序相當複雜，更新為與之 XML 語法的`.browser`檔案可以是複雜，無法使用，並定義。 什麼會讓這個程序更容易是如果有個常見的瀏覽器定義語法，或包含最新的瀏覽器的定義或甚至是這類資料庫的 Web 服務的資料庫。 新的瀏覽器功能提供者功能會讓這些案例可能和實務的第三方開發人員。

有兩種主要的方法，使用新的 ASP.NET 4 的瀏覽器功能提供者功能： 擴充 ASP.NET 瀏覽器功能定義功能，或完全取代它。 如何取代的功能，以及如何加以擴充，則下列各節會說明第一次。

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>取代 ASP.NET 瀏覽器功能的功能

若要完全取代 ASP.NET 瀏覽器功能定義功能，請遵循下列步驟：

1. 建立提供者類別衍生自*HttpCapabilitiesProvider*它會覆寫*GetBrowserCapabilities*方法，如下列範例所示： 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    在此範例中的程式碼會建立新*HttpBrowserCapabilities*物件，指定只將名為瀏覽器，並將該功能設定為 MyCustomBrowser 的功能。
2. 註冊應用程式提供者。 

    若要使用的提供者與應用程式，您必須新增*提供者*屬性設定為*browserCaps*一節中`Web.config`或`Machine.config`檔案。 (您也可以定義中的提供者屬性*位置*針對特定的目錄，在應用程式，例如特定的行動裝置所需的資料夾中的項目。)下列範例示範如何設定*提供者*組態檔中的屬性：

    [!code-xml[Main](overview/samples/sample30.xml)]

    若要註冊新的瀏覽器功能定義的另一種方式是使用程式碼，如下列範例所示：

    [!code-csharp[Main](overview/samples/sample31.cs)]

    此程式碼必須執行*應用程式\_開始*事件的`Global.asax`檔案。 對任何變更*BrowserCapabilitiesProvider*類別必須在應用程式中的任何程式碼執行時，若要確定快取仍處於有效狀態的已解析之前進行*HttpCapabilitiesBase*物件。

#### <a name="caching-the-httpbrowsercapabilities-object"></a>快取 HttpBrowserCapabilities 物件

上述範例中有一個問題，也就是執行程式碼會叫用自訂提供者以取得每次*HttpBrowserCapabilities*物件。 這可能會多次發生在每個要求。 在範例中，提供者的程式碼無法做太多事。 不過，如果您的自訂提供者中的程式碼執行中的重要工作以取得*HttpBrowserCapabilities*物件，這可能會影響效能。 為了避免這種情況，您可以快取*HttpBrowserCapabilities*物件。 請依照下列步驟：

1. 建立衍生自類別*HttpCapabilitiesProvider*，在下列範例類似： 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    在範例中，程式碼產生快取索引鍵呼叫自訂的 BuildCacheKey 方法，而且它會藉由呼叫自訂的 GetCacheTime 方法取得的快取的時間長度。 然後程式碼新增解決*HttpBrowserCapabilities*至快取的物件。 物件可以擷取從快取和重複使用在上進行的後續要求使用的自訂提供者。
2. 在前述程序中所述，則您可以向應用程式提供者。

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>擴充 ASP.NET 瀏覽器功能的功能

上一節說明如何建立新*HttpBrowserCapabilities* ASP.NET 4 中的物件。 您也可以藉由新增新的瀏覽器功能定義於已經在 ASP.NET 中，擴充 ASP.NET 瀏覽器功能的功能。 您可以不使用 XML 的瀏覽器定義來這樣做。 下列程序顯示如何。

1. 建立衍生自類別*HttpCapabilitiesEvaluator*它會覆寫*GetBrowserCapabilities*方法，如下列範例所示： 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    第一次，此程式碼會嘗試識別瀏覽器使用 ASP.NET 瀏覽器功能的功能。 不過，如果瀏覽器不會識別根據在要求中所定義的資訊 (亦即，如果*瀏覽器*屬性*HttpBrowserCapabilities*物件是 「 Unknown 」 字串)，程式碼會呼叫自訂提供者 (MyBrowserCapabilitiesEvaluator) 來識別瀏覽器。
2. 在上述範例中所述，則您可以向應用程式提供者。

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>擴充瀏覽器功能的功能加入到現有的功能定義的新功能

除了建立自訂瀏覽器的定義提供者，並以動態方式建立新的瀏覽器的定義，您可以擴充現有的瀏覽器定義，使用額外的功能。 這可讓您使用的定義，很接近您想要但缺少只有少數的功能。 若要這麼做，請使用下列步驟。

1. 建立衍生自類別*HttpCapabilitiesEvaluator*它會覆寫*GetBrowserCapabilities*方法，如下列範例所示： 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    範例程式碼擴充現有的 ASP.NET *HttpCapabilitiesEvaluator*類別，並取得*HttpBrowserCapabilities*符合目前的要求定義，使用下列程式碼的物件:

    [!code-csharp[Main](overview/samples/sample35.cs)]

    程式碼則可以新增或修改此瀏覽器的功能。 有兩種方式可以指定新的瀏覽器功能：

    - 將索引鍵/值組加入*IDictionary*所公開的物件*功能*屬性*HttpCapabilitiesBase*物件。 在上述範例中，程式碼會加入一項功能使用的值為多點觸控 *，則為 true*。
    - 設定現有的屬性*HttpCapabilitiesBase*物件。 在上述範例中，程式碼會設定*框架*屬性設 *，則為 true*。 這個屬性是直接存取子*IDictionary*所公開的物件*功能*屬性。 

        > [!NOTE]
        > 請注意此模型的任何屬性會套用*HttpBrowserCapabilities*，包括控制項配接器。
2. 先前的程序中所述，則您可以向應用程式提供者。

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>ASP.NET 4 中的路由

ASP.NET 4 新增內建支援使用 Web form 的路由。 路由可讓您設定應用程式以接受要求不會對應至實體檔案的 Url。 相反地，您可以使用路由定義的使用者有意義，而且可以幫助您的應用程式的搜尋引擎最佳化 (SEO) 的 Url。 例如，顯示產品類別目錄中現有的應用程式頁面的 URL 看起來可能如下列範例所示：

[!code-console[Main](overview/samples/sample36.cmd)]

藉由使用路由，您可以設定應用程式接受下列的 URL，來呈現相同的資訊：

[!code-console[Main](overview/samples/sample37.cmd)]

路由已可開始使用 ASP.NET 3.5 SP1。 (如需如何使用 ASP.NET 3.5 SP1 中的路由的範例，請參閱文章[使用路由與 WebForms](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "這個項目的標題。") Phil Haack 的部落格。）不過，ASP.NET 4 包含一些功能，使您更輕鬆地使用路由，包括下列：

- *PageRouteHandler*類別，這是簡單的 HTTP 處理常式時定義的路由使用。 類別會將資料傳遞至要求會路由傳送至頁面。
- 新的屬性*HttpRequest.RequestContext*並*Page.RouteData* (這是 proxy *HttpRequest.RequestContext.RouteData*物件)。 這些屬性可讓您更輕鬆地傳遞來自路由的存取資訊。
- 下列新運算式產生器，是定義於*System.Web.Compilation.RouteUrlExpressionBuilder*並*System.Web.Compilation.RouteValueExpressionBuilder*:
- *RouteUrl*，以提供簡單的方法，來建立對應至 ASP.NET 伺服器控制項中的路由 URL 的 URL。
- *RouteValue*，以提供簡單的方法來擷取資訊，從*RouteContext*物件。
- *RouteParameter*類別，可讓您更輕鬆地將中所包含的資料傳遞*RouteContext*的查詢，資料來源控制項的物件 (類似[ *FormParameter*](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Web Form 網頁的路由

下列範例示範如何定義使用新的 Web Form 的路由*MapPageRoute*方法*路由*類別：

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 引進*MapPageRoute*方法。 下列範例相當於先前的範例所示的 SearchRoute 定義，但改用*PageRouteHandler*類別。

[!code-csharp[Main](overview/samples/sample39.cs)]

此範例中的程式碼對應至實體頁面的路由 (在第一個路由中，以`~/search.aspx`)。 第一個路由定義也會指定應該從 URL 擷取具名 searchterm 參數，然後傳遞至頁面。

*MapPageRoute*方法支援下列方法多載：

- *MapPageRoute 字串 routeName、 字串 routeUrl、 字串 physicalFile (bool checkPhysicalUrlAccess）*
- *MapPageRoute 字串 routeName、 字串 routeUrl、 字串 physicalFile、 bool checkPhysicalUrlAccess (RouteValueDictionary 預設值）*
- *MapPageRoute 字串 routeName、 字串 routeUrl、 字串 physicalFile、 bool checkPhysicalUrlAccess、 RouteValueDictionary 預設值 (RouteValueDictionary 條件約束）*

*CheckPhysicalUrlAccess*參數會指定路由是否應該檢查路由傳送至實體頁面的安全性權限 (在此情況下，search.aspx) 以及傳入 URL 的權限 （在此情況下，搜尋/ {searchterm})。 如果值*checkPhysicalUrlAccess*是*false*，要檢查的傳入 URL 的權限。 在 定義這些權限`Web.config`檔案中，使用下列設定：

[!code-xml[Main](overview/samples/sample40.xml)]

在範例組態中，存取被拒的實體頁面`search.aspx`除外身為系統管理員角色中的所有使用者。 當*checkPhysicalUrlAccess*參數設為 *，則為 true* （此為其預設值），只有系統管理員的使用者允許存取 URL /search/ {searchterm}，因為實體頁面 search.aspx限制為該角色中的使用者。 如果*checkPhysicalUrlAccess*設為*false*和此站台已在上述範例所示，允許所有已驗證的使用者存取 URL /search/ {searchterm}。

#### <a name="reading-routing-information-in-a-web-forms-page"></a>讀取 Web Forms 網頁中的路由資訊

在 Web Form 實體頁面的程式碼中，您可以存取路由已從 URL 擷取的資訊 (或加入另一個物件，以其他資訊*RouteData*物件) 使用兩個新屬性： *HttpRequest.RequestContext*並*Page.RouteData*。 (*Page.RouteData*包裝*HttpRequest.RequestContext.RouteData*。)下列範例示範如何使用*Page.RouteData*。

[!code-csharp[Main](overview/samples/sample41.cs)]

程式碼會擷取傳遞給 searchterm 參數，如稍早範例路由中所定義的值。 請考慮下列的要求 URL:

[!code-console[Main](overview/samples/sample42.cmd)]

此提出要求時，會以"scott"這個字`search.aspx`頁面。

#### <a name="accessing-routing-information-in-markup"></a>存取在標記中的路由資訊

上一節中所述的方法顯示如何在 Web Forms 網頁中的程式碼取得路由資料。 您也可以使用相同的資訊可讓您存取在標記中的運算式。 運算式產生器是功能強大且簡潔的方式來使用宣告式程式碼。 (如需詳細資訊，請參閱文章[Express 自行使用自訂運算式產生器](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx)Phil Haack 的部落格上。)

ASP.NET 4 包含兩個新的運算式產生器，Web Form 的路由。 下列範例示範如何使用它們。

[!code-aspx[Main](overview/samples/sample43.aspx)]

在範例中， *RouteUrl*運算式用來定義為基礎的路由參數的 URL。 這可讓您不必使用硬式編碼到標記的完整 URL，並可讓您稍後變更 URL 結構，而不需要此連結的任何變更。

根據先前定義的路由，則此標記會產生下列 URL:

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET 會自動運作出正確的路由 （也就是它會產生正確的 URL） 根據輸入參數。 您也可以包含在運算式中，可讓您指定要使用的路由的路由名稱。

下列範例示範如何使用*RouteValue*運算式。

[!code-aspx[Main](overview/samples/sample45.aspx)]

包含此控制項的網頁執行時，值"scott"會顯示在標籤中。

*RouteValue*運算式可簡化在標記中，使用路由資料，而且還能避免需要使用更複雜的 Page.RouteData["x"] 在標記中的語法。

#### <a name="using-route-data-for-data-source-control-parameters"></a>使用路由資料的資料來源控制項參數

*RouteParameter*類別可讓您指定作為資料來源控制項中的查詢的參數值的路由資料。 它[運作方式就像是](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)類別，如下列範例所示：

[!code-aspx[Main](overview/samples/sample46.aspx)]

在此情況下，路由參數 searchterm 的值將用於@companyname中的參數<em>選取</em>陳述式。

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>設定用戶端識別碼

新*ClientIDMode*屬性解決長久以來的問題，在 ASP.NET 中，也就是如何控制項建立*識別碼*它們所呈現的項目屬性。 了解*識別碼*呈現項目的屬性是很重要，如果您的應用程式包含用戶端指令碼會參考這些項目。

*識別碼*呈現 Web 伺服器控制項的 HTML 中的屬性就會產生根據*ClientID*控制項的屬性。 直到 ASP.NET 4 中，產生的演算法*識別碼*屬性從*ClientID*屬性已經以串連命名的容器 （如果有的話） 的識別碼，並在重複 （作為中控制項的情況下資料控制項），將前置詞和序號。 雖然這有一定的頁面中的控制項 Id 是唯一的此演算法已經產生的控制不是可預測，並因此很難進行用戶端指令碼中參考的識別碼。

新*ClientIDMode*屬性可讓您指定更精確的用戶端識別碼產生方式的控制項。 您可以設定*ClientIDMode*任何控制項，包含頁面的屬性。 可能的設定如下所示：

- *AutoID* – 這相當於產生演算法*ClientID*舊版 ASP.NET 中使用的屬性值。
- *靜態*– 這會指定*ClientID*值將會是相同的識別碼，而不需要串連父命名容器的識別碼。 這可用於 Web 使用者控制項。 因為 Web 使用者控制項可能位於不同的頁面上，並在不同的容器控制項，所以很難撰寫使用控制項的用戶端指令碼*AutoID*演算法因為您無法預測的 ID 值將會.
- *可預測*– 此選項主要是針對使用重複的範本的資料控制項中使用。 它會控制的命名容器，但產生的識別碼屬性*ClientID*值並沒有包含"ctlxxx 」 等的字串。 此設定可搭配*ClientIDRowSuffix*控制項的屬性。 您設定*ClientIDRowSuffix*資料欄位的名稱和該欄位的值的屬性產生時，可作為尾碼*ClientID*值。 通常您會使用為資料錄的主索引鍵*ClientIDRowSuffix*值。
- *繼承*– 此設定是控制項的預設行為; 它會指定控制項的識別碼產生作業是其父代相同。

您可以設定*ClientIDMode*頁面層級的屬性。 這會定義預設值*ClientIDMode*本頁中的所有控制項的值。

預設值*ClientIDMode*頁面層級的值是*AutoID*，和預設*ClientIDMode*控制層級的值是*繼承*. 如此一來，如果您未設定這個屬性隨處在程式碼中，所有控制項將會都預設為*AutoID*演算法。

您設定頁面層級值 *@ Page*指示詞，如下列範例所示：

[!code-aspx[Main](overview/samples/sample47.aspx)]

您也可以設定*ClientIDMode*組態檔，在電腦 （電腦） 層級或應用程式層級中的值。 這會定義預設值*ClientIDMode*設定應用程式中的所有頁面中的所有控制項。 如果您在電腦層級設定值，它會定義預設值*ClientIDMode*該電腦上設定的所有網站。 下列範例所示*ClientIDMode*組態檔中設定：

[!code-xml[Main](overview/samples/sample48.xml)]

如前文所述，windows 7 *ClientID*屬性衍生自命名的容器控制項的父代。 在某些情況下，例如當您使用主版頁面中，控制項可以得到識別碼類似下列中呈現 HTML:

[!code-html[Main](overview/samples/sample49.html)]

即使*輸入*標記中所顯示的項目 (從*TextBox*控制項) 是只有兩個命名容器頁面中的深度 (巢狀*ContentPlaceholder*控制項)，由於處理主版頁面的方式，最終結果會是如下所示的控制項識別碼：

[!code-console[Main](overview/samples/sample50.cmd)]

此識別碼保證是唯一在頁面中，但不必要地長，不適用於大部分用途。 假設您想減少轉譯的識別碼，長度或需要更充分掌控所產生的識別碼。 （例如，您想要消除 「 ctlxxx"前置詞。）若要這麼做最簡單方式是藉由設定*ClientIDMode*屬性，如下列範例所示：

[!code-aspx[Main](overview/samples/sample51.aspx)]

在此範例中， *ClientIDMode*屬性設定為*靜態*的最外層*NamingPanel*項目，並設定為*可預測*為內部*NamingControl*項目。 這些設定會導致下列標記 （頁面和主版頁面的其餘部分會假設相同，如先前範例所示）：

[!code-html[Main](overview/samples/sample52.html)]

*靜態*設定已重設所有控制項最外層命名階層的效果*NamingPanel*項目，以及消除*ContentPlaceHolder*並*MasterPage* Id 的來源產生的識別碼。 (*名稱*呈現項目的屬性會受到影響，事件會保留一般的 ASP.NET 功能，讓檢視狀態，依此類推。)重設命名階層架構的副作用是，即使您移動的標記*NamingPanel*到不同的項目*ContentPlaceholder*控制項，轉譯的用戶端識別碼維持不變。

> [!NOTE]
> 請注意，則您必須確定呈現的控制項識別碼都是唯一的。 如果他們不這樣做，可能會中斷任何需要對個別的 HTML 項目，例如用戶端的唯一識別碼的功能*document.getElementById*函式。


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>建立資料繫結控制項中的可預測的用戶端識別碼

*ClientID*所產生舊的演算法所資料繫結清單控制項中的控制項可以是值，且不是真的可預測。 *ClientIDMode*功能可協助您將更多控制透過這些方式 Id 的產生。

在下列範例中的標記包括*ListView*控制項：

[!code-aspx[Main](overview/samples/sample53.aspx)]

在上述範例中， *ClientIDMode*並*RowClientIDRowSuffix*標記中設定屬性。 *ClientIDRowSuffix*屬性只能用於資料繫結控制項，其行為的差異根據哪一個控制項使用。 這些差異︰

- *GridView*控制項 — 您可以指定一或多個資料行名稱在資料來源，在執行階段建立用戶端識別碼一起使用。 例如，如果您設定*RowClientIDRowSuffix* "ProductName ProductId"，來呈現項目的的格式如下所示的控制項 Id:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView*控制項 — 您可以指定單一資料行中的資料來源，會附加至用戶端識別碼。 例如，如果您設定*ClientIDRowSuffix* "ProductName"，以呈現的控制項識別碼的格式如下所示：

- [!code-console[Main](overview/samples/sample55.cmd)]

- 在此情況下尾端 1 被衍生自目前資料項目的產品識別碼。

- *Repeater*控制項，此控制項不支援*ClientIDRowSuffix*屬性。 在  *Repeater*控制項中，目前的資料列的索引使用。 當您使用 ClientIDMode = 「 預測 」 *Repeater*控制項，用戶端就能產生識別碼的格式如下：

- [!code-console[Main](overview/samples/sample56.cmd)]

- 結尾 0 為目前的資料列的索引。

*FormView*並*DetailsView*控制項不會顯示多個資料列，因此它們不支援*ClientIDRowSuffix*屬性。

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>在資料控制項中的保存資料列選取範圍

*GridView*並*ListView*控制項可以讓使用者選取一個資料列。 在舊版 ASP.NET 中，選取具有已根據頁面上的資料列索引。 比方說，如果您選取第 1 頁上的第三個項目，然後移至第 2 頁時，就會選取該頁面上的第三個項目。

一開始只能在.NET Framework 3.5 SP1 中的動態資料專案中支援保存的選取項目。 啟用這項功能時，目前選取的項目根據項目的資料索引鍵。 這表示如果您選取第 1 頁的第三個資料列，並移至第 2 頁，不會選取在第 2 頁。 當您移回至第 1 頁時，第三個資料列仍然會選取狀態。 現在支援保存的選取*GridView*並*ListView*使用的所有專案中的控制項*EnablePersistedSelection*屬性中所示下列範例：

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>ASP.NET 的 Chart 控制項

ASP.NET*圖表*控制項擴充.NET Framework 中的資料視覺效果供應項目。 使用*圖表*控制項，您可以建立有直覺式且引人注目的圖表進行複雜的統計或財務分析的 ASP.NET 網頁。 ASP.NET*圖表*控制項以.NET Framework 3.5 SP1 版中導入成 附加元件和是.NET Framework 4 版本的一部分。

控制項包含下列功能：

- 35 種不同的圖表類型。
- 不限的數目的圖表區域、 標題、 圖例和附註。
- 各種不同的所有圖表項目的外觀設定。
- 大部分的圖表類型支援 3d。
- 可以自動配合資料點周圍的智慧型資料標籤。
- 帶狀線、 刻度斷層和對數縮放比例。
- 超過 50 種財務和統計公式可供資料分析與轉換。
- 簡單繫結和操作的圖表資料。
- 常見的資料格式，例如日期、 時間和貨幣的支援。
- 支援互動性和事件驅動的自訂，包括用戶端按一下事件使用 Ajax。
- 狀態管理。
- 二進位資料流。

下圖顯示範例的財務 ASP.NET 的 Chart 控制項所產生的圖表。

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

圖 2: ASP.NET 圖表控制項範例

如需詳細範例，示範如何使用 ASP.NET 的 Chart 控制項中，下載範例程式碼上[之 Microsoft Chart Controls 的範例環境](https://go.microsoft.com/fwlink/?LinkId=128300)MSDN 網站上的頁面。 您可以找到更多社群內容範例在[Chart 控制項論壇](https://go.microsoft.com/fwlink/?LinkId=128713)。

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>圖表控制項加入 ASP.NET 網頁

下列範例示範如何新增*圖表*ASP.NET 網頁使用標記的控制項。 在範例中，*圖表*控制項產生靜態的資料點的直條圖。

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>使用 3d 圖表

*圖表*控制項包含*ChartAreas*集合，其中可以包含*ChartArea*定義特性的圖表區域的物件。 例如，若要使用 3d 圖表區域中，使用*的 [area3dstyle]* 屬性，如下列範例所示：

[!code-aspx[Main](overview/samples/sample59.aspx)]

下圖顯示的四個數列的 3d 圖表*列*圖表類型。

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

圖 3: 3d 橫條圖

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>使用刻度斷層和對數標尺

刻度斷層和對數標尺是其他兩種方式可以加入至圖表的複雜度。 這些功能專屬於每個圖表區域中的座標軸。 例如，若要使用這些功能主要 Y 軸的圖表區域上，使用*AxisY.IsLogarithmic*並*ScaleBreakStyle*中的屬性*ChartArea*物件。 下列程式碼片段示範如何使用主要 Y 軸的刻度斷層。

[!code-aspx[Main](overview/samples/sample60.aspx)]

下圖顯示具有啟用刻度斷層的 Y 軸。

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

圖 4： 刻度斷層

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>QueryExtender 控制項使用的篩選資料

建立資料導向的網頁開發人員的常見工作是篩選資料。 這過去來執行建置*其中*子句中的資料來源控制項。 這種方法可能很複雜，在某些情況下*其中*語法不會讓您充分利用基礎資料庫的完整功能。

若要讓篩選更容易，新*QueryExtender* ASP.NET 4 中已加入控制項。 這個控制項可以加入至*EntityDataSource*或是*LinqDataSource*控制項，以篩選這些控制項所傳回的資料。 因為*QueryExtender*控制項要依賴 LINQ、 將資料傳送至頁面上，會導致非常有效率的作業之前在資料庫伺服器上套用篩選。

*QueryExtender*控制項支援各種不同的篩選選項。 下列各節說明這些選項，並提供範例，示範如何使用它們。

#### <a name="search"></a>搜尋

搜尋選項時， *QueryExtender*控制項中指定的欄位執行搜尋。 在下列範例中，控制項使用的 TextBoxSearch 控制項和其內容的搜尋中所輸入的文字`ProductName`並`Supplier.CompanyName`傳回的資料中的資料行*LinqDataSource*控制項。

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>範圍

範圍選項類似於 [搜尋] 選項，但指定一組值來定義範圍。 在下列範例中， *QueryExtender*控制搜尋`UnitPrice`傳回的資料中的資料行*LinqDataSource*控制項。 範圍是讀取自 TextBoxFrom 和 TextBoxTo 控制項在頁面上。

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

屬性的 [運算式] 選項可讓您定義的屬性值的比較。 如果運算式評估為 *，則為 true*，會傳回正在檢查的資料。 在下列範例中， *QueryExtender*控制項來比較中的資料篩選資料`Discontinued`從 CheckBoxDiscontinued 控制項在頁面上的資料行的值。

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

最後，您可以指定要搭配使用的自訂運算式*QueryExtender*控制項。 此選項可讓您呼叫的函式中定義自訂篩選器邏輯的頁面。 下列範例示範如何以宣告方式指定 使用自訂的運算式*QueryExtender*控制項。

[!code-aspx[Main](overview/samples/sample64.aspx)]

下列範例示範自訂函式所叫用*QueryExtender*控制項。 在此情況下，而不是使用包含的資料庫查詢*其中*子句，將程式碼使用 LINQ 查詢來篩選資料。

[!code-csharp[Main](overview/samples/sample65.cs)]

下列範例將示範中所用的只有一個運算式*QueryExtender*控制項一次。 不過，您可以包含多個運算式內*QueryExtender*控制項。

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>Html 編碼的程式碼運算式

有些的 ASP.NET 網站 （特別是使用 ASP.NET MVC) 依賴使用`<%` =  `expression %>` （通常稱為 「 程式碼區塊 」） 的語法，將一些文字寫入回應。 當您使用程式碼運算式時，很容易忘了將 HTML 編碼的文字，如果文字是來自使用者輸入，它可以保留頁面開啟 （跨網站指令碼） 的 XSS 攻擊。

ASP.NET 4 引進下列新的程式碼運算式語法：

[!code-aspx[Main](overview/samples/sample66.aspx)]

此語法會使用 HTML 編碼預設，寫入回應時。 這個新的運算式有效地將資料轉譯為下列：

[!code-aspx[Main](overview/samples/sample67.aspx)]

例如， &lt;%: 要求 ["u"] %&gt;執行值的 HTML 編碼*要求 ["u"]*。

這項功能的目標是讓舊語法的所有執行個體取代新語法，因此，您不強制每個步驟，決定要使用哪一個。 不過，一些情況下所輸出的文字是 html 或已編碼，在此情況下這可能會導致重複編碼。

針對這些情況下，ASP.NET 4 引進了新的介面*IHtmlString*的具象實作，以及*HtmlString*。 這些類型的執行個體可讓您表示，傳回的值是已正確編碼 （或否則檢查） 顯示為 HTML，並，因此值不應該經過 HTML 編碼一次。 例如，下列不應該 （和不） 編碼的 HTML:

[!code-aspx[Main](overview/samples/sample68.aspx)]

ASP.NET MVC 2 的 helper 方法已在已更新為使用這種新語法，使它們不雙重編碼，但只當您執行 ASP.NET 4。 這種新語法無法運作時執行的應用程式使用 ASP.NET 3.5 SP1。

請注意這不保證 XSS 攻擊的保護。 例如，使用不在引號中的屬性值的 HTML 可以包含仍然容易受到影響的使用者輸入。 請注意，ASP.NET 控制項和 ASP.NET MVC 協助程式的輸出一律包含屬性值引號括起來，這是建議的方法。

同樣地，此語法不會執行 JavaScript 的編碼，例如當您建立 JavaScript 字串，根據使用者輸入。

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>專案範本的變更

在舊版 ASP.NET 中，當您使用 Visual Studio 來建立新的網站專案或 Web 應用程式專案，產生的專案包含只 Default.aspx 頁面上，預設值`Web.config`檔案，而`App_Data`資料夾，如下列所示圖：

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Visual Studio 也支援空白網站專案型別，不包含任何檔案根本，如下圖所示：

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

結果是針對初學者，有極少的指引如何建置生產 Web 應用程式。 因此，ASP.NET 4 將介紹三個新的範本、 一個用於空的 Web 應用程式專案，以及分別指派給 Web 應用程式和網站專案。

#### <a name="empty-web-application-template"></a>空的 Web 應用程式範本

顧名思義，空白 Web 應用程式範本是精簡的 Web 應用程式專案。 選取此專案範本從 Visual Studio [新增專案] 對話方塊中，在下圖所示：

[![](overview/_static/image7.png)](overview/_static/image6.png)

([按一下以檢視完整大小的影像](overview/_static/image8.png))

當您建立空白的 ASP.NET Web 應用程式時，Visual Studio 會建立下列資料夾配置：

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

這是從舊版 ASP.NET 中，有一個例外狀況類似的空白網站配置。 在 Visual Studio 2010 中，空的 Web 應用程式和空白網站專案都包含下列最小`Web.config`檔案，其中包含 Visual Studio 用來識別專案目標 framework 的資訊：

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

如果沒有這麼做*targetFramework*屬性，若要開啟較舊的應用程式時，保留相容性目標.NET Framework 2.0 的 Visual Studio 預設值。

#### <a name="web-application-and-web-site-project-templates"></a>Web 應用程式和網站專案範本

其他兩個新專案範本所隨附的 Visual Studio 2010 包含重大變更。 下圖顯示當您建立新的 Web 應用程式專案建立專案配置。 （網站專案的版面配置是幾乎完全相同）。

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

專案包含不建立在舊版本中的檔案數目。 此外，新的 Web 應用程式專案已使用 基本成員資格功能，可讓您快速地開始在保護新的應用程式的存取。 加入此項目，因為`Web.config`檔案為新的專案包含用來設定成員資格、 角色和設定檔的項目。 下列範例所示`Web.config`新的 Web 應用程式專案檔。 (在此情況下， *roleManager*已停用。)

[![](overview/_static/image13.png)](overview/_static/image12.png)

([按一下以檢視完整大小的影像](overview/_static/image14.png))

專案也包含第二個`Web.config`檔案中`Account`目錄。 第二個組態檔可用來安全存取的 ChangePassword.aspx 頁面非登入的使用者。 下列範例顯示的第二個內容`Web.config`檔案。

![](overview/_static/image15.png)

建立新的專案範本中預設的頁面也會包含比在舊版中的其他內容。 專案包含預設主版頁面和 CSS 檔案，並預設網頁 (Default.aspx) 時，已設定為使用主版頁面上，依預設。 結果是，當您第一次執行 Web 應用程式或網站上，預設 （首頁） 網頁已經功能。 事實上，它是類似於您啟動新的 MVC 應用程式時，您會看到預設頁面。

[![](overview/_static/image17.png)](overview/_static/image16.png)

([按一下以檢視完整大小的影像](overview/_static/image18.png))

這些變更的專案範本的目的是要提供有關如何開始建置新的 Web 應用程式的指引。 語意不正確、 嚴格的 XHTML 1.0 相容標記與指定使用 CSS 的版面配置，將範本中的頁面會代表建置 ASP.NET 4 Web 應用程式的最佳作法。 預設頁面也會有兩欄版面配置，您可以輕鬆地自訂。

例如，想像一下，新的 Web 應用程式想要變更的某些色彩，並插入您的公司標誌來取代我的 ASP.NET 應用程式標誌。 若要這樣做，您會建立新的目錄下`Content`來儲存您的標誌影像：

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

若要將影像加入至頁面中，您再開啟`Site.Master`檔案中，尋找 我的 ASP.NET 應用程式文字的已定義，其中，並將它取代為*映像*項目其*src*屬性設定為新的標誌映像，如下列範例所示：

[![](overview/_static/image21.png)](overview/_static/image20.png)

([按一下以檢視完整大小的影像](overview/_static/image22.png))

然後，您可以進入 Site.css 檔案，並修改 CSS 類別定義，以變更頁面背景色彩，以及的標頭。

這些變更的結果是，您可以顯示自訂的首頁上，使用少量的工作：

[![](overview/_static/image24.png)](overview/_static/image23.png)

([按一下以檢視完整大小的影像](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>CSS 增強功能

ASP.NET 4 中重點之一就是工作的協助呈現 HTML 相容的最新的 HTML 標準。 這包括 ASP.NET Web 伺服器控制項的 CSS 樣式的使用方式的變更。

#### <a name="compatibility-setting-for-rendering"></a>轉譯的相容性設定

根據預設，Web 應用程式或 Web 站台為目標的.NET Framework 4 中，當*controlRenderingCompatibilityVersion*屬性*頁*元素設定為"4.0"。 此項目會定義在電腦層級`Web.config`檔案，並根據預設會套用到所有的 ASP.NET 4 應用程式：

[!code-xml[Main](overview/samples/sample69.xml)]

值*controlRenderingCompatibility*是一個字串，可讓潛在的新版本定義在未來的版本。 在目前的版本中，此屬性支援下列值：

- "3.5". 這個設定表示舊版轉譯和標記。 控制項所呈現的標記是 100%相容，以及設定*xhtmlConformance*屬性會接受。
- "4.0". 如果屬性具有這項設定，ASP.NET Web 伺服器控制項執行下列作業：
- *XhtmlConformance*屬性一律會視為"Strict"。 如此一來，控制項呈現的 XHTML 1.0 Strict 的標記。
- 停用非輸入控制項不再轉譯不正確的樣式。
- *div*隱藏的欄位前後的項目現在的樣式，因此它們不會干擾使用者建立 CSS 規則。
- 功能表控制項呈現語意正確且符合協助工具指導方針的標記。
- 驗證控制項不會轉譯內嵌樣式。
- 控制先前呈現的框線 ="0"(衍生自 ASP.NET 控制項*表格*控制項，以及 ASP.NET*映像*控制項) 不再呈現此屬性。

#### <a name="disabling-controls"></a>停用控制項

在 ASP.NET 3.5 SP1 和舊版中，架構會呈現*停用*屬性 (attribute) 的 HTML 標記中任何控制其*已啟用*屬性設定為*false*。 不過，根據 HTML 4.01 規格，只有*輸入*項目應具有此屬性。

在 ASP.NET 4 中，您可以設定*controlRenderingCompatabilityVersion* "3.5"，如下列範例所示的屬性：

[!code-xml[Main](overview/samples/sample70.xml)]

您可以建立標記*標籤*如下所示，它會停用控制項的控制項：

[!code-aspx[Main](overview/samples/sample71.aspx)]

*標籤*控制項會轉譯下列 HTML:

[!code-html[Main](overview/samples/sample72.html)]

在 ASP.NET 4 中，您可以設定*controlRenderingCompatabilityVersion*設為"4.0"。 在此情況下，只會控制轉譯的*輸入*項目會呈現*停用*屬性時的控制項*已啟用*屬性設定為*false*. 不會轉譯 HTML 的控制項*輸入*項目改為轉譯*類別*參考可用來定義了停用控制項的 CSS 類別的屬性。 例如，*標籤*先前範例所示的控制項就會產生下列標記：

[!code-html[Main](overview/samples/sample73.html)]

這個控制項指定類別的預設值為"aspNetDisabled"。 不過，您可以變更此預設值，藉由設定靜態*DisabledCssClass*靜態屬性*WebControl*類別。 對控制項開發人員，要使用特定控制項的行為也可以定義使用*SupportsDisabledAttribute*屬性。

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>隱藏 div 周圍隱藏欄位的項目

ASP.NET 2.0 和更新版本的呈現系統特定隱藏的欄位 (例如*隱藏*用來儲存檢視狀態資訊的項目) 內*div*為了符合 XHTML 標準的項目。 不過，這可能會造成問題時的 CSS 規則會影響*div*頁面上的項目。 例如，可能會導致頁面中出現一個像素列大約隱藏*div*項目。 在 ASP.NET 4 中， *div*括住 ASP.NET 產生的隱藏的欄位的項目新增 CSS 類別參考，如下列範例所示：

[!code-html[Main](overview/samples/sample74.html)]

然後，您可以定義 CSS 類別，僅適用於*隱藏*由 ASP.NET，產生如下列範例所示的項目：

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>樣板化控制項呈現的外部資料表

根據預設，下列的 ASP.NET Web 伺服器控制項支援範本自動包裝在外部資料表，可用來套用內嵌樣式：

- *FormView*
- *登入*
- *PasswordRecovery*
- *ChangePassword*
- *精靈*
- *CreateUserWizard*

名為的新屬性*RenderOuterTable*已新增至可讓外部資料表從標記中移除這些控制項。 例如，請考慮下列的範例*FormView*控制項：

[!code-aspx[Main](overview/samples/sample76.aspx)]

此標記會轉譯到頁面上，其中包含 HTML 表格的下列輸出：

[!code-html[Main](overview/samples/sample77.html)]

若要防止資料表所呈現，您可以設定*FormView*控制項的*RenderOuterTable*屬性，如下列範例所示：

[!code-aspx[Main](overview/samples/sample78.aspx)]

前一個範例會呈現下列的輸出，無需*表格*， *tr*，並*td*項目：

> 內容


這項增強功能可讓您更輕鬆地利用 CSS，控制項內容的樣式因為沒有非預期的標記控制項所呈現。

> [!NOTE]
> 請注意這項變更會停用支援自動格式化函式，在 Visual Studio 2010 設計工具中，因為目前已不再*資料表*可以主控樣式屬性的自動格式化選項所產生的項目。


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>ListView 控制項的增強功能

*ListView*控制項已在 ASP.NET 4 中使用的工作變得更容易。 較早版本的控制項需要您指定包含具有已知識別碼的伺服器控制項的版面配置範本 下列標記示範如何使用的典型範例*ListView* ASP.NET 3.5 中的控制項。

[!code-aspx[Main](overview/samples/sample79.aspx)]

在 ASP.NET 4 中， *ListView*控制項不需要的版面配置範本。 在上述範例中所顯示的標記，都可以使用下列標記取代：

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>CheckBoxList 和 RadioButtonList 控制項的增強功能

在 ASP.NET 3.5 中，您可以指定版面配置*CheckBoxList*並*RadioButtonList*使用下列兩項設定：

- *流程*。 控制項呈現*跨越*包含其內容的項目。
- *資料表*。 控制項呈現*資料表*項目來包含其內容。

下列範例會顯示這些控制項的每個標記。

[!code-aspx[Main](overview/samples/sample81.aspx)]

根據預設，控制項會呈現 HTML 如下所示：

[!code-html[Main](overview/samples/sample82.html)]

因為這些控制項包含的項目清單，來呈現語意正確的 HTML，其應該會轉譯它們使用 HTML 清單的內容 (*li*) 項目。 這可讓您更輕鬆讀取網頁使用輔助技術，並讓您更輕鬆地使用 CSS 樣式控制項的使用者。

在 ASP.NET 4 中， *CheckBoxList*並*RadioButtonList*控制項支援下列新值*RepeatLayout*屬性：

- *OrderedList* – 內容會轉譯成*li*內的項目*ol*項目。
- *UnorderedList* – 內容會轉譯成*li*內的項目*ul*項目。

下列範例示範如何使用這些新的值。

[!code-aspx[Main](overview/samples/sample83.aspx)]

上述標記會產生下列 HTML:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> 請注意是否您設定*RepeatLayout*要*OrderedList*或*UnorderedList*，則*Flow*屬性無法再使用，並將如果屬性已設定您的標記或程式碼中，會擲回執行階段例外狀況。 屬性會有任何值，因為這些控制項的視覺化版面配置改為使用 CSS 定義。


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>功能表控制項的改善

ASP.NET 4 以前* 功能表*呈現一系列 HTML 表格的控制項。 這變得更難套用 CSS 樣式以外設定內嵌屬性，且也不符合協助工具標準。

在 ASP.NET 4 中，控制項現在使用的未排序的清單和清單項目所組成的語意標記的 HTML 呈現。 下列範例示範針對 ASP.NET 網頁中的標記* 功能表*控制項。

[!code-aspx[Main](overview/samples/sample85.aspx)]

當頁面呈現時，控制項就會產生下列 HTML ( *onclick*為了清楚起見已省略的程式碼):

[!code-html[Main](overview/samples/sample86.html)]

除了轉譯改進鍵盤瀏覽功能表的已改善使用焦點管理。 當* 功能表*控制項取得焦點時，您可以使用方向鍵來瀏覽項目。 * 功能表*控制項現在也將可存取豐富的網際網路應用程式 (ARIA) 角色和屬性下列[wing](http://www.w3.org/TR/wai-aria-practices/#menu "功能表 ARIA 指導方針")改善存取範圍。

功能表控制項的樣式會呈現的樣式區塊頂端的頁面上，而不是依照呈現的 HTML 項目中。 如果您想要全權掌控控制項樣式時，您可以設定的新*IncludeStyleBlock*屬性設*false*，在此情況下不會發出樣式區塊。 使用這個屬性的一個方式是使用 Visual Studio 設計工具中的自動格式化功能，來設定功能表的外觀。 您可以執行的頁面、 開啟網頁原始檔，並接著將呈現的樣式區塊複製到外部 CSS 檔案。 在 Visual Studio 中，復原樣式設定和設定*IncludeStyleBlock*要*false*。 結果是功能表外觀，定義在外部樣式表中使用樣式。

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>精靈和 CreateUserWizard 控制項

ASP.NET*精靈*並*CreateUserWizard*控制項都支援可讓您定義其轉譯 HTML 的範本。 (*CreateUserWizard*衍生自*精靈*。)下列範例顯示完整的樣板化的標記*CreateUserWizard*控制項：

[!code-aspx[Main](overview/samples/sample87.aspx)]

控制項會呈現 HTML 如下所示：

[!code-html[Main](overview/samples/sample88.html)]

在 ASP.NET 3.5 SP1 中，雖然您可以變更範本內容中，您仍有受限制的控制權的輸出*精靈*控制項。 在 ASP.NET 4 中，您可以建立*LayoutTemplate*範本和 insert*預留位置*控制項 （使用保留的名稱），指定您要如何*精靈控制項*來呈現。 下列範例會示範這個：

[!code-aspx[Main](overview/samples/sample89.aspx)]

此範例包含下列名為中的預留位置*LayoutTemplate*項目：

- *headerPlaceholder* – 在執行階段，將會取代運算式的內容*HeaderTemplate*項目。
- *sideBarPlaceholder* – 在執行階段，將會取代運算式的內容*SideBarTemplate*項目。
- *wizardStepPlaceHolder* – 在執行階段，將會取代運算式的內容*WizardStepTemplate*項目。
- *navigationPlaceholder* – 在執行階段，這會取代您已定義的任何瀏覽範本。

在範例中，會使用預留位置的標記會轉譯下列 HTML （而不需要在範本中實際定義的內容）：

[!code-html[Main](overview/samples/sample90.html)]

只是現在沒有使用者定義的 html*跨越*項目。 (我們預期這在未來版本中，甚至*跨越*項目不會呈現出來。)這可讓您完整控制所產生的幾乎所有內容*精靈*控制項。

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC 是在 2009 年 3 月，做為附加元件架構引進到 ASP.NET 3.5 SP1。 Visual Studio 2010 包含 ASP.NET MVC 2，其中包含新特色與功能。

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>區域支援

區域可讓您群組控制器和檢視表到大型應用程式相對的獨立於其他區段的區段。 每個區域可以實作為個別的 ASP.NET MVC 專案，然後主應用程式參考。 這有助於管理複雜度，當您建置大型應用程式，並讓多個小組一起工作，在單一應用程式更容易。

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>資料註解屬性驗證支援

*DataAnnotations*屬性可以讓您附加至模型的驗證邏輯使用中繼資料屬性。 *DataAnnotations* ASP.NET 3.5 SP1 中的 ASP.NET Dynamic Data 中導入的屬性。 這些屬性已整合到預設模型繫結器，並提供中繼資料導向的方式，來驗證使用者輸入。

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>樣板化 Helper

樣板化 helper 可讓您自動產生關聯的編輯，並顯示與資料類型的範本。 比方說，您可以使用範本協助程式，來指定日期選擇器 UI 項目，如自動呈現*System.DateTime*值。 這是類似於 ASP.NET Dynamic Data 中的欄位範本。

*Html.EditorFor*並*Html.DisplayFor* helper 方法轉譯標準的資料類型以及複雜的物件，具有多個屬性，具有內建支援。 它們也可讓您套用資料註解屬性，例如自訂轉譯*DisplayName*並*ScaffoldColumn*來*ViewModel*物件。

通常您會想要自訂 UI 協助程式更進一步的輸出，並擁有完整控制權產生的內容。 *Html.EditorFor*並*Html.DisplayFor* helper 方法支援此使用樣板化機制，可讓您定義可以覆寫的外部範本和控制項轉譯的輸出。 範本可以個別轉譯的類別。

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Dynamic Data

動態資料引進在 2008 年發行的.NET Framework 3.5 SP1。 這項功能提供了許多增強功能，來建立資料驅動應用程式，包括下列：

- 快速建置資料導向網站 RAD 體驗。
- 資料模型中定義的條件約束為基礎的自動驗證。
- 能夠輕鬆地變更針對欄位中所產生的標記*GridView*並*DetailsView*使用動態資料專案一部分的欄位範本的控制項。

> [!NOTE]
> 請注意，如需詳細資訊，請參閱 <<c0> [ 動態資料文件](https://msdn.microsoft.com/library/cc488545.aspx)MSDN Library 中。


針對 ASP.NET 4 中，動態資料已經過增強，讓開發人員快速建置資料導向網站的更強大。

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>對於現有的專案中啟用動態資料

隨附於.NET Framework 3.5 SP1 的動態資料功能帶入新的功能，如下所示：

- 欄位樣板 – 這些會提供資料類型為基礎的範本資料繫結控制項。 欄位範本會提供更簡單的方式，來自訂比使用範本欄位的每個欄位的資料控制項的外觀。
- 驗證-動態資料可讓您在資料類別上使用屬性來指定必要的欄位、 範圍檢查、 型別檢查、 使用規則運算式比對的模式的常見案例的驗證和自訂驗證。 資料控制項，強制執行驗證。

不過，這些功能有下列需求：

- 資料存取層，就必須根據 Entity Framework 或 LINQ to SQL。
- 唯一的資料來源支援這些功能的控制項*EntityDataSource*或是*LinqDataSource*控制項。
- 功能需要 Web 專案以支援此功能所需的所有檔案使用的動態資料或動態資料實體範本所建立的。

在 ASP.NET 4 中的動態資料支援，主要目的是啟用任何 ASP.NET 應用程式的動態資料的新功能。 下列範例顯示可以利用動態資料功能的現有網頁中的控制項的標記。

[!code-aspx[Main](overview/samples/sample91.aspx)]

中頁面的程式碼，您必須加入下列的程式碼，若要讓這些控制項的動態資料支援：

[!code-csharp[Main](overview/samples/sample92.cs)]

當*GridView*控制項處於編輯模式、 動態的資料會自動驗證輸入的資料是正確的格式。 如果不存在，則會顯示一則錯誤訊息。

這項功能也提供其他好處，例如能夠指定預設值插入模式。 沒有動態的資料，來實作的預設值 欄位中，您必須附加至事件，找不到控制項 (使用*FindControl*)，並設定其值。 在 ASP.NET 4 中， *EnableDynamicData*呼叫支援第二個參數，可讓您傳遞，在物件上的任何欄位的預設值，在此範例中所示：

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>宣告式 DynamicDataManager 控制項語法

*DynamicDataManager*控制項已經過增強，以便您可以設定以宣告方式，如同在 ASP.NET 中，而不是只在程式碼中大部分的控制項。 標記*DynamicDataManager*控制項看起來如下列範例所示：

[!code-aspx[Main](overview/samples/sample94.aspx)]

這個標記可讓 GridView1 控制項中所參考的動態資料行為*度*一節*DynamicDataManager*控制項。

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>實體範本

實體範本提供自訂的資料版面配置，而不需要您建立自訂頁面的新方式。 頁面上，使用範本*FormView*控制項 (而非*DetailsView*控制項，在舊版的動態資料的頁面範本中所使用) 和*DynamicEntity*要呈現實體範本的控制項。 這可讓您更充分掌控 Dynamic Data 所呈現的標記。

下列清單顯示新的專案目錄配置，其中包含實體範本：

[!code-console[Main](overview/samples/sample95.cmd)]

`EntityTemplate`目錄包含如何顯示的資料模型物件的範本。 根據預設，物件會使用呈現`Default.ascx`範本，可看起來像是所建立的標記的標記*DetailsView* ASP.NET 3.5 SP1 中的 Dynamic Data 所使用的控制項。 下列範例顯示的標記`Default.ascx`控制項：

[!code-aspx[Main](overview/samples/sample96.aspx)]

若要變更整個網站的外觀及操作，可以編輯預設範本。 範本顯示、 編輯和插入作業。 新的範本可以加入根據資料物件的名稱以變更的外觀與風格只是一種類型的物件。 例如，您可以新增下列範本：

[!code-console[Main](overview/samples/sample97.cmd)]

範本可能包含下列標記：

[!code-aspx[Main](overview/samples/sample98.aspx)]

新的實體範本時，會顯示在頁面上，使用新*DynamicEntity*控制項。 在執行階段，此控制項被取代的實體範本的內容。 下列標記示範*FormView*控制中`Detail.aspx`使用的實體範本的頁面範本。 請注意*DynamicEntity*在標記中的項目。

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>新的欄位範本 Url 和電子郵件地址

ASP.NET 4 引進了兩個新的內建欄位範本，`EmailAddress.ascx`和`Url.ascx`。 這些範本用於標示欄位*EmailAddress*或是*Url*使用*DataType*屬性。 針對*EmailAddress*物件時，欄位會顯示為超連結建立利用*mailto:* 通訊協定。 當使用者按一下連結時，它會開啟使用者的電子郵件用戶端的連線，並建立基本架構的訊息。 物件的型別為*Url*會顯示為一般的超連結。

下列範例會顯示欄位會標示。

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>建立與 DynamicHyperLink 控制項的連結

動態資料會使用.NET Framework 3.5 SP1，來控制存取網站時，請參閱 < 終端使用者的 Url 中加入新的路由功能。 新*DynamicHyperLink*控制項可讓您輕鬆建置動態資料站台中的網頁的連結。 下列範例示範如何使用*DynamicHyperLink*控制項：

[!code-aspx[Main](overview/samples/sample101.aspx)]

此標記會建立指向的 「 清單 」 頁面的連結`Products`資料表中所定義的路由為基礎`Global.asax`檔案。 控制會自動使用動態資料頁面為基礎的預設資料表名稱。

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>支援的資料模型中的繼承

Entity Framework 和 LINQ to SQL 支援繼承其資料模型中。 這個範例可能是資料庫具有`InsurancePolicy`資料表。 它可能也包含`CarPolicy`並`HousePolicy`具有相同的欄位的資料表，`InsurancePolicy`然後新增更多的欄位。 動態資料已經過修改，以了解資料模型中的繼承的物件，以及繼承的資料表支援 scaffolding。

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>支援多對多關聯性 (Entity Framework)

Entity Framework 提供豐富的支援，藉由在上公開為集合關聯性的資料表之間的多對多關聯性*實體*物件。 新`ManyToMany.ascx`和`ManyToMany_Edit.ascx`欄位範本已新增至可顯示和編輯多對多關聯性中涉及的資料。

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>新的屬性，以控制顯示，並支援列舉

*DisplayAttribute*已新增，讓您進一步控制欄位的顯示方式。 *DisplayName*屬性在舊版的動態資料允許您變更用來當做標題欄位的名稱。 新*DisplayAttribute*類別可讓您指定多個選項來顯示一個欄位，例如欄位會顯示以及是否將使用欄位作為篩選條件的順序。 屬性也會提供獨立的使用中的標籤名稱的控制項*GridView*控制項，用於的名稱*DetailsView*控制項，[] 欄位中的說明文字和浮水印用於欄位 （如果欄位可接受文字輸入）。

*EnumDataTypeAttribute*類別已新增可讓您將欄位對應至列舉型別。 當您將這個屬性套用至欄位時，您會指定列舉型別。 使用新的動態資料`Enumeration.ascx`建立 UI，來顯示和編輯列舉值的欄位樣板。 範本會對應資料庫中的值給列舉型別中的名稱。

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>篩選器的增強的支援

動態資料 1.0 隨附內建布林資料行和外部索引鍵資料行的篩選器。 篩選不允許您指定是否已顯示，請將它們，或它們所顯示的順序。 新*DisplayAttribute*屬性的地址這兩個這些問題，讓您控制是否顯示資料行做為篩選條件和順序將會顯示。

其他的增強功能是篩選支援已經過重新[編寫成使用新](#0.2__QueryExtender "_QueryExtender") Web Form 的功能。 這可讓您建立篩選器，而不需要篩選器可搭配資料來源控制項的知識。 這些擴充功能，以及篩選有也已轉換成範本的控制項，可讓您新增新的。 最後， *DisplayAttribute*先前所述的類別可讓預設篩選，以覆寫，在同一個採用的方式*UIHint*允許覆寫的資料行的預設欄位樣板。

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Visual Studio 2010 Web 開發方面的改進

已增強 Visual Studio 2010 中的 web 開發更 CSS 相容性，透過 HTML 和 ASP.NET 的標記程式碼片段和新動態的 IntelliSense JavaScript 的提升產能。

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>改良的 CSS 相容性

Visual Web Developer 設計工具，Visual Studio 2010 中的有已更新並改良 CSS 2.1 標準相容性。 設計工具更好會保留的 HTML 原始檔的完整性，並且會比在舊版的 Visual Studio 更健全。 在幕後，架構也進行了改進的就是好讓未來的增強功能，在轉譯、 版面配置，與服務性。

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>HTML 和 JavaScript 程式碼片段

在 HTML 編輯器中，IntelliSense 自動完成標記名稱。 IntelliSense 程式碼片段功能會自動完成整個標籤和更多功能。 在 Visual Studio 2010 中，JavaScript 與 C# 和 Visual Basic 中，不支援在舊版的 Visual Studio 支援 IntelliSense 程式碼片段。

Visual Studio 2010 包含 200 多個程式碼片段，協助您自動完成一般 ASP.NET 和 HTML 標記，包括必要的屬性 (例如 runat ="server") 和標記特有的通用屬性 (例如*識別碼*， *DataSourceID*， *ControlToValidate*，以及*文字*)。

您可以下載其他程式碼片段，或您可以撰寫您自己片段，可封裝您或您的小組使用的常見工作的標記區塊。

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>JavaScript IntelliSense 的增強功能

在 Visual 2010 中，JavaScript IntelliSense 已重新設計，提供更豐富的編輯體驗。 IntelliSense 現在會表彰動態產生的方法這類的物件*registerNamespace*和其他 JavaScript 架構所使用的類似技術。 已改善效能，分析大型的程式庫的指令碼，並顯示幾乎沒有任何處理延遲的 IntelliSense。 為支援幾乎所有的協力廠商程式庫，並支援各種不同的編碼樣式已大幅增加相容性。 當您輸入，並立即利用 IntelliSense，現在會剖析文件註解。

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>使用 Visual Studio 2010 的 web 應用程式部署

當 ASP.NET 開發人員部署 Web 應用程式時，他們經常遇到問題，如下所示：

- 部署到共用裝載站台需要技術，例如 FTP，可能會很慢。 此外，您必須手動執行工作，例如執行 SQL 指令碼，以設定資料庫，而且您必須變更 IIS 設定，例如做為應用程式中設定虛擬目錄資料夾。
- 在企業環境中，除了部署 Web 應用程式檔案中，系統管理員通常必須修改 ASP.NET 組態檔和 IIS 設定。 資料庫管理員必須執行一系列的 SQL 指令碼，以取得執行的應用程式資料庫。 這類安裝大量的人力，通常需要較小時才能完成，且必須仔細記錄。

Visual Studio 2010 包含的技術，以解決這些問題，並讓您順暢地部署 Web 應用程式。 這些技術的其中之一是 IIS Web Deployment Tool (MsDeploy.exe)。

Visual Studio 2010 中的 web 部署功能包括下列重點：

- Web 封裝
- Web.config 轉換
- 資料庫部署
- 單鍵發行為 Web 應用程式

下列各節提供有關這些功能的詳細資料。

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Web 封裝

Visual Studio 2010 使用 MSDeploy 工具，來建立您的應用程式，這指的壓縮 (.zip) 檔案*網頁套件*。 封裝檔案會包含您的應用程式加上下列內容的相關中繼資料：

- IIS 設定，其中包括應用程式集區設定、 錯誤頁面設定，等等。
- 實際的 Web 內容，其中包含 Web 網頁、 使用者控制項、 靜態內容 （映像和 HTML 檔案），等等。
- SQL Server 資料庫結構描述和資料。
- 安全性憑證，將安裝在 GAC、 登錄設定等等的元件。

網頁套件可以複製到任何伺服器，並再使用 IIS 管理員以手動方式安裝。 或者，用於自動化部署，封裝可以安裝使用命令列命令，或使用部署 Api。

Visual Studio 2010 提供內建的 MSBuild 工作與建立 Web 封裝的目標。 如需詳細資訊，請參閱 < [ASP.NET Web 應用程式專案部署概觀](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx)MSDN 網站上並[10 + 20 的原因，為什麼您應該建立網頁套件](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html)Vishal Joshi 部落格上。

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Web.config 轉換

部署 Web 應用程式，Visual Studio 2010 推出[XML 文件轉換 (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)，這是一個功能，可讓您轉換`Web.config`開發設定生產環境設定的檔案。 轉換設定會指定於名為轉換檔案`web.debug.config`， `web.release.config`，依此類推。 （這些檔案的名稱符合 MSBuild 組態）。轉換檔包含只變更，您必須先部署`Web.config`檔案。 您可以使用簡單的語法，以指定所做的變更。

下列範例會顯示部分`web.release.config`部署您的發行設定檔可能會產生。 取代關鍵字，在範例中所指定的是，在部署期間*connectionString*中的節點`Web.config`檔案將會取代此範例中所列的值。

[!code-xml[Main](overview/samples/sample102.xml)]

如需詳細資訊，請參閱 < [Web 應用程式專案部署的 Web.config 轉換語法](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx)msdn <a id="0.2_a"> </a>的網站和[Web 部署： Web.Config 轉換](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)Vishal Joshi 部落格上。

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>資料庫部署

Visual Studio 2010 的部署套件可以包含在 SQL Server 資料庫上的相依性。 封裝定義的一部分中,，您可以提供連接字串的來源資料庫。 當您建立的 Web 套件時，Visual Studio 2010 建立 SQL 指令碼的資料庫結構描述以及選擇性的資料，並再將這些封裝。 您也可以提供自訂的 SQL 指令碼，並指定在伺服器應該執行的順序。 您可以在部署期間提供適用於目標伺服器的連接字串部署程序接著會使用此連接字串執行的指令碼所建立的資料庫結構描述並加入資料。

此外，使用單鍵發行，您可以設定部署至應用程式發佈到遠端共用裝載站台時，直接發佈您的資料庫。 如需詳細資訊，請參閱[如何： 部署資料庫與 Web 應用程式專案](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx)MSDN 網站上並[VS 2010 中的資料庫部署](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)Vishal Joshi 部落格上。

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>單鍵發行為 Web 應用程式

Visual Studio 2010 也可讓您使用 IIS 遠端管理服務發佈至遠端伺服器的 Web 應用程式。 針對您的裝載帳戶或測試伺服器或臨時伺服器，您可以建立發行設定檔。 每個設定檔可以安全地儲存適當的認證。 然後您可以部署到任何目標伺服器，只要按一下，使用 Web 單鍵發行工具列。 Visual Studio 2010 中，您也可以使用 MSBuild 命令列發佈。 這可讓您設定持續整合模型中包含發行您小組的建置環境。

如需詳細資訊，請參閱 <<c0> [ 如何： 部署 Web 應用程式專案使用單鍵發行和 Web Deploy](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) MSDN 網站上並[Web 1-按一下發佈與 VS 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) Vishal Joshi 部落格上。 若要檢視 Visual Studio 2010 中的 Web 應用程式部署相關的視訊簡報，請參閱[VS 2010 進行 Web 開發人員預覽](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html)Vishal Joshi 部落格上。

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>資源

欞眭寍鯚提供 ASP.NET 4 和 Visual Studio 2010 的其他資訊。

- [ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) — MSDN 網站上的 ASP.NET 4 的官方文件。
- [https://www.asp.net/](https://www.asp.net/) -ASP.NET 小組自己的網站。
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) 並[ASP.NET 動態資料內容對應](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx)— ASP.NET 小組網站和 ASP.NET Dynamic Data 的官方文件中的線上資源。
- [https://www.asp.net/ajax/](../../ajax/index.md) — ASP.NET Ajax 開發主要 Web 資源。
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) -Visual Web Developer 小組部落格，其中包含 Visual Studio 2010 中的功能的相關資訊。
- [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) — ASP.NET 的 preview 版本的主要 Web 資源。

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>免責聲明

這是一份初稿，內容在本文所述的軟體於正式商業發行前都可能有所更動。

本文件中的資訊表示直到文件發行日前 Microsoft Corporation 針對問題的看法。 Microsoft 必須因應不斷變化的市場狀況，因此本文件不代表 Microsoft 的保證，且 Microsoft 不保證這些資訊在文件發行後的正確性。

本技術白皮書僅供參考。 MICROSOFT 對本文件中的資訊不提供任何明示、暗示或法定擔保。

承諾遵守所有適用的著作權法是使用者的責任。 著作權法沒有針對某種權利加以限制，但在未獲得 Microsoft Corporation 書面同意的情況下，本文件的任何部分不得複製、以檢索系統存放或擷取、以任何形式或方法傳送 (電子、機械、影像複製、錄音或其他任何方法)、或基於任何其他不良意圖。

本文件所提及的主要事務，Microsoft 得擁有專利、專利應用程式、商標、著作權或其他智慧財產權。 除了 Microsoft 於授權合約書中書面提供的之外，本文件所述內容並未賦予您這些專利、商標、著作權、或其他智慧財產的任何授權或使用權利。

除非另有說明，範例公司、 組織、 產品、 網域名稱、 電子郵件地址、 標誌、 人物、 地點及事件本文所述之屬虛構，以及與任何真實公司、 組織、 產品、 網域名稱、 電子郵件沒有關聯地址、 標誌、 人員、 位置或事件是純屬巧合。

© 2009 Microsoft Corporation. 著作權所有，並保留一切權利。

Microsoft 和 Windows 是 Microsoft Corporation 在美國及/或其他國家/地區的註冊商標或商標。

本文件中所提實際公司和產品，可能為各所有人所有之商標。
