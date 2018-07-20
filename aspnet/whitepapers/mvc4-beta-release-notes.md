---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 本文件說明 ASP.NET MVC 4 Beta for Visual Studio 2010 的版本。
ms.author: aspnetcontent
ms.date: 09/09/2011
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: b9d50114a239b67b1adc263f6ea6d3a811bcc8f7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816687"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> 本文件說明 ASP.NET MVC 4 Beta for Visual Studio 2010 的版本。
> 
> > [!NOTE]
> > 這不是最新的版本。 ASP.NET MVC 4 RC 版本資訊可從[此處](mvc4-release-notes.md)。


- [安裝注意事項](#_Toc303253802)
- [文件](#_Toc303253803)
- [支援](#_Toc303253804)
- [軟體需求](#_Toc303253805)
- [將 ASP.NET MVC 3 專案升級至 ASP.NET MVC 4](#_Toc303253806)
- [ASP.NET MVC 4 beta 版的新功能](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [ASP.NET 單一頁面應用程式](#_Toc317096198)
    - [預設專案範本的增強功能](#_Toc303253808)
    - [行動裝置專案範本](#_Toc303253809)
    - [顯示模式](#_Toc303253810)
    - [jQuery Mobile，檢視切換器，以及瀏覽器覆寫](#_Toc303253811)
    - [Visual Studio 中的程式碼產生的訣竅](#_Toc303253812)
    - [非同步控制器的的工作支援](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [已知的問題和重大變更](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>安裝注意事項

您可以從安裝 Visual Studio 2010 的 ASP.NET MVC 4 Beta [ASP.NET MVC 4 首頁](../mvc/mvc4.md)使用 Web Platform Installer。

您必須先解除安裝任何先前安裝的預覽，再安裝 ASP.NET MVC 4 beta 版的 ASP.NET MVC 4。

此版本不相容於.NET Framework 4.5 Developer Preview。 您必須解除安裝.NET 4.5 Developer Preview，然後再安裝 ASP.NET MVC 4 Beta。

ASP.NET MVC 4 可以安裝，而且可以執行並排顯示 ASP.NET MVC 3。

<a id="_Toc303253803"></a>
## <a name="documentation"></a>文件

ASP.NET mvc 的文件位於 MSDN 網站，下列 URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

教學課程和 ASP.NET MVC 的其他資訊位於 ASP.NET 網站的 MVC 4 頁 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md))。

<a id="_Toc303253804"></a>
## <a name="support"></a>支援

這是預覽版本，而且未正式支援。 如果您有使用此版本的相關問題，將其張貼至 ASP.NET MVC 論壇 ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx))，其中的 ASP.NET 社群成員可經常提供非正式的支援。

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>軟體需求

Visual Studio 的 ASP.NET MVC 4 元件需要 PowerShell 2.0 和為 Service Pack 1 的 Visual Studio 2010 或 Visual Web Developer Express 2010 Service Pack 1。

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>將 ASP.NET MVC 3 專案升級至 ASP.NET MVC 4

ASP.NET MVC 4 可以與 ASP.NET MVC 3 並存安裝在同一部電腦，讓您彈性地選擇何時要升級至 ASP.NET MVC 4 的 ASP.NET MVC 3 應用程式。

若要建立新的 ASP.NET MVC 4 專案，並複製所有檢視、 控制器、 程式碼和內容都檔案從現有的 MVC 3 專案至新的專案，然後若要更新組件參考中新的專案，以符合舊的專案最簡單的方式升級。 如果您變更至 MVC 3 專案中的 Web.config 檔案的內容，您必須也那些變更合併至 MVC 4 專案中的 Web.config 檔案。

若要手動升級現有的 ASP.NET MVC 3 應用程式第 4 版，執行下列作業：

1. 在專案 （有一個專案，在 [Views] 資料夾中，一個在專案中的每個區域的 [Views] 資料夾的根目錄中） 中的所有 Web.config 檔案，將每個執行個體的下列文字：

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    使用下列相對應的文字：

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. 在根 Web.config 檔案中，更新*webPages:Version*為"2.0.0.0"的項目並新增一個新*PreserveLoginUrl*具有值"true"的機碼：

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. 在 [方案總管] 中，刪除其參考*System.Web.Mvc* （指向版本 3 的 DLL）。 然後將參考加入*System.Web.Mvc* (v4.0.0.0)。 特別是，進行下列變更來更新組件參考。 詳細資料如下：

    1. 在 [方案總管] 中，刪除下列組件參考： 

        - *System.Web.Mvc*(v3.0.0.0)
        - *System.Web.WebPages*(v1.0.0.0)
        - *System.Web.Razor*(v1.0.0.0)
        - *System.Web.WebPages.Deployment*(v1.0.0.0)
        - *System.Web.WebPages.Razor*(v1.0.0.0)
    2. 新增下列組件的參考： 

        - *System.Web.Mvc*(v4.0.0.0)
        - *System.Web.WebPages*(v2.0.0.0)
        - *System.Web.Razor*(v2.0.0.0)
        - *System.Web.WebPages.Deployment*(v2.0.0.0)
        - *System.Web.WebPages.Razor*(v2.0.0.0)
4. 在 方案總管 中，以滑鼠右鍵按一下專案名稱，然後選取 卸載專案 然後再次以滑鼠右鍵按一下名稱，然後選取 編輯*ProjectName*.csproj。
5. 找出*ProjectTypeGuids*項目，並取代 {E53F8FEA-EAE0-44A6-8774-FFD645390401} 與 {E3E379DF-F4C6-4180-9B81-6769533ABE47}。
6. 儲存變更，關閉您所編輯的專案 (.csproj) 檔案，以滑鼠右鍵按一下專案，，然後選取重新載入專案。
7. 如果專案參考會編譯使用舊版 ASP.NET MVC 的任何協力廠商程式庫，請開啟根 Web.config 檔案，並新增下列三個*bindingRedirect*下方的項目*設定*區段： 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>ASP.NET MVC 4 beta 版的新功能

本節說明已引進的功能在 ASP.NET MVC 4 Beta 版本。

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4 現在包含 ASP.NET Web API，新的架構，用於建立 HTTP 服務可以連線到各種包括瀏覽器和行動裝置的用戶端。 ASP.NET Web API 也是建置 RESTful 服務的理想平台。

ASP.NET Web API 包括下列功能的支援：

- **現代的 HTTP 程式設計模型：** 直接存取和處理 HTTP 要求和回應中使用新的強型別 HTTP 物件模型對 Web Api。 相同程式設計模型和 HTTP 管線功能數採對稱式用戶端透過新的 HttpClient 型別。
- **路由的完整支援**: Web Api 現在支援一組完整的路由功能，一直 Web 堆疊，包括路由參數和條件約束的一部分。 此外，對應至動作必須完整支援慣例，所以您不再需要套用屬性，例如 [HttpPost] 您的類別和方法。
- **內容交涉**： 用戶端和伺服器可一起運作來判斷正確的格式，從 API 傳回的資料。 我們會提供預設的支援，如 XML、 JSON 和表單 URL 編碼格式，而且您可以新增您自己的格式器，來擴充這項支援，或甚至取代預設的內容交涉策略。
- **模型繫結和驗證：** 模型繫結器提供簡單的方式擷取 HTTP 要求的各種組件中的資料，並將這些訊息部分轉換成.NET 物件可由 Web API 動作。
- **篩選器：** Web Api 現在支援篩選，包括知名的篩選條件，例如 [Authorize] 屬性。 您可以撰寫，並插入您自己的篩選動作、 授權和例外狀況處理。
- **查詢組合：** 只要傳回 IQueryable&lt;T&gt;，您的 Web API 會支援查詢透過 OData URL 慣例。
- **改善可測試性的詳細資料，HTTP:** 而不是靜態的內容物件中設定 HTTP 詳細資料之外，Web API 動作現在可以搭配 HttpRequestMessage 和 HttpResponseMessage 的執行個體。 可讓您使用您的自訂類型，除了 HTTP 型別也有這些物件的泛型版本。
- **改善的控制反轉 (IoC) 透過 dependencyresolver 來註冊：** Web API 現在使用 MVC 的相依性解析程式所實作的服務定位程式模式，來取得執行個體的許多不同的功能。
- **程式碼為基礎的組態：** Web API 完成只會透過程式碼，讓您設定檔案清除。
- **自我裝載：** Web Api 可以裝載在自己的程序，除了 IIS 之外，同時仍然使用路由的完整功能和其他功能的 Web API。

如需 ASP.NET Web API 的詳細資訊，請造訪[ https://www.asp.net/web-api ](../web-api/index.md)。

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>ASP.NET 單一頁面應用程式

ASP.NET MVC 4 現在包含建置單一頁面應用程式的重要使用 JavaScript 和 Web Api 的用戶端互動的體驗的早期預覽。 這項支援包括：

- 一組更豐富互動本機快取資料的 JavaScript 程式庫
- 額外的工作單位和 DAL 支援的 Web API 元件
- 若要快速開始使用 scaffolding MVC 專案範本

支援 ASP.NET MVC 4 中的單一頁面應用程式上的更多詳細資料請造訪[ https://www.asp.net/single-page-application ](../single-page-application/index.md)。

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>預設專案範本的增強功能

已更新用來建立新的 ASP.NET MVC 4 專案的範本來建立更現代化外觀的網站：

![](mvc4-beta-release-notes/_static/image1.png)

除了外觀的增強功能，已改進新的範本中的功能。 範本會針對稱為適應性呈現看來，在桌面瀏覽器和行動瀏覽器，而不需要任何自訂的技術。

![](mvc4-beta-release-notes/_static/image2.png)

若要查看作用中的自適性轉譯，您可以使用行動裝置的模擬器，或試試調整大小變得更小的桌面瀏覽器 視窗。 當瀏覽器視窗中取得夠小時，將變更頁面的配置。

預設專案範本的其他增強功能是使用 JavaScript 來提供更豐富的 UI。 使用範本中的登入和註冊連結是有關如何使用 jQuery UI 對話方塊呈現豐富的登入畫面的範例：

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>行動裝置專案範本

如果您正在新的專案，並想要建立站台，專為行動和平板電腦瀏覽器，您可以使用新的行動應用程式專案範本。 這是以基礎的 jQuery Mobile，開放原始碼程式庫建置觸控最佳化的 UI:

![](mvc4-beta-release-notes/_static/image4.png)

此範本包含與網際網路應用程式範本的應用程式結構相同 （而且控制器程式碼幾乎完全相同），但樣式呈現最佳效果，並能在觸控式行動裝置的行為使用 jQuery Mobile。 若要深入了解如何建構和設定行動裝置的 UI 的樣式，請參閱[jQuery 行動專案網站](http://jquerymobile.com/)。

如果您已經有的桌面導向的網站，您想要新增行動裝置最佳化的檢視，或如果您想要建立單一站台做桌面和行動瀏覽器的不同樣式化的檢視，您可以使用新的顯示模式功能。 （請參閱下一節。）

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>顯示模式

新的顯示模式功能可讓應用程式選取根據提出要求的瀏覽器的檢視。 例如，如果桌面瀏覽器要求首頁上，應用程式可能使用 Views\Home\Index.cshtml 範本。 如果在行動瀏覽器要求首頁上，應用程式可能會傳回 Views\Home\Index.mobile.cshtml 範本。

版面配置和部分可以也覆寫特定瀏覽器類型。 例如: 

- 如果您的 Views\Shared 資料夾同時包含\_Layout.cshtml 和\_Layout.mobile.cshtml 範本，根據預設，應用程式會使用\_Layout.mobile.cshtml與行動瀏覽器要求期間\_Layout.cshtml 期間其他要求。
- 如果資料夾同時包含\_MyPartial.cshtml 和\_MyPartial.mobile.cshtml，指示@Html.Partial(「\_MyPartial") 會轉譯\_MyPartial.mobile.cshtml 期間從行動裝置的要求瀏覽器和\_MyPartial.cshtml 期間其他要求。

如果您想要建立更具體的檢視表、 版面配置或針對其他裝置的部分檢視，您可以註冊新*DefaultDisplayMode*指定的名稱： 要求符合特定條件時，要搜尋的執行個體。 例如，您可以在其中新增下列程式碼*應用程式\_啟動*註冊"iPhone"字串做為適用於 Apple iPhone 瀏覽器提出要求時的顯示模式的 Global.asax 檔案中的方法：

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

執行此程式碼，當 Apple iPhone 瀏覽器要求之後，您的應用程式將使用 Views\Shared\\_Layout.iPhone.cshtml 版面配置 （若有的話）。

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile，檢視切換器，以及瀏覽器覆寫

jQuery Mobile 是開放原始碼程式庫建置觸控最佳化的 web UI。 如果您想要與 ASP.NET MVC 4 應用程式中使用 jQuery Mobile，您可以下載並安裝 NuGet 套件，可協助您開始著手。 若要安裝它從 Visual Studio Package Manager 主控台中，輸入下列命令：

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

這會安裝 jQuery Mobile 和一些協助程式檔案，包括下列：

- Views/Shared/\_Layout.Mobile.cshtml，這是 jQuery mobile 的版面配置。
- 檢視切換器元件，其中包含Views/Shared/\_ViewSwitcher.cshtml 部分檢視和 ViewSwitcherController.cs 控制站。

在安裝套件之後，執行您的應用程式使用行動瀏覽器 (或同等權限，例如 Firefox[使用者代理程式切換器](http://chrispederick.com/work/user-agent-switcher/)附加元件)。 您會看到，頁面看起來相當不同，因為 jQuery Mobile 處理配置和樣式設定。 若要利用這一點，您可以執行下列作業：

- 建立行動專用檢視覆寫，如底下所述[顯示模式](#_Toc303253810)之前 （例如，建立覆寫行動瀏覽器 Views\Home\Index.cshtml Views\Home\Index.mobile.cshtml）。
- 讀取[jQuery Mobile 文件](http://jquerymobile.com/)若要深入了解如何將觸控最佳化的 UI 項目加入行動檢視。

最適合行動裝置的網頁通常會加入其文字是像桌面檢視] 或 [完整的網站模式可讓使用者切換到桌面版本的頁面連結。 JQuery.Mobile.MVC 套件包含針對此目的的範例檢視切換器元件。 它會在預設 Views\Shared\\_Layout.Mobile.cshtml 檢視中，並在呈現的頁面時，其外觀如下：

![](mvc4-beta-release-notes/_static/image5.png)

如果造訪者按一下連結時，它們被切換到桌面版本的同一個頁面。

因為您桌面的配置不會包含檢視切換器，根據預設，訪客不會有行動模式取得的方法。 若要啟用此功能，加入下列參考 *\_ViewSwitcher*至您的桌面配置，只是內部*&lt;主體&gt;* 項目：

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

檢視切換程式會使用稱為 瀏覽器覆寫的新功能。 這項功能可讓您的應用程式處理要求，如同它們即將從不同的瀏覽器 （使用者代理程式） 與它們實際從。 下表列出可在瀏覽器覆寫的方法。

| `HttpContext.SetOverriddenBrowser(userAgentString)` | 使用指定的使用者代理程式要求的實際使用者代理程式 」 值會覆寫。 |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | 如果已不指定任何覆寫，會傳回要求的使用者代理程式覆寫值或實際使用者代理字串。 |
| `HttpContext.GetOverriddenBrowser()` | 傳回*HttpBrowserCapabilitiesBase*對應至目前設定為要求的使用者代理程式的執行個體 （實際或覆寫）。 您可以使用此值，取得屬性，例如*IsMobileDevice*。 |
| `HttpContext.ClearOverriddenBrowser()` | 移除目前要求的任何覆寫的使用者代理程式。 |

瀏覽器覆寫是 ASP.NET MVC 4 的核心功能，並為可用，即使您未安裝 jQuery.Mobile.MVC 封裝。 不過，它會影響檢視、 配置和部分檢視的選取範圍，它不會影響其他任何 ASP.NET 功能，取決於*Request.Browser*物件。

根據預設，使用者代理程式覆寫會使用 cookie 來儲存。 如果您想要存放覆寫其他地方 （例如，在資料庫中），您可以取代預設的提供者 (*BrowserOverrideStores.Current*)。 此提供者的文件會隨附了一個版本的 ASP.NET MVC。

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Visual Studio 中的程式碼產生的訣竅

新的 「 配方 」 功能可讓 Visual Studio 來產生您可以使用 NuGet 來安裝的套件為基礎的解決方案特定程式碼。 配方 framework 可方便開發人員撰寫程式碼產生外掛程式，您也可以使用新增區域、 新增控制器及 新增檢視取代內建的程式碼產生器。 因為配方會部署為 NuGet 套件時，可以輕鬆地簽入原始檔控制並自動共用的專案中的所有開發人員。 它們也有每個方案為基礎。

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>非同步控制器的的工作支援

您現在可以撰寫非同步動作方法傳回的型別物件的單一方法*任務*或是*工作&lt;ActionResult&gt;*。

例如，如果您使用 Visual C# 5 (或使用[Async CTP](https://msdn.microsoft.com/vstudio/async.aspx))，您可以建立非同步動作方法看起來如下所示：

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

在上一個動作方法中，呼叫*newsService.GetHeadlinesAsync*並*sportsService.GetScoresAsync*非同步呼叫，而不會封鎖執行緒集區的執行緒。

非同步動作方法會傳回*任務*執行個體也可以支援逾時。 若要讓您的動作方法可取消，新增類型的參數*CancellationToken*至動作方法簽章。 下列範例示範非同步動作方法具有逾時為 2500年毫秒，且會顯示*TimedOut*檢視用戶端，如果發生逾時。

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 Beta 支援年 9 月 2011 1.5 版的 Windows Azure SDK。

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的問題和重大變更

- **安裝 ASP.NET MVC 4 Beta 之後, CSHTML/VBHTML 編輯器，在 Visual Studio 2010 Service Pack 1 CSHTML/VBHTML 編輯器可能會暫停很長的時間之後輸入 cshtml 」 或 「 vbhtml 檔案內的程式碼片段或 JavaScript。** 這只會發生在具有剛剛建立，而且尚未經過編譯的 ASP.NET MVC 4 應用程式。

    因應措施是編譯專案，以取得組件的 bin 資料夾中。 請注意，是否您清除 [bin] 資料夾中移除組件的專案時，會回復編輯器問題。

    這將會在下一個版本中更正。
- **Visual Studio 11 beta 版的 C# 專案範本包含 Global.asax.cs 中不正確的連接字串。** 指定應用程式中的預設連接\_Start 方法，在 Visual Studio 11 Beta 中建立的專案包含 LocalDB 連接字串，其中包含未逸出反斜線 (\)字元。 這會導致連線錯誤時嘗試存取 Entity Framework DbContext，它會產生 SqlException。

    若要修正此問題，逸出反斜線字元在應用程式\_Start Global.asax.cs 的方法，使其內容，如下所示：

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **.NET 4.5 為目標的 ASP.NET MVC 4 應用程式將會擲回 FileLoadException 時嘗試存取 System.Net.Http.dll 組件時執行.NET 4.0。** ASP.NET MVC 4 應用程式在.NET 4.5 之下，建立包含繫結重新導向，將會導致 FileLoadException，指出 「 無法載入檔案或組件 'System.Net.Http' 或其中一個相依性。 」 應用程式執行時的系統上安裝.NET 4.0。 若要修正此問題，請將下列繫結重新導向移除 web.config:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    在已修改的 web.config 中的組件繫結項目應如下所示：

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <strong>Visual Basic 專案中的 「 新增控制器 」 項目範本會產生不正確的命名空間時叫用</strong><strong>從區域中。</strong> 當您將控制器加入 ASP.NET MVC 專案會使用 Visual Basic 中的區域時，項目範本會插入控制器的命名空間錯誤。 當您瀏覽至控制器中的任何動作時，結果會是 「 找不到檔案 」 錯誤。  
  
  產生的命名空間會省略所有項目之後的根命名空間。 比方說，產生的命名空間是*RootNamespace*但應該*RootNamespace.Areas.AreaName.Controllers* 。
- **Razor 檢視引擎的重大變更。** 隨著重寫 Razor 剖析器的詳細資訊，下列類型移除了*System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  也已移除下列方法： 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **當 WebMatrix.WebData.dll 包含 ASP.NET MVC 4 應用程式的 /bin 目錄中時，它會高於表單驗證的 URL。** （例如，藉由使用 [新增可部署的相依性] 對話方塊時，請選取 「 ASP.NET 網頁使用 Razor 語法 」） 時，將 WebMatrix.WebData.dll 組件新增至您的應用程式將會覆寫/帳戶/登入來驗證登入重新導向而 /帳戶/登入如預期般預設 ASP.NET MVC 帳戶控制器。 若要避免這種行為，並使用指定的 URL 已經在 web.config 的 [驗證] 區段中，您可以加入稱為 PreserveLoginUrl appSetting，並將它設定為 true: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **NuGet 套件管理員安裝嘗試安裝 ASP.NET MVC 4 的 Visual Studio 2010 和 Visual Web Developer 2010 的並存安裝時失敗。** 若要執行 Visual Studio 2010 和 Visual Web Developer 2010 與 ASP.NET MVC 4 中，您必須安裝 ASP.NET MVC 4 之後已安裝這兩個版本的 Visual Studio。
- **如果已解除安裝必要條件，解除安裝 ASP.NET MVC 4 就會失敗。** 若要完全解除安裝 ASP.NET MVC 4you 必須解除安裝 ASP.NET MVC 4，然後再解除安裝 Visual Studio。
- **執行預設的 Web API 專案會顯示不正確地將使用者導向至新增路由使用 RegisterApis 方法不存在的指示。** 使用 ASP.NET 路由表 RegisterRoutes 方法中應該新增路由。
- **安裝 ASP.NET MVC 4 Beta 中斷 ASP.NET MVC 3 RTM 應用程式。** ASP.NET MVC 3 應用程式所建立的 rtm 版本 （不含 ASP.NET MVC 3 Tools Update 版本） 必須執行下列變更才能使用 ASP.NET MVC 4 Beta 並存。 建置專案，而不需要進行這些更新結果中的編譯錯誤。 

    **必要的更新**

  1. 在根 Web.config 檔案中，加入新*&lt;appSettings&gt;* 與索引鍵的項目*webPages:Version* ，而*1.0.0.0*。

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. 在 方案總管 中，以滑鼠右鍵按一下專案名稱，然後選取 卸載專案 然後再次以滑鼠右鍵按一下名稱，然後選取 編輯*ProjectName*.csproj。
  3. 找出下列組件參考： 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      您可以將它們取代為下列：

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. 儲存變更，請關閉您所編輯，然後以滑鼠右鍵按一下專案並選取 重新載入專案 (.csproj) 檔案。
