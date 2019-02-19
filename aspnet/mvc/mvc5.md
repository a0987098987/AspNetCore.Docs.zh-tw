---
uid: mvc/mvc5
title: ASP.NET MVC 5 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 5 的 ASP.NET MVC 5 是一個來建置可調整、 以標準為基礎的 web 應用程式，使用堅實的設計模式和威力 AS....
ms.author: riande
ms.date: 10/11/2018
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: 14fcf863a4ef5f6c9180cdf9e7b632ccdb1ebcb0
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410465"
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>什麼是 ASP.NET MVC 5 的新功能

### <a name="one-aspnet"></a>One ASP.NET

Web MVC 專案範本順暢地與體驗整合在 One ASP.NET。 您可以自訂您的 MVC 專案，並使用 [One ASP.NET 專案建立精靈] 設定驗證。 ASP.NET MVC 5 入門教學課程，請參閱[Getting Started with ASP.NET MVC 5](overview/getting-started/introduction/getting-started.md)。

將 MVC 4 專案升級至 MVC 5 的資訊，請參閱[如何將 ASP.NET MVC 4 和 Web API 專案升級至 ASP.NET MVC 5 和 Web API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)。

### <a name="aspnet-identity"></a>ASP.NET Identity

若要使用 ASP.NET 身分識別進行驗證和身分識別管理 MVC 專案範本已更新。 Facebook 和 Google 驗證以及新的成員資格 API 的教學課程，請參閱[建立 ASP.NET MVC 5 應用程式使用 Facebook 和 Google OAuth2 和 OpenID 登入](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)和[部署使用的安全 ASP.NET MVC 應用程式成員資格、 OAuth 和 SQL 資料庫至 Windows Azure 網站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。

### <a name="bootstrap"></a>啟動程序

MVC 專案範本已更新為使用[Bootstrap](http://getbootstrap.com/)提供簡潔且回應迅速的外觀和操作可以輕易自訂。 如需詳細資訊，請參閱 < [Visual Studio web 專案範本中的 Bootstrap](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)。

### <a name="authentication-filters"></a>驗證篩選條件

[驗證篩選條件](http://www.dotnetcurry.com/showarticle.aspx?ID=957)是一種新的 ASP.NET MVC 管線中的授權篩選條件之前執行，並可讓您指定驗證邏輯每個動作，ASP.NET MVC 中的篩選每個控制器，或全域的所有控制站。 驗證篩選條件會處理在要求中的認證，並提供對應的主體。 驗證篩選條件也可以加入驗證挑戰回應未經授權的要求。 請參閱[ASP.NET MVC 5 的驗證篩選](http://www.dotnetcurry.com/showarticle.aspx?ID=957)， [ASP.NET MVC 5 中的驗證篩選](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/)。

### <a name="filter-overrides"></a>篩選覆寫

您現在可以覆寫所指定的篩選會套用至指定的動作方法或控制器[覆寫篩選](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)。 覆寫篩選器指定一組不應該執行給定的範圍 （動作或控制器） 的篩選類型。 這可讓您設定適用於全球，但再排除特定的全域篩選器套用至特定動作或控制器的篩選條件。 請參閱[ASP.NET MVC 5 和 ASP.NET Web API 2 中的新篩選覆寫功能](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx)，[如何使用 ASP.NET MVC 5 篩選器會覆寫功能](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/)，和[篩選覆寫 ASP.NET MVC 5](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>屬性路由

ASP.NET MVC 現在支援[屬性路由](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx)，由 Tim McCall，著作貢獻感謝[AttributeRouting](https://github.com/mccalltd/AttributeRouting)。 使用屬性路由中，您可以指定您的路由動作和控制器加上附註。

## <a name="new-web-project-experience"></a>新的 Web 專案體驗

Visual Studio 中增強啟動 Visual Studio 2013 中建立新的 web 專案的體驗。 在 **新的 ASP.NET Web 專案**對話方塊，您可以選取專案類型、 設定技術 (Web Form、 MVC、 Web API) 的任何組合，設定驗證選項、 新增 Docker 支援，並將單元測試專案。

![新增 ASP.NET 專案](mvc5/_static/new-aspnet-web-app-dialog.png)

對話方塊可讓您變更許多範本的預設驗證選項。 比方說，當建立 ASP.NET Web Form 專案時，您可以選取下列任何選項：

- 沒有驗證
- 個別使用者帳戶 （ASP.NET 成員資格或社交提供者登入）
- 工作或學校帳戶 (Active Directory 中的網際網路應用程式)
- Windows 驗證 (內部網路應用程式中的 Active Directory)

![驗證選項](mvc5/_static/change-authentication-dialog.png)

如需有關建立 web 專案的程序的詳細資訊，請參閱 <<c0> [ 在 Visual Studio 中建立 ASP.NET Web 專案](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md)。 如需詳細的驗證選項的詳細資訊，請參閱[ASP.NET Identity](../identity/overview/index.md)。

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

ASP.NET Scaffold 是 ASP.NET Web 應用程式的程式碼產生架構。 它可讓您輕鬆地將未定案程式碼新增至您的資料模型進行互動的專案。

在 Visual studio 2013 之前的版本，scaffolding 已限制為 ASP.NET MVC 專案。 從 Visual Studio 2013 開始，您可以針對任何 ASP.NET 專案，包括 Web Form 使用 scaffolding。 Visual Studio 目前不支援產生的頁面針對 Web Form 專案，但您仍然可以使用與 Web Forms scaffolding，將 MVC 相依性新增至專案。 未來版本中，將會新增支援產生 Web Form 頁面。

當使用 scaffolding，所有必要的相依性安裝到專案。 比方說，如果您開始使用 ASP.NET Web Form 專案，然後使用 scaffolding Web API 控制器，將必要的 NuGet 套件和參考會加入至您的專案會自動。

若要加入 Web Form 專案中的 MVC scaffolding，新增**新的 Scaffold 項目**，然後選取**MVC 5 相依性**對話視窗中。 有兩個選項，scaffolding MVC;**基本相依性**並**完整相依性**。 如果您選取**基本相依性**，只有 NuGet 套件和 ASP.NET mvc 的參考加入至專案。 如果您選取**完整相依性**，最少的相依性會新增 MVC 專案所需的內容檔案以及。

![在 Visual Studio 中 [新增 Scaffold] 對話方塊](overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-scaffold.png)

支援 scaffolding 非同步控制器會使用 Entity Framework 6 的非同步功能。

如需詳細資訊和教學課程，請參閱 < [ASP.NET Scaffolding 概觀](../visual-studio/overview/2013/aspnet-scaffolding-overview.md)。

### <a name="get-help-and-report-issues"></a>取得協助與回報問題

- [已知的問題和重大變更清單](../visual-studio/overview/2013/release-notes.md#knownissues)
- 取得說明並討論在 ASP.NET MVC 5[論壇](https://forums.asp.net/1146.aspx)
- [報告 bug，以在 ASP.NET MVC 5](https://github.com/aspnet/AspNetWebStack/issues)
- [提出功能要求](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrade-from-aspnet-mvc-4"></a>從 ASP.NET MVC 4 升級

請參閱[如何升級 ASP.NET MVC 4 和 Web API 專案，以 ASP.NET MVC 5 和 Web API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
