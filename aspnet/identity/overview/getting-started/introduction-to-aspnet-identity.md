---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: "ASP.NET Identity 簡介 |Microsoft 文件"
author: jongalloway
description: "ASP.NET 成員資格系統中引入 ASP.NET 2.0 後在 2005 中，而由於然後中已有許多變更方法的 web 應用程式 typicall..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 7c7dcb7903b0d0772acc560161ff39c6869c599a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-aspnet-identity"></a>ASP.NET 識別簡介
====================
由[Jon Galloway](https://github.com/jongalloway)， [Pranav Rastogi](https://github.com/rustd)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

> ASP.NET 成員資格系統被引進 ASP.NET 2.0 後在 2005 中，而由於然後中已有許多變更 web 應用程式通常會處理驗證和授權的方式。 ASP.NET Identity 是全新查看成員資格系統應該是什麼當您在建立 web、 電話或平板電腦的現代應用程式。
> 
> 這篇文章 Pranav Rastogi 所撰寫 ([@rustd](https://twitter.com/rustd))，Jon Galloway ([@jongalloway](https://twitter.com/jongalloway))，Tom Dykstra 和 Rick Anderson ([ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。


## <a name="background-membership-in-aspnet"></a>背景： 在 ASP.NET 中的成員資格

### <a name="aspnet-membership"></a>ASP.NET 成員資格

[ASP.NET 成員資格](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx)用來解決常見 2005，涉及表單驗證，以及使用者名稱、 密碼及分析資料的 SQL Server 資料庫中的站台的成員資格需求。 目前沒有更廣泛陣列的 web 應用程式資料儲存選項，而且大部分的開發人員想要啟用其網站社交身分識別提供者用於驗證和授權功能。 ASP.NET 成員資格設計的限制進行這項轉換困難：

- 針對 SQL Server 資料庫結構描述而設計，您無法變更它。 您可以新增設定檔資訊，但其他的資料壓縮成不同的資料表，不容易存取透過任何方式除了透過設定檔提供者 API。
- 提供者系統可讓您變更支援的資料存放區，但系統針對假設適用於關聯式資料庫設計。 您可以撰寫提供者來儲存非關聯式儲存體機制，例如 Azure 儲存體資料表中的成員資格資訊，但是您需要撰寫大量程式碼和許多來解決關聯式設計`System.NotImplementedException`例外狀況之不方法套用到 NoSQL 資料庫。
- 由於記錄輸入/記錄檔超出功能根據表單驗證，無法使用成員資格系統[OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)。 OWIN 包含中介軟體元件進行驗證，包括支援的登入使用外部識別提供者 （例如 Microsoft 帳戶、 Facebook、 Google、 Twitter)，並記錄集使用組織帳戶從內部部署 Active Directory 或Azure Active Directory。 OWIN 也包含支援 OAuth 2.0 JWT，CORS。

### <a name="aspnet-simple-membership"></a>ASP.NET 簡易成員資格

[ASP.NET 簡易成員資格](../../../web-pages/overview/security/16-adding-security-and-membership.md)的 ASP.NET Web Pages 做為成員資格系統所開發。 WebMatrix 和 Visual Studio 2010 SP1 一起發行它。 簡單成員資格的目的是為了方便網頁應用程式中加入成員資格功能。

簡單成員資格未更輕鬆地自訂使用者設定檔資訊，但它仍會共用的其他問題與 ASP.NET 成員資格，而且有一些限制：

- 您很難保存非關聯式存放區中的成員資格系統資料。
- 您無法使用它與 OWIN。
- 它不適用於現有的 ASP.NET 成員資格提供者，而且無法擴充。

### <a name="aspnet-universal-providers"></a>ASP.NET Universal Providers

[ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx)使保存成員資格資訊。 在 Microsoft Azure SQL Database，而且它們也與搭配使用 SQL Server Compact 所開發。 通用的提供者已根據 Entity Framework Code First，這表示通用的提供者可用於保存 EF 所支援的任何存放區中的資料。 通用的提供者，很多也已清除的資料庫結構描述。

通用的提供者已內建的 ASP.NET 成員資格基礎，因此它們仍會執行 SqlMembership 提供者相同的限制。 也就是說，它們的關聯式資料庫設計，並且很難自訂設定檔和使用者資訊。 這些提供者也依然可以使用表單驗證登入和登出的功能。

## <a name="aspnet-identity"></a>ASP.NET Identity

與成員資格中 ASP.NET 的劇本發展多年來，從來自客戶的意見反應的 「 ASP.NET 團隊已學到許多。

使用者可以輸入使用者名稱及它們自己的應用程式中註冊了密碼登入的假設不再有效。 網站變得更社交。 使用者透過社交通道，例如 Facebook、 Twitter 和其他社交網站即時與彼此互動。 開發人員想要讓使用者能夠登入及其社交身分識別，以便它們可以在他們的網站上有豐富的體驗。 現代的成員資格系統必須啟用重新導向式記錄集驗證提供者，例如 Facebook、 Twitter 和其他項目。

為 web 程式開發演變，因此未 web 程式開發的模式。 單元測試應用程式程式碼變成應用程式開發人員核心考量。 在 2008年中 ASP.NET 中已加入新的架構，根據模型檢視控制器 (MVC) 模式中，部分可協助開發人員建立單元測試的 ASP.NET 應用程式。 開發人員想要單元測試他們也想要能夠這樣做，成員資格系統的應用程式邏輯。

這些 web 應用程式開發中的變更，請考慮 ASP.NET Identity 被開發以下列目標：

- **一個 ASP.NET 識別系統**

    - ASP.NET Identity 可以與所有 ASP.NET 架構，例如 ASP.NET MVC、 Web Form、 網頁、 Web API 和 SignalR 一起都使用。
    - 當您在建立 web、 電話、 存放區或混合式應用程式，可以使用 ASP.NET Identity。
- **簡易的插入有關使用者的設定檔資料**

    - 您可以控制使用者和設定檔資訊的結構描述。 比方說，您可以輕鬆地啟用系統上，以儲存他們註冊應用程式中的帳戶時，使用者所輸入的出生日期。

- **持續性控制項**

    - 根據預設，ASP.NET Identity 系統會將所有使用者資訊都儲存在資料庫中。 ASP.NET Identity 使用 Entity Framework Code First 實作所有持續性機制。
    - 由於您控制的資料庫結構描述、 一般工作，例如變更資料表名稱，或變更主索引鍵的資料型別很容易做到。
    - 所以可以輕鬆地插入不同的儲存機制，例如 SharePoint、 Azure 儲存體資料表服務、 NoSQL 資料庫等等，而不需擲回`System.NotImplementedExceptions`例外狀況。
- **單元測試能力**

    - ASP.NET 識別可讓 web 應用程式更多的單元測試。 您可以撰寫單元測試您的應用程式使用 ASP.NET 識別的組件。
- **角色提供者**

    - 沒有可讓您限制存取部分應用程式角色的角色提供者。 您可以輕鬆地建立角色，例如"Admin"，並將使用者新增至角色。
- **宣告式**

    - ASP.NET Identity 支援宣告式的驗證，而使用者的身分識別都代表一組宣告。 宣告可讓開發人員會在描述使用者的身分識別，比角色允許更加易懂。 角色成員資格是只布林值 （成員或非成員），而宣告可以包含大量使用者的身分識別和成員資格有關的資訊。
- **社交登入提供者**

    - 您可以輕鬆加入應用程式，例如 Microsoft 帳戶、 Facebook、 Twitter、 Google、 與其他人的社交記錄集和應用程式中儲存使用者專屬資料。
- **Azure Active Directory**

    - 您也可以加入登入功能使用 Azure Active Directory，和應用程式中儲存使用者專屬資料。 如需詳細資訊，請參閱[組織帳戶](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth)Visual Studio 2013 中建立 ASP.NET Web 專案中
- **OWIN 整合**

    - ASP.NET 驗證現在會根據可用在任何 OWIN 架構的主機的 OWIN 中介軟體。 ASP.NET Identity System.Web 上沒有任何相依性。 它是完全相容的 OWIN 架構，並可用於任何裝載的 OWIN 應用程式。
    - ASP.NET Identity OWIN 驗證使用中/記錄檔-登出的使用者在網站上。 這表示除了使用 FormsAuthentication 產生 cookie，應用程式使用 OWIN CookieAuthentication 執行此作業。
- **NuGet 封裝**

    - ASP.NET Identity 會以 NuGet 套件安裝 ASP.NET MVC、 Web Form 和 Web API 範本隨附於 Visual Studio 2013 中重新發佈。 您可以在 NuGet gallery 下載此 NuGet 封裝。
    - 釋放 ASP.NET 識別為 NuGet 封裝可讓 ASP.NET 團隊逐一查看新功能和 bug 修正，並以敏捷的方式傳遞這些開發人員更容易。

## <a name="getting-started-with-aspnet-identity"></a>開始使用 ASP.NET Identity

ASP.NET Identity 在 Visual Studio 2013 專案範本用於 ASP.NET MVC、 Web Form、 Web API 和 SPA。 在本逐步解說中，我們將會說明如何專案範本使用 ASP.NET Identity 中加入功能，以註冊、 登入和登出使用者。

ASP.NET Identity 是使用下列程序來實作。 本文的目的是要讓您的 ASP.NET Identity; 高層級概觀您可以接著依序逐步解說，或只閱讀詳細資料。 如需有關使用 ASP.NET Identity，包括新增使用者、 角色和設定檔資訊，請使用新的應用程式開發介面建立應用程式的詳細指示，請參閱本文結尾處的後續步驟 > 一節。

1. 建立 ASP.NET MVC 應用程式使用個別的帳戶。 您可以使用 ASP.NET Identity 中 ASP.NET MVC、 Web Form、 Web API SignalR 等等。本文章中我們會啟動 ASP.NET MVC 應用程式。  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. 建立的專案包含 ASP.NET Identity 的下列三個封裝。

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
 此套件有 ASP.NET 識別，它將會保存到 SQL Server 結構描述與 ASP.NET 識別資料的實體架構實作。
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
 此套件具有 ASP.NET Identity 的核心介面。 此封裝可以用來寫入 ASP.NET 識別的目標的其他持續性存放區，例如 Azure 資料表儲存體，NoSQL 資料庫等實作。
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
 此套件包含用來插入 OWIN 驗證中使用 ASP.NET Identity 在 ASP.NET 應用程式的功能。 當您將記錄功能加入至應用程式和產生 cookie OWIN 的 Cookie 驗證中介軟體呼叫，會使用這項目。
3. 建立使用者。  
 啟動應用程式，然後按一下**註冊**連結，以建立使用者。 下圖顯示 [註冊] 頁面會收集使用者名稱和密碼。  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
 當使用者按一下**註冊** 按鈕，`Register`帳戶控制器的動作會建立使用者藉由呼叫 ASP.NET 識別的 API，以反白顯示，如下：

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. 登入。  
 如果使用者已成功建立，她中會記錄由`SignInAsync`方法。  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample2.cs?highlight=12)]

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample3.cs?highlight=5-6)]

 在上方的反白顯示程式碼`SignInAsync`方法會產生[ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx)。 因為 ASP.NET Identity 與 OWIN 的 Cookie 驗證以宣告為基礎的系統，架構就會需要應用程式，以產生使用者的 ClaimsIdentity。 身分識別的使用者，例如使用者所屬的角色的所有宣告資訊。 您也可以在這個階段中加入更多的使用者宣告。  
  
 在 反白顯示下列程式碼`SignInAsync`方法登入使用者呼叫與 OWIN AuthenticationManager`SignIn`並傳入 ClaimsIdentity。  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample4.cs?highlight=8-11)]
