---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: "本文件說明 ASP.NET MVC 4 Beta for Visual Studio 2010 的版本。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: 58ae178a0e6578d8353e1a4e9d67fc1026e99f55
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> 本文件說明 ASP.NET MVC 4 Beta for Visual Studio 2010 的版本。
> 
> > [!NOTE]
> > 這不是最新的版本。 ASP.NET MVC 4 RC 版本資訊可用[這裡](mvc4-release-notes.md)。


- [安裝注意事項](#_Toc303253802)
- [文件 (英文)](#_Toc303253803)
- [支援](#_Toc303253804)
- [軟體需求](#_Toc303253805)
- [ASP.NET MVC 3 專案升級至 ASP.NET MVC 4](#_Toc303253806)
- [ASP.NET MVC 4 Beta 的新功能](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [ASP.NET 單一頁面應用程式](#_Toc317096198)
    - [預設專案範本的增強功能](#_Toc303253808)
    - [行動裝置的專案範本](#_Toc303253809)
    - [顯示模式](#_Toc303253810)
    - [jQuery Mobile，檢視切換程式，並覆寫瀏覽器](#_Toc303253811)
    - [在 Visual Studio 中的程式碼產生的訣竅](#_Toc303253812)
    - [非同步控制器的工作支援](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [已知的問題和重大變更](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>安裝注意事項

可以從安裝 ASP.NET MVC 4 Beta for Visual Studio 2010 [ASP.NET MVC 4 首頁](../mvc/mvc4.md)使用 Web Platform Installer。

您必須解除安裝 ASP.NET MVC 4 Beta 之前的 ASP.NET MVC 4 的任何先前安裝的預覽。

此版本不相容的.NET Framework 4.5 Developer Preview。 您必須解除安裝.NET 4.5 Developer Preview，然後再安裝 ASP.NET MVC 4 Beta。

可以安裝 ASP.NET MVC 4，並且可以執行的並行使用 ASP.NET MVC 3。

<a id="_Toc303253803"></a>
## <a name="documentation"></a>文件

ASP.NET mvc 的文件位於 MSDN 網站，位於下列 URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

教學課程和其他關於 ASP.NET MVC 的資訊位於 ASP.NET 網站的 MVC 4 頁 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md))。

<a id="_Toc303253804"></a>
## <a name="support"></a>支援

這是預覽版本，而且未正式支援。 如果您有關於使用此版本的問題，張貼至 ASP.NET MVC 論壇 ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx))，其中的 ASP.NET 社群成員可經常提供非正式的支援。

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>軟體需求

Visual Studio 的 ASP.NET MVC 4 元件需要 PowerShell 2.0 和 Service Pack 1 的 Visual Studio 2010 或 Visual Web Developer Express 2010 Service Pack 1。

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>ASP.NET MVC 3 專案升級至 ASP.NET MVC 4

ASP.NET MVC 4 可以在同一部電腦，讓您彈性選擇何時要升級至 ASP.NET MVC 4 的 ASP.NET MVC 3 應用程式的 ASP.NET MVC 3 並存安裝。

升級的最簡單方式是建立新的 ASP.NET MVC 4 專案，並複製所有的檢視、 控制器、 程式碼和內容檔案從現有的 MVC 3 專案加入新的專案，然後更新組件來參考新的專案，以符合舊的專案中。 如果您已經變更 MVC 3 專案中的 Web.config 檔案，您也必須合併那些變更成 MVC 4 專案中的 Web.config 檔案。

若要手動升級現有的 ASP.NET MVC 3 應用程式第 4 版，執行下列作業：

1. 在專案 （有一個根目錄中的專案在 [檢視] 資料夾中，一個在您的專案中的每個區域的 [檢視] 資料夾） 中的所有 Web.config 檔案，將每個執行個體的下列文字：

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    使用下列相對應的文字：

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. 在根 Web.config 檔案中，更新*webPages:Version* "2.0.0.0"的項目並加入新*PreserveLoginUrl*具有值"true"的機碼：

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. 在 [方案總管] 中，刪除其參考*System.Web.Mvc* （指向版本 3 DLL）。 然後將參考加入*System.Web.Mvc* (v4.0.0.0)。 特別是，進行下列變更來更新組件參考。 詳細資料如下：

    1. 在方案總管 中，刪除下列組件的參考： 

        - *System.Web.Mvc*(v3.0.0.0)
        - *System.Web.WebPages*(v1.0.0.0)
        - *System.Web.Razor*(v1.0.0.0)
        - *System.Web.WebPages.Deployment*(v1.0.0.0)
        - *System.Web.WebPages.Razor*(v1.0.0.0)
    2. 加入下列組件的參考： 

        - *System.Web.Mvc*(v4.0.0.0)
        - *System.Web.WebPages*(v2.0.0.0)
        - *System.Web.Razor*(v2.0.0.0)
        - *System.Web.WebPages.Deployment*(v2.0.0.0)
        - *System.Web.WebPages.Razor*(v2.0.0.0)
4. 在 方案總管 中，以滑鼠右鍵按一下專案名稱，然後選取 卸載專案。 然後名稱上按一下滑鼠右鍵，然後選取 編輯*ProjectName*.csproj。
5. 找出*ProjectTypeGuids*項目和取代 {E53F8FEA-EAE0-44A6-8774-FFD645390401} {E3E379DF-F4C6-4180-9B81-6769533ABE47}。
6. 儲存的變更，關閉您所編輯的專案 (.csproj) 檔案、 以滑鼠右鍵按一下專案，然後選取重新載入專案。
7. 如果專案參考任何協力廠商程式庫編譯使用舊版 ASP.NET MVC，請開啟根 Web.config 檔案，並加入下列三個*bindingRedirect*下的項目*組態*> 一節： 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>ASP.NET MVC 4 Beta 的新功能

本節說明已引進的功能在 ASP.NET MVC 4 Beta 版本。

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4 現在包含 ASP.NET Web API，用來建立 HTTP 服務的新架構可達到廣泛的用戶端包括瀏覽器和行動裝置。 ASP.NET Web 應用程式開發介面也是用於建立 RESTful 服務的理想平台。

ASP.NET Web API 包含下列功能的支援：

- **現代 HTTP 程式設計模型：**直接存取及管理 HTTP 要求和回應您 Web 應用程式開發介面使用新的強型別 HTTP 物件模型。 相同程式設計模型和 HTTP 管線功能對稱用戶端透過新的 HttpClient 型別。
- **完整支援路由**: Web Api 現在支援一組完整的路由功能已經 Web 堆疊，包括路由參數和條件約束的一部分。 此外，對應到動作具有完整支援慣例，因此您不再需要套用屬性，例如 [HttpPost] 到您的類別和方法。
- **內容交涉**： 用戶端和伺服器可一起運作來判斷正確的格式從 API 傳回的資料。 我們提供預設的支援，如 XML、 JSON 和表單 URL 編碼格式，而且您可以擴充此一支援藉由新增您自己的格式器，或甚至取代預設內容交涉的策略。
- **模型繫結和驗證：**模型繫結器提供一個簡單的方式，從 HTTP 要求的各種組件中擷取資料，並將這些訊息部分轉換成.NET 物件可由 Web API 的動作。
- **篩選條件：** Web Api 現在支援篩選，包括知名的篩選條件，例如 [Authorize] 屬性。 您可以撰寫，並插入您自己的篩選動作、 授權和例外狀況處理。
- **在查詢組合：**只要傳回 IQueryable&lt;T&gt;，Web API 將支援查詢透過 OData URL 慣例。
- **改善可測試性的詳細資料，HTTP:**而不是靜態內容物件中設定 HTTP 詳細資料，Web API 動作現在可以搭配 HttpRequestMessage 和 HttpResponseMessage 的執行個體。 這些物件的泛型版本也存在於讓您使用您的自訂類型，除了 HTTP 型別。
- **改善的逆轉控制 (IoC) 透過 dependencyresolver 來註冊：** Web API 現在使用 MVC 的相依性解析程式所實作的服務定位程式模式來取得執行個體的許多不同的功能。
- **程式碼為基礎的組態：**等作業只透過程式碼已完成 Web API 設定、 清除離開您的組態檔。
- **自我裝載：** Web Api 可以裝載於自己的處理序，除了 IIS 時仍在使用路由的完整功能及其他功能的 Web API。

如需 ASP.NET Web API 的詳細資訊，請造訪[https://www.asp.net/web-api](../web-api/index.md)。

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>ASP.NET 單一頁面應用程式

ASP.NET MVC 4 現在包含早期預覽中建置單一頁面應用程式的重要使用 JavaScript 和 Web Api 的用戶端互動的體驗。 這項支援包括：

- 一組更豐富的本機互動與快取資料的 JavaScript 程式庫
- 為單位的工作，以及 DAL 支援的其他 Web API 元件
- 與以便迅速開始使用 scaffolding MVC 專案範本

針對支援 ASP.NET MVC 4 中的單一頁面應用程式上的更多詳細資料，請瀏覽[https://www.asp.net/single-page-application](../single-page-application/index.md)。

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>預設專案範本的增強功能

已更新用來建立新的 ASP.NET MVC 4 專案的範本來建立更現代尋找網站：

![](mvc4-beta-release-notes/_static/image1.png)

除了外觀的增強功能，已改善功能的新範本。 範本會使用稱為調整看來，桌面瀏覽器和行動裝置沒有任何自訂的瀏覽器中呈現的技術。

![](mvc4-beta-release-notes/_static/image2.png)

若要查看作用中的適應性呈現，您可以使用行動裝置的模擬器，或只要再試一次調整大小較小的桌面瀏覽器視窗。 當瀏覽器視窗中取得夠小時，將變更的頁面配置。

預設專案範本的其他增強功能，就是使用 JavaScript，提供更豐富的 UI。 使用範本中的登入和註冊連結是有關如何使用 jQuery UI 對話方塊呈現的豐富的登入畫面的範例：

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>行動裝置的專案範本

如果您開始新的專案，並想要建立站台專為行動和平板電腦瀏覽器，您可以使用新的行動應用程式專案範本。 這根據 jQuery Mobile，用於建置觸控最佳化 UI 的開放原始碼程式庫：

![](mvc4-beta-release-notes/_static/image4.png)

此範本包含網際網路應用程式範本相同的應用程式結構 （而且控制器的程式碼幾乎完全相同），但卻能呈現最佳效果，並能在觸控式行動裝置的行為使用 jQuery Mobile 的樣式。 若要了解有關如何在結構和樣式行動 UI 的詳細資訊，請參閱[jQuery 行動專案網站](http://jquerymobile.com/)。

如果您已經有您想要加入，行動裝置最佳化的檢視，或如果您想要建立單一站台提供不同樣式桌上型電腦和行動瀏覽器來檢視桌面導向網站，您可以使用新的顯示模式功能。 （請參閱下一節）。

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>顯示模式

新的顯示模式功能可讓應用程式選取依提出要求的瀏覽器的檢視。 例如，如果桌面瀏覽器要求在首頁上，應用程式可能會使用 Views\Home\Index.cshtml 範本。 如果行動瀏覽器要求在首頁上，應用程式可能會傳回 Views\Home\Index.mobile.cshtml 範本。

版面配置和 partials 也會覆寫特定瀏覽器類型。 例如: 

- 如果同時 Views\Shared 資料夾包含\_Layout.cshtml 和\_Layout.mobile.cshtml 範本，應用程式會使用預設\_行動瀏覽器和要求期間Layout.mobile.cshtml\_Layout.cshtml 期間其他要求。
- 如果資料夾包含同時\_MyPartial.cshtml 和\_MyPartial.mobile.cshtml，指示@Html.Partial("\_MyPartial") 會轉譯\_MyPartial.mobile.cshtml 期間從行動裝置的要求瀏覽器和\_MyPartial.cshtml 期間其他要求。

如果您想要建立更詳細的檢視、 配置、 或其他裝置的部分檢視，您可以註冊新*DefaultDisplayMode*指定的名稱： 要求符合特定條件時，要搜尋的執行個體。 例如，您可以加入下列程式碼加入*應用程式\_啟動*Global.asax 檔案以登錄為適用於 Apple iPhone 瀏覽器提出要求時的顯示模式的字串"iPhone"中的方法：

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

執行此程式碼，當提出要求時 Apple iPhone 瀏覽器之後，您的應用程式都會使用 _layout.cshtml\\_Layout.iPhone.cshtml 配置 （如果有的話）。

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile，檢視切換程式，並覆寫瀏覽器

jQuery Mobile 是開放原始碼程式庫建置觸控最佳化 web UI。 如果您想要的 ASP.NET MVC 4 應用程式使用 jQuery Mobile，您可以下載並安裝的 NuGet 封裝，可協助您開始。 若要安裝的 Visual Studio Package Manager Console 中，請輸入下列命令：

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

這會安裝 jQuery Mobile 和一些協助程式檔案，包括下列：

- Views/Shared/\_Layout.Mobile.cshtml，為 jQuery Mobile 架構版面配置。
- 檢視切換程式元件，其中包含Views/Shared/\_ViewSwitcher.cshtml 部分檢視和 ViewSwitcherController.cs 控制站。

安裝封裝之後，執行應用程式使用行動裝置瀏覽器 (或同等權限，例如 Firefox[使用者代理程式切換器](http://chrispederick.com/work/user-agent-switcher/)附加元件)。 您會看到，您的網頁看起來非常不同，因為 jQuery Mobile 處理配置和樣式設定。 若要利用這一點，您可以執行下列作業：

- 建立行動裝置的特定檢視覆寫，如底下所述[顯示模式](#_Toc303253810)之前 （例如，建立 Views\Home\Index.mobile.cshtml 來覆寫 Views\Home\Index.cshtml 行動瀏覽器）。
- 讀取[jQuery Mobile 說明文件](http://jquerymobile.com/)若要深入了解如何新增行動檢視觸控最佳化 UI 項目。

行動裝置最佳化網頁的慣例是頁面的加入連結，其文字是頁面的像桌面檢視或完整的網站模式可讓使用者切換到桌面版本。 JQuery.Mobile.MVC 封裝包含可用於此用途範例檢視切換程式元件。 在預設 _layout.cshtml\\_Layout.Mobile.cshtml 檢視中，並在呈現的頁面時，看起來像這樣：

![](mvc4-beta-release-notes/_static/image5.png)

訪客按一下連結，如果它們正在切換到桌面版本的相同的頁面。

因為您桌面的配置不會包含檢視切換程式預設的訪客將不能為行動模式取得的方法。 若要啟用此功能，加入下列參考 *\_ViewSwitcher*到您桌面的配置，只是內部*&lt;主體&gt;*項目：

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

檢視切換程式會使用新功能，稱為 瀏覽器覆寫。 這項功能可讓您的應用程式要求視為就像是從不同的瀏覽器 （使用者代理程式） 與它們實際從。 下表列出可在瀏覽器覆寫的方法。

| `HttpContext.SetOverriddenBrowser(userAgentString)` | 使用指定的使用者代理程式要求的實際使用者代理程式值會覆寫。 |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | 如果尚未指定任何覆寫會傳回要求的使用者代理程式覆寫值或實際使用者代理字串。 |
| `HttpContext.GetOverriddenBrowser()` | 傳回*HttpBrowserCapabilitiesBase*對應於目前針對要求設定的使用者代理程式執行個體 （實際或覆寫）。 您可以使用此值來取得屬性，例如*IsMobileDevice*。 |
| `HttpContext.ClearOverriddenBrowser()` | 移除目前要求的任何覆寫的使用者代理程式。 |

瀏覽器正在覆寫 ASP.NET MVC 4 的核心功能，而且可以使用，即使您未安裝 jQuery.Mobile.MVC 封裝。 不過，它會影響檢視、 配置和部分檢視的選取範圍，不會影響其他任何 ASP.NET 功能相依於*Request.Browser*物件。

根據預設，使用者代理程式覆寫會使用 cookie 來儲存。 如果您想要存放覆寫其他位置 （例如，在資料庫中），您可以取代預設提供者 (*BrowserOverrideStores.Current*)。 此提供者的文件會隨附較新版的 ASP.NET MVC。

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>在 Visual Studio 中的程式碼產生的訣竅

新的配方功能可讓 Visual Studio 為套件產生您可以使用 NuGet 來安裝的套件為基礎的解決方案專用程式碼。 配方架構，簡化開發人員撰寫程式碼產生外掛程式，您也可以使用新增區域、 加入控制器，並加入檢視來取代內建的程式碼產生器。 配方部署為 NuGet 封裝，因為它們可輕鬆簽入原始檔控制及自動在專案上的所有開發人員與共用。 它們也會提供每個解決方案為基礎。

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>非同步控制器的工作支援

現在您可以撰寫非同步動作方法為單一方法的傳回型別的物件*工作*或*工作&lt;ActionResult&gt;*。

例如，如果您使用 Visual C# 5 (或使用[非同步 CTP](https://msdn.microsoft.com/vstudio/async.aspx))，您可以建立非同步動作方法看起來如下所示：

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

在上一個動作方法的呼叫*newsService.GetHeadlinesAsync*和*sportsService.GetScoresAsync*非同步呼叫，而不會封鎖執行緒集區的執行緒。

非同步動作方法會傳回*工作*執行個體也可以支援逾時。 若要在動作方法可取消，請加入類型參數的*CancellationToken*至動作方法簽章。 下列範例示範非同步動作方法，有 2500年毫秒的逾時，而且會顯示*TimedOut*檢視用戶端，如果發生逾時。

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 Beta 支援年 9 月 2011 1.5 版的 Windows Azure sdk。

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的問題和重大變更

- **安裝 ASP.NET MVC 4 Beta 之後, CSHTML/VBHTML 編輯器，在 Visual Studio 2010 Service Pack 1 CSHTML/VBHTML 編輯器中的可能會暫停輸入 cshtml 」 或 「 vbhtml 檔內的程式碼片段或 JavaScript 之後很長的時間。** 這只會發生在具有剛剛建立，而且尚未編譯的 ASP.NET MVC 4 應用程式。

    因應措施是編譯的專案，以取得組件的 bin 資料夾中。 請注意，是否您清除它會從 bin 資料夾移除組件專案，將會返回編輯器問題。

    這將會更正的下一個版本。
- **Visual Studio 11 Beta 的 C# 專案範本包含 Global.asax.cs 中不正確的連接字串。** 指定應用程式中的預設連接\_Start 方法的 Visual Studio 11 beta 版中建立的專案包含 LocalDB 連接字串，其中包含未逸出反斜線 (\)字元。 這會導致連接錯誤時嘗試存取 Entity Framework DbContext，它會產生 SqlException。

    若要更正此問題，逸出反斜線字元的應用程式中\_啟動 Global.asax.cs 方法，使它會讀取，如下所示：

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **.NET 4.5 為目標的 ASP.NET MVC 4 應用程式將會擲回 FileLoadException 時嘗試存取 System.Net.Http.dll 組件時執行.NET 4.0。** 在.NET 4.5 下建立的 ASP.NET MVC 4 應用程式包含的繫結重新導向，將會導致 FileLoadException，指出 「 無法載入檔案或組件 'System.Net.Http' 或其中一個相依性。 」 當應用程式執行時的系統上安裝.NET 4.0。 若要更正此問題，請從 web.config 中移除下列繫結重新導向：

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    已修改的 web.config 中的組件繫結項目應該會出現，如下所示：

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- **「 新增控制器 」 項目範本在 Visual Basic 專案會產生不正確的命名空間時叫用 * * * 從區域內。** 當您將控制器加入 ASP.NET MVC 專案會使用 Visual Basic 中的區域時，項目範本會插入控制器錯誤的命名空間。 當您巡覽至控制器中的任何動作時，結果會是 「 找不到檔案 」 錯誤。  
  
 產生的命名空間會省略所有項目之後的根命名空間。 例如，產生的命名空間是*RootNamespace*但應該是*RootNamespace.Areas.AreaName.Controllers* 。
- **Razor 檢視引擎中的重大變更。** 重寫成的 Razor 剖析器的一部分，下列類型已從移除*System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

 也已移除下列方法： 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **當 WebMatrix.WebData.dll 隨附於 ASP.NET MVC 4 應用程式的 /bin 目錄中時，因此它會接管表單驗證的 URL。** WebMatrix.WebData.dll 組件加入至應用程式 （例如，藉由使用 [新增可部署的相依性] 對話方塊時，請選取 「 ASP.NET Web 網頁使用 Razor 語法 」） 將會覆寫/帳戶/登入驗證登入重新導向而 /帳戶/登入以預期的 ASP.NET MVC 帳戶控制器的預設方式。 要避免這種行為，並使用 web.config 的 [驗證] 區段中已指定的 URL，您可以加入稱為 PreserveLoginUrl appSetting 並將它設定為 true: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **NuGet 套件管理員安裝嘗試安裝 ASP.NET MVC 4 的 Visual Studio 2010 和 Visual Web Developer 2010 的並存安裝時失敗。** 若要執行 Visual Studio 2010 和 Visual Web Developer 2010 並存 ASP.NET MVC 4 中，您必須安裝 ASP.NET MVC 4 之後已安裝 Visual Studio 的兩個版本。
- **如果已經已解除安裝先決條件，解除安裝 ASP.NET MVC 4 會失敗。** 若要完全解除安裝 ASP.NET MVC 4you 必須解除安裝 ASP.NET MVC 4，然後再解除安裝 Visual Studio。
- **執行預設的 Web API 專案會顯示不正確地將使用者導向至加入路由使用 RegisterApis 方法不存在的指示。** 使用 ASP.NET 路由表 RegisterRoutes 方法中，加入路由。
- **安裝 ASP.NET MVC 4 Beta 中斷 ASP.NET MVC 3 RTM 應用程式。** ASP.NET MVC 3 應用程式所建立的 rtm 發行版本 （不能搭配 ASP.NET MVC 3 Tools Update 版本） 會需要下列變更才能使用 ASP.NET MVC 4 Beta 並存。 建置專案，而不需要進行這些更新的結果中的編譯錯誤。 

    **必要的更新**

    1. 在根 Web.config 檔案中，加入新 *&lt;appSettings&gt;* 具有索引鍵的項目*webPages:Version*和值*1.0.0.0*。

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
    2. 在 方案總管 中，以滑鼠右鍵按一下專案名稱，然後選取 卸載專案。 然後名稱上按一下滑鼠右鍵，然後選取 編輯*ProjectName*.csproj。
    3. 找出下列組件參考： 

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

        它們取代成下列：

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
    4. 儲存變更，請關閉您先前所編輯，然後以滑鼠右鍵按一下專案並選取重新載入專案 (.csproj) 檔案。
