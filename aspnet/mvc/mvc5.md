---
uid: mvc/mvc5
title: ASP.NET MVC 5 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 5 ASP.NET MVC 5 是建置可擴充、 以標準為基礎的 web 應用程式使用信譽良好的設計模式與強大的 AS.架構...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: 1b3f920b51a70757ec0e20e36fa8e7dc329e663d
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>ASP.NET MVC 5 中最新消息

### <a name="one-aspnet"></a>一個 ASP.NET

Web MVC 專案範本緊密整合新的一個 ASP.NET 體驗中。 您可以自訂 MVC 專案，並使用一個 ASP.NET 專案建立精靈設定驗證。 ASP.NET MVC 5 需入門教學課程，請參閱[開始使用 ASP.NET MVC 5](overview/getting-started/introduction/getting-started.md)。

將 MVC 4 專案升級至 MVC 5 的資訊，請參閱[如何將 ASP.NET MVC 4 和 Web API 專案升級至 ASP.NET MVC 5 和 Web API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)。

### <a name="aspnet-identity"></a>ASP.NET Identity

要用於驗證和身分識別管理 ASP.NET 識別的 MVC 專案範本已更新。 將 Facebook 和 Google 驗證和新的成員資格應用程式開發介面設為特色的教學課程，請參閱[建立 ASP.NET MVC 5 應用程式與 Facebook、 Google OAuth2 和 OpenID 登入](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)和[部署安全的 ASP.NET MVC 應用程式成員資格、 OAuth、 及 Windows Azure 網站的 SQL Database](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。

### <a name="bootstrap"></a>Bootstrap

MVC 專案範本已更新為使用[Bootstrap](http://getbootstrap.com/)提供精緻且回應迅速外觀及操作，您可以輕鬆地自訂。 如需詳細資訊，請參閱[啟動 Visual Studio 2013 的 web 專案範本中的程序](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)。

### <a name="authentication-filters"></a>驗證篩選條件

[驗證篩選條件](http://www.dotnetcurry.com/showarticle.aspx?ID=957)是一種新的 ASP.NET MVC 管線中的授權篩選條件之前執行，並允許您指定驗證邏輯每個動作，ASP.NET MVC 中篩選每個控制器，或全域的所有控制站。 驗證篩選條件處理要求中的認證，並提供對應的主體。 驗證篩選條件也可以將驗證挑戰回應未經授權的要求。 請參閱[ASP.NET MVC 5 驗證篩選條件](http://www.dotnetcurry.com/showarticle.aspx?ID=957)， [ASP.NET MVC 5 中的驗證篩選條件](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/)。

### <a name="filter-overrides"></a>篩選會覆寫

您可以現在覆寫的篩選會套用至指定的動作方法或控制器藉由指定[覆寫篩選](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)。 覆寫篩選條件會指定一組篩選器型別，且不應該執行給定的範圍 （動作或控制器）。 這可讓您設定篩選器，全域套用，但無法套用至特定動作或控制器中排除某些全域篩選器。 請參閱[新篩選器會覆寫功能，在 ASP.NET MVC 5 和 ASP.NET Web API 2](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx)，[如何使用 ASP.NET MVC 5 篩選會覆寫功能](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/)，和[篩選會覆寫在 ASP.NET MVC 5](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>屬性路由

ASP.NET MVC 現在支援[屬性路由](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx)，這點受惠 Tim McCall，作者比重[ http://attributerouting.net ](http://attributerouting.net)。 路由屬性中，您可以指定您的路由加上附註您的動作與控制器。

## <a name="new-web-project-experience"></a>新的 Web 專案經驗

我們有增強 Visual Studio 2013 中建立新的 web 專案的經驗。 在**新的 ASP.NET Web 專案**對話方塊，您可以選取專案類型，設定技術 (Web Forms、 MVC、 Web API) 的任何組合，設定驗證選項，然後加入單元測試專案。

![新增 ASP.NET 專案](mvc5/_static/image1.png)

[新增] 對話方塊可讓您變更許多範本的預設驗證選項。 例如，當您建立的 ASP.NET Web Form 專案時您可以選取任何下列選項：

- 無驗證
- 個別使用者帳戶 （ASP.NET 成員資格或社交提供者登入）
- 組織帳戶 (網際網路應用程式中的 Active Directory)
- Windows 驗證 (內部網路應用程式中的 Active Directory)

![驗證選項](mvc5/_static/image2.png)

如需建立 web 專案的新程序的詳細資訊，請參閱[Visual Studio 2013 中建立 ASP.NET Web 專案](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md)。 如需新的驗證選項的詳細資訊，請參閱[ASP.NET Identity](../identity/overview/index.md)。

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

ASP.NET Scaffolding 是 ASP.NET Web 應用程式的程式碼產生架構。 它可讓您輕鬆地將未定案程式碼加入至您的專案與資料模型互動。

在舊版的 Visual Studio 中，scaffolding 限於 ASP.NET MVC 專案。 使用 Visual Studio 2013，您現在可以針對任何 ASP.NET 專案，包括 Web Form 使用 scaffolding。 Visual Studio 2013 目前不支援產生頁面的 Web Form 專案，但您仍然可以使用與 Web Form scaffolding，將 MVC 相依性加入至專案。 在未來的更新中，將會加入產生的 Web Form 網頁的支援。

當使用 scaffolding，我們可以確定所有必要的相依性安裝到專案。 比方說，如果您從 ASP.NET Web Form 專案開始，然後使用 scaffolding 新增 Web API 控制器所需的 NuGet 封裝和參考會加入至您的專案會自動。

MVC 樣板加入至 Web Form 專案，加入**新的 Scaffold 項目**選取**MVC 5 相依性**對話視窗中。 有兩個選項的 scaffolding MVC;最少且完整。 如果您選取最少，只有 NuGet 封裝和 ASP.NET MVC 的參考會加入至您的專案。 如果您選取 [完整] 選項，加入最少的相依性，以及所需的內容檔案的 MVC 專案。

支援的 scaffolding 非同步控制器會使用新的非同步功能，從 Entity Framework 6。

如需詳細資訊和教學課程，請參閱[ASP.NET Scaffolding 概觀](../visual-studio/overview/2013/aspnet-scaffolding-overview.md)。

### <a name="getting-help-and-reporting-issues"></a>取得說明和報告問題

- [已知的問題和重大變更清單](../visual-studio/overview/2013/release-notes.md#knownissues)
- 如需協助，並討論在 ASP.NET MVC 5[論壇](https://forums.asp.net/1146.aspx)
- [在 ASP.NET MVC 5 錯誤報告](https://github.com/aspnet/AspNetWebStack/issues)
- [提出功能要求](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrading-from-aspnet-mvc-4"></a>從 ASP.NET MVC 4 升級

請參閱[如何升級 ASP.NET MVC 4 和 Web API 專案，以 ASP.NET MVC 5 和 Web API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
