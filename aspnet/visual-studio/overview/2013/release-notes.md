---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET 及 Web Tools for Visual Studio 2013 版本資訊 |Microsoft Docs
author: microsoft
description: 本文件說明版本的 ASP.NET 和 Web Tools for Visual Studio 2013。
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: 43878bc101ef97e8bbb6c150f4125707da7660c9
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244953"
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET 及 Web Tools for Visual Studio 2013 版本資訊
====================
by [Microsoft](https://github.com/microsoft)

> 本文件說明版本的 ASP.NET 和 Web Tools for Visual Studio 2013。


## <a name="contents"></a>內容

- [安裝注意事項](#TOC1)
- [文件](#TOC2)
- [軟體需求](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>在 ASP.NET 和 Web Tools for Visual Studio 2013 中的新功能

- [One ASP.NET](#TOC6)
- [新的 Web 專案體驗](#newproj)
- [ASP.NET Scaffolding](#scaffold)
- [瀏覽器連結](#browser-link)
- [Visual Studio Web 編輯器增強功能](#web-editor)
- [Visual Studio 中的 azure App Service Web 應用程式支援](#waws)
- [Web 發行增強功能](#publish)
- [NuGet 2.7](#nuget)
- [ASP.NET Web Form](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.NET Web API 2](#TOC11)
- [ASP.NET SignalR](#TOC13)
- [ASP.NET 身分識別](#TOC8)
- [Microsoft OWIN 元件](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [ASP.NET 應用程式暫止](#TOC15)
- [已知的問題和重大變更](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>安裝注意事項

ASP.NET 及 Web Tools for Visual Studio 2013 中主要的安裝程式會配套，並可下載[此處](https://www.asp.net/downloads)。

<a id="TOC2"></a>
## <a name="documentation"></a>文件

教學課程和其他相關 ASP.NET 及 Web Tools for Visual Studio 2013 的資訊都是從[ASP.NET 網站](https://www.asp.net/)。

<a id="TOC4"></a>
## <a name="software-requirements"></a>軟體需求

ASP.NET 和 Web 工具需要 Visual Studio 2013。

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>在 ASP.NET 和 Web Tools for Visual Studio 2013 中的新功能

下列各節會說明已在版本中引進的功能。

<a id="TOC6"></a>
## <a name="one-aspnet"></a>One ASP.NET

Visual Studio 2013 版本中，我們已達到統一的體驗，讓您輕鬆混合和比對您想的要使用 ASP.NET 技術的步驟。 比方說，您可以開始使用 MVC 專案並輕鬆地更新版本中，新增至專案的 Web Form 網頁或 Web Form 專案中建立 Web Api 的結構。 One ASP.NET 是方便您身為開發人員執行動作，您所喜愛的 ASP.NET。 無論您選擇何種技術，您可以讓您 One ASP.NET 的信任基礎架構建置的信心。

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>新的 Web 專案體驗

我們已增強 Visual Studio 2013 中建立新的 web 專案的體驗。 在 **新的 ASP.NET Web 專案**對話方塊，您可以選取專案類型、 設定技術 (Web Form、 MVC、 Web API) 的任何組合，設定驗證選項，並將單元測試專案。

![新增 ASP.NET 專案](release-notes/_static/image1.png)

[新增] 對話方塊可讓您變更許多範本的預設驗證選項。 比方說，當建立 ASP.NET Web Form 專案時，您可以選取下列任何選項：

- 沒有驗證
- 個別使用者帳戶 （ASP.NET 成員資格或社交提供者登入）
- 組織帳戶 (Active Directory 中的網際網路應用程式)
- Windows 驗證 (內部網路應用程式中的 Active Directory)

![驗證選項](release-notes/_static/image2.png)

如需有關建立 web 專案的新程序的詳細資訊，請參閱 < [Visual Studio 2013 中建立 ASP.NET Web 專案](creating-web-projects-in-visual-studio.md)。 如需新的驗證選項的詳細資訊，請參閱[ASP.NET Identity](#TOC8)本文件稍後的。

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

ASP.NET Scaffold 是 ASP.NET Web 應用程式的程式碼產生架構。 它可讓您輕鬆地將未定案程式碼新增至您的資料模型進行互動的專案。

在舊版的 Visual Studio 中，scaffolding 已限制為 ASP.NET MVC 專案。 使用 Visual Studio 2013 中，您現在可以針對任何 ASP.NET 專案，包括 Web Form，以使用 scaffolding。 Visual Studio 2013 目前不支援產生的頁面針對 Web Form 專案，但您仍然可以使用與 Web Forms scaffolding，將 MVC 相依性新增至專案。 將在未來的更新中新增支援產生的 Web Form 的頁面。

當使用 scaffolding，我們會確保所有必要的相依性安裝到專案。 比方說，如果您開始使用 ASP.NET Web Form 專案，然後使用 scaffolding Web API 控制器，將必要的 NuGet 套件和參考會加入至您的專案會自動。

若要加入 Web Form 專案中的 MVC scaffolding，新增**新的 Scaffold 項目**，然後選取**MVC 5 相依性**對話視窗中。 有兩個選項，scaffolding MVC;最少且完整。 如果您選取最小，只有 NuGet 套件和 ASP.NET mvc 的參考會加入到專案中。 如果您選取 [完整] 選項中，加入基本的相依性，以及 MVC 專案所需的內容檔案。

Scaffolding 非同步控制器的支援會使用 Entity Framework 6 的新非同步功能。

如需詳細資訊和教學課程，請參閱 < [ASP.NET Scaffolding 概觀](aspnet-scaffolding-overview.md)。

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>瀏覽器連結-瀏覽器和 Visual Studio 之間的 SignalR 通道

新[瀏覽器連結](using-browser-link.md)功能可讓您連接到 Visual Studio 的多個瀏覽器，並加以重新整理所有依序按一下工具列中的按鈕。 您可以連接到您的開發網站，包括行動裝置的模擬器的多個瀏覽器，並按一下 重新整理以重新整理所有的瀏覽器，全都在同一時間。 瀏覽器連結也會公開 API，以讓開發人員撰寫瀏覽器連結延伸模組。

![](release-notes/_static/image3.png)

藉由啟用開發人員可以善用瀏覽器連結 API，因此您可以建立 Visual Studio 和任何已連線的瀏覽器之間跨越界限的非常進階的案例。 Web Essentials 會利用 API 來建立 Visual Studio 和瀏覽器的開發人員工具、 遠端控制行動裝置的模擬器和更多之間的整合式的體驗。

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Visual Studio Web 編輯器增強功能

Visual Studio 2013 包含新的 HTML 編輯器，Razor 檔案和 HTML 檔案中的 web 應用程式。 新的 HTML 編輯器提供單一的統一結構描述，以 HTML5 為基礎。 它有括號自動完成、 jQuery UI 和 AngularJS 屬性 IntelliSense 中，屬性 IntelliSense 群組、 識別碼和類別名稱 Intellisense，以及其他功能改良，包括更佳的效能、 格式化和智慧標籤。

下列螢幕擷取畫面示範如何使用 HTML 編輯器中的啟動程序屬性的 IntelliSense。

![HTML 編輯器中的 Intellisense](release-notes/_static/image4.png)

Visual Studio 2013 也是這兩個 CoffeeScript 與小於內建的編輯器。 LESS 編輯器的所有酷炫的功能來自 CSS 編輯器，且跨所有較少的文件中有特定的 Intellisense，變數和 mixin@import鏈結。

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Visual Studio 中的 azure App Service Web 應用程式支援

在使用 Azure SDK for.NET 2.2 的 Visual Studio 2013，您可以使用**伺服器總管**直接與遠端的 web 應用程式互動。 您可以登入您的 Azure 帳戶、 建立新的 web 應用程式、 設定應用程式、 檢視即時記錄，以及更多。 即將推出不久之後 SDK 2.2 發行，您將能夠在 Azure 中遠端偵錯模式執行。 當您安裝最新版的 Azure SDK for.NET 時，大部分的 Azure App Service Web Apps 的新功能也在 Visual Studio 2012 中運作。

如需詳細資訊，請參閱下列資源：

- [在 Azure App Service 中建立 ASP.NET web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Web 發行增強功能

Visual Studio 2013 包含全新和增強的 Web 發佈功能。 以下是幾個：

- 輕鬆地[自動化 Web.config 檔案加密](https://go.microsoft.com/fwlink/?LinkId=325529)。 （此連結和下列兩個點，可能無法在 10/17 傍晚之前使用 MSDN 上的文件。）
- 輕鬆地[自動化取得應用程式離線部署期間](https://go.microsoft.com/fwlink/?LinkId=325530)。
- 設定 Web Deploy[使用檔案總和檢查碼，而不是最後變更日期](https://go.microsoft.com/fwlink/?LinkId=325531)來判斷哪些檔案應該複製到伺服器。
- 當您使用 FTP 或檔案系統發佈方法時，以及利用 Web Deploy，快速發行個別選取的檔案 （包括 Web.config）。

如需有關 ASP.NET web 部署的詳細資訊，請參閱[ASP.NET 網站](https://go.microsoft.com/fwlink/?LinkId=322027)。

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 包含一組豐富的新功能所說明的詳細討論[NuGet 2.7 版本資訊](http://docs.nuget.org/docs/release-notes/nuget-2.7)。

這個版本的 NuGet 也會移除需要提供明確的同意，以下載套件的 NuGet 的套件還原功能。 藉由安裝 NuGet 現在授與同意 （與 NuGet 的 [喜好設定] 對話方塊中的相關聯的核取方塊）。 現在套件還原預設中只會搭配運作。

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>ASP.NET Web Form

### <a name="one-aspnet"></a>One ASP.NET

Web Form 專案範本緊密整合新的 One ASP.NET 體驗中。 您可以新增至您的 Web Form 專案，支援的 MVC 和 Web API，您可以設定使用 [One ASP.NET 專案建立精靈] 的驗證。 如需詳細資訊，請參閱 < [Visual Studio 2013 中建立 ASP.NET Web 專案](creating-web-projects-in-visual-studio.md)。

### <a name="aspnet-identity"></a>ASP.NET Identity

Web Form 專案範本支援新的 ASP.NET 身分識別架構。 此外，範本現在支援建立 Web Form 內部網路專案。 如需詳細資訊，請參閱 <<c0> [ 驗證方法](creating-web-projects-in-visual-studio.md#auth)中**在 Visual Studio 2013 中建立 ASP.NET Web 專案**。

### <a name="bootstrap"></a>啟動程序

Web Form 範本會使用[Bootstrap](http://twitter.github.io/bootstrap/)提供簡潔且回應迅速的外觀和操作可以輕易自訂。 如需詳細資訊，請參閱 < [Visual Studio 2013 web 專案範本中的進行啟動載入](creating-web-projects-in-visual-studio.md#bootstrap)。

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>One ASP.NET

Web MVC 專案範本緊密整合新的 One ASP.NET 體驗中。 您可以自訂您的 MVC 專案，並使用 [One ASP.NET 專案建立精靈] 設定驗證。 ASP.NET MVC 5 入門教學課程，請參閱[Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md)。

將 MVC 4 專案升級至 MVC 5 的資訊，請參閱[如何將 ASP.NET MVC 4 和 Web API 專案升級至 ASP.NET MVC 5 和 Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)。

### <a name="aspnet-identity"></a>ASP.NET Identity

若要使用 ASP.NET 身分識別進行驗證和身分識別管理 MVC 專案範本已更新。 Facebook 和 Google 驗證以及新的成員資格 API 的教學課程，請參閱[建立 ASP.NET MVC 5 應用程式使用 Facebook 和 Google OAuth2 和 OpenID 登入](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)和[驗證建立 ASP.NET MVC 應用程式和SQL DB 並部署至 Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)。

### <a name="bootstrap"></a>啟動程序

MVC 專案範本已更新為使用[Bootstrap](http://getbootstrap.com/)提供簡潔且回應迅速的外觀和操作可以輕易自訂。 如需詳細資訊，請參閱 < [Visual Studio 2013 web 專案範本中的進行啟動載入](creating-web-projects-in-visual-studio.md#bootstrap)。

### <a name="authentication-filters"></a>驗證篩選條件

驗證篩選條件是一種新的 ASP.NET MVC 管線中的授權篩選條件之前執行，並可讓您指定驗證邏輯每個動作，ASP.NET MVC 中的篩選每個控制器，或全域的所有控制站。 驗證篩選條件會處理在要求中的認證，並提供對應的主體。 驗證篩選條件也可以加入驗證挑戰回應未經授權的要求。

### <a name="filter-overrides"></a>篩選覆寫

此外，您現在可以覆寫指定覆寫篩選條件的篩選會套用至指定的動作方法或控制器。 覆寫篩選器指定一組不應該執行給定的範圍 （動作或控制器） 的篩選類型。 這可讓您設定適用於全球，但再排除特定的全域篩選器套用至特定動作或控制器的篩選條件。

### <a name="attribute-routing"></a>屬性路由

ASP.NET MVC 現在支援屬性路由中，由 Tim McCall，著作貢獻感謝[ http://attributerouting.net ](http://attributerouting.net)。 使用屬性路由中，您可以指定您的路由動作和控制器加上附註。

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET Web API 2

### <a name="attribute-routing"></a>屬性路由

ASP.NET Web API 現在支援屬性路由中，由 Tim McCall，著作貢獻感謝[ http://attributerouting.net ](http://attributerouting.net)。 使用屬性路由中，您可以指定您的 Web API 路由的註解動作和控制器，就像這樣：

[!code-csharp[Main](release-notes/samples/sample1.cs)]

屬性路由可讓您更充分掌控 Uri 在 web API 中。 例如，您可以輕鬆地定義資源階層，使用單一的 API 控制器：

[!code-csharp[Main](release-notes/samples/sample2.cs)]

屬性路由也提供方便的語法來指定選擇性參數，預設值，以及路由條件約束：

[!code-csharp[Main](release-notes/samples/sample3.cs)]

如需有關屬性路由的詳細資訊，請參閱 < [Web API 2 中的屬性路由](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md)。

### <a name="oauth-20"></a>OAuth 2.0

Web API 和單一頁面應用程式專案範本現在支援使用 OAuth 2.0 授權。 OAuth 2.0 是用來授權用戶端存取受保護資源的架構。 它適用於各種不同的用戶端，包括瀏覽器和行動裝置。

支援 OAuth 2.0 是以新的安全性中介軟體由承載驗證的 Microsoft OWIN 元件和實作授權伺服器角色為基礎。 或者，可以使用組織的授權伺服器，例如 Azure Active Directory 或 Windows Server 2012 R2 中的 ADFS 授權用戶端。

### <a name="odata-improvements"></a>OData 的改進

**展開 $select，$ 的支援，$batch 和 $value**

ASP.NET Web API OData 現在提供完整支援，如 $select、 $expand、 和 $value。 您也可以使用 $batch 批次和處理變更集的要求。

$Select 及 $expand 選項可讓您變更資料時所傳回的 OData 端點的形式。 如需詳細資訊，請參閱 <<c0> [ 簡介 $select 和 $展開 Web API OData 中的支援](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md)。

**改善擴充性**

OData 格式器現在是可擴充的。 您可以新增 Atom 項目中繼資料、 支援具名資料流和媒體連結項目、 新增執行個體註釋，並自訂產生連結的方式。

**無類型的支援**

您現在可以建置 OData 服務，而不需要定義您的實體類型的 CLR 型別。 相反地，OData 控制器可以接受或傳回的執行個體**IEdmObject**、 哪些是 OData 格式器序列化/還原序列化。

**重複使用現有的模型**

如果您已經有現有的實體資料模型 (EDM) 中，您可以立即重複使用它直接，而不需要建置一個新。 例如，如果您使用 Entity Framework，您可以使用 EF 為您建立的 EDM。

### <a name="request-batching"></a>要求批次處理

批次要求結合成單一 HTTP POST 要求，以減少網路流量，並提供更平滑、 較多對話的使用者介面的多個作業。 ASP.NET Web API 現在支援數種策略的批次要求：

- 使用 OData 服務的 $batch 端點。
- 封裝成單一的 MIME 多組件要求的多個要求。
- 使用自訂的批次格式。

若要啟用批次處理的要求，只要新增至您的 Web API 組態的批次的處理常式的路由：

[!code-csharp[Main](release-notes/samples/sample4.cs)]

您也可以控制是否要求或循序或依任何順序執行。

### <a name="portable-aspnet-web-api-client"></a>可攜式 ASP.NET Web API 用戶端

您現在可以跨 Windows 市集和 Windows Phone 8 應用程式建立可攜式類別程式庫，可使用 ASP.NET Web API 用戶端。 您也可以建立可以跨用戶端與伺服器共用的可攜式格式器。

### <a name="improved-testability"></a>改善的可測試性

Web API 2 讓您更輕鬆地單元測試您的 API 控制器。 只會初始化您的 API 控制器，與您的要求訊息設定，並接著呼叫您想要測試的動作方法。 它也很容易就能模擬**UrlHelper**類別，適用於執行連結產生的動作方法。

### <a name="ihttpactionresult"></a>IHttpActionResult

您現在可以實作 IHttpActionResult 來封裝您的 Web API 動作方法的結果。 從 Web API 動作方法傳回 IHttpActionResult 執行 ASP.NET Web API 執行階段，以產生結果的回應訊息。 IHttpActionResult 可以傳回從任何 Web API 動作，若要簡化單元測試您的 Web API 實作。 為許多 IHttpActionResult 實作會提供現成包括結果以便傳回特定狀態碼的方便起見，格式化內容，或按一下 內容交涉的回應。

### <a name="httprequestcontext"></a>HttpRequestContext

新**HttpRequestContext**追蹤會繫結至要求，但不是立即可用，請從要求的任何狀態。 例如，您可以使用**HttpRequestContext**若要取得路由資料主體要求時，用戶端憑證，相關聯**UrlHelper**和虛擬路徑根。 您可以輕鬆地建立**HttpRequestContext**進行單元測試之用。

因為要求的主體流動與要求，而不是依賴**Thread.CurrentPrincipal**，主體已提供要求的存留期，在 Web API 管線時。

### <a name="cors"></a>CORS

由於 Brock Allen 從另一個很大的貢獻，ASP.NET 現在完全支援跨原始要求共用 (CORS)。

瀏覽器安全性可防止網頁對另一個網域提出 AJAX 要求。 [CORS](http://www.w3.org/TR/cors/)是 W3C 標準，可讓伺服器放寬同源原則。 使用 CORS，伺服器可以明確允許某些跨源要求並拒絕其他。

Web API 2 現在支援 CORS，包括自動處理預檢要求。 如需詳細資訊，請參閱 < [ASP.NET Web API 中啟用跨源要求](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md)。

### <a name="authentication-filters"></a>驗證篩選條件

驗證篩選條件是一種新的 ASP.NET Web API 管線中的授權篩選條件之前執行，並可讓您指定驗證邏輯每個動作，ASP.NET Web API 中的篩選每個控制器，或全域的所有控制站。 驗證篩選條件會處理在要求中的認證，並提供對應的主體。 驗證篩選條件也可以加入驗證挑戰回應未經授權的要求。

### <a name="filter-overrides"></a>篩選覆寫

此外，您現在可以覆寫的篩選會套用至指定的動作方法或控制器，藉由指定覆寫篩選條件。 覆寫篩選器指定一組不應該執行給定的範圍 （動作或控制器） 的篩選類型。 這可讓您新增全域篩選條件，但再排除某些特定動作或控制器。

### <a name="owin-integration"></a>OWIN 整合

ASP.NET Web API 現在完全支援 OWIN，可以在任何 OWIN 功能的主機上執行。 此外也包含**HostAuthenticationFilter** ，提供與 OWIN 驗證系統整合。

使用 OWIN 整合，您可以在自己的程序，以及其他 OWIN 中介軟體，例如 SignalR 自我裝載 Web API。 如需詳細資訊，請參閱 <<c0> [ 使用 OWIN 自我裝載 ASP.NET Web API](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)。

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET SignalR 2.0

下列各節說明 SignalR 2.0 的功能。

- [在 OWIN 上建置](#builtonowin)
- [MapHubs 和 MapConnection 現在是 MapSignalR](#MapSignalR)
- [跨網域支援](#crossdomain)
- [iOS 和 Android 支援透過 MonoTouch 和 MonoDroid](#mobile)
- [可攜式.NET 用戶端](#portable)
- [新的自我裝載套件](#selfhost)
- [回溯相容性伺服器支援](#backwardcompat)
- [移除.NET 4.0 的伺服器支援](#remove40)
- [將訊息傳送至用戶端和群組的清單](#messagelist)
- [傳送訊息給特定使用者](#sendtouser)
- [更好的錯誤處理支援](#errorhandling)
- [更輕鬆的單元測試的中樞](#unittesting)
- [JavaScript 錯誤處理](#javascripterror)

如需如何將現有的 1.x 專案升級至 SignalR 2.0 的範例，請參閱 <<c0> [ 升級 SignalR 1.x 專案](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md)。

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>在 OWIN 上建置

SignalR 2.0 內建在完全[OWIN (Open Web Interface for.NET)](http://owin.org/)。 這項變更讓 signalr 的安裝程序更為一致 web 主控和自我裝載的 SignalR 應用程式，但也需要一些 API 變更。

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs 和 MapConnection 現在是 MapSignalR

OWIN 標準的相容性，這些方法已重新命名`MapSignalR`。 `MapSignalR` 呼叫沒有參數會對應所有中樞 (作為`MapHubs`在版本 1.x); 若要將都對應個別**PersistentConnection**物件，指定連線類型為型別參數，以及做為連接的 URL 擴充功能第一個引數。

`MapSignalR` Owin 啟動類別中呼叫方法。 Visual Studio 2013 包含 Owin 啟動類別; 新的範本若要使用此範本，執行下列作業：

1. 以滑鼠右鍵按一下專案
2. 選取 **新增**，**新項目...**
3. 選取  **Owin 啟動類別**。 新類別命名**Startup.cs**。

在  **Web 應用程式，** Owin 啟動類別包含`MapSignalR`方法接著會新增至 Owin 的啟動程序使用中的 Web.Config 檔案中的 應用程式設定 節點的項目，如下所示。

在 **自我裝載應用程式**，啟動類別型別參數所傳遞`WebApp.Start`方法。

**對應中樞及 signalr 的連線 （從 web 應用程式中的全域應用程式檔案） 的 1.x:** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**中樞與 SignalR 2.0 （從 Owin 啟動類別檔案） 中的連線對應：** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

在 **自我裝載應用程式**，啟動類別的型別參數當做傳遞`WebApp.Start`方法，如下所示。

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>跨網域支援

Signalr 1.x 中的，跨網域要求是由單一 EnableCrossDomain 旗標控制。 這個旗標控制 JSONP 及 CORS 要求。 所有的 CORS 支援較大的彈性，已經移除了 SignalR 的伺服器元件 （JavaScript lients 仍然使用 CORS 通常如果它偵測到瀏覽器支援它），以及新的 OWIN 中介軟體已可支援這些案例。

在 SignalR 2.0 中，如果 JSONP，才能在用戶端上 （支援跨網域要求在較舊的瀏覽器中），您必須明確啟用藉由設定`EnableJSONP`上`HubConfiguration`物件到`true`，如下所示。 JSONP 是預設為停用，其會比 CORS 較不安全。

若要加入新的 CORS 中介軟體 SignalR 2.0 中，加入`Microsoft.Owin.Cors`程式庫，以您的專案，並呼叫`UseCors`您 SignalR 中介軟體之前下, 一節中所示。

**您的專案中加入 Microsoft.Owin.Cors**： 若要安裝此程式庫，請在套件管理員主控台中執行下列命令：

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

此命令會將 2.0.0 新增至您的專案封裝的版本。

**呼叫 UseCors**

下列程式碼片段示範如何實作跨網域連接 signalr 1.x 和 2.0 版。

**實作跨網域要求 signalr 1.x （從全域應用程式檔案）**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**實作 SignalR 2.0 （從 C# 程式碼檔案） 中的跨網域要求**

下列程式碼示範如何啟用 CORS 或 JSONP SignalR 2.0 專案中。 此程式碼範例會使用`Map`並`RunSignalR`而不是`MapSignalR`，如此一來，只會針對需要 CORS 支援 SignalR 要求的 CORS 中介軟體會執行 (而不是針對在指定的路徑上的所有流量`MapSignalR`。)`Map`也可以用於任何其他中介軟體需要執行特定的 URL 前置詞，而不是整個應用程式。

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>iOS 和 Android 支援透過 MonoTouch 和 MonoDroid

已新增支援，適用於 iOS 和 Android 用戶端使用 MonoTouch 和 MonoDroid 元件[Xamarin 程式庫](https://xamarin.com/)。 如需有關如何使用它們的詳細資訊，請參閱 <<c0> [ 使用 Xamarin 元件](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln)。 這些元件將可在[Xamarin 市集](https://store.xamarin.com/)SignalR RTW 版本可用時。

<a id="portable"></a> # # # 可攜式.NET 用戶端

為了進一步簡化跨平台開發、 Silverlight、 WinRT 和 Windows Phone 用戶端已取代為單一可攜式.NET 用戶端支援下列平台：

- NET 4.5
- Silverlight 5
- WinRT (Windows 市集應用程式的.NET)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>新的自我裝載套件

目前沒有 NuGet 封裝來輕鬆地開始使用 SignalR 自我裝載 （裝載處理序中的 SignalR 應用程式或其他應用程式，而非裝載於 web 伺服器）。 若要升級自助代管所建置的專案使用 SignalR 1.x 中，移除 Microsoft.AspNet.SignalR.Owin 封裝，並新增 Microsoft.AspNet.SignalR.SelfHost 套件。 如需有關如何開始使用的自我裝載套件的詳細資訊，請參閱[教學課程： SignalR 自我裝載](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)。

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>回溯相容性伺服器支援

在舊版的 SignalR、 SignalR 封裝用於用戶端和伺服器必須是相同的版本。 若要支援會很難更新的大型用戶端應用程式，SignalR 2.0 現在支援使用較新的伺服器版本與舊版本的用戶端。 **注意： SignalR 2.0 不支援使用新的用戶端建置與較舊版本的伺服器。**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>移除.NET 4.0 的伺服器支援

SignalR 2.0 已經卸除之伺服器的互通性，.NET 4.0 的支援。 與 SignalR 2.0 伺服器，必須使用.NET 4.5。 仍有 SignalR 2.0 的.NET 4.0 用戶端。

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>將訊息傳送至用戶端和群組的清單

SignalR 2.0，就可以傳送訊息使用的用戶端與群組識別碼清單。 下列程式碼片段示範如何執行這項操作。

**將訊息傳送到用戶端和使用 PersistentConnection 的群組清單**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**將訊息傳送至用戶端和使用中樞群組的清單**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>傳送訊息給特定使用者

這項功能可讓使用者指定的使用者識別碼的是根據透過新的介面 IUserIdProvider IRequest:

**IUserIdProvider 介面**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

根據預設會使用使用者的 IPrincipal.Identity.Name 作為使用者名稱的實作。

在中樞，您將能夠將訊息傳送給這些使用者透過新的 API:

**使用 Clients.User API**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>更好的錯誤處理支援

使用者現在可能會擲回**HubException**從任何中樞叫用。 建構函式**HubException**字串訊息和物件可能需要額外的錯誤資料。 SignalR 將自動序列化例外狀況，並將它傳送至用戶端它用來拒絕/失敗中樞方法叫用。

**顯示詳細的中樞例外狀況**設定沒有任何關係**HubException**傳送回用戶端或不會，它一律會傳送。

**展示 HubException 傳送給用戶端的伺服器端程式碼**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**展示 HubException，從伺服器傳送回應的 JavaScript 用戶端程式碼**

[!code-html[Main](release-notes/samples/sample16.html)]

**展示 HubException，從伺服器傳送回應的.NET 用戶端程式碼**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>更輕鬆的單元測試的中樞

SignalR 2.0 包含名為介面`IHubCallerConnectionContext`上更輕鬆地建立模擬 （mock） 的用戶端端引動過程的中樞。 下列程式碼片段會示範如何使用此介面與熱門的測試控管[xUnit.net](https://github.com/xunit/xunit)並[moq](https://code.google.com/p/moq/)。

**使用單元測試 SignalR xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**使用單元測試 SignalR moq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>JavaScript 錯誤處理

SignalR 2.0，在所有的 JavaScript 錯誤處理回呼會傳回 JavaScript 錯誤物件，而非原始的字串。 這可讓流程更豐富的資訊，以您的錯誤處理常式的 SignalR。 您可以取得內部的例外狀況，從`source`錯誤的屬性。

**處理 Start.Fail 例外狀況的 JavaScript 用戶端程式碼**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>新的 ASP.NET 成員資格系統

ASP.NET 身分識別是 ASP.NET 應用程式的新成員資格系統。 ASP.NET 身分識別可讓您更輕鬆地整合應用程式資料的特定使用者設定檔資料。 ASP.NET 身分識別也可讓您選擇在您的應用程式中的使用者設定檔的持續性模型。 您可以將資料儲存在 SQL Server 資料庫或其他資料存放區，包括 NoSQL 資料存放區，例如 Azure 儲存體資料表。 如需詳細資訊，請參閱 <<c0> [ 個別使用者帳戶](creating-web-projects-in-visual-studio.md#indauth)中**在 Visual Studio 2013 中建立 ASP.NET Web 專案**。

### <a name="claims-based-authentication"></a>宣告架構驗證

ASP.NET 現在支援宣告式的驗證，而使用者的身分識別都代表一組來自受信任的簽發者的宣告。 使用者可以是已驗證的使用者名稱和密碼保存在應用程式資料庫，或使用社交識別提供者 (例如： Microsoft 帳戶、 Facebook、 Google、 Twitter)，或使用透過 Azure Active Directory 的組織帳戶或Active Directory Federation Services (ADFS)。

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>與 Azure Active Directory 和 Windows Server Active Directory 整合

您現在可以建立使用 Azure Active Directory 或 Windows Server Active Directory (AD) 進行驗證的 ASP.NET 專案。 如需詳細資訊，請參閱 <<c0> [ 組織帳戶](creating-web-projects-in-visual-studio.md#orgauth)中**在 Visual Studio 2013 中建立 ASP.NET Web 專案**。

### <a name="owin-integration"></a>OWIN 整合

ASP.NET 驗證現在根據可用在以 OWIN 為基礎的任何主機的 OWIN 中介軟體。 如需 OWIN 的詳細資訊，請參閱下列[Microsoft OWIN 元件](#TOC7)一節。

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Microsoft OWIN 元件

[Open Web Interface for.NET](http://owin.org/) (OWIN) 定義.NET web 伺服器和 web 應用程式之間的抽象概念。 OWIN 減少 web 應用程式，從伺服器進行 web 應用程式主控件無關。 例如，您也可以裝載在 IIS 中以 OWIN 為基礎的 web 應用程式，或自我裝載的自訂處理序中。

Microsoft OWIN 元件 （也稱為 Katana 專案） 中導入的變更包括新的伺服器和主機元件、 新的協助程式庫中介軟體，與新的驗證中介軟體。

如需有關 OWIN 和 Katana 的詳細資訊，請參閱[OWIN 和 Katana 中最新消息](../../../aspnet/overview/owin-and-katana/index.md)。

**注意︰ [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)應用程式無法執行 IIS 的傳統模式中，它們必須以整合模式執行。**

**注意︰ [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)必須以完全信任執行的應用程式。**

### <a name="new-servers-and-hosts"></a>新的伺服器和主機

此版本中，新元件加入啟用自我裝載案例。 這些元件包括下列 NuGet 封裝：

- **Microsoft.Owin.Host.HttpListener**。 提供 OWIN 伺服器，會使用**HttpListener**接聽 HTTP 要求，並將他們導向至 OWIN 管線。
- **Microsoft.Owin.Hosting**提供程式庫的開發人員想要在自我裝載的 OWIN 管線，以自訂的程序，例如主控台應用程式或 Windows 服務。
- **OwinHost**。 提供獨立的可執行檔包裝`Microsoft.Owin.Hosting`，並讓您在自我裝載的 OWIN 管線，而不需要撰寫自訂主應用程式。

颾魤 ㄛ`Microsoft.Owin.Host.SystemWeb`套件現在會啟用中介軟體來提供提示**SystemWeb**指出中介軟體應該被呼叫特定的 ASP.NET 管線階段的伺服器。 這項功能是特別適用於驗證中介軟體，應該及早在 ASP.NET 管線中執行。

### <a name="helper-libraries-and-middleware"></a>協助程式程式庫和中介軟體

雖然您可以撰寫 OWIN 元件使用只有函式和型別定義 OWIN 規格中，從新`Microsoft.Owin`套件提供使用者更容易使用的一組抽象概念。 此套件會結合數個舊版的封裝 (例如`Owin.Extensions`， `Owin.Types`) 成單一結構良好的物件模型，然後輕鬆地可由其他 OWIN 元件。 事實上，大部分的 Microsoft OWIN 元件現在會使用此套件。

> [!NOTE]
> [OWIN](http://www.owin.org)應用程式無法執行 IIS 的傳統模式中，它們必須以整合模式執行。

> [!NOTE]
> [OWIN](http://www.owin.org)必須以完全信任執行的應用程式。

此版本也包含 Microsoft.Owin.Diagnostics 封裝，其中包含中介軟體驗證執行的 OWIN 應用程式，再加上以協助調查失敗的錯誤頁面中介軟體。

### <a name="authentication-components"></a>驗證元件

使用下列的驗證元件。

- **Microsoft.Owin.Security.ActiveDirectory**. 啟用使用內部部署或雲端式目錄服務的驗證。
- **Microsoft.Owin.Security.Cookies**使用 cookie 啟用驗證。 此封裝先前命名為`Microsoft.Owin.Security.Forms`。
- **Microsoft.Owin.Security.Facebook**啟用驗證使用 Facebook 的 OAuth 型服務。
- **Microsoft.Owin.Security.Google**使用 Google OpenID 型服務的啟用驗證。
- **Microsoft.Owin.Security.Jwt**啟用驗證使用 JWT 權杖。
- **Microsoft.Owin.Security.MicrosoftAccount**使用 Microsoft 帳戶啟用驗證。
- **Microsoft.Owin.Security.OAuth**。 提供 OAuth 授權伺服器，以及中介軟體驗證持有人權杖。
- **Microsoft.Owin.Security.Twitter**啟用驗證使用 Twitter 的 OAuth 型服務。

此版本也包含`Microsoft.Owin.Cors`封裝，其中包含用於處理跨原始來源 HTTP 要求中的介軟體。

> [!NOTE]
> Visual Studio 2013 的最終版本中已移除支援 JWT 簽章。

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

如需新功能和 Entity Framework 6 中的其他變更的清單，請參閱 < [Entity Framework 版本歷程記錄](https://msdn.com/data/jj574253)。

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

ASP.NET Razor 3 包含下列新功能：

- 支援 索引標籤上編輯。 先前**格式化文件**命令，自動縮排，並自動在 Visual Studio 中格式化未正確運作時使用**保留定位點**選項。 這項變更會修正的格式設定 索引標籤上的 Razor 程式碼格式設定的 Visual Studio。
- 支援 URL Rewrite 規則時產生連結。
- 安全性透明屬性移除。
  > [!NOTE]
  > 這是重大變更，並使 Razor 3 不相容使用 MVC4 和舊版中，Razor 2 時與 MVC5 或針對 MVC5 編譯的組件不相容。

修正從發行前版本的 Visual Studio 2013 中的 razor 3 問題找[此處](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0)。

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>ASP.NET 應用程式暫止

ASP.NET 應用程式暫止是.NET Framework 4.5.1，徹底改變使用者體驗和裝載在單一機器上的 ASP.NET 網站的數目很大的經濟模型中改變遊戲規則的功能。 如需詳細資訊，請參閱 < [.NET web 裝載的 ASP.NET 應用程式暫止 – 回應共用](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx)。

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的問題和重大變更

本章節會描述已知的問題和重大變更 ASP.NET 及 Web Tools for Visual Studio 2013。

### <a name="nuget"></a>NuGet

- [使用 SLN 檔案時，將無法運作在 Mono 上的新的套件還原](https://nuget.codeplex.com/workitem/3596)-即將推出的 nuget.exe 下載中將會修正並[NuGet.CommandLine 封裝](http://www.nuget.org/packages/NuGet.CommandLine/)更新。
- [新的套件還原不適用於 Wix 專案](https://nuget.codeplex.com/workitem/3598)-即將推出的 nuget.exe 下載中將會修正並[NuGet.CommandLine 封裝](http://www.nuget.org/packages/NuGet.CommandLine/)更新。
- [自動套件還原不適用於專案的方案資料夾底下](https://nuget.codeplex.com/workitem/3625)– NuGet 2.8 會修正問題。

### <a name="aspnet-web-api"></a>ASP.NET Web API

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` 不會傳回`IQueryable<T>`往常，我們已新增支援，如`$select`和`$expand`。

    我們先前的範例，如`ODataQueryOptions<T>`一律轉型的傳回值從`ApplyTo`至`IQueryable<T>`。 此查詢選項，因為先前正常運作我們早支援 (`$filter`， `$orderby`， `$skip`， `$top`) 不會變更查詢的圖形。 現在，我們支援`$select`並`$expand`的傳回值`ApplyTo`不會`IQueryable<T>`一律。

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    如果您使用的稍早的範例程式碼，它就會繼續運作，如果用戶端不會傳送`$select`和`$expand`。 不過，如果您想要支援`$select`和`$expand`您必須將該程式碼變更如下。

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url RequestContext.Url 設為 null 或批次要求期間**

    在批次的案例中， **UrlHelper**為 null 時從存取**Request.Url**或是**RequestContext.Url**。

    此問題目前這裡追蹤： [BatchRequestContext.Url 為 null 的批次要求](http://aspnetwebstack.codeplex.com/workitem/1301)。

    此問題的因應措施為建立的新執行個體**UrlHelper**，如下列範例所示：

    **建立 UrlHelper 的新執行個體**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. 使用時 MVC5 以及 OrgAuth，如果您有檢視 AntiForgerToken 驗證來完成此動作，您可能會遇到下列錯誤張貼至檢視的資料時：

    **錯誤**:

    *'/' 應用程式中的伺服器錯誤。*

    <em>宣告類型 '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>'或'<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' 已不存在於上提供的 ClaimsIdentity。若要啟用防偽權杖支援使用宣告式驗證，請確認已設定的宣告提供者提供這兩個 ClaimsIdentity 執行個體，它會產生這些宣告。如果已設定的宣告提供者改為使用不同的宣告類型唯一識別碼，它可以藉由設定靜態屬性 AntiForgeryConfig.UniqueClaimTypeIdentifier 設定。</em>

    **因應措施**：

    若要修正此問題的 Global.asax 中加入下面這一行：

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    這將會在下一版的修正。
2. 升級之後的 MVC4 應用程式到 MVC5，建置方案並啟動它。 您應該會看到下列錯誤：

    [A]System.Web.WebPages.Razor.Configuration.HostSection 無法轉換成 [B]System.Web.WebPages.Razor.Configuration.HostSection。 類型 A 源自 'System.Web.WebPages.Razor，version=2.0.0.0，Culture = neutral，PublicKeyToken = 31bf3856ad364e35' 中的位置之' Default' 內容 ' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'。 類型 B 源自 'System.Web.WebPages.Razor，version=3.0.0.0，Culture = neutral，PublicKeyToken = 31bf3856ad364e35' 中的位置之' Default' 內容 ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'。

    若要修正上述錯誤，請開啟*所有*中的 Web.config 檔案 （包括 [Views] 資料夾中的項目） 您的專案並執行下列：

   1. 更新版本 」 4.0.0.0 」"System.Web.Mvc 」 到 「 version=5.0.0.0"的所有項目。
   2. 更新 「 System.Web.Helpers"，"2.0.0.0"版的所有項目&quot;System.Web.WebPages&quot;並&quot;System.Web.WebPages.Razor&quot;來 「 3.0.0.0"

      例如，您進行上述變更之後，組件繫結看起來應該像這樣：

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      將 MVC 4 專案升級至 MVC 5 的資訊，請參閱[如何將 ASP.NET MVC 4 和 Web API 專案升級至 ASP.NET MVC 5 和 Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)。
3. 時與 jQuery 低調驗證使用用戶端驗證，驗證訊息有時會不正確的 HTML input 元素，類型 = 'number'。 驗證錯誤是必要值 （「 時間欄位必要 」) 會顯示無效的數字而不是正確的訊息有效的數字是必要的輸入時。

    此問題經常發現使用 scaffold 程式碼的整數屬性，在 建立 和 編輯檢視模型。

    若要解決此問題，請變更從編輯器協助程式：

    `@Html.EditorFor(person => person.Age)`

    收件者:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 不再支援部分信任。 連結至 MVC 或 WebAPI 的二進位檔的專案應該移除[SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx)屬性並[AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx)屬性。 移除這些屬性會消除編譯器錯誤，如下所示。

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > 請注意，因為副作用的您無法在相同的應用程式中使用 4.0 和 5.0 的組件。 您需要將所有人都更新至 5.0。

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>使用 Facebook 授權的 SPA 範本可能會造成不穩定在 IE 中雖然網站裝載在內部網路區域

SPA 範本提供使用 Facebook 的外部記錄檔。 當使用範本建立的專案在本機執行時，登入可能會導致損毀的 IE。

解決方案:

1. 將網站裝載在網際網路區域中。或

2. 在非 IE 的瀏覽器中測試案例。

### <a name="web-forms-scaffolding"></a>Web Forms Scaffolding

Web Forms Scaffolding 已經移除了 VS2013，並將可在 Visual Studio 的未來更新中。 不過，您仍然可以使用 scaffolding Web Form 專案中的新增 MVC 相依性，並產生 MVC scaffolding。 您的專案會包含 Web Form 和 MVC 的組合。

若要新增至您的 Web Form 專案的 MVC，加入新的 scaffold 項目，然後選取**MVC 5 相依性**。 選取最少或完全取決於您是否需要的所有內容檔案，例如指令碼。 然後，mvc，會在您的專案中建立檢視和控制器加入 scaffold 項目。

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC 和 Web API Scaffolding-HTTP 404 找不到錯誤

如果專案中加入 scaffold 項目時，會遇到錯誤，，很可能您的專案將會處於不一致的狀態。 部分 scaffolding 所做的變更將會回復，但其他的變更，例如已安裝的 NuGet 套件，將不會回復。 如果路由的組態變更會回復，使用者會收到 HTTP 404 錯誤，當瀏覽至包含 scaffold 的項目。

因應措施：

- 若要修正這個錯誤，mvc，加入新的 scaffold 項目，然後選取 MVC 5 相依性 （基本或完整）。 此程序會加入所有必要的變更至您的專案。
- 若要修正此錯誤的 Web API:

  1. 將 WebApiConfig 類別新增至您的專案。

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. 在 應用程式中設定 WebApiConfig.Register\_在 Global.asax 中啟動方法時，也將，如下所示：

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
