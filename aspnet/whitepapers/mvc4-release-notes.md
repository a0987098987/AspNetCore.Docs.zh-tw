---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 本文件說明 ASP.NET MVC 4 的版本。
ms.author: aspnetcontent
ms.date: 09/09/2011
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: 9e4d73349c4c01e0717aabe7da9cb126d2c545c0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826569"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> 本文件說明 ASP.NET MVC 4 的版本。


- [安裝注意事項](#_Toc303253802)
- [文件](#_Toc303253803)
- [支援](#_Toc303253804)
- [軟體需求](#_Toc303253805)
- [ASP.NET MVC 4 中的新功能](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [預設專案範本的增強功能](#_Toc303253808)
    - [行動裝置專案範本](#_Toc303253809)
    - [顯示模式](#_Toc303253810)
    - [jQuery Mobile，檢視切換器，以及瀏覽器覆寫](#_Toc303253811)
    - [非同步控制器的的工作支援](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [資料庫移轉](#_Toc303253818)
    - [空白專案範本](#_Toc303253819)
    - [將控制器新增至任何專案資料夾](#_Toc303253820)
    - [統合和縮製](#_Toc303253821)
    - [啟用登入和來自 Facebook 與其他站台使用 OAuth 和 OpenID](#_Toc303253822)
- [將 ASP.NET MVC 3 專案升級至 ASP.NET MVC 4](#_Toc303253806)
- [從 ASP.NET MVC 4 發行候選版本的變更](#_Toc303253817)
- [已知的問題和重大變更](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>安裝注意事項

您可以從安裝 ASP.NET MVC 4 for Visual Studio 2010 [ASP.NET MVC 4 首頁](../mvc/mvc4.md)使用 Web Platform Installer。

我們建議您解除安裝任何先前安裝的預覽安裝 ASP.NET MVC 4 之前的 ASP.NET MVC 4。 您可以為 ASP.NET MVC 4 升級 ASP.NET MVC 4 beta 版及 Release Candidate，但不解除安裝。

此版本不相容於.NET Framework 4.5 的任何預覽版本。 您分開必須升級任何已安裝的預覽版本的.NET Framework 4.5 安裝 ASP.NET MVC 4 之前的最後一個版本。

ASP.NET MVC 4 可以安裝和執行的並行使用 ASP.NET MVC 3。

<a id="_Toc303253803"></a>
## <a name="documentation"></a>文件

ASP.NET mvc 的文件位於 MSDN 網站，下列 URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

教學課程和 ASP.NET MVC 的其他資訊位於 ASP.NET 網站的 MVC 4 頁 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md))。

<a id="_Toc303253804"></a>
## <a name="support"></a>支援

完全支援 ASP.NET MVC 4。 如果您有使用此版本的相關問題您可以也將其張貼至 ASP.NET MVC 論壇 ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx))，其中的 ASP.NET 社群成員可經常提供非正式的支援。

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>軟體需求

Visual Studio 的 ASP.NET MVC 4 元件需要 PowerShell 2.0 和為 Service Pack 1 的 Visual Studio 2010 或 Visual Web Developer Express 2010 Service Pack 1。

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>ASP.NET MVC 4 中的新功能

本節說明已引進的功能在 ASP.NET MVC 4 版本。

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4 包含 ASP.NET Web API、 將新的架構，用於建立可以連線到各種包括瀏覽器和行動裝置的用戶端的 HTTP 服務。 ASP.NET Web API 也是建置 RESTful 服務的理想平台。

ASP.NET Web API 包括下列功能的支援：

- **現代的 HTTP 程式設計模型：** 直接存取和處理 HTTP 要求和回應中使用新的強型別 HTTP 物件模型對 Web Api。 相同程式設計模型和 HTTP 管線是用戶端透過新的對稱可用*HttpClient*型別。
- **路由的完整支援：** ASP.NET Web API 支援一組完整的 ASP.NET 路由，包括路由參數和條件約束的路由功能。 此外，使用簡單的慣例來對應至 HTTP 方法的動作。
- **內容交涉：** 用戶端和伺服器可一起運作來判斷正確的格式從 web API 所傳回的資料。 ASP.NET Web API 提供預設的支援，如 XML、 JSON 和表單 URL 編碼格式，而且您可以新增您自己的格式器，來擴充這項支援，或甚至取代預設的內容交涉策略。
- **模型繫結和驗證：** 模型繫結器提供簡單的方式擷取 HTTP 要求的各種組件中的資料，並將這些訊息部分轉換成.NET 物件可由 Web API 動作。 根據資料註解的動作參數，也會執行驗證。
- **篩選器：** ASP.NET Web API 支援包括下列已知的篩選條件的篩選器 *[Authorize]* 屬性。 您可以撰寫，並插入您自己的篩選動作、 授權和例外狀況處理。
- **查詢組合：** 使用 *[Queryable]* 傳回的動作篩選條件屬性*IQueryable*啟用查詢您的 web API 透過 OData 查詢轉換的支援。
- **改善可測試性：** 而不是靜態的內容物件中設定 HTTP 詳細資料之外，web API 動作使用的執行個體*HttpRequestMessage*並*HttpResponseMessage*。 建立單元測試專案，以及您的 Web API 專案，開始快速撰寫適用於您 Web API 的功能。
- **程式碼為基礎的組態：** ASP.NET Web API 完成只會透過程式碼，讓您設定檔案清除。 您可以使用提供的服務定位器模式來設定擴充點。
- **改善的控制反轉 (IoC) 容器的支援：** ASP.NET Web API 提供絕佳的支援，適用於透過改良的相依性解析程式抽象概念的 IoC 容器
- **自我裝載：** Web Api 可以裝載在自己的程序，除了 IIS 之外，同時仍然使用路由的完整功能和其他功能的 Web API。
- **建立自訂的說明和測試的網頁：** 您現在可以輕鬆地建置自訂的說明和測試您的 web Api 的頁面，使用新*IApiExplorer*服務，以取得您的 web Api 的完整執行階段描述。
- **監視和診斷：** ASP.NET Web API 現在提供輕量追蹤基礎結構，可讓您輕鬆地與現有的記錄解決方案，例如 System.Diagnostics、 ETW 和協力廠商記錄架構整合。 您可以藉由提供啟用追蹤*ITraceWriter*實作，並將它新增至您的 web API 的組態。
- **連結產生：** 使用 ASP.NET Web API *UrlHelper*產生相同的應用程式的相關資源的連結。
- **Web API 專案範本：** 選取新的 Web API 專案表單新增 MVC 4 專案精靈，快速地啟動並執行 ASP.NET Web api。
- **Scaffolding:** 使用**新增控制器**對話方塊，即可快速建立以 Entity Framework 為基礎的 web API 控制器的結構以基礎的模型類型。

如需 ASP.NET Web API 的詳細資訊，請造訪[ https://www.asp.net/web-api ](../web-api/index.md)。

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>預設專案範本的增強功能

已更新用來建立新的 ASP.NET MVC 4 專案的範本來建立更現代化外觀的網站：

![](mvc4-release-notes/_static/image1.png)

除了外觀的增強功能，已改進新的範本中的功能。 範本會針對稱為適應性呈現看來，在桌面瀏覽器和行動瀏覽器，而不需要任何自訂的技術。

![](mvc4-release-notes/_static/image2.png)

若要查看作用中的自適性轉譯，您可以使用行動裝置的模擬器，或試試調整大小變得更小的桌面瀏覽器 視窗。 當瀏覽器視窗中取得夠小時，將變更頁面的配置。

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>行動裝置專案範本

如果您正在新的專案，並想要建立站台，專為行動和平板電腦瀏覽器，您可以使用新的行動應用程式專案範本。 這是以基礎的 jQuery Mobile，開放原始碼程式庫建置觸控最佳化的 UI:

![](mvc4-release-notes/_static/image3.png)

此範本包含與網際網路應用程式範本的應用程式結構相同 （而且控制器程式碼幾乎完全相同），但樣式呈現最佳效果，並能在觸控式行動裝置的行為使用 jQuery Mobile。 若要深入了解如何建構和設定行動裝置的 UI 的樣式，請參閱[jQuery 行動專案網站](http://jquerymobile.com/)。

如果您已經有的桌面導向的網站，您想要新增行動裝置最佳化的檢視，或如果您想要建立單一站台做桌面和行動瀏覽器的不同樣式化的檢視，您可以使用新的顯示模式功能。 （請參閱下一節。）

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>顯示模式

新的顯示模式功能可讓應用程式選取根據提出要求的瀏覽器的檢視。 例如，如果桌面瀏覽器要求首頁上，應用程式可能使用 Views\Home\Index.cshtml 範本。 如果在行動瀏覽器要求首頁上，應用程式可能會傳回 Views\Home\Index.mobile.cshtml 範本。

版面配置和部分可以也覆寫特定瀏覽器類型。 例如: 

- 如果您的 Views\Shared 資料夾同時包含\_Layout.cshtml 和\_Layout.mobile.cshtml 範本，根據預設，應用程式會使用\_Layout.mobile.cshtml與行動瀏覽器要求期間\_Layout.cshtml 期間其他要求。
- 如果資料夾同時包含\_MyPartial.cshtml 和\_MyPartial.mobile.cshtml，指示@Html.Partial(「\_MyPartial") 會轉譯\_MyPartial.mobile.cshtml 期間從行動裝置的要求瀏覽器和\_MyPartial.cshtml 期間其他要求。

如果您想要建立更具體的檢視表、 版面配置或針對其他裝置的部分檢視，您可以註冊新*DefaultDisplayMode*指定的名稱： 要求符合特定條件時，要搜尋的執行個體。 例如，您可以在其中新增下列程式碼*應用程式\_啟動*註冊"iPhone"字串做為適用於 Apple iPhone 瀏覽器提出要求時的顯示模式的 Global.asax 檔案中的方法：

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

執行此程式碼，當 Apple iPhone 瀏覽器要求之後，您的應用程式將使用 Views\Shared\\_Layout.iPhone.cshtml 版面配置 （若有的話）。 如需有關顯示模式的詳細資訊，請參閱[ASP.NET MVC 4 Mobile 功能](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md)。 應該安裝應用程式使用 DisplayModeProvider[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet 套件。 [ASP.NET Fall 2012 Update](https://go.microsoft.com/fwlink/?LinkID=271322)包含[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes)新的專案範本中的 NuGet 套件。 請參閱[ASP.NET MVC 4 Mobile 快取 Bug Fixedd](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)如修正程式的詳細資訊。

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>jQuery Mobile 和 Mobile 功能

如需建置 ASP.NET MVC 4 的行動應用程式使用 jQuery Mobile，請參閱本教學課程[ASP.NET MVC 4 Mobile 功能](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md)。

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>非同步控制器的的工作支援

您現在可以撰寫非同步動作方法傳回的型別物件的單一方法*任務*或是*工作&lt;ActionResult&gt;*。

 如需詳細資訊，請參閱[ASP.NET MVC 4 中使用非同步方法](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)。

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 支援 Windows Azure SDK 1.6 版及較新的版本。

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>資料庫移轉

ASP.NET MVC 4 專案現在會包含 Entity Framework 5。 在 Entity Framework 5 的絕佳功能之一是資料庫移轉的支援。 這項功能可讓您輕鬆地發展您的資料庫結構描述，使用程式碼為主的移轉，同時保留資料庫中的資料。 如需有關資料庫移轉的詳細資訊，請參閱[將新欄位新增至電影模型和資料表](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md)中[ASP.NET MVC 4 的教學課程簡介](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)。

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>空白專案範本

空白的 MVC 專案範本現在是真正空的以便您可以從完全從頭開始。 舊版的空白專案範本已重新命名為基本。

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>將控制器新增至任何專案資料夾

您現在可以以滑鼠右鍵按一下並選取**新增控制器**MVC 專案中的任何資料夾中。 這可讓您更大的彈性來組織您的控制器，但您想要包括將您的 MVC 和 Web API 控制器放在個別的資料夾。

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>統合和縮製

統合和縮製的架構可讓您減少網頁需要藉由結合成單一的配套的檔案，指令碼和 CSS 的個別檔案進行的 HTTP 要求數目。 它接著可以縮小搭售方案的內容，以減少這些要求的整體大小。 縮小，可以包含活動，例如排除空白字元以縮短甚至摺疊其語意為基礎的 CSS 選取器的變數名稱。 宣告和設定套件組合的程式碼並輕鬆地透過協助程式方法可以產生的檢視中的套件組合的單一連結或參考，偵錯時，多個連結的套件組合的個別內容。 如需詳細資訊，請參閱[統合和縮製](../mvc/overview/performance/bundling-and-minification.md)。

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>啟用登入和來自 Facebook 與其他站台使用 OAuth 和 OpenID

ASP.NET MVC 4 網際網路專案範本中的預設範本現在包含使用 DotNetOpenAuth 程式庫中的 OAuth 和 OpenID 登入的支援。 如需設定的 OAuth 或 OpenID 提供者的資訊，請參閱[OAuth/OpenID 支援 WebForms、 MVC 和網頁](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx)並[OAuth 和 OpenID 功能文件中 ASP.NET Web Pages](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup)。

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>將 ASP.NET MVC 3 專案升級至 ASP.NET MVC 4

ASP.NET MVC 4 可以與 ASP.NET MVC 3 並存安裝在同一部電腦，讓您彈性地選擇何時要升級至 ASP.NET MVC 4 的 ASP.NET MVC 3 應用程式。

升級的最簡單方式是建立新的 ASP.NET MVC 4 專案和複製所有檢視、 控制器、 程式碼及內容都檔案從現有的 MVC 3 專案至新的專案，然後若要更新組件參考來比對所有非 MVC 範本的新專案中您使用 cluded 2.0、visual。 如果您變更至 MVC 3 專案中的 Web.config 檔案的內容，您必須也那些變更合併至 MVC 4 專案中的 Web.config 檔案。

若要手動升級現有的 ASP.NET MVC 3 應用程式第 4 版，執行下列作業：

1. 在所有的 Web.config 檔案在專案中 （有一個專案，在 [Views] 資料夾中，一個在專案中的每個區域的 [Views] 資料夾的根目錄中），會取代下列文字的每個執行個體 (請注意： System.Web.WebPages，版本 = 中找不到 1.0.0.0建立的專案使用 Visual Studio 2012）： 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    使用下列相對應的文字：

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. 在根 Web.config 檔案中，更新*webPages:Version*為"2.0.0.0"的項目並新增一個新*PreserveLoginUrl*具有值"true"的機碼： 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. 在 方案總管 中，以滑鼠右鍵按一下參考，並選取 管理 NuGet 套件。 在左窗格中，選取**Online\NuGet 官方封裝來源**，然後更新下列：

    - ASP.NET MVC 4
    - （選擇性） jQuery、 jQuery 驗證和 jQuery UI
    - （選擇性）Entity Framework
    - (Optonal)Modernizr
4. 在 方案總管 中，以滑鼠右鍵按一下專案名稱，然後選取 卸載專案 然後再次以滑鼠右鍵按一下名稱，然後選取 編輯*ProjectName*.csproj。
5. 找出*ProjectTypeGuids*項目，並取代 {E53F8FEA-EAE0-44A6-8774-FFD645390401} 與 {E3E379DF-F4C6-4180-9B81-6769533ABE47}。
6. 儲存變更，關閉您所編輯的專案 (.csproj) 檔案，以滑鼠右鍵按一下專案，，然後選取重新載入專案。
7. 如果專案參考會編譯使用舊版 ASP.NET MVC 的任何協力廠商程式庫，請開啟根 Web.config 檔案，並新增下列三個*bindingRedirect*下方的項目*設定*區段： 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>從 ASP.NET MVC 4 發行候選版本的變更

ASP.NET MVC 4 Release Candidate 版本資訊可以在這裡找到：

以下摘要說明從 ASP.NET MVC 4 Release Candidate 在此版本中的重大變更：

- **每個控制站組態：** ASP.NET Web API 控制器可以屬性具有自訂屬性，以實作*IControllerConfiguration*設定他們自己的格式器、 動作選取器和參數繫結器. *HttpControllerConfigurationAttribute*已移除。
- **每個路由訊息處理常式：** 您現在可以指定在指定的路由的要求鏈結中的最後一個訊息處理常式。 這可讓支援使用路由來分派到他們自己的賽車沿著架構 (非*IHttpController*) 端點。
- **進度通知：** *ProgressMessageHandler*產生要求實體正在上傳和下載的回應實體的進度通知。 使用這個處理常式就可以持續多久您要上傳的要求內文，或正在下載回應內文追蹤。
- **將內容推送：** *PushStreamContent*類別可讓您的資料產生者，想要直接寫入要求或回應 （同步或非同步方式） 使用的資料流的案例。 當*PushStreamContent*已準備好接受它會呼叫外面動作委派與輸出資料流的資料。 開發人員可以再寫入資料流的資料流寫入時只要必要和關閉已完成。 *PushStreamContent*偵測到資料流的結尾，並完成基礎非同步*工作*寫出內容。
- **建立錯誤回應：** 使用*HttpError*型別來以一致的方式表示錯誤的資訊從例如驗證錯誤和例外狀況而且仍然承認*IncludeErrorDetailPolicy*. 使用新*CreateErrorResponse*擴充方法，以輕鬆地建立具有錯誤回應*HttpError*做為內容。 *HttpError*內容是完整的內容交涉。
- **移除 MediaRangeMapping:** 媒體類型的範圍現在會由預設器。
- **簡單的型別參數的預設參數繫結現在是 [FromUri]:** 在舊版的 ASP.NET Web API 的預設參數繫結使用模型繫結的簡單型別參數。 簡單的型別參數的預設參數繫結現 *[FromUri]*。
- **動作選取接受必要的參數：** 動作選取 ASP.NET Web API 中的現在會只選取選取動作若未提供來自 URI 的所有必要的參數。 參數可以指定為選擇性，藉由提供動作方法簽章中的引數的預設值。
- **自訂 HTTP 參數繫結：** 使用*ParameterBindingAttribute*來自訂特定動作參數的參數繫結，或使用*ParameterBindingRules*上*HttpConfiguration*自訂參數繫結更廣泛。
- **MediaTypeFormatter 增強功能：** 格式器現在可以存取完整*HttpContent*執行個體。
- **緩衝原則選取的主機：** 實作，並設定*IHostBufferPolicySelector*服務在 ASP.NET Web API，可讓主機判斷緩衝時要使用的原則。
- **主機無從驗證的方式存取用戶端憑證：** 使用*GetClientCertificate*擴充方法來取得所提供的用戶端憑證從要求訊息。
- **內容交涉擴充性：** 藉由衍生自自訂內容交涉*DefaultContentNegotiator*並覆寫任何您想要的內容交涉的層面。
- **傳回 「 406 無法接受回應的支援：** 您現在可以傳回 406 無法接受的回應 ASP.NET Web API 中藉由建立找不到適合的格式器時*DefaultContentNegotiator* 與*excludeMatchOnTypeOnly*參數設定為 *，則為 true*。
- **NameValueCollection 為 JToken 讀取表單資料：** 您可以讀取的 URI 查詢字串中或做為要求主體中的表單資料*NameValueCollection*使用*ParseQueryString*並*ReadAsFormDataAsync*擴充方法分別。 同樣地，您可以在其中讀取 URI 查詢字串中或做為要求主體中的表單資料*JToken*使用*TryReadQueryAsJson*並*ReadAsAsync*&lt;T&gt;擴充方法分別。
- **多部分的增強功能：** 現在便能夠撰寫*MultipartStreamProvider*可完全量身打造的 MIME 多組件資料它可以用來讀取，並以最佳方式，向使用者呈現結果的型別。 您也可以攔截的後續處理步驟上*MultipartStreamProvider* ，允許實作進行任何處理它想要在 MIME 多組件主體組件上的文章。 例如， *MultipartFormDataStreamProvider*實作讀取 HTML 表單資料組件，並將其加入*NameValueCollection*使其能夠輕鬆地在從呼叫者。
- **連結產生的增強功能：** *UrlHelper*不再相依於*HttpControllerContext*。 您現在可以存取*UrlHelper*從任何內容位置*HttpRequestMessage*可用。
- **訊息處理常式執行順序變更：** 訊息處理常式現在已設定而不是以反向順序的順序執行。
- **連接訊息處理常式的協助程式：** 新*HttpClientFactory* ，可以接通*Delegatinghandlerelementcollection*並建立*HttpClient*與想要的管線準備好了。 它也提供連接使用替代的內部處理常式的功能 (預設值是*HttpClientHandler*)，以及使用時，請勿將連接*HttpMessageInvoker*或其他*DelegatingHandler*而非*HttpClient*做為 top 啟動程式。
- **ASP.NET Web 最佳化的 cdn 支援：** ASP.NET Web 最佳化現在提供支援，針對 CDN 替代的路徑，讓您指定每個項目組合額外的 URL 指向該相同的資源，在內容傳遞網路。 支援 Cdn 可讓您以您指令碼和樣式套組地理上靠近前往 Web 應用程式的取用者。 無法使用 CDN 時，生產環境應用程式應該實作後援。 測試此後援。
- **ASP.NET Web API 路由和組態移至*WebApiConfig.Register*可以 resused 測試程式碼中的靜態方法。** ASP.NET Web API 路由先前已加入*RouteConfig.RegisterRoutes*以及標準的 MVC 路由傳送。 設定與 ASP.NET Web API 路由的預設值現在在個別處理*WebApiConfig.Register*方法，以方便進行測試。

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的問題和重大變更

- **ASP.NET MVC 4 RC 和 RTM 版本不正確地傳回快取的桌面檢視時，應該會傳回行動檢視。**

    - 請參閱[ASP.NET MVC 4 Mobile 快取 Bug 已修正](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)如修正程式的詳細資訊。 可以從安裝 fix[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet 套件。
- **Razor 檢視引擎的重大變更**。 下列類型移除了*System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  也已移除下列方法： 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **當 WebMatrix.WebData.dll 包含 ASP.NET MVC 4 應用程式的 /bin 目錄中時，它會高於表單驗證的 URL。** （例如，藉由使用 [新增可部署的相依性] 對話方塊時，請選取 「 ASP.NET 網頁使用 Razor 語法 」） 時，將 WebMatrix.WebData.dll 組件新增至您的應用程式將會覆寫/帳戶/登入來驗證登入重新導向而 /帳戶/登入如預期般預設 ASP.NET MVC 帳戶控制器。 若要避免這種行為，並使用指定的 URL 已經在 web.config 的 [驗證] 區段中，您可以加入稱為 PreserveLoginUrl appSetting，並將它設定為 true: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **NuGet 套件管理員安裝嘗試安裝 ASP.NET MVC 4 的 Visual Studio 2010 和 Visual Web Developer 2010 的並存安裝時失敗。** 若要執行 Visual Studio 2010 和 Visual Web Developer 2010 與 ASP.NET MVC 4 中，您必須安裝 ASP.NET MVC 4 之後已安裝這兩個版本的 Visual Studio。
- **如果已解除安裝必要條件，解除安裝 ASP.NET MVC 4 就會失敗。** 若要完全解除安裝 ASP.NET MVC 4you 必須解除安裝 ASP.NET MVC 4，然後再解除安裝 Visual Studio。
- **安裝 ASP.NET MVC 4 中斷 ASP.NET MVC 3 RTM 應用程式。** Rtm 所建立的 ASP.NET MVC 3 應用程式版本 (不能搭配[ASP.NET MVC 3 Tools Update](https://www.microsoft.com/download/details.aspx?id=1491)版本) 才能與 ASP.NET MVC 4 並存需要下列變更。 建置專案，而不需要進行這些更新結果中的編譯錯誤。 

    **必要的更新**

  1. 在根 Web.config 檔案中，加入新*&lt;appSettings&gt;* 與索引鍵的項目*webPages:Version* ，而*1.0.0.0*。 

      [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
  2. 在 方案總管 中，以滑鼠右鍵按一下專案名稱，然後選取 卸載專案 然後再次以滑鼠右鍵按一下名稱，然後選取 編輯*ProjectName*.csproj。
  3. 找出下列組件參考： 

      [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

      您可以將它們取代為下列：

      [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
  4. 儲存變更，請關閉您所編輯，然後以滑鼠右鍵按一下專案並選取 重新載入專案 (.csproj) 檔案。

- **從 4.5 變更目標 4.0 ASP.NET MVC 4 專案不會更新 EntityFramework 組件參考：** 如果您變更的 ASP.NET MVC 4 專案之後以 4.5 EntityFramework 組件的參考仍然會指向目標 4.04.5 的版本。 若要修正此問題的解除安裝並重新安裝 EntityFramework NuGet 套件。
- **403 禁止從 4.5 變更至目標 4.0 之後，Azure 上執行的 ASP.NET MVC 4 應用程式時：** 如果您以 4.5 之後，變更為 4.0 目標的 ASP.NET MVC 4 專案，然後再部署到 Azure 可能會看到 403 禁止錯誤，在執行階段。 若要解決此問題，請將下列內容加入 web.config: `<modules runAllManagedModulesForAllRequests="true" />`
- **當您輸入時，損毀 visual Studio 2012 '\'字串常值中的 Razor 檔案中。** 可解決此問題輸入右引號常值字串的第一次。
- <strong>若要瀏覽&quot;帳戶/管理&quot;網際網路範本結果中的簡體中文、 土耳其文和繁體中文語言執行階段錯誤。</strong> 若要修正此問題會修改頁面，以區隔<em>@User.Identity.Name</em>將其做為唯一的內容內<em>&lt;強式&gt;</em>標記。
- **Google 和 LinkedIn 的提供者不支援在 Azure Web Sites。** 部署至 Azure 網站時，請使用替代驗證提供者。
- **使用 UriPathExtensionMapping 具有 IIS 8 Express/IIS 時，您會收到 404 找不到的錯誤，當您嘗試使用延伸模組。** 靜態檔案處理常式會干擾 web 使用的 Api 的要求*UriPathExtensionMappings*。 設定*runAllManagedModulesForAllRequests = true*若要解決此問題的 web.config 中。
- **不會再呼叫 Controller.Execute 方法。** 所有的 MVC 控制器現在一律以非同步方式執行。
