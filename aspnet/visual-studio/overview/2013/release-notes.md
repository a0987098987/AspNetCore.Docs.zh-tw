---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET 及 Web Tools for Visual Studio 2013 版本資訊 |Microsoft 文件
author: microsoft
description: 本文件說明 Visual Studio 2013 ASP.NET 及 Web 工具版本。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: e9ddd96f186564834ff6bb2c30cf0ed5444cbf1b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877797"
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET 及 Web Tools for Visual Studio 2013 版本資訊
====================
by [Microsoft](https://github.com/microsoft)

> 本文件說明 Visual Studio 2013 ASP.NET 及 Web 工具版本。


## <a name="contents"></a>內容

- [安裝注意事項](#TOC1)
- [文件 (英文)](#TOC2)
- [軟體需求](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>在 ASP.NET 和 Web Tools for Visual Studio 2013 中的新功能

- [一個 ASP.NET](#TOC6)
- [新的 Web 專案經驗](#newproj)
- [ASP.NET Scaffolding](#scaffold)
- [瀏覽器連結](#browser-link)
- [Visual Studio Web 編輯器增強功能](#web-editor)
- [在 Visual Studio 中的 azure App Service Web 應用程式支援](#waws)
- [Web 發行的增強功能](#publish)
- [NuGet 2.7](#nuget)
- [ASP.NET Web Form](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.NET Web API 2](#TOC11)
- [ASP.NET SignalR](#TOC13)
- [ASP.NET Identity](#TOC8)
- [Microsoft OWIN 元件](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [ASP.NET 應用程式暫停](#TOC15)
- [已知的問題和重大變更](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>安裝注意事項

ASP.NET 及 Web Tools for Visual Studio 2013 會集結在主要安裝程式，而且可以下載[這裡](https://www.asp.net/downloads)。

<a id="TOC2"></a>
## <a name="documentation"></a>文件

教學課程和其他資訊 ASP.NET 及 Web Tools for Visual Studio 2013 都是從[ASP.NET 網站](https://www.asp.net/)。

<a id="TOC4"></a>
## <a name="software-requirements"></a>軟體需求

ASP.NET 及 Web 工具需要 Visual Studio 2013。

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>在 ASP.NET 和 Web Tools for Visual Studio 2013 中的新功能

下列章節說明在版本中已引進的功能。

<a id="TOC6"></a>
## <a name="one-aspnet"></a>一個 ASP.NET

使用 Visual Studio 2013 版本，我們已了進一步的整合，讓您輕鬆混合和比對您想的要使用 ASP.NET 技術的體驗。 比方說，您可以開始使用 MVC 專案並輕鬆地更新版本中，加入至專案的 Web Form 網頁或 scaffold Web Form 專案中的 Web 應用程式開發介面。 一個 ASP.NET 就是要讓更容易為您身為開發人員要您喜歡在 ASP.NET 中的項目。 無論您選擇何種技術，您可以讓您一個 ASP.NET 的信任基礎架構建置的信心。

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>新的 Web 專案經驗

我們有增強 Visual Studio 2013 中建立新的 web 專案的經驗。 在**新的 ASP.NET Web 專案**對話方塊，您可以選取專案類型，設定技術 (Web Forms、 MVC、 Web API) 的任何組合，設定驗證選項，然後加入單元測試專案。

![新增 ASP.NET 專案](release-notes/_static/image1.png)

[新增] 對話方塊可讓您變更許多範本的預設驗證選項。 例如，當您建立的 ASP.NET Web Form 專案時您可以選取任何下列選項：

- 無驗證
- 個別使用者帳戶 （ASP.NET 成員資格或社交提供者登入）
- 組織帳戶 (網際網路應用程式中的 Active Directory)
- Windows 驗證 (內部網路應用程式中的 Active Directory)

![驗證選項](release-notes/_static/image2.png)

如需建立 web 專案的新程序的詳細資訊，請參閱[Visual Studio 2013 中建立 ASP.NET Web 專案](creating-web-projects-in-visual-studio.md)。 如需新的驗證選項的詳細資訊，請參閱[ASP.NET Identity](#TOC8)本文件後面。

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

ASP.NET Scaffolding 是 ASP.NET Web 應用程式的程式碼產生架構。 它可讓您輕鬆地將未定案程式碼加入至您的專案與資料模型互動。

在舊版的 Visual Studio 中，scaffolding 限於 ASP.NET MVC 專案。 使用 Visual Studio 2013，您現在可以針對任何 ASP.NET 專案，包括 Web Form 使用 scaffolding。 Visual Studio 2013 目前不支援產生頁面的 Web Form 專案，但您仍然可以使用與 Web Form scaffolding，將 MVC 相依性加入至專案。 在未來的更新中，將會加入產生的 Web Form 網頁的支援。

當使用 scaffolding，我們可以確定所有必要的相依性安裝到專案。 比方說，如果您從 ASP.NET Web Form 專案開始，然後使用 scaffolding 新增 Web API 控制器所需的 NuGet 封裝和參考會加入至您的專案會自動。

MVC 樣板加入至 Web Form 專案，加入**新的 Scaffold 項目**選取**MVC 5 相依性**對話視窗中。 有兩個選項的 scaffolding MVC;最少且完整。 如果您選取最少，只有 NuGet 封裝和 ASP.NET MVC 的參考會加入至您的專案。 如果您選取 [完整] 選項，加入最少的相依性，以及所需的內容檔案的 MVC 專案。

支援的 scaffolding 非同步控制器會使用新的非同步功能，從 Entity Framework 6。

如需詳細資訊和教學課程，請參閱[ASP.NET Scaffolding 概觀](aspnet-scaffolding-overview.md)。

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>瀏覽器連結-瀏覽器和 Visual Studio 之間的 SignalR 通道

新[瀏覽器連結](using-browser-link.md)功能可讓您連接至 Visual Studio 的多個瀏覽器，並加以重新整理所有依序按一下工具列中的按鈕。 您可以連接到您的開發網站，包括行動模擬器的多個瀏覽器，並按一下 重新整理以重新整理所有瀏覽器，所有在相同的時間。 瀏覽器連結也會公開可讓開發人員來寫入瀏覽器連結延伸的 API。

![](release-notes/_static/image3.png)

讓開發人員充分利用瀏覽器連結 API，它會變成可以建立 Visual Studio 和任何連接的瀏覽器之間跨越界限的非常進階的案例。 Web Essentials 利用 API 來建立 Visual Studio 和瀏覽器的開發人員工具，遠端控制行動模擬器以及其他的之間的整合式的經驗。

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Visual Studio Web 編輯器增強功能

Visual Studio 2013 web 應用程式中包含新的 HTML 編輯器，Razor 檔案和 HTML 檔。 新的 HTML 編輯器提供 HTML5 為基礎的單一統一結構描述。 它有自動大括號完成、 jQuery UI 和 AngularJS 屬性 IntelliSense，屬性 IntelliSense 群組、 ID 和類別名稱 Intellisense，以及其他改進還包括提升效能、 格式和智慧標籤。

下列螢幕擷取畫面將示範如何使用啟動程序屬性 IntelliSense HTML 編輯器中。

![HTML 編輯器中的 Intellisense](release-notes/_static/image4.png)

Visual Studio 2013 也提供使用這兩個 CoffeeScript 和較低的內建的編輯器。 LESS 編輯器所有酷炫的功能來自 CSS 編輯器，且所有較少文件中都有特定的 Intellisense 的變數和 mixins@import鏈結。

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>在 Visual Studio 中的 azure App Service Web 應用程式支援

在與 Azure SDK for.NET 2.2 的 Visual Studio 2013，您可以使用**伺服器總管**直接與遠端的 web 應用程式互動。 您可以登入您的 Azure 帳戶、 建立新的 web 應用程式，設定應用程式、 檢視即時記錄，以及更多。 推出之後 SDK 2.2 即發行，您可以在 Azure 中的遠端偵錯模式執行。 當您安裝最新版的 Azure sdk for.NET 時，大部分的 Azure App Service Web 應用程式的新功能也在 Visual Studio 2012 中運作。

如需詳細資訊，請參閱下列資源：

- [在 Azure App Service 中建立 ASP.NET web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [疑難排解 Azure App Service 使用 Visual Studio 中的 web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Web 發行的增強功能

Visual Studio 2013 包含新功能和增強 Web Publish 功能。 以下是少數幾個：

- 輕鬆地[自動化 Web.config 檔案加密](https://go.microsoft.com/fwlink/?LinkId=325529)。 （此連結和下面兩個點上可能無法使用之前在 10/17 當天的 MSDN 文件。）
- 輕鬆地[自動化取得應用程式離線以在部署期間](https://go.microsoft.com/fwlink/?LinkId=325530)。
- 設定 Web Deploy 以[檔案總和檢查碼使用而不是上次變更日期](https://go.microsoft.com/fwlink/?LinkId=325531)來判斷哪些檔案應該複製到伺服器。
- 當您使用 FTP 或檔案系統發行方法時，以及使用 Web Deploy，快速地發佈個別選取的檔案 （包括 Web.config）。

如需 ASP.NET web 部署的詳細資訊，請參閱[ASP.NET 網站](https://go.microsoft.com/fwlink/?LinkId=322027)。

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 包含一組豐富的新功能所說明的詳細討論[NuGet 2.7 版本資訊](http://docs.nuget.org/docs/release-notes/nuget-2.7)。

這個版本的 NuGet 也不再需要明確同意下載的套件，NuGet 的封裝還原功能。 藉由安裝 NuGet 現在授與同意 （與在 NuGet 的喜好設定對話方塊相關聯的核取方塊）。 現在套件還原只運作方式是預設值。

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>ASP.NET Web Form

### <a name="one-aspnet"></a>一個 ASP.NET

Web Form 專案範本緊密整合新的一個 ASP.NET 體驗中。 您可以將 MVC 和 Web 應用程式開發介面支援至 Web Form 專案，而且您可以設定使用一個 ASP.NET 專案建立精靈的驗證。 如需詳細資訊，請參閱[Visual Studio 2013 中建立 ASP.NET Web 專案](creating-web-projects-in-visual-studio.md)。

### <a name="aspnet-identity"></a>ASP.NET Identity

Web Form 專案範本支援新的 ASP.NET Identity framework。 此外，範本現在支援建立 Web Form 內部網路專案。 如需詳細資訊，請參閱[驗證方法](creating-web-projects-in-visual-studio.md#auth)中**Visual Studio 2013 中建立 ASP.NET Web 專案**。

### <a name="bootstrap"></a>Bootstrap

Web Form 範本使用[Bootstrap](http://twitter.github.io/bootstrap/)提供精緻且回應迅速外觀及操作，您可以輕鬆地自訂。 如需詳細資訊，請參閱[啟動 Visual Studio 2013 的 web 專案範本中的程序](creating-web-projects-in-visual-studio.md#bootstrap)。

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>一個 ASP.NET

Web MVC 專案範本緊密整合新的一個 ASP.NET 體驗中。 您可以自訂 MVC 專案，並使用一個 ASP.NET 專案建立精靈設定驗證。 ASP.NET MVC 5 需入門教學課程，請參閱[開始使用 ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md)。

將 MVC 4 專案升級至 MVC 5 的資訊，請參閱[如何將 ASP.NET MVC 4 和 Web API 專案升級至 ASP.NET MVC 5 和 Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)。

### <a name="aspnet-identity"></a>ASP.NET Identity

要用於驗證和身分識別管理 ASP.NET 識別的 MVC 專案範本已更新。 將 Facebook 和 Google 驗證和新的成員資格應用程式開發介面設為特色的教學課程，請參閱[建立 ASP.NET MVC 5 應用程式與 Facebook、 Google OAuth2 和 OpenID 登入](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)和[建立 ASP.NET MVC 應用程式具有驗證和SQL 資料庫並部署至 Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)。

### <a name="bootstrap"></a>Bootstrap

MVC 專案範本已更新為使用[Bootstrap](http://getbootstrap.com/)提供精緻且回應迅速外觀及操作，您可以輕鬆地自訂。 如需詳細資訊，請參閱[啟動 Visual Studio 2013 的 web 專案範本中的程序](creating-web-projects-in-visual-studio.md#bootstrap)。

### <a name="authentication-filters"></a>驗證篩選條件

驗證篩選條件是一種新的 ASP.NET MVC 管線中的授權篩選條件之前執行，並允許您指定驗證邏輯每個動作，ASP.NET MVC 中篩選每個控制器，或全域的所有控制站。 驗證篩選條件處理要求中的認證，並提供對應的主體。 驗證篩選條件也可以將驗證挑戰回應未經授權的要求。

### <a name="filter-overrides"></a>篩選會覆寫

您現在可以覆寫的篩選會套用至指定的動作方法或控制器藉由指定覆寫篩選條件。 覆寫篩選條件會指定一組篩選器型別，且不應該執行給定的範圍 （動作或控制器）。 這可讓您設定篩選器，全域套用，但無法套用至特定動作或控制器中排除某些全域篩選器。

### <a name="attribute-routing"></a>屬性路由

ASP.NET MVC 現在支援路由屬性，這點受惠 Tim McCall，作者比重[ http://attributerouting.net ](http://attributerouting.net)。 路由屬性中，您可以指定您的路由加上附註您的動作與控制器。

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET Web API 2

### <a name="attribute-routing"></a>屬性路由

ASP.NET Web API 現在支援路由屬性，這點受惠 Tim McCall，作者比重[ http://attributerouting.net ](http://attributerouting.net)。 屬由路由中，您可以指定您的 Web API 路由加上附註您的動作和控制站，像這樣：

[!code-csharp[Main](release-notes/samples/sample1.cs)]

路由屬性讓您更充分掌控 Uri 在您的 web API。 例如，您可以輕鬆地定義資源階層，使用單一的 API 控制器：

[!code-csharp[Main](release-notes/samples/sample2.cs)]

屬性路由也提供方便的語法指定選擇性參數、 預設值和路由條件約束：

[!code-csharp[Main](release-notes/samples/sample3.cs)]

如需路由屬性的詳細資訊，請參閱[屬性路由中 Web API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md)。

### <a name="oauth-20"></a>OAuth 2.0

Web API 和單一頁面應用程式專案範本現在支援使用 OAuth 2.0 授權。 OAuth 2.0 是一種架構用戶端存取受保護資源的授權。 這也適用於各種不同的用戶端包括瀏覽器和行動裝置。

OAuth 2.0 的支援根據新安全性中介軟體提供由 Microsoft OWIN 元件承載驗證和執行授權伺服器角色。 或者，您可以使用組織的授權伺服器，例如 Azure Active Directory 或 Windows Server 2012 R2 中的 ADFS 進行授權用戶端。

### <a name="odata-improvements"></a>OData 增強功能

**展開 支援 $select，$，$batch 和 $value**

ASP.NET Web API OData 現在會有完整支援 $select、 $expand、 和 $value。 您也可以使用 $batch 批次和變更集的處理要求。

$Select 及 $expand 選項可讓您變更 從 OData 端點會傳回資料的外觀。 如需詳細資訊，請參閱[簡介 $select 和 $expand Web API OData 中的支援](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md)。

**改善擴充性**

OData 格式器現在是可擴充的。 您可以加入 Atom 項目中繼資料、 支援具名資料流與媒體連結項目、 加入執行個體註解，並自訂連結的產生方式。

**無類型的支援**

您現在可以建置 OData 服務，而不需要定義實體類型的 CLR 型別。 相反地，OData 控制器可以接受或傳回執行個體**IEdmObject**、 哪些是序列化/還原序列化的 OData 格式器。

**重複使用現有的模型**

如果您已經有現有的實體資料模型 (EDM)，您可以立即重複使用它直接管理，而不必建立新的。 例如，如果您使用 Entity Framework，您可以使用 EF 為您建立 EDM。

### <a name="request-batching"></a>要求批次處理

批次處理要求結合成單一的 HTTP POST 要求，以減少網路流量並提供更順暢，較多對話的使用者介面的多個作業。 ASP.NET Web API 現在支援數種策略的批次要求：

- 使用 OData 服務的 $batch 端點。
- 封裝的多個要求結合為單一的 MIME 多組件的要求。
- 使用自訂的批次處理格式。

若要啟用批次處理的要求，只需新增與批次的處理常式路由到 Web API 組態：

[!code-csharp[Main](release-notes/samples/sample4.cs)]

您也可以控制是否要求或循序或依任何順序執行。

### <a name="portable-aspnet-web-api-client"></a>可攜式的 ASP.NET Web API 的用戶端

您現在可以使用 ASP.NET Web API 的用戶端建立可攜式類別庫可透過 Windows 市集和 Windows Phone 8 應用程式。 您也可以建立可攜式格式器會在用戶端和伺服器之間共用。

### <a name="improved-testability"></a>改良的測試能力

Web API 2 可讓它更容易單元測試您的 API 控制器。 只會初始化您的應用程式開發介面控制站與您的要求訊息的組態，並接著呼叫您要測試的動作方法。 所以也可以輕鬆地模擬**UrlHelper**類別，適用於執行連結產生的動作方法。

### <a name="ihttpactionresult"></a>IHttpActionResult

您現在可以實作 IHttpActionResult 來封裝您的 Web API 的動作方法的結果。 從 Web API 的動作方法傳回 IHttpActionResult 執行 ASP.NET Web API 執行階段，以產生結果的回應訊息。 可傳回 IHttpActionResult 從任何 Web 應用程式開發介面 動作，以簡化單元測試的 Web 應用程式開發介面實作。 為了方便起見 IHttpActionResult 實作 (implementation) 的數字會提供現成的包括傳回特定的狀態碼的結果，格式化內容或內容交涉的回應。

### <a name="httprequestcontext"></a>HttpRequestContext

新**HttpRequestContext**會追蹤任何繫結至要求，但沒有立即可用從要求的狀態。 例如，您可以使用**HttpRequestContext**取得路由資料的要求，用戶端憑證，與關聯的主體**UrlHelper**和虛擬路徑根。 您可以輕鬆地建立**HttpRequestContext**進行單元測試。

因為要求主體流動的要求，而不是依賴**Thread.CurrentPrincipal**，在 Web API 管線時，就可存留期中的要求主體。

### <a name="cors"></a>CORS

這點受惠 Brock Allen 從另一個絕佳的比重，ASP.NET 現在完全支援跨原始要求共用 (CORS)。

瀏覽器安全性防止網頁 AJAX 要求到另一個網域。 [CORS](http://www.w3.org/TR/cors/)是 W3C 標準，可放寬的相同來源原則的伺服器。 使用 CORS，伺服器可以明確地允許某些跨原始要求時拒絕其他人。

Web API 2 現在支援 CORS，包括自動處理的預檢要求。 如需詳細資訊，請參閱[啟用 ASP.NET Web API 中的跨原始要求](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md)。

### <a name="authentication-filters"></a>驗證篩選條件

驗證篩選條件是一種新的 ASP.NET Web API 管線中的授權篩選條件之前執行，並允許您指定驗證邏輯每個動作，ASP.NET Web API 中的篩選每個控制器，或全域的所有控制站。 驗證篩選條件處理要求中的認證，並提供對應的主體。 驗證篩選條件也可以將驗證挑戰回應未經授權的要求。

### <a name="filter-overrides"></a>篩選條件覆寫

您現在可以覆寫的篩選會套用至指定的動作方法或控制器，藉由指定覆寫篩選條件。 覆寫篩選條件會指定一組不應該執行給定的範圍 （動作或控制器） 的篩選器型別。 這可讓您將加入全域的篩選，但是再排除某些特定的動作或控制器。

### <a name="owin-integration"></a>OWIN 整合

ASP.NET Web API 現在完全支援 OWIN，並可以在任何 OWIN 功能的主機上執行。 另外還包括**HostAuthenticationFilter**提供與 OWIN 驗證系統的整合。

與 OWIN 整合，您可以連同其他 OWIN 中介軟體，例如 SignalR 自己處理序中自我裝載 Web 應用程式開發介面。 如需詳細資訊，請參閱[Self-Host ASP.NET Web 應用程式開發介面使用 OWIN](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)。

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET SignalR 2.0

下列章節說明 SignalR 2.0 的功能。

- [在 OWIN 上建置](#builtonowin)
- [MapHubs 和 MapConnection 現在是 MapSignalR](#MapSignalR)
- [跨網域支援](#crossdomain)
- [iOS 和 Android 支援透過 MonoTouch 和 MonoDroid](#mobile)
- [可攜式.NET 用戶端](#portable)
- [新的自我裝載的封裝](#selfhost)
- [回溯相容的伺服器支援](#backwardcompat)
- [移除.NET 4.0 的伺服器支援](#remove40)
- [將訊息傳送到用戶端和群組的清單](#messagelist)
- [傳送訊息給特定使用者](#sendtouser)
- [更好的錯誤處理支援](#errorhandling)
- [更容易的單元測試中樞](#unittesting)
- [JavaScript 錯誤處理](#javascripterror)

如需如何將現有的 1.x 專案升級至 SignalR 2.0 的範例，請參閱[升級 SignalR 1.x 專案](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md)。

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>在 OWIN 上建置

完全在建置 SignalR 2.0 [OWIN （開啟 Web 介面 for.NET）](http://owin.org/)。 這項變更讓 web 主控和自我裝載 SignalR 應用程式之間更為一致 SignalR 的安裝程序，但也有必要的應用程式開發介面變更數目。

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs 和 MapConnection 現在是 MapSignalR

為了與 OWIN 標準相容，這些方法已重新命名為`MapSignalR`。 `MapSignalR` 呼叫沒有參數會對應所有中樞 (做為`MapHubs`中版本 1.x); 若要將都對應個別**PersistentConnection**物件，指定連接類型為型別參數，以及做為連線的 URL 擴充功能第一個引數。

`MapSignalR` Owin 啟動類別中呼叫方法。 Visual Studio 2013 包含 Owin 啟動類別; 事件類別的新範本若要使用此範本，執行下列作業：

1. 以滑鼠右鍵按一下專案
2. 選取**新增**，**新項目...**
3. 選取**Owin 啟動 「 類別**。 將新類別**Startup.cs**。

在**Web 應用程式，** Owin 啟動類別包含`MapSignalR`方法接著會新增至 Owin 的啟動程序使用的項目中的應用程式設定節點的 Web.Config 檔案中，如下所示。

在**自我裝載應用程式**，啟動類別會傳遞做為型別參數的`WebApp.Start`方法。

**對應集線器與 SignalR 連線 1.x （從 web 應用程式中的全域應用程式檔案）：** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**對應集線器與 SignalR 2.0 （從 Owin 啟動 「 類別檔案） 中的連線：** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

在**自我裝載應用程式**，啟動類別會當做型別參數傳遞`WebApp.Start`方法，如下所示。

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>跨網域支援

SignalR 1.x，跨網域要求由單一 EnableCrossDomain 中的旗標。 這個旗標控制 JSONP 及 CORS 要求。 所有的 CORS 支援較大的彈性，已經移除了 SignalR 的伺服器元件 （JavaScript lients 仍然使用 CORS 通常如果它偵測到瀏覽器支援它），而且新的 OWIN 中介軟體進行可用來支援這些案例。

在 SignalR 2.0 中，如果 JSONP 才能在用戶端 （支援跨網域要求中舊的瀏覽器），它必須明確啟用藉由設定`EnableJSONP`上`HubConfiguration`物件`true`，如下所示。 JSONP 已停用根據預設，因為它比 CORS 較不安全。

若要加入新的 CORS 中介軟體 SignalR 2.0 中，加入`Microsoft.Owin.Cors`至您的專案，並呼叫的程式庫`UseCors`之前 SignalR 中介軟體下, 一節中所示。

**專案中加入 Microsoft.Owin.Cors**： 若要安裝此程式庫，請在 Package Manager Console 中執行下列命令：

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

此命令將新增 2.0.0 封裝至您的專案版本。

**呼叫 UseCors**

下列程式碼片段示範如何實作 SignalR 中跨網域連線 1.x 和 2.0 版。

**實作 SignalR 中跨網域要求 1.x （從全域應用程式檔案）**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**實作 SignalR 2.0 （從 C# 程式碼檔案） 中的跨網域要求**

下列程式碼會示範如何啟用 CORS 或 JSONP SignalR 2.0 專案中。 這個程式碼範例會使用`Map`和`RunSignalR`而不是`MapSignalR`，如此一來，CORS 中介軟體執行只會針對需要 CORS 支援 SignalR 要求 (而不會針對在指定的路徑上的所有流量`MapSignalR`。)`Map`也可以用於任何其他中介軟體，必須執行特定的 URL 前置詞，而不是整個應用程式。

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>iOS 和 Android 支援透過 MonoTouch 和 MonoDroid

已新增支援適用於 iOS 和 Android 的用戶端使用 MonoTouch 和 MonoDroid 元件從[Xamarin 程式庫](https://xamarin.com/)。 如需使用方式的詳細資訊，請參閱[使用 Xamarin 元件](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln)。 這些元件可在[Xamarin 市集](https://store.xamarin.com/)SignalR RTW 發行時使用。

<a id="portable"></a> # # # 可攜式.NET 用戶端

以更好地簡化跨平台開發、 Silverlight、 WinRT 和 Windows Phone 用戶端已取代為單一可攜式.NET 用戶端支援下列平台：

- NET 4.5
- Silverlight 5
- WinRT (Windows 市集應用程式的.NET)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>新的自我裝載的封裝

現在是 NuGet 封裝來更輕鬆地開始使用 SignalR 自我裝載 （所裝載處理序中的 SignalR 應用程式或其他應用程式，而非裝載在網頁伺服器）。 若要升級自我裝載所建置的專案與 SignalR 1.x 移除 Microsoft.AspNet.SignalR.Owin 封裝，並新增 Microsoft.AspNet.SignalR.SelfHost 封裝。 如需有關如何開始使用的自我裝載套件的詳細資訊，請參閱[教學課程： 自我裝載的 SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)。

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>回溯相容的伺服器支援

在舊版的 SignalR、 SignalR 封裝用於用戶端和伺服器必須是相同的版本。 為了支援大型用戶端應用程式會很難更新，SignalR 2.0 現在支援使用舊版用戶端較新的伺服器版本。 **注意： SignalR 2.0 不支援與較新的用戶端所建置與較舊版本的伺服器。**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>移除.NET 4.0 的伺服器支援

SignalR 2.0 已卸除伺服器互通性與.NET 4.0 的支援。 與 SignalR 2.0 伺服器，必須使用.NET 4.5。 仍有 SignalR 2.0 針對.NET 4.0 用戶端。

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>將訊息傳送到用戶端和群組的清單

在 SignalR 2.0 中，很可能傳送訊息使用的用戶端與群組識別碼清單。 下列程式碼片段會示範如何執行這項操作。

**將訊息傳送到用戶端與使用 PersistentConnection 群組的清單**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**將訊息傳送到用戶端與使用中樞群組的清單**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>傳送訊息給特定使用者

這項功能可讓使用者指定的使用者識別碼的是根據透過新的介面 IUserIdProvider IRequest:

**IUserIdProvider 介面**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

依預設會有的實作會透過使用者的 IPrincipal.Identity.Name 的使用者名稱。

在中樞，您可以將訊息傳送給這些使用者，透過新的 API:

**使用 Clients.User 應用程式開發介面**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>更好的錯誤處理支援

使用者現在可以擲回**HubException**從任何中樞叫用。 建構函式**HubException**字串訊息和物件可能需要額外的錯誤資料。 SignalR 將自動序列化例外狀況，並將它傳送至用戶端它用來拒絕/失敗中樞方法叫用。

**顯示詳細的中樞例外狀況**設定沒有任何關係**HubException**或; 傳送回用戶端一律傳送出去。

**伺服器端程式碼示範 HubException 傳送給用戶端**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**JavaScript 用戶端程式碼示範 HubException，從伺服器傳送回應**

[!code-html[Main](release-notes/samples/sample16.html)]

**.NET 用戶端程式碼示範 HubException，從伺服器傳送回應**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>更容易的單元測試中樞

SignalR 2.0 包含呼叫的介面`IHubCallerConnectionContext`輕鬆地建立模擬用戶端端引動過程的中樞上。 下列程式碼片段將示範如何使用這個介面與熱門測試控管[xUnit.net](https://github.com/xunit/xunit)和[moq](https://code.google.com/p/moq/)。

**使用單元測試 SignalR xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**使用單元測試 SignalR moq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>JavaScript 錯誤處理

在 SignalR 2.0 中，所有的 JavaScript 錯誤處理回呼會傳回 JavaScript 錯誤物件，而不是原始字串。 這可讓 SignalR 流動至您的錯誤處理常式更豐富的資訊。 您可以取得內部例外狀況，從`source`錯誤的屬性。

**處理 Start.Fail 例外狀況的 JavaScript 用戶端程式碼**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>新的 ASP.NET 成員資格系統

ASP.NET Identity 是新的成員資格系統，ASP.NET 應用程式。 ASP.NET Identity 輕鬆地整合應用程式資料的使用者特定設定檔資料。 ASP.NET Identity 也可讓您選擇您的應用程式中的使用者設定檔的持續性模型。 您可以將資料儲存在 SQL Server 資料庫或其他資料存放區，包括 NoSQL 資料存放區，例如 Azure 儲存體資料表。 如需詳細資訊，請參閱[個別使用者帳戶](creating-web-projects-in-visual-studio.md#indauth)中**Visual Studio 2013 中建立 ASP.NET Web 專案**。

### <a name="claims-based-authentication"></a>宣告架構驗證

ASP.NET 現在支援宣告式的驗證，而使用者的身分識別都代表一組從受信任的簽發者的宣告。 使用者可以是已驗證的使用者名稱和密碼保存在應用程式資料庫，或使用社交身分識別提供者 (例如： Microsoft 帳戶、 Facebook、 Google、 Twitter)，或使用透過 Azure Active Directory 組織帳戶或Active Directory Federation Services (ADFS)。

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>與 Azure Active Directory 和 Windows Server Active Directory 整合

您現在可以建立使用 Azure Active Directory 或 Windows Server Active Directory (AD) 進行驗證的 ASP.NET 專案。 如需詳細資訊，請參閱[組織帳戶](creating-web-projects-in-visual-studio.md#orgauth)中**Visual Studio 2013 中建立 ASP.NET Web 專案**。

### <a name="owin-integration"></a>OWIN 整合

ASP.NET 驗證現在會根據可用在任何 OWIN 架構的主機的 OWIN 中介軟體。 如需 OWIN 的詳細資訊，請參閱下列[Microsoft OWIN 元件](#TOC7)> 一節。

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Microsoft OWIN 元件

[開啟適用於.NET 的 Web 介面](http://owin.org/)(OWIN) 定義.NET web 伺服器和 web 應用程式之間的抽象概念。 OWIN 分隔與 web 應用程式，從伺服器進行 web 應用程式主機無從驗證的。 例如，您可以裝載在 IIS 中 OWIN web 應用程式，或自我裝載的自訂程序中。

Microsoft OWIN 元件 （也稱為 Katana 專案） 中導入的變更包括新的伺服器和主機的元件、 新的協助程式程式庫中介軟體，以及新的驗證中介軟體。

OWIN 和 Katana 相關的詳細資訊，請參閱[OWIN 和 Katana 中最新消息](../../../aspnet/overview/owin-and-katana/index.md)。

**注意： [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)應用程式無法在 IIS 傳統模式執行，則必須在整合式模式中執行。**

**注意： [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)必須完全信任中執行應用程式。**

### <a name="new-servers-and-hosts"></a>新的伺服器與主控件

與此版本中，加入了新元件，以啟用自我裝載案例。 這些元件包括下列 NuGet 套件：

- **Microsoft.Owin.Host.HttpListener**. 提供 OWIN 使用的伺服器， **HttpListener**接聽 HTTP 要求，並將他們導向至 OWIN 管線。
- **Microsoft.Owin.Hosting**提供文件庫的開發人員想要自助代管 OWIN 管線中的自訂處理序，例如主控台應用程式或 Windows 服務。
- **OwinHost**。 提供獨立的可執行檔包裝`Microsoft.Owin.Hosting`並可讓您在自我裝載 OWIN 管線，而不需要撰寫自訂主應用程式。

此外，`Microsoft.Owin.Host.SystemWeb`封裝現在可讓中介軟體提供提示給**SystemWeb**伺服器，表示在特定的 ASP.NET 管線階段期間，應該呼叫中介軟體。 這項功能就特別有用，驗證中介軟體，應該在早期的 ASP.NET 管線中執行的。

### <a name="helper-libraries-and-middleware"></a>協助程式程式庫和中介軟體

雖然您可以撰寫 OWIN 元件使用的函式和類型定義 OWIN 規格中，從新`Microsoft.Owin`套件提供一組更加易懂易記的抽象概念。 此套件會結合數個舊版的封裝 (例如`Owin.Extensions`， `Owin.Types`) 到單一結構完善的物件模型可以輕鬆地使用其他 OWIN 元件。 事實上，大部分的 Microsoft OWIN 元件現在會使用此封裝。

> [!NOTE]
> [OWIN](http://www.owin.org)應用程式無法在 IIS 傳統模式執行，則必須在整合式模式中執行。

> [!NOTE]
> [OWIN](http://www.owin.org)必須完全信任中執行應用程式。

此版本也包含 Microsoft.Owin.Diagnostics 套件，其中包含中介軟體驗證執行的 OWIN 應用程式，再加上以協助調查失敗的錯誤頁面中介軟體。

### <a name="authentication-components"></a>驗證元件

使用下列的驗證元件。

- **Microsoft.Owin.Security.ActiveDirectory**. 啟用使用在內部部署或雲端式目錄服務的驗證。
- **Microsoft.Owin.Security.Cookies**使用 cookie 會啟用驗證。 此套件先前命名為`Microsoft.Owin.Security.Forms`。
- **Microsoft.Owin.Security.Facebook**啟用驗證使用 Facebook 的 OAuth 基礎服務。
- **Microsoft.Owin.Security.Google**使用 Google OpenID 型服務啟用驗證。
- **Microsoft.Owin.Security.Jwt**啟用驗證使用 JWT 權杖。
- **Microsoft.Owin.Security.MicrosoftAccount**啟用驗證使用 Microsoft 帳戶。
- **Microsoft.Owin.Security.OAuth**. 提供 OAuth 授權伺服器與中介軟體驗證持有人權杖。
- **Microsoft.Owin.Security.Twitter**啟用驗證使用 Twitter 的 OAuth 基礎服務。

此版本也包含`Microsoft.Owin.Cors`包含處理跨原始 HTTP 要求的中介軟體套件。

> [!NOTE]
> 在 Visual Studio 2013 的最終版本中移除了來簽署 JWT 的支援。

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

如需新功能和 Entity Framework 6 中的其他變更的清單，請參閱[Entity Framework 版本記錄](https://msdn.com/data/jj574253)。

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

ASP.NET Razor 3 包含下列新功能：

- 支援編輯索引標籤。 Preivously，**格式化文件**命令、 自動縮排，並自動在 Visual Studio 中格式化未正確運作時使用**保留定位點**選項。 這項變更就會更正格式的格式設定 索引標籤的 Razor 程式碼的 Visual Studio。
- 支援 URL 重寫規則時產生連結。
- 安全性透明屬性中移除。
  > [!NOTE]
  > 這是一項重大變更，並使 Razor 3 相容 MVC4 和舊版中，Razor 2 時與 MVC5 或針對 MVC5 編譯的組件不相容。

修正 Visual Studio 2013 中，從發行前版本的 razor 3 問題可以找到[這裡](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0)。

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>ASP.NET 應用程式暫停

ASP.NET 應用程式暫停是.NET Framework 4.5.1 會徹底變更的使用者經驗與裝載在單一機器上的 ASP.NET 網站的數目很大的經濟模型遊戲變更功能。 如需詳細資訊，請參閱[.NET web 裝載的 ASP.NET 應用程式暫止 – 回應共用](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx)。

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的問題和重大變更

本章節描述已知的問題和重大變更 ASP.NET 及 Web Tools for Visual Studio 2013。

### <a name="nuget"></a>NuGet

- [新的封裝還原不適用於單聲道上使用 SLN 檔](https://nuget.codeplex.com/workitem/3596)– 即將 nuget.exe 下載將會修正和[NuGet.CommandLine 封裝](http://www.nuget.org/packages/NuGet.CommandLine/)更新。
- [新的封裝還原不適合用於 Wix 專案](https://nuget.codeplex.com/workitem/3598)– 即將 nuget.exe 下載將會修正和[NuGet.CommandLine 封裝](http://www.nuget.org/packages/NuGet.CommandLine/)更新。
- [自動封裝還原不適用於專案的方案資料夾下](https://nuget.codeplex.com/workitem/3625)– NuGet 2.8 會修正問題。

### <a name="aspnet-web-api"></a>ASP.NET Web API

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` 不會傳回`IQueryable<T>`一律，我們加入了支援`$select`和`$expand`。

    我們先前的範例如`ODataQueryOptions<T>`一律轉型的傳回值從`ApplyTo`至`IQueryable<T>`。 此查詢選項，因為先前使用過我們支援早 (`$filter`， `$orderby`， `$skip`， `$top`) 不會變更查詢的圖形。 現在，我們支援`$select`和`$expand`的傳回值`ApplyTo`將不會`IQueryable<T>`一律。

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    如果您使用從先前的範例程式碼，它就會繼續工作，如果用戶端不會傳送`$select`和`$expand`。 不過，如果您想要支援`$select`和`$expand`您必須將該程式碼變更為。

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url RequestContext.Url 設為 null 或批次要求期間**

    在批次處理的案例中， **UrlHelper**為 null 時從存取**Request.Url**或**RequestContext.Url**。

    此問題目前追蹤以下： [BatchRequestContext.Url 為 null 的批次要求](http://aspnetwebstack.codeplex.com/workitem/1301)。

    此問題的因應措施是建立的新執行個體**UrlHelper**，如下列範例所示：

    **建立 UrlHelper 的新執行個體**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. 使用 MVC5 和時 OrgAuth，如果您的檢視完成這件事 AntiForgerToken 驗證，您可能會遇到下列錯誤時張貼至檢視的資料：

    **錯誤**:

    *'/' 應用程式中的伺服器錯誤。*

    <em>宣告類型 '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>'或'<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' 不存在於上提供的身分識別。若要啟用防偽語彙基元支援使用宣告式驗證，請確認已設定的宣告提供者會提供這兩個身分識別執行個體就會產生這些宣告。如果已設定的宣告提供者改為使用不同的宣告類型做為唯一的識別項，則它可以設定的靜態屬性 AntiForgeryConfig.UniqueClaimTypeIdentifier 設定。</em>

    **因應措施**：

    若要修正此問題的 Global.asax 中加入下行：

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    這將在下一版的修正。
2. 升級之後的 MVC4 應用程式至 MVC5，建置方案並啟動它。 您應該會看到下列錯誤：

    [A]System.Web.WebPages.Razor.Configuration.HostSection 無法轉換成 [B]System.Web.WebPages.Razor.Configuration.HostSection。 類型為 A 源自 'System.Web.WebPages.Razor，Version = 2.0.0.0，Culture = neutral，PublicKeyToken = 31bf3856ad364e35' 內容位置的' Default' 中 ' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'。 型別 B 源自 'System.Web.WebPages.Razor，Version = 3.0.0.0，Culture = neutral，PublicKeyToken = 31bf3856ad364e35' 中的內容位置的' Default' ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'。

    若要修正上述錯誤，請開啟*所有*中的 Web.config 檔案 （包括 [檢視] 資料夾中） 您的專案並執行下列：

   1. 更新版本的"4.0.0.0 」"system.web.mvc 的參考 」 到 「 5.0.0.0 」 的所有項目。
   2. 更新 「 System.Web.Helpers"，版本"2.0.0.0"的所有項目&quot;System.Web.WebPages&quot;和&quot;System.Web.WebPages.Razor&quot;至 「 3.0.0.0"

      例如，您進行上述變更之後，組件繫結看起來應該像這樣：

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      將 MVC 4 專案升級至 MVC 5 的資訊，請參閱[如何將 ASP.NET MVC 4 和 Web API 專案升級至 ASP.NET MVC 5 和 Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)。
3. 當使用 jQuery 驗證不顯眼的用戶端驗證，驗證訊息有時不正確的 HTML input 元素，具有類型 = 'number'。 驗證錯誤必要值 （「 時間欄位必要 」) 會顯示無效的數字輸入而不是正確的有效數字是必要的訊息時。

    此問題通常找到具有 scaffold 的程式碼的整數屬性上建立與編輯檢視的模型。

    若要解決此問題，請變更編輯器協助專家從：

    `@Html.EditorFor(person => person.Age)`

    收件者:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 已不再支援部分信任。 專案和 MVC 或 WebAPI 二進位檔連結應該移除[SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx)屬性和[AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx)屬性。 移除這些屬性會消除編譯器錯誤，如下所示。

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > 請注意，當副作用： 您無法在相同的應用程式中使用 4.0 和 5.0 的組件。 您必須全部更新至 5.0。

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>SPA Facebook 授權範本可能會造成不穩定在 IE 中時的網站裝載在內部網路區域

SPA 範本提供使用 Facebook 的外部記錄檔。 當執行使用範本建立專案時，請在本機登入可能會導致 IE 損毀。

解決方案:

1. 裝載在網際網路區域; 中的網站或

2. 在非 IE 的瀏覽器中測試案例。

### <a name="web-forms-scaffolding"></a>Web Form 的 Scaffolding

Web Form Scaffolding 已經移除了 VS2013，並將可在 Visual Studio 的未來更新中。 不過，您仍然可以藉由新增 MVC 相依性產生 scaffolding mvc 中使用 scaffolding Web Form 專案中。 您的專案會包含 Web Form 和 MVC 的組合。

若要新增 MVC Web Form 專案，加入新的 scaffold 項目，然後選取**MVC 5 相依性**。 選取最少或完全根據您是否需要的所有內容檔案，例如指令碼。 然後，將會在您的專案建立檢視和控制器的 mvc 加入 scaffold 項目。

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC 和 Web API Scaffolding-HTTP 404 找不到錯誤

如果專案中加入 scaffold 項目時，發生錯誤，，很可能您的專案將會處於不一致的狀態。 部分 scaffolding 所做的變更將會回復，但其他變更，例如安裝的 NuGet 套件，將不會回復。 如果路由的組態變更會回復，使用者會收到 HTTP 404 錯誤，當巡覽至 scaffold 項目。

因應措施：

- 若要修正這個錯誤 mvc 中，加入新的 scaffold 項目，然後選取 MVC 5 相依性 （基本或完整）。 此程序將會加入所有必要的變更您的專案。
- 若要修正這個錯誤 Web api:

  1. 視情況將類別加入至您的專案。

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. 在應用程式中設定 WebApiConfig.Register\_，如下所示在 Global.asax 中啟動方法：

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
