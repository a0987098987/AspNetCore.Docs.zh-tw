---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: 建立 ASP.NET Web 專案在 Visual Studio 2013 |Microsoft Docs
author: tdykstra
description: 本主題說明與這裡 Update 3 的 Visual Studio 2013 中建立 ASP.NET web 專案的選項有一些新功能，適用於 web 開發 c...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/01/2014
ms.topic: article
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 6033163b7b8e9e1d0524935217dc53f938470cb6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363464"
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>在 Visual Studio 2013 中建立 ASP.NET Web 專案
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

> 本主題說明在 Visual Studio 2013 Update 3 中建立 ASP.NET web 專案的選項
> 
> 以下是一些相較於舊版的 Visual Studio web 程式開發的新功能：
> 
> - 建立簡單的 UI 專案該供應項目[支援多個 ASP.NET 架構](#add)（Web Form、 MVC 和 Web API）。
> - [ASP.NET Identity](#indauth)，新的 ASP.NET 成員資格系統，與 web 裝載軟體，而不是 IIS 的運作方式在所有 ASP.NET framework 和運作方式相同。
> - 善用[Bootstrap](#bootstrap)提供回應式設計和佈景主題的功能。
> - 新功能，例如提供只針對 MVC、 Web Form[自動測試專案的建立](#testproj)並[內部網路網站範本](#winauth)。
> 
> 如需有關如何為 Azure 雲端服務或 Azure 行動服務中建立 web 專案的資訊，請參閱[開始使用 Azure 雲端服務和 ASP.NET](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/)和[使用 Azure 行動服務.NET 建立排行榜應用程式後端](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)。


<a id="prerequisites"></a>
## <a name="prerequisites"></a>必要條件

本文適用於[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)具有[Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409)安裝。

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>與網站專案的 web 應用程式專案

ASP.NET 可讓您可以選擇兩種類型的 web 專案： *web 應用程式專案*並*網站專案*。 我們建議新的程式開發的 web 應用程式專案，而且這篇文章只適用於 web 應用程式專案。 如需詳細資訊，請參閱 < [Web 應用程式專案與 Visual Studio 中的網站專案](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx)MSDN 網站上。

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>建立 web 應用程式專案的概觀

下列步驟示範如何建立 web 專案：

1. 按一下 **新的專案**中**開始**頁面或在**檔案**功能表。
2. 在**新的專案** 對話方塊中，按一下**Web**的左窗格中並**ASP.NET Web 應用程式**在中間窗格中。

    ![[新增專案] 對話](creating-web-projects-in-visual-studio/_static/image1.png)

    您可以選擇**雲端**若要建立的左窗格中[Azure 雲端服務](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)， [Azure 行動服務](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx)，或[Azure WebJob](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs)。 本主題並未涵蓋這些範本。
3. 在右窗格中，按一下**將 Application Insights 加入專案**核取方塊，如果您想要的健康情況和使用情況監視您的應用程式。 如需詳細資訊，請參閱 <<c0> [ 監視 web 應用程式的效能](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/)。
4. 指定專案**名稱**，**位置**，和其他選項] 和 [**確定**。

    **新的 ASP.NET 專案**對話方塊隨即出現。

    ![[新增專案] 對話](creating-web-projects-in-visual-studio/_static/image2.png)
5. 按一下 [範本]。

    ![選取範本](creating-web-projects-in-visual-studio/_static/image3.png)
6. 如果您想要新增未包含在範本中的其他架構的支援，請按一下適當的核取方塊。 （在範例中所示，您可以將 MVC 和/或 Web API 加入 Web Form 專案。）

    ![新增架構](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>如果您想要新增單元測試專案，按一下**加入單元測試**。

    ![加入單元測試](creating-web-projects-in-visual-studio/_static/image5.png)
8. 如果您想要不同的驗證方法，比預設提供的範本，請按一下**變更驗證**。

    ![設定驗證按鈕](creating-web-projects-in-visual-studio/_static/image6.png)

    ![設定驗證對話方塊](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>在 Azure 中建立的 web 應用程式或虛擬機器

Visual Studio 包含可讓您輕鬆地使用 Azure 服務來裝載 web 應用程式功能。 例如，您可以直接從 Visual Studio IDE 執行下列各項：

- 建立和管理 web 應用程式或將應用程式可以透過網際網路提供的虛擬機器。
- 檢視執行中雲端應用程式建立的記錄檔。
- 在 偵錯模式中從遠端執行時在雲端執行的應用程式。
- Viiew 及管理其他 Azure 服務，例如 SQL 資料庫。

您可以[建立 Azure 帳戶](https://www.windowsazure.com/pricing/free-trial/)免費，包含基本的服務，例如 web 應用程式，且如果您是 MSDN 訂閱者可以[啟用權益](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/)，可以讓您向其他 Azure 每月信用額度服務。 

依預設**新的 ASP.NET 專案**對話方塊可讓您建立 web 應用程式或新的 web 專案的虛擬機器。 如果您不想要建立新的 web 應用程式或虛擬機器，請清除**雲端中的主機**核取方塊。

![建立遠端資源](creating-web-projects-in-visual-studio/_static/image8.png)

核取方塊標題可能**在雲端託管**或**建立遠端資源**，並在任一情況下的效果是一樣。 如果您保留選取的核取方塊，則 Visual Studio 會建立在 Azure App Service web 應用程式預設。 您可以把它改為使用下拉式清單方塊**虛擬機器**如果您偏好。 如果您在尚未登入至 Azure，系統會提示您輸入 Azure 認證。 登入之後，對話方塊可讓您設定 Visual Studio 會為您的專案建立的資源。 下圖顯示 [web 應用程式;] 對話方塊如果您選擇建立虛擬機器，則會顯示不同的選項。

![設定 Azure 應用程式設定](creating-web-projects-in-visual-studio/_static/image9.png)

如需如何使用此程序來建立 Azure 資源的詳細資訊，請參閱[開始使用 Azure 和 ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)並[使用 Visual Studio 中建立虛擬機器網站](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/)。

這篇文章的其餘部分會提供可用的範本和其選項的詳細資訊。 本文也會介紹啟動程序、 版面配置和佈景主題的範本中所使用的架構。

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Visual Studio 2013 Web 專案範本

Visual Studio 會使用範本來建立 web 專案。 專案範本可以建立新的專案中的檔案和資料夾、 安裝 NuGet 套件，並提供範例程式碼的基本的可行應用程式。 範本會實作最新的 web 標準，並用來示範最佳作法來使用 ASP.NET 技術，以及讓您跳開始建立您自己的應用程式。

Visual Studio 2013 的.NET 4.5 或更新版本的.NET framework 為目標的專案的 web 專案範本提供下列選項：

- [空白範本](#empty)
- [Web Forms 範本](#wf)
- [MVC 範本](#mvc)
- [Web API 範本](#webapi)
- [單一頁面應用程式範本](#spa)
- [Azure 行動服務範本](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Visual Studio 2012 的範本](#vs2012)

您也可以安裝 Visual Studio 延伸模組，可[Facebook 範本](#facebook)。

如需如何建立.NET 4 為目標的專案資訊，請參閱[Visual Studio 2012 範本](#vs2012)本主題稍後的。

如需有關如何建立行動用戶端的 ASP.NET 應用程式的資訊，請參閱 <<c0> [ 在 ASP.NET 中的行動裝置版支援](../../../mobile/index.md)。

<a id="empty"></a>
### <a name="empty-template"></a>空白範本

空白範本提供的裸機最小的資料夾和 ASP.NET web 應用程式，例如專案檔案的檔案 (*.csproj*或。*vbproj*) 和*Web.config*檔案。 您可以使用下的核取方塊，以便新增 Web Form、 MVC、 和/或 Web API 的支援**新增的資料夾和核心參考：** 標籤。

空白範本沒有驗證的選項可供使用。 在範例應用程式中實作驗證功能和空白的範本不會建立範例應用程式。

<a id="wf"></a>
### <a name="web-forms-template"></a>Web Forms 範本

Web Form 架構提供下列功能，可讓您快速建置豐富的 UI 與資料的 web 站台存取功能：

- 在 Visual Studio 中的 WYSIWYG 設計工具。
- 呈現 HTML 與您可以藉由設定屬性和樣式自訂的伺服器控制項。
- 各式各樣豐富的資料存取與資料顯示控制項。
- 公開 （expose） 您可以像您一樣進行程式設計的事件的事件模型會程式，例如 WPF 用戶端應用程式。
- 自動保留之間的 HTTP 要求的狀態 （資料）。

一般情況下，建立 Web Form 應用程式需要比建立相同的應用程式使用 ASP.NET MVC 架構程式設計事半功倍。 不過，Web Form 不只是快速應用程式開發。 有許多複雜的商業應用程式和 Web Form 為基礎的架構。

Web Form 頁面和頁面上的控制項自動產生的大部分會傳送至瀏覽器的標記，因為您不需要 ASP.NET MVC 提供 HTML 更細微的控制的類型。 設定網頁及控制項的宣告式模型盡量縮短程式碼，您必須撰寫，但會隱藏一些 HTML 和 HTTP 的行為。 比方說，它就不一定能夠指定完全哪些標記可能會產生由控制項。

Web Form 架構不本身就立即為 ASP.NET MVC 模式為基礎的開發實務這類[測試導向開發](http://en.wikipedia.org/wiki/Test-driven_development)，[關注點分離](http://en.wikipedia.org/wiki/Separation_of_concerns)，[逆轉控制項](http://en.wikipedia.org/wiki/Inversion_of_control)，並[相依性插入](http://en.wikipedia.org/wiki/Dependency_injection)。 如果您想要撰寫程式碼中分解出來這樣一來，您可以;不只是因為它是在 ASP.NET MVC 架構中的自動。 [ASP.NET Web Form MVP](http://webformsmvp.com/)專案會顯示一種方法，可加速的關注點分離可測試性維持快速開發 Web Form 所建置來傳遞。 Microsoft SharePoint 的基礎 Web Form MVP。

Web Form 範本會建立使用的範例 Web Form 應用程式[Bootstrap](#bootstrap)提供回應式設計和佈景主題的功能。 下圖顯示 [首頁] 頁面。

![Web Form 範本應用程式首頁](creating-web-projects-in-visual-studio/_static/image10.png)

如需有關 Web Form 的詳細資訊，請參閱[ASP.NET Web Form](https://asp.net/web-forms)。 Web Form 範本為您執行作業的相關資訊，請參閱[建置基本的 Web Form 應用程式，使用 Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx)。

<a id="mvc"></a>
### <a name="mvc-template"></a>MVC 範本

ASP.NET MVC 的用意是要促進模式為基礎的開發作法，例如[測試導向開發](http://en.wikipedia.org/wiki/Test-driven_development)，[關注點分離](http://en.wikipedia.org/wiki/Separation_of_concerns)，[逆轉控制](http://en.wikipedia.org/wiki/Inversion_of_control)，並[相依性插入](http://en.wikipedia.org/wiki/Dependency_injection)。 此架構有助於區隔商務邏輯層，web 應用程式從其展示層。 藉由將應用程式分割成模型 (M)、 (V)，檢視和控制器 (C)，ASP.NET MVC 可輕鬆地管理大型應用程式的複雜度。

ASP.NET MVC 中，您可以使用更直接處理 HTML 和 HTTP 比在 Web Form 中。 例如，Web Form 可以自動保留 HTTP 要求之間的狀態，但您不必撰寫程式碼，明確地在 MVC 中。 MVC 模型的優點是它可讓您完全應用程式做什麼，以及在 web 環境中的運作方式的完整控制權。 缺點是，您必須撰寫更多程式碼。

MVC 被設計成可延伸，提供電源開發人員能夠自訂他們的應用程式需求的架構。 此外，ASP.NET MVC 來源程式碼可依據 OSI 授權。

MVC 範本會建立使用的範例 MVC 5 應用程式[Bootstrap](#bootstrap)提供回應式設計和佈景主題的功能。 下圖顯示 [首頁] 頁面。

![MVC 範例應用程式](creating-web-projects-in-visual-studio/_static/image11.png)

如需有關 MVC 的詳細資訊，請參閱[ASP.NET MVC](https://asp.net/mvc)。 如需有關如何選取 MVC 4 範本的資訊，請參閱[Visual Studio 2012 範本](#vs2012)本文稍後。

<a id="webapi"></a>
### <a name="web-api-template"></a>Web API 範本

Web API 範本會建立 Web API，包括 MVC 為基礎的 API 說明頁面為基礎的範例 web 服務。

ASP.NET Web API 是一種架構，可讓您更輕鬆建置 HTTP 服務並擴及各種用戶端，包括瀏覽器和行動裝置。 ASP.NET Web API 是在.NET Framework 上建置 RESTful 服務的理想平台。

Web API 範本會建立範例 web 服務。 下圖顯示範例 [說明] 頁面。

![Web API 說明頁面](creating-web-projects-in-visual-studio/_static/image12.png)

![取得 API 的 web API 說明頁面](creating-web-projects-in-visual-studio/_static/image13.png)

如需有關 Web API 的詳細資訊，請參閱 < [ASP.NET Web API](https://asp.net/web-api)。

<a id="spa"></a>
### <a name="single-page-application-template"></a>單一頁面應用程式範本

單一頁面應用程式 (SPA) 範本會建立使用 JavaScript，HTML 5 的範例應用程式和[KnockoutJS](http://knockoutjs.com/)上用戶端和伺服器上的 ASP.NET Web API。

SPA 範本的唯一驗證選項是[個別使用者帳戶](#indauth)。

下圖顯示 SPA 範本就會建置範例應用程式的初始狀態。

![SPA 範例應用程式](creating-web-projects-in-visual-studio/_static/image14.png)

如需有關如何使用 SPA 範本建立應用程式的資訊，請參閱[Web API-外部驗證服務](../../../web-api/overview/security/external-authentication-services.md)。

如需有關 ASP.NET 單一頁面應用程式，以及其他使用 KnockoutJS 以外的 JavaScript 架構的 SPA 範本的詳細資訊，請參閱下列資源：

- [ASP.NET 單一頁面應用程式](../../../single-page-application/index.md)。
- [了解 VS2013 RC SPA 範本中的安全性功能](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [單一頁面應用程式： 建置新式、 反應靈敏的 Web 應用程式，使用 ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Facebook 範本

您可以安裝[Visual Studio 擴充功能，提供 Facebook 範本](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409)。 此範本會建立範例應用程式是設計用來在 Facebook 網站內執行。 它以 ASP.NET MVC 為基礎，並使用 Web API 進行即時的更新功能。

因為 Facebook 應用程式的 Facebook 站台內執行，並依賴 Facebook 的驗證，沒有驗證的選項可供使用 Facebook 範本。

如需有關 ASP.NET 的 Facebook 應用程式的詳細資訊，請參閱[更新 MVC Facebook API](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx)。

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Visual Studio 2012 的範本

Visual Studio 2013 的 web 專案建立對話方塊不提供存取 Visual Studio 2012 中有可用的一些範例。 如果您想要使用其中一個範本，您可以按一下 Visual Studio 新增專案 對話方塊的左窗格中的 Visual Studio 2012 節點。

![Visual Studio 2012 範本](creating-web-projects-in-visual-studio/_static/image15.png)

**Visual Studio 2012**節點可讓您選擇下列 web 範本，您不需要存取在預設的範本清單中的 Visual Studio 2013:

- ASP.NET MVC 4 Web 應用程式
- ASP.NET Dynamic Data 實體 Web 應用程式
- ASP.NET AJAX 伺服器控制項
- ASP.NET AJAX 伺服器控制項擴充項
- ASP.NET 伺服器控制項

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>啟動 Visual Studio 2013 web 專案範本

Visual Studio 2013 專案範本會使用[Bootstrap](http://getbootstrap.com/)，由 Twitter 的版面配置和佈景主題的架構。 啟動程序會使用 CSS3 來提供表示配置可以動態地適應不同的瀏覽器視窗大小的回應式設計。 比方說，在寬瀏覽器視窗中 Web Form 範本所建立的 首頁 頁面看起來像下圖：

![Web Form 範本應用程式首頁](creating-web-projects-in-visual-studio/_static/image16.png)

讓視窗變窄，並水平排列的資料行移到垂直排列：

![啟動程序的垂直資料行的排列方式](creating-web-projects-in-visual-studio/_static/image17.png)

稍微，縮小視窗和水平頂端的功能表會變成您可以按一下以展開至垂直方向的功能表圖示：

![啟動程序的功能表圖示](creating-web-projects-in-visual-studio/_static/image18.png)

![啟動程序垂直的功能表](creating-web-projects-in-visual-studio/_static/image19.png)

您也可以使用 Bootstrap 的佈景主題功能，輕鬆地達成應用程式的外觀與風格變更。 例如，您可以執行下列步驟來變更佈景主題。

1. 在瀏覽器中，移至[ http://Bootswatch.com ](http://Bootswatch.com)，選擇佈景主題，，然後按一下**下載**。 (這會下載*bootstrap.min.css*根據預設，如果您想要檢查的 CSS 程式碼，取得*bootstrap.css*而不是縮製的版本。)
2. 複製下載的 CSS 檔案的內容。
3. 在 Visual Studio 中，建立新**樣式表**檔案名*bootstrap theme.css*中*內容*到其中的資料夾，然後貼上已下載的 CSS 程式碼。
4. 開啟*應用程式\_Start/Bundle.config*並將變更*bootstrap.css*來*bootstrap theme.css*。

同樣地，執行專案和應用程式有新的外觀。 下圖顯示 Amelia 佈景主題的效果：

![Bootstrap Amelia 佈景主題](creating-web-projects-in-visual-studio/_static/image20.png)

許多的 Bootstrap 佈景主題可供使用，免費和高階版本。 啟動程序也會提供各種不同的 UI 元件，例如[下拉式清單](http://twitter.github.io/bootstrap/components.html#dropdowns)，[按鈕群組](http://twitter.github.io/bootstrap/components.html#buttonGroups)，並[圖示](http://twitter.github.io/bootstrap/base-css.html#images)。 如需有關啟動程序的詳細資訊，請參閱[啟動程序的站台](http://twitter.github.io/bootstrap/)。

如果您在 Visual Studio 中使用 Web Form 設計工具，請注意，設計工具並不支援 CSS3，因此它不會正確顯示所有的 Bootstrap 佈景主題] 或 [回應式配置的變更的效果。 不過，Web Form 頁面會顯示正確的瀏覽器檢視時。

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>新增對其他架構的支援

當您選取範本時，會自動選取範本使用架構來核取方塊。 例如，如果您選取**Web Form**範本**Web Form**核取方塊已選取，並且您無法清除它。

![選取範本](creating-web-projects-in-visual-studio/_static/image21.png)

![新增架構](creating-web-projects-in-visual-studio/_static/image22.png)

您可以選取核取方塊，不包含在範本中，若要建立專案時加入支援該架構的架構。 例如，若要啟用 Web Form *.aspx*頁，當您選取 [MVC] 範本中，選取**Web Form**核取方塊。 或當您使用 Web Forms 範本，請啟用 MVC，請按一下**MVC**核取方塊。 新增一個架構，可讓設計階段，以及執行階段支援。 比方說，如果您將 MVC 支援新增至 Web Form 專案時，您將能夠建立控制器和檢視的結構。

如果您結合 Web Form 和 MVC 專案中，並啟用[Friendly URLs](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx)在 Web Form，可能會有未預期的其中一個 URL 都有多個可能目標的路由問題。 第一次所定義的路由將會優先。 比方說，如果您有`Home`控制器並*Home.aspx*頁面上， `http://contoso.com/home` URL 將會移至*Home.aspx*如果您呼叫`EnableFriendlyUrls`方法之前呼叫`MapRoute`方法中的*RouteConfig.cs*，或相同的 URL 將會前往的預設檢視您`Home`如果您呼叫的控制站`MapRoute`再`EnableFriendlyUrls`。

加入一種架構不會新增任何範例應用程式的功能。 比方說，如果您加入 Web Form 支援您無法選取 MVC 範本，當*Default.aspx*建立首頁上的檔案。 會加入只資料夾、 檔案和支援的架構所需的參考。 基於這個理由，新增架構不會變更範本所建立的範例應用程式中的程式碼會實作的驗證選項。 例如，如果您選取 空白的範本，並新增 Web Form 或 MVC 支援，**設定驗證**仍會停用按鈕。

下列各節簡短描述每個核取方塊的效果。

### <a name="add-web-forms-support"></a>加入 Web Form 支援

建立空*應用程式\_資料*並*模型*資料夾和*Global.asax*檔案。 因此選取 Web Form 核取方塊可不讓任何其他範本的差異，這些已被建立空白的範本，以外的所有範本。

根據預設，但是當您將 Web Form 支援新增至其他範本，藉由選取 [Web Form] 核取方塊不會自動啟用 Friendly URLs 時，Web Form 範本會啟用易記 Url。

### <a name="add-mvc-support"></a>新增 MVC 支援

將 MVC、 Razor 及網頁 NuGet 套件安裝，則會建立空*應用程式\_資料*，*控制器*，*模型*，以及*檢視*資料夾，建立*應用程式\_開始*資料夾中的，使用*RouteConfig.cs*檔案，並建立*Global.asax*檔案。

### <a name="add-web-api-support"></a>新增 Web API 的支援

安裝 WebApi 和 Newtonsoft.Json NuGet 套件時，會建立空*應用程式\_資料*，*控制器*，和*模型*資料夾建立*應用程式\_開始*資料夾中的，使用*WebApiConfig.cs*檔案，並建立*Global.asax*檔案。

<a id="auth"></a>
## <a name="authentication-methods"></a>驗證方法

Visual Studio 2013 提供 Web Form、 MVC 和 Web API 範本的數個驗證的選項：

- [沒有驗證](#noauth)
- [個別使用者帳戶](#indauth)（ASP.NET 身分識別，前身為 ASP.NET 成員資格）
- [組織帳戶](#orgauth)（Windows Server Active Directory 或 Azure Active Directory）
- [Windows 驗證](#winauth)（內部網路）

![設定驗證對話方塊](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>沒有驗證

如果您選取**不需要驗證**，範例應用程式會包含任何的網頁登入，不表示使用者已登入 UI，沒有實體類別的成員資格資料庫中，並沒有成員資格資料庫的連接字串。

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>個別使用者帳戶

如果您選取**個別使用者帳戶**，範例應用程式會設定為使用 ASP.NET Identity （前身為 ASP.NET 成員資格） 來進行使用者驗證。 ASP.NET 身分識別可讓使用者註冊帳戶，站台上建立的使用者名稱和密碼，或登入 Facebook、 Google、 Microsoft 帳戶或 Twitter 等社交提供者。 在 ASP.NET 身分識別的使用者設定檔的預設資料存放區是 SQL Server LocalDB 資料庫，供您為生產網站部署到 SQL Server 或 Azure SQL Database。

Visual Studio 2013 中這些功能是 Visual Studio 2012 中，相同，但已重寫 ASP.NET 成員資格系統的基礎程式碼。 新的程式碼基底的優點包括下列各項：

- 新的成員資格系統根據[OWIN](http://owin.org/)而不是 ASP.NET 表單驗證模組。 這表示，您就可以使用相同的驗證機制，無論您使用 Web Form 或 MVC 在 IIS 中，或您自我裝載 Web API 或 SignalR。
- 新的成員資格資料庫由 Entity Framework Code First，和所有的資料表由您可以修改的實體類別。 這表示您可以輕鬆地自訂設定檔相關的 web UI 以符合您自己的需求，與資料庫結構描述，而且您可以輕鬆部署您使用 Code First 移轉的更新。

在新的範本中，自動實作新的成員資格系統，它可以是實作在任何以.NET 4.5 為目標的專案中手動或更新版本。

如果您要建立網際網路網站，也就是主要是供外部客戶，ASP.NET 身分識別會是不錯的選擇。 如果您的組織使用 Active Directory 或 Office 365 和您想要建立單一登入的專案，可讓員工和商務夥伴**組織帳戶**選項可能是較好的選擇。

如需有關個別使用者帳戶選項的詳細資訊，請參閱下列資源：

- [www.asp.net/identity](../../../identity/index.md)。 ASP.NET 身分識別的相關文件在 ASP.NET 網站上。
- [建立 ASP.NET MVC 5 應用程式使用 Facebook 和 Google OAuth2 和 OpenID 登入](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。 也會示範如何自訂使用者設定檔資料。
- [Web API-外部驗證服務](../../../web-api/overview/security/external-authentication-services.md)
- [將外部登入新增至您在 Visual Studio 2013 的 ASP.NET 應用程式](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>組織帳戶

如果您選取**組織帳戶**，範例應用程式會設定為使用 Windows Identity Foundation (WIF) Azure Active Directory (Azure AD 中，其中包含 Office 365) 中的使用者帳戶為基礎的驗證或Windows Server Active Directory。 如需詳細資訊，請參閱 <<c0> [ 組織帳戶驗證選項](#orgauthoptions)本主題稍後的。

<a id="winauth"></a>
### <a name="windows-authentication"></a>Windows 驗證

如果您選取**Windows 驗證**，範例應用程式會設定為使用 Windows 驗證的 IIS 模組進行驗證。 應用程式會顯示 Active directory 或本機電腦帳戶登入 Windows，但不會包含使用者註冊或登入 UI 的網域和使用者識別碼。 此選項適用於內部網路網站。

或者，您可以在其中建立內部網路網站選擇使用 AD 驗證[在內部部署組織的帳戶下的選項](#orgauthonprem)。 [內部] 選項會使用 Windows Identity Foundation (WIF)，而不是 Windows 驗證模組。 若要設定 [內部] 選項中，需要一些額外的步驟，但是 WIF 可讓無法使用 Windows 驗證模組的功能。 例如，使用 WIF 中，您可以設定應用程式存取 Active Directory 與查詢目錄資料。

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>組織帳戶驗證選項

**設定驗證**對話方塊可讓您的 Azure Active Directory (Azure AD 中，其中包含 Office 365) 或 Windows Server Active Directory (AD) 帳戶驗證的數個選項：

- [雲端-單一組織](#orgauthsingle)(Azure AD 或使用與 Azure AD 目錄整合的 AD)
- [雲端-多重組織](#orgauthmulti)(Azure AD 或使用與 Azure AD 目錄整合的 AD)
- [在內部部署環境](#orgauthonprem)(AD)

如果您想要嘗試其中一個 Azure AD 的選項，但還沒有帳戶，[按一下這裡以註冊 Azure AD 帳戶](https://go.microsoft.com/fwlink/?LinkId=309942)。

> [!NOTE]
> 如果您選擇其中一個 Azure AD 的選項，您的專案所需的資料庫，而且您有全域系統管理員帳戶登入您的 Azure AD 租用戶。 輸入組織帳戶的名稱和密碼 (例如admin@contoso.onmicrosoft.com) 可讓您的 Azure AD 租用戶的系統管理權限。
> 
> **請勿輸入 Microsoft 帳戶認證 (例如contoso@hotmail.com) 在登入對話方塊中。**


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>雲端-單一組織驗證

![單一組織驗證](creating-web-projects-in-visual-studio/_static/image24.png)

如果您想要啟用的一個 Azure AD 中所定義的使用者帳戶的驗證，請選擇這個選項[租用戶](https://technet.microsoft.com/library/jj573650.aspx)。 例如，網站為 contoso.com，而它將會對可用的 Contoso 公司在 contoso.onmicrosoft.com 租用戶中的員工。 您無法設定 Azure AD，以允許來自其他租用戶存取應用程式的使用者。

#### <a name="domain"></a>Domain

輸入您想要將應用程式設定中，例如 Azure AD 網域： `contoso.onmicrosoft.com`。 如果您有[自訂網域](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/)，例如`contoso.com`而不是`contoso.onmicrosoft.com`，您可以在此處輸入。

#### <a name="access-level"></a>存取層級

如果應用程式需要查詢或使用圖形 API 來更新目錄資訊，請選擇**單一登入，讀取目錄資料**或是**單一登入，讀取和寫入目錄資料**。 否則，請選擇**單一登入**。 如需詳細資訊，請參閱 <<c0> [ 應用程式的存取層級](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels)並[使用 Graph API 查詢 Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx)。

#### <a name="application-id-uri"></a>應用程式識別碼 URI

根據預設，此範本會將專案名稱附加到 Azure AD 網域中建立您的應用程式識別碼 URI。 例如，如果專案名稱`Example`網域，而且`contoso.onmicrosoft.com`，應用程式識別碼 URI 會變成`https://contoso.onmicrosoft.com/Example`。 如果您想要手動指定應用程式識別碼 URI，依序展開**更多選項**區段，並在文字方塊中輸入應用程式識別碼 URI。 應用程式識別碼 URI 必須以開頭`https://`。

根據預設，如果已佈建 Azure AD 中的應用程式有相同的應用程式識別碼 URI 做為其中一個 Visual Studio 使用的專案，專案將會連線到現有的應用程式，而不是佈建一個新。 如果您想要在此情況下佈建的新應用程式，清除**覆寫應用程式項目，如果已經存在一個具有相同識別碼**核取方塊。

如果**覆寫**清除核取方塊，和 Visual Studio 會尋找現有的應用程式使用相同的應用程式識別碼 URI，它會建立新的 URI 將數字附加到它所要使用的 URI。 例如，假設 專案名稱`Example`中，您將文字方塊保留空白，則清除**覆寫**核取方塊，並在 Azure AD 租用戶已有應用程式的 uri `https://contoso.onmicrosoft.com/Example`。 在此情況下，新的應用程式將會佈建與應用程式識別碼 URI 喜歡`https://contoso.onmicrosoft.com/Example_20130619330903`。

#### <a name="provisioning-the-application-in-azure-ad"></a>佈建 Azure AD 中的應用程式

若要佈建 Azure AD 中的應用程式，或將專案連接至現有的應用程式，Visual Studio 會需要網域的全域系統管理員認證。 當您按一下 [ **[確定]** 中**設定驗證**] 對話方塊中，系統會提示您輸入使用者名稱及您所指定之網域的全域系統管理員的密碼。 稍後，當您按一下**建立專案**中**新增 ASP.NET 專案** 對話方塊中，Visual Studio 會佈建 Azure AD 中的應用程式。 請注意，此程序的一部分 Visual Studio 內嵌用戶端祕密值過期之後建立的一年的 Web.config 檔案中。

如需有關如何建立使用的應用程式資訊**雲端-單一組織**驗證，請參閱下列資源：

- [Azure 驗證](../2012/windows-azure-authentication.md)
- [使用 Azure AD Web 應用程式新增登入](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [使用 Azure Active Dirctory 開發 ASP.NET 應用程式](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [保護 ASP.NET Web API 與 Azure AD 與 Microsoft OWIN 元件](https://msdn.microsoft.com/magazine/dn463788.aspx)

教學課程有尚未更新適用於 Visual Studio 2013;部分的教學課程引導您手動執行會自動化 Visual Studio 2013 中。

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>雲端-多重組織驗證

![多個組織驗證](creating-web-projects-in-visual-studio/_static/image25.png)

如果您想要啟用多個 Azure AD 中所定義的使用者帳戶的驗證，請選擇這個選項[租用戶](https://technet.microsoft.com/library/jj573650.aspx)。 例如，網站為 contoso.com，而它將可供在 contoso.onmicrosoft.com 租用戶中，Contoso 公司的員工和員工的 Fabrikam 公司 fabrikam.onmicrosoft.com 租用戶中。

您輸入的設定和佈建步驟的應用程式會類似[單一組織驗證](#orgauthsingle)。

如需有關如何建立使用的應用程式資訊**雲端-多重組織**驗證，請參閱下列資源：

- [簡單 Web 應用程式與 Azure Active Directory 中，ASP.NET 整合&amp;Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) Active Directory 團隊部落格上。
- [開發多租用戶 Web 應用程式與 Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx)教學課程。 本教學課程還未更新的 Visual Studio 2013;某些功能教學課程會引導您手動執行會自動化 Visual Studio 2013 中。
- [您必須註冊您的多個組織 ASP.NET 應用程式之前，您可以登入](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/)。 建立專案，使用多組織驗證時，會遇到 Vittorio bertocci，說明如何解決常見的問題人員的部落格。

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>在內部部署組織驗證

![在內部部署組織驗證](creating-web-projects-in-visual-studio/_static/image26.png)

如果您想要啟用驗證的使用者帳戶會定義在 Windows Server Active Directory (AD)，而且您不想要使用 Azure AD，請選擇這個選項。 若要建立內部網路網站或網際網路網站，您可以使用此選項。 網際網路網站，將 Active Directory Federation Services (ADFS) 提供存取至 AD。 如需詳細資訊，請參閱 < [Visual Studio 2013 中使用內部組織的驗證選項 (ADFS) 使用 ASP.NET](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/)。

對於內部網路網站，或者您可以選擇[Windows 驗證](#winauth)來代替此選項。 Windows 驗證選項適用的您不必提供中繼資料文件 URL。 不過，Windows 驗證並不授予您的能力來控制在 Active Directory 中的應用程式存取，或查詢目錄資料。

#### <a name="on-premises-authority"></a>在內部部署授權單位

輸入 URL 指向中繼資料文件。 中繼資料文件包含的授權單位的座標。 應用程式將使用這些座標來推動 web 登入流程。

#### <a name="application-id-uri"></a>應用程式識別碼 URI

提供 AD 可用來識別此應用程式，或者保留空白，讓 Visual Studio 建立一個唯一的 URI。

<a id="nextsteps"></a>
## <a name="next-steps"></a>後續步驟

本文件提供一些基本的說明 Visual Studio 2013 中建立新的 ASP.NET web 專案。 如需使用 Visual studio web 程式開發的詳細資訊，請參閱[ https://www.asp.net/visual-studio/ ](../../index.md)。