5. 登出。  
 按一下**登出**連結帳戶控制器中呼叫登出動作。 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

 反白顯示程式碼顯示上述 OWIN`AuthenticationManager.SignOut`方法。 這是類似於[FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx)所使用的方法[FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) Web Form 中的模組。

## <a name="components-of-aspnet-identity"></a>ASP.NET 識別的元件

下圖顯示 ASP.NET 識別系統的元件 (按一下[這](introduction-to-aspnet-identity/_static/image3.png)或將它放大圖表上)。 以綠色的封裝便會產生 ASP.NET 識別系統。 所有其他封裝都要使用 ASP.NET 識別系統在 ASP.NET 應用程式所需的相依性。

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

以下是先前未提及的 NuGet 套件的簡短描述：

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 中介軟體，可讓應用程式使用 cookie 型驗證，類似於 ASP。網路的表單驗證。
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework 是 Microsoft 的建議的資料存取技術，用於關聯式資料庫。

## <a name="migrating-from-membership-to-aspnet-identity"></a>從成員資格移轉至 ASP.NET Identity

我們希望於即將提供的指引移轉到新的 ASP.NET 識別系統中使用 ASP.NET 成員資格或簡單成員資格的現有應用程式。

## <a name="next-steps"></a>後續步驟

- [建立 ASP.NET MVC 5 應用程式與 Facebook、 Google OAuth2 和 OpenID 登入](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 教學課程會使用 ASP.NET 識別應用程式開發介面來新增至使用者資料庫中，設定檔資訊及如何使用 Google 和 Facebook 驗證。
- [驗證與 SQL 資料庫中建立 ASP.NET MVC 應用程式並部署至 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 本教學課程會示範如何使用身分識別應用程式開發介面加入使用者和角色。
- [個別使用者帳戶](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#indauth)ASP.NET Web 專案在 Visual Studio 2013 中建立
- [組織帳戶](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth)ASP.NET Web 專案在 Visual Studio 2013 中建立
- [自訂設定檔資訊在 ASP.NET Identity 中在 VS 2013 範本](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)
- [從 VS 2013 專案範本中所使用的社交提供者取得詳細資訊](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 示範如何新增基本角色和使用者的支援，以及如何進行角色和使用者管理的範例應用程式。
