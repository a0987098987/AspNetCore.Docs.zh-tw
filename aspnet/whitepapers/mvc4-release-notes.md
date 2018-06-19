---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 本文件說明 ASP.NET MVC 4 的版本。
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: dbcea6090a0376b8732e02c0891721672bfe50f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898577"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> 本文件說明 ASP.NET MVC 4 的版本。


- [安裝注意事項](#_Toc303253802)
- [文件 (英文)](#_Toc303253803)
- [支援](#_Toc303253804)
- [軟體需求](#_Toc303253805)
- [ASP.NET MVC 4 的新功能](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [預設專案範本的增強功能](#_Toc303253808)
    - [行動裝置的專案範本](#_Toc303253809)
    - [顯示模式](#_Toc303253810)
    - [jQuery Mobile，檢視切換程式，並覆寫瀏覽器](#_Toc303253811)
    - [非同步控制器的工作支援](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [資料庫移轉](#_Toc303253818)
    - [空白專案範本](#_Toc303253819)
    - [將控制器加入的任何專案資料夾](#_Toc303253820)
    - [統合和縮製](#_Toc303253821)
    - [啟用從 Facebook 和其他站台使用 OAuth 和 OpenID 登入](#_Toc303253822)
- [ASP.NET MVC 3 專案升級至 ASP.NET MVC 4](#_Toc303253806)
- [從 ASP.NET MVC 4 發行候選版本變更](#_Toc303253817)
- [已知的問題和重大變更](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>安裝注意事項

可以從安裝 ASP.NET MVC 4 for Visual Studio 2010 [ASP.NET MVC 4 首頁](../mvc/mvc4.md)使用 Web Platform Installer。

我們建議您解除安裝 ASP.NET MVC 4 再安裝 ASP.NET MVC 4 的任何先前安裝的預覽。 您可以與 ASP.NET MVC 4 升級的 ASP.NET MVC 4 Beta 和發行候選版本，卻不用解除安裝。

此版本不相容的任何發行前版本的.NET Framework 4.5。 您個別必須升級至最終版本再安裝 ASP.NET MVC 4 的任何已安裝的預覽版本的.NET Framework 4.5。

ASP.NET MVC 4 可以是已安裝並執行由並存使用 ASP.NET MVC 3。

<a id="_Toc303253803"></a>
## <a name="documentation"></a>文件

ASP.NET mvc 的文件位於 MSDN 網站，位於下列 URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

教學課程和其他關於 ASP.NET MVC 的資訊位於 ASP.NET 網站的 MVC 4 頁 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md))。

<a id="_Toc303253804"></a>
## <a name="support"></a>支援

完全支援 ASP.NET MVC 4。 如果您有使用此版本的相關問題您也可以張貼它們至 ASP.NET MVC 論壇 ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx))，其中的 ASP.NET 社群成員可經常提供非正式的支援。

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>軟體需求

Visual Studio 的 ASP.NET MVC 4 元件需要 PowerShell 2.0 和 Service Pack 1 的 Visual Studio 2010 或 Visual Web Developer Express 2010 Service Pack 1。

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>ASP.NET MVC 4 的新功能

本節說明已引進的功能在 ASP.NET MVC 4 版本。

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4 包含 ASP.NET Web API，新的架構，用於建立可達到廣泛的包括瀏覽器和行動裝置的用戶端的 HTTP 服務。 ASP.NET Web 應用程式開發介面也是用於建立 RESTful 服務的理想平台。

ASP.NET Web API 包含下列功能的支援：

- **現代 HTTP 程式設計模型：** 直接存取及管理 HTTP 要求和回應您 Web 應用程式開發介面使用新的強型別 HTTP 物件模型。 相同程式設計模型和 HTTP 管線是用戶端透過新的對稱可用*HttpClient*型別。
- **完整支援路由：** ASP.NET Web API 支援完整的 ASP.NET 路由，路由參數和條件約束包括的路由功能集。 此外，使用簡單的慣例來對應至 HTTP 方法的動作。
- **內容交涉：** 用戶端和伺服器可一起運作來判斷正確的格式，從 web API 所傳回的資料。 ASP.NET Web 應用程式開發介面支援預設 xml、 JSON 和表單 URL 編碼格式，而且您可以擴充此一支援藉由新增您自己的格式器，或甚至取代預設內容交涉的策略。
- **模型繫結和驗證：** 模型繫結器提供一個簡單的方式，從 HTTP 要求的各種組件中擷取資料，並將這些訊息部分轉換成.NET 物件可由 Web API 的動作。 資料註解為基礎的動作參數也會執行驗證。
- **篩選條件：** ASP.NET Web API 支援篩選器包括知名的篩選條件，例如 *[Authorize]* 屬性。 您可以撰寫，並插入您自己的篩選動作、 授權和例外狀況處理。
- **在查詢組合：** 使用 *[Queryable]* 上傳回的動作篩選條件屬性*IQueryable*可啟用查詢您的 web 應用程式開發介面透過 OData 查詢轉換的支援。
- **改良的測試能力：** 而不是靜態內容物件中設定 HTTP 詳細資料，web 應用程式開發介面執行工作的執行個體*HttpRequestMessage*和*HttpResponseMessage*。 建立單元測試專案，以及您的 Web API 專案開始快速撰寫單元測試的 Web API 功能。
- **程式碼為基礎的組態：** 等作業只透過程式碼會完成的 ASP.NET Web API 設定、 清除離開您的組態檔。 您可以使用提供的服務定位程式模式來設定擴充性點。
- **改善的逆轉控制 (IoC) 容器的支援：** ASP.NET Web API IoC 容器透過改良的相依性解析程式的抽象提供絕佳的支援
- **自我裝載：** Web Api 可以裝載於自己的處理序，除了 IIS 時仍在使用路由的完整功能及其他功能的 Web API。
- **建立自訂的說明和測試頁：** 您現在可以輕鬆地建置自訂說明及測試頁面為您的 web 應用程式開發介面使用新*IApiExplorer*服務以取得您的 web 應用程式開發介面的完整執行階段描述。
- **監視和診斷：** ASP.NET Web API 現在提供輕量追蹤基礎結構，能輕鬆整合與現有的記錄解決方案，例如 System.Diagnostics、 ETW 和第三方記錄架構。 您可以藉由啟用追蹤*ITraceWriter*實作，並將其加入您的 web API 組態。
- **連結產生：** 使用 ASP.NET Web API *UrlHelper*產生相同的應用程式中的相關資源的連結。
- **Web API 專案範本：** 選取新的 Web API 專案表單在新的 MVC 4 專案精靈快速啟動並執行與 ASP.NET Web API。
- **Scaffolding:** 使用**加入控制器**快速 scaffold Entity Framework 為基礎的 web API 控制器的對話方塊架構的模型型別。

如需 ASP.NET Web API 的詳細資訊，請造訪[ https://www.asp.net/web-api ](../web-api/index.md)。

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>預設專案範本的增強功能

已更新用來建立新的 ASP.NET MVC 4 專案的範本來建立更現代尋找網站：

![](mvc4-release-notes/_static/image1.png)

除了外觀的增強功能，已改善功能的新範本。 範本會使用稱為調整看來，桌面瀏覽器和行動裝置沒有任何自訂的瀏覽器中呈現的技術。

![](mvc4-release-notes/_static/image2.png)

若要查看作用中的適應性呈現，您可以使用行動裝置的模擬器，或只要再試一次調整大小較小的桌面瀏覽器視窗。 當瀏覽器視窗中取得夠小時，將變更的頁面配置。

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>行動裝置的專案範本

如果您開始新的專案，並想要建立站台專為行動和平板電腦瀏覽器，您可以使用新的行動應用程式專案範本。 這根據 jQuery Mobile，用於建置觸控最佳化 UI 的開放原始碼程式庫：

![](mvc4-release-notes/_static/image3.png)

此範本包含網際網路應用程式範本相同的應用程式結構 （而且控制器的程式碼幾乎完全相同），但卻能呈現最佳效果，並能在觸控式行動裝置的行為使用 jQuery Mobile 的樣式。 若要了解有關如何在結構和樣式行動 UI 的詳細資訊，請參閱[jQuery 行動專案網站](http://jquerymobile.com/)。

如果您已經有您想要加入，行動裝置最佳化的檢視，或如果您想要建立單一站台提供不同樣式桌上型電腦和行動瀏覽器來檢視桌面導向網站，您可以使用新的顯示模式功能。 （請參閱下一節）。

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>顯示模式

新的顯示模式功能可讓應用程式選取依提出要求的瀏覽器的檢視。 例如，如果桌面瀏覽器要求在首頁上，應用程式可能會使用 Views\Home\Index.cshtml 範本。 如果行動瀏覽器要求在首頁上，應用程式可能會傳回 Views\Home\Index.mobile.cshtml 範本。

版面配置和 partials 也會覆寫特定瀏覽器類型。 例如: 

- 如果同時 Views\Shared 資料夾包含\_Layout.cshtml 和\_Layout.mobile.cshtml 範本，應用程式會使用預設\_行動瀏覽器和要求期間Layout.mobile.cshtml\_Layout.cshtml 期間其他要求。
- 如果資料夾包含同時\_MyPartial.cshtml 和\_MyPartial.mobile.cshtml，指示@Html.Partial("\_MyPartial") 會轉譯\_MyPartial.mobile.cshtml 期間從行動裝置的要求瀏覽器和\_MyPartial.cshtml 期間其他要求。

如果您想要建立更詳細的檢視、 配置、 或其他裝置的部分檢視，您可以註冊新*DefaultDisplayMode*指定的名稱： 要求符合特定條件時，要搜尋的執行個體。 例如，您可以加入下列程式碼加入*應用程式\_啟動*Global.asax 檔案以登錄為適用於 Apple iPhone 瀏覽器提出要求時的顯示模式的字串"iPhone"中的方法：

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

執行此程式碼，當提出要求時 Apple iPhone 瀏覽器之後，您的應用程式都會使用 _layout.cshtml\\_Layout.iPhone.cshtml 配置 （如果有的話）。 如需有關顯示模式的詳細資訊，請參閱[ASP.NET MVC 4 行動功能](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md)。 使用 DisplayModeProvider 應用程式應該安裝[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet 封裝。 [ASP.NET 改 2012 Update](https://go.microsoft.com/fwlink/?LinkID=271322)包含[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes)新的專案範本中的 NuGet 封裝。 請參閱[ASP.NET MVC 4 行動快取 Bug Fixedd](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)如修正程式的詳細資訊。

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>jQuery Mobile 和行動功能

如需建置 ASP.NET MVC 4 行動應用程式使用 jQuery Mobile，請參閱本教學課程[ASP.NET MVC 4 行動功能](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md)。

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>非同步控制器的工作支援

現在您可以撰寫非同步動作方法為單一方法的傳回型別的物件*工作*或*工作&lt;ActionResult&gt;*。

 如需詳細資訊，請參閱[使用 ASP.NET MVC 4 中的非同步方法](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)。

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 支援的 Windows Azure sdk 1.6 版及較新版本。

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>資料庫移轉

ASP.NET MVC 4 專案現在包含 Entity Framework 5。 資料庫移轉的支援其中一個 Entity Framework 5 中強大的功能。 這項功能可讓您輕鬆地發展您的資料庫結構描述，使用程式碼為重心的移轉，同時保留資料庫中的資料。 如需有關資料庫移轉的詳細資訊，請參閱[影片模型和資料表中加入新欄位](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md)中[ASP.NET MVC 4 教學課程簡介](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)。

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>空白專案範本

空白的 MVC 專案範本現在是真的空的，如此您可以從完全從頭開始。 舊版的空白專案範本已重新命名為 Basic。

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>將控制器加入的任何專案資料夾

您現在可以以滑鼠右鍵按一下並選取**加入控制器**MVC 專案中的任何資料夾中。 這可讓您更大的彈性來組織您的控制站，但想，包括讓您 MVC 和 Web API 的控制站維持在個別的資料夾。

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>統合及縮製

統合及縮製的架構可讓您減少必須結合成單一的配套的檔案，以指令碼和 CSS 的個別檔案，藉此網頁的 HTTP 要求的數目。 它接著可以縮短組合內容，以減少這些要求的整體大小。 縮短可以包含像是消除空白處，縮短於甚至摺疊的 CSS 選取器根據其語意的變數名稱的活動。 組合所宣告，且設定程式碼並容易參照透過 helper 方法會產生任何檢視中組合的單一連結，或偵錯時，多個連結的組合的個別內容。 如需詳細資訊，請參閱[組合和縮製](../mvc/overview/performance/bundling-and-minification.md)。

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>啟用從 Facebook 和其他站台使用 OAuth 和 OpenID 登入

ASP.NET MVC 4 網際網路專案範本中的預設範本現在包含支援 OAuth 和 OpenID 登入使用 DotNetOpenAuth 程式庫。 如需設定的 OAuth 或 OpenID 提供者資訊，請參閱[WebForms、 MVC 和網頁的 OAuth/OpenID 支援](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx)和[OAuth 和 OpenID 功能文件中 ASP.NET Web Pages](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup)。

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>ASP.NET MVC 3 專案升級至 ASP.NET MVC 4

ASP.NET MVC 4 可以在同一部電腦，讓您彈性選擇何時要升級至 ASP.NET MVC 4 的 ASP.NET MVC 3 應用程式的 ASP.NET MVC 3 並存安裝。

升級的最簡單方式是建立新的 ASP.NET MVC 4 專案，並複製所有的檢視、 控制器、 程式碼和內容檔案從現有的 MVC 3 專案加入新的專案，然後更新組件參考來比對所有非 MVC 範本的新的專案中您使用 cluded assembiles。 如果您已經變更 MVC 3 專案中的 Web.config 檔案，您也必須合併那些變更成 MVC 4 專案中的 Web.config 檔案。

若要手動升級現有的 ASP.NET MVC 3 應用程式第 4 版，執行下列作業：

1. 在所有的 Web.config 檔案在專案中 （有一個根目錄中的專案在 [檢視] 資料夾中，一個在您的專案中的每個區域的 [檢視] 資料夾中），取代下列文字的每個執行個體 (注意： System.Web.WebPages，Version = 1.0.0.0 找不到建立專案與 Visual Studio 2012）： 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    使用下列相對應的文字：

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. 在根 Web.config 檔案中，更新*webPages:Version* "2.0.0.0"的項目並加入新*PreserveLoginUrl*具有值"true"的機碼： 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. 在方案總管 中，參考上按一下滑鼠右鍵，然後選取管理 NuGet 封裝。 在左窗格中，選取**Online\NuGet 官方套件來源**，然後更新下列：

    - ASP.NET MVC 4
    - （選擇性） jQuery、 jQuery 驗證與 jQuery UI
    - （選擇性）Entity Framework
    - (Optonal)Modernizr
4. 在 方案總管 中，以滑鼠右鍵按一下專案名稱，然後選取 卸載專案。 然後名稱上按一下滑鼠右鍵，然後選取 編輯*ProjectName*.csproj。
5. 找出*ProjectTypeGuids*項目和取代 {E53F8FEA-EAE0-44A6-8774-FFD645390401} {E3E379DF-F4C6-4180-9B81-6769533ABE47}。
6. 儲存的變更，關閉您所編輯的專案 (.csproj) 檔案、 以滑鼠右鍵按一下專案，然後選取重新載入專案。
7. 如果專案參考任何協力廠商程式庫編譯使用舊版 ASP.NET MVC，請開啟根 Web.config 檔案，並加入下列三個*bindingRedirect*下的項目*組態*> 一節： 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>從 ASP.NET MVC 4 發行候選版本變更

ASP.NET MVC 4 發行候選版本資訊可以在這裡找到：

以下摘要說明此版本中從 ASP.NET MVC 4 發行候選版本的重大變更：

- **每個控制站組態：** ASP.NET Web API 控制器可以以自訂屬性來實作屬性*IControllerConfiguration*設定他們自己的格式器、 動作選取器和參數繫結器. *HttpControllerConfigurationAttribute*已移除。
- **每個路由訊息處理常式：** 您現在可以指定給定路由的要求鏈結中的最後一個訊息處理常式。 這可讓支援使用路由來分派到他們自己的賽車沿著架構 (非*IHttpController*) 端點。
- **進度通知：** *ProgressMessageHandler*產生要求實體正在上傳和下載的回應實體的進度通知。 使用這個處理常式就可以是追蹤的幅度您要上傳要求主體，或下載的回應主體。
- **內容推播：** *PushStreamContent*類別可讓您希望直接寫入要求或回應 （同步或非同步方式） 使用資料流資料 producer 狀況。 當*PushStreamContent*已準備好接受它呼叫動作委派與輸出資料流的資料。 開發人員可以再寫入資料流的資料流時寫入只要必要和關閉已完成。 *PushStreamContent*偵測到資料流的結尾，並完成基礎非同步*工作*如寫出內容。
- **建立錯誤回應：** 使用*HttpError*要以一致的方式代表錯誤的資訊從例如驗證錯誤和例外狀況時仍接受型別*IncludeErrorDetailPolicy*. 使用新*CreateErrorResponse*擴充方法，以輕鬆地建立錯誤回應的*HttpError*做為內容。 *HttpError*內容是完全內容交涉。
- **移除 MediaRangeMapping:** 媒體類型的範圍現在會由預設的內容交涉器。
- **簡單的型別參數的預設參數繫結現在是 [FromUri]:** 在舊版的預設參數繫結使用簡單的型別參數的模型繫結的 ASP.NET Web API。 現在是簡單的型別參數的預設參數繫結 *[FromUri]*。
- **動作選取接受必要的參數：** 動作選取 ASP.NET Web API 中的將現在只選取動作若未提供所有必要的參數來自 URI。 可以藉由提供動作方法簽章中的引數的預設值，指定的參數為選擇性。
- **自訂 HTTP 參數繫結：** 使用*ParameterBindingAttribute*自訂特定動作參數的參數繫結，或使用*ParameterBindingRules*上*HttpConfiguration*自訂參數繫結更廣泛。
- **MediaTypeFormatter 增強功能：** 格式器現在可以存取完整*HttpContent*執行個體。
- **緩衝原則選取的主機：** 實作和設定*IHostBufferPolicySelector*服務的 ASP.NET Web API，來讓主機判斷緩衝時要使用的原則。
- **主機無從驗證的方式存取用戶端憑證：** 使用*GetClientCertificate*從要求訊息中取得提供用戶端憑證的擴充方法。
- **內容交涉擴充性：** 由衍生自自訂內容交涉*DefaultContentNegotiator*和覆寫您想要的內容交涉的任何層面。
- **傳回 406 無法接受回應的支援：** 您現在可以傳回 406 無法接受回應 ASP.NET Web API 中藉由建立找不到適合的格式器時*DefaultContentNegotiator*與*excludeMatchOnTypeOnly*參數設定為*true*。
- **表單資料讀取為 NameValueCollection 或 JToken:** 可以讀取 URI 查詢字串中，或為要求主體中的表單資料*NameValueCollection*使用*ParseQueryString*和*ReadAsFormDataAsync*擴充方法分別。 同樣地，您可以讀取 URI 查詢字串中，或為要求主體中的表單資料*JToken*使用*TryReadQueryAsJson*和*ReadAsAsync*&lt;T&gt;擴充方法分別。
- **多部分的增強功能：** 就可以撰寫*MultipartStreamProvider* ，完全適合的可讀取並以最佳方式向使用者呈現結果 MIME 多組件資料類型。 您也可以攔截的後置處理步驟上*MultipartStreamProvider* ，允許實作進行任何後置處理它想 MIME 多組件主體組件。 例如， *MultipartFormDataStreamProvider*實作讀取 HTML 表單資料組件，並將它們加入至*NameValueCollection*因此可輕鬆地在收到呼叫端。
- **連結產生的改進：** *UrlHelper*不再相依於*HttpControllerContext*。 您現在可以存取*UrlHelper*從任何內容位置*HttpRequestMessage*可用。
- **訊息處理常式執行順序變更：** 它們都以反向順序設定而不是按順序來立即執行訊息處理常式。
- **訊息處理常式連接 helper:** 新*HttpClientFactory* ，可以連接*DelegatingHandlers*並建立*HttpClient*與想要的管線準備就緒。 它也提供連接的替代的內部處理常式的功能 (預設值是*HttpClientHandler*)，以及使用時，請勿將連接*HttpMessageInvoker*或另一個*DelegatingHandler*而不是*HttpClient*做為 top 啟動程式。
- **支援在 ASP.NET 網頁最佳化 Cdn:** ASP.NET 網頁最佳化現在支援的 CDN 替代路徑，讓您指定每個組合額外的 URL 會指向該內容傳遞網路上的相同資源。 支援 Cdn 可讓您取得您指令碼和樣式組合地理位置較接近結束取用者的 Web 應用程式。 無法使用 CDN 時，實際執行應用程式應該實作後援。 測試此後援。
- **ASP.NET Web API 路由並設定移至*WebApiConfig.Register*可以是 resused 測試程式碼中的靜態方法。** ASP.NET Web API 路由先前已加入*RouteConfig.RegisterRoutes*以及標準的 MVC 路由傳送。 ASP.NET Web API 路由的預設和組態現在處理在個別*WebApiConfig.Register*以促進測試方法。

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的問題和重大變更

- **ASP.NET MVC 4 的 RC 和 RTM 版本不正確地傳回快取的桌面檢視時應傳回行動檢視。**

    - 請參閱[ASP.NET MVC 4 行動快取 Bug 固定](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)如修正程式的詳細資訊。 可以從安裝修正[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet 封裝。
- **Razor 檢視引擎中的重大變更**。 下列類型已從移除*System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  也已移除下列方法： 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **當 ASP.NET MVC 4 應用程式的 /bin 目錄中包含 WebMatrix.WebData.dll 時，它會高於表單驗證的 URL。** WebMatrix.WebData.dll 組件加入至應用程式 （例如，藉由使用 [新增可部署的相依性] 對話方塊時，請選取 「 ASP.NET Web 網頁使用 Razor 語法 」） 將會覆寫/帳戶/登入驗證登入重新導向而 /帳戶/登入以預期的 ASP.NET MVC 帳戶控制器的預設方式。 要避免這種行為，並使用 web.config 的 [驗證] 區段中已指定的 URL，您可以加入稱為 PreserveLoginUrl appSetting 並將它設定為 true: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **NuGet 套件管理員安裝嘗試安裝 ASP.NET MVC 4 的 Visual Studio 2010 和 Visual Web Developer 2010 的並存安裝時失敗。** 若要執行 Visual Studio 2010 和 Visual Web Developer 2010 並存 ASP.NET MVC 4 中，您必須安裝 ASP.NET MVC 4 之後已安裝 Visual Studio 的兩個版本。
- **如果已經已解除安裝先決條件，解除安裝 ASP.NET MVC 4 會失敗。** 若要完全解除安裝 ASP.NET MVC 4you 必須解除安裝 ASP.NET MVC 4，然後再解除安裝 Visual Studio。
- **安裝 ASP.NET MVC 4 中斷 ASP.NET MVC 3 RTM 應用程式。** ASP.NET MVC 3 應用程式所建立 rtm 發行 (不能搭配[ASP.NET MVC 3 Tools Update](https://www.microsoft.com/download/details.aspx?id=1491)版本) 才能與 ASP.NET MVC 4 並存需要下列變更。 建置專案，而不需要進行這些更新的結果中的編譯錯誤。 

    **必要的更新**

  1. 在根 Web.config 檔案中，加入新*&lt;appSettings&gt;* 具有索引鍵的項目*webPages:Version*和值*1.0.0.0*。 

      [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
  2. 在 方案總管 中，以滑鼠右鍵按一下專案名稱，然後選取 卸載專案。 然後名稱上按一下滑鼠右鍵，然後選取 編輯*ProjectName*.csproj。
  3. 找出下列組件參考： 

      [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

      它們取代成下列：

      [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
  4. 儲存變更，請關閉您先前所編輯，然後以滑鼠右鍵按一下專案並選取重新載入專案 (.csproj) 檔案。

- **ASP.NET MVC 4 專案變更為 4.0 目標從 4.5 不會更新 EntityFramework 組件參考：** 如果您變更 ASP.NET MVC 4 專案之後以 4.5 EntityFramework 組件的參考仍會指向目標 4.04.5 版本。 若要修正此問題解除安裝，然後重新安裝 EntityFramework NuGet 封裝。
- **403 禁止從 4.5 變更目標 4.0 之後，在 Azure 上執行 ASP.NET MVC 4 應用程式時：** 如果您以 4.5 之後，變更為 4.0 目標的 ASP.NET MVC 4 專案，然後部署至 Azure，您會看到在執行階段 403 禁止錯誤。 若要解決此問題，請將下列內容加入 web.config: `<modules runAllManagedModulesForAllRequests="true" />`
- **當您輸入時，visual Studio 2012 損毀 '\'字串常值中 Razor 檔案中。** 才能解決此問題輸入右引號字串常值的第一次。
- <strong>瀏覽至&quot;帳戶/管理&quot;網際網路範本結果中的簡體中文、 TRK 和繁體中文語言執行階段錯誤。</strong> 若要修正此問題修改頁面，來區隔<em>@User.Identity.Name</em>做為唯一的內容中將它放入<em>&lt;強式&gt;</em>標記。
- **Google 和 LinkedIn 的提供者不支援在 Azure 網站。** 部署至 Azure 網站時，請使用替代驗證提供者。
- **當使用 UriPathExtensionMapping 具有 IIS 8 Express/IIS 時，您會收到 404 找不到錯誤，當您嘗試使用延伸模組。** 靜態檔案處理常式會干擾到 web 應用程式開發介面中使用的要求*UriPathExtensionMappings*。 設定*runAllManagedModulesForAllRequests = true*在 web.config 中若要解決此問題。
- **不會再呼叫 Controller.Execute 方法。** 所有 MVC 控制器現在一律以非同步方式都執行。
