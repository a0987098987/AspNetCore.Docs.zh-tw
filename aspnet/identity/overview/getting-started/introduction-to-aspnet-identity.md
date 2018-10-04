---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: ASP.NET Identity 簡介 |Microsoft Docs
author: jongalloway
description: ASP.NET 成員資格系統被引進與 ASP.NET 2.0 後，在 2005 中，而由於然後中已有許多的變更方式的 web 應用程式通常...
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 1938de2b57c8fafa7ea8a656c0a42d2d3f1a6c81
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577869"
---
<a name="introduction-to-aspnet-identity"></a>ASP.NET Identity 簡介
====================
藉由[Jon Galloway](https://github.com/jongalloway)，[請參閱 Pranav Rastogi](https://github.com/rustd)， [Rick Anderson]((https://twitter.com/RickAndMSFT))， [Tom Dykstra](https://github.com/tdykstra)

> ASP.NET 成員資格系統被引進 ASP.NET 2.0 後在 2005 中，而由於則已有許多變更 web 應用程式通常會處理驗證和授權方式。 ASP.NET 身分識別是成員資格系統應該是什麼當您在建立 web、 手機或平板電腦的現代化應用程式的全新觀點。
> 
> 請參閱 Pranav rastogi 撰寫本文時 ([@rustd](https://twitter.com/rustd))，Jon Galloway ([@jongalloway](https://twitter.com/jongalloway))，Tom Dykstra 和 Rick Anderson ([ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。


## <a name="background-membership-in-aspnet"></a>背景： 在 ASP.NET 中的成員資格

### <a name="aspnet-membership"></a>ASP.NET 成員資格

[ASP.NET 成員資格](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx)用來解決常用的 2005，因為它牽涉到表單驗證，以及使用者名稱、 密碼和設定檔資料的 SQL Server 資料庫的站台成員資格需求。 目前沒有更廣泛的 web 應用程式，資料儲存體選項，而且大部分的開發人員想要啟用自己的網站使用社交識別提供者驗證和授權功能。 ASP.NET 成員資格的設計限制進行這項轉換不容易：

- 資料庫結構描述專為 SQL Server，您無法變更它。 您可以新增設定檔資訊，但其他的資料封裝至不同的資料表，很難存取的設定檔提供者 API 透過任何方式除外。
- 提供者系統可讓您變更 支援的資料存放區，但系統針對假設適用於關聯式資料庫設計。 您可以撰寫的提供者，將成員資格資訊儲存在非關聯式儲存體機制，例如 Azure 儲存體資料表，但是，您必須解決關聯式的設計，撰寫了大批程式碼和許多`System.NotImplementedException`不的方法的例外狀況適用於 NoSQL 資料庫。
- 由於記錄輸入/記錄檔外的功能根據表單驗證，不能使用成員資格系統[OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)。 OWIN 包含中介軟體元件進行驗證，包括支援登入使用外部識別提供者 （例如 Microsoft 帳戶、 Facebook、 Google、 Twitter)，並登入使用組織帳戶，從內部部署 Active Directory 或Azure Active Directory。 OWIN 也包含對 OAuth 2.0 JWT，CORS 支援。

### <a name="aspnet-simple-membership"></a>ASP.NET 簡易成員資格

[ASP.NET 簡易成員資格](../../../web-pages/overview/security/16-adding-security-and-membership.md)ASP.NET Web Pages 做為成員資格系統所開發。 使用 WebMatrix 和 Visual Studio 2010 SP1 釋出。 簡單成員資格的目標是要讓您輕鬆將成員資格功能加入網頁應用程式。

簡單成員資格未讓您更輕鬆地自訂使用者設定檔資訊，但它仍共用其他問題與 ASP.NET 成員資格，而且具有一些限制：

- 真的很難保存非關聯式存放區中的成員資格系統資料。
- 您無法搭配 OWIN 使用它。
- 它不適用於現有的 ASP.NET 成員資格提供者，而且無法擴充。

### <a name="aspnet-universal-providers"></a>ASP.NET Universal Providers

[ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx)所開發，讓保存 Azure SQL Database，而且它們也適用於 SQL Server Compact 在 Microsoft 中的成員資格資訊。 通用的提供者已根據 Entity Framework Code First，這表示通用提供者可用來保存任何 EF 所支援的存放區中的資料。 通用提供者中，資料庫結構描述已清除也頗大的數目。

Universal Providers ASP.NET 成員資格基礎結構之上，因此它們仍會帶 SqlMembership 提供者相同的限制。 也就是說，會依照針對關聯式資料庫設計，而且很難自訂設定檔和使用者的資訊。 這些提供者也依然使用表單驗證登入和登出的功能。

## <a name="aspnet-identity"></a>ASP.NET Identity

成員資格的方式在 ASP.NET 中的劇本已發展多年來，來自客戶的意見回應的 「 ASP.NET 團隊有獲益良多。

使用者可以輸入使用者名稱和已註冊您自己的應用程式中的密碼登入的假設不再有效。 Web 已經成為其他社交。 使用者透過社交管道，例如 Facebook、 Twitter 和其他社交網站的即時與彼此互動。 開發人員想要讓使用者能夠登入其社交身分識別，以便它們可以在其於網站上有豐富的體驗。 現代的成員資格系統必須啟用重新導向以登入來驗證提供者，例如 Facebook、 Twitter 和其他項目。

為 web 開發演變，因此未 web 開發的模式。 單元測試的應用程式程式碼成為應用程式開發人員的核心考量。 在 2008 ASP.NET 會新增模型-檢視-控制器 (MVC) 模式，部分可協助開發人員建置單元測試的 ASP.NET 應用程式為基礎的新架構。 開發人員需要單元測試應用程式邏輯也希望能利用成員資格系統。

考慮這些變更的 web 應用程式開發中，ASP.NET 身分識別開發人員在開發下列目標：

- **一個 ASP.NET 身分識別系統**

    - 可以與所有的 ASP.NET 架構，例如 ASP.NET MVC、 Web Form、 Web 頁面、 Web API，以及 SignalR 使用 ASP.NET 身分識別。
    - 當您在建立 web、 電話、 存放區或混合式應用程式，可以使用 ASP.NET 身分識別。
- **簡易的插入使用者相關的設定檔資料**

    - 您可以控制使用者與設定檔資訊的結構描述。 比方說，您可以輕鬆地啟用系統所儲存的應用程式中註冊帳戶時，輸入使用者的出生日期。

- **持續性控制項**

    - 根據預設，ASP.NET 身分識別系統會將所有的使用者資訊儲存在資料庫中。 ASP.NET 身分識別會使用 Entity Framework Code First 來實作所有持續性機制。
    - 由於您控制的資料庫結構描述、 常見的工作，例如變更資料表名稱或變更主索引鍵的資料型別很容易做到。
    - 很容易插入不同的儲存機制，例如 SharePoint、 Azure 儲存體表格服務、 NoSQL 資料庫 」 等，而不需擲回`System.NotImplementedExceptions`例外狀況。
- **單元測試**

    - ASP.NET 身分識別可讓 web 應用程式更多單元測試。 您可以撰寫單元測試，您的應用程式使用 ASP.NET 身分識別的組件。
- **角色提供者**

    - 沒有角色提供者可讓您限制對您的應用程式的組件的角色存取。 您可以輕鬆地建立角色，例如"Admin"，並將使用者新增至角色。
- **宣告式**

    - ASP.NET 身分識別支援宣告式的驗證，而使用者的身分識別都代表一組宣告。 宣告讓開發人員可在描述使用者的身分識別，比角色，可讓更多易懂。 角色成員資格是只是布林值 （成員或非成員），而宣告可以包含使用者的身分識別與成員資格的豐富資訊。
- **社交登入提供者**

    - 可以輕鬆地將社交登入等的 Microsoft 帳戶、 Facebook、 Twitter、 Google 和其他人新增至您的應用程式，並將使用者專屬資料儲存在您的應用程式。
- **Azure Active Directory**

    - 您也可以加入使用 Azure Active Directory 的登入功能，並將使用者專屬資料儲存在您的應用程式。 如需詳細資訊，請參閱 <<c0> [ 組織帳戶](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth)在 Visual Studio 2013 中建立 ASP.NET Web 專案
- **OWIN 整合**

    - ASP.NET 驗證現在根據可用在以 OWIN 為基礎的任何主機的 OWIN 中介軟體。 在 System.Web，ASP.NET 身分識別並沒有任何相依性。 它是完全相容的 OWIN 架構，並可以用於任何裝載的 OWIN 應用程式。
    - ASP.NET Identity 中/記錄檔-登出的使用者在網站上使用 OWIN 驗證。 這表示，而不是使用 FormsAuthentication 產生 cookie，應用程式使用 OWIN CookieAuthentication 若要這麼做。
- **NuGet 套件**

    - ASP.NET 身分識別，則會重新分配以 NuGet 套件安裝隨附於 Visual Studio 2013 的 ASP.NET MVC、 Web Form 和 Web API 範本中。 您可以從 NuGet 資源庫下載這個 NuGet 封裝。
    - 釋出 ASP.NET 身分識別為 NuGet 套件可讓 ASP.NET 小組的新功能及 bug 修正，逐一查看，並以敏捷的方式提供這些開發人員更輕鬆。

## <a name="getting-started-with-aspnet-identity"></a>開始使用 ASP.NET Identity

ASP.NET 身分識別是在 Visual Studio 2013 專案範本中用於 ASP.NET MVC、 Web Form、 Web API 和 SPA。 在本逐步解說中，我們將說明的專案範本如何使用 ASP.NET 身分識別來將功能新增至註冊、 登入和登出使用者。

使用下列程序來實作 ASP.NET 身分識別。 本文的目的是讓您的 ASP.NET 身分識別; 的高階概觀您可以遵循逐步或只是讀取的詳細資料。 如需建立使用 ASP.NET 身分識別，包括新增使用者、 角色和設定檔資訊，請使用新的 API 應用程式的詳細指示，請參閱本文結尾 「 後續步驟 」 一節。

1. 建立 ASP.NET MVC 應用程式使用個別的帳戶。 您可以使用 ASP.NET MVC、 Web Form、 Web API 等等 SignalR 的 ASP.NET 身分識別。在本文中，我們將開始使用 ASP.NET MVC 應用程式。  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. 建立的專案會包含下列三個封裝，ASP.NET 身分識別。

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
   此套件有 Entity Framework 實作會將 ASP.NET Identity 的資料和 SQL server 的結構描述保存 ASP.NET 身分識別。
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
   此套件有 ASP.NET Identity 的核心介面。 此套件可用來撰寫 ASP.NET 身分識別的目標不同的持續性儲存例如 Azure 資料表儲存體，NoSQL 資料庫等實作。
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
   此套件包含用來在 ASP.NET 應用程式中插入 OWIN 驗證中使用 ASP.NET Identity 的功能。 您將記錄功能新增至您的應用程式和呼叫 OWIN 的 Cookie 驗證中介軟體以產生 cookie 時，會使用這項目。
3. 建立使用者。  
   啟動應用程式，然後按一下**註冊**連結，以建立使用者。 下圖顯示 [註冊] 頁面會收集使用者名稱和密碼。  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
   當使用者按一下**註冊** 按鈕，`Register`帳戶控制器的動作會建立使用者，方法是呼叫 ASP.NET 身分識別 API，以反白顯示如下：

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. 登入。  
   如果已成功建立使用者，她藉由登入`SignInAsync`方法。  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample2.cs?highlight=12)]

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample3.cs?highlight=5-6)]

   在 反白顯示上述程式碼`SignInAsync`方法會產生[ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx)。 由於 ASP.NET 身分識別和 OWIN Cookie 驗證以宣告為基礎的系統，framework 就會需要應用程式來產生使用者的 ClaimsIdentity。 ClaimsIdentity 有使用者，例如使用者屬於何種角色的所有宣告的相關資訊。 您也可以在這個階段中新增更多的使用者宣告。  
  
   在反白顯示下列程式碼`SignInAsync`方法的使用者會使用登入從 OWIN 及呼叫 AuthenticationManager`SignIn`並傳入 ClaimsIdentity。  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample4.cs?highlight=8-11)]
5. 登出。  
   按一下 **登出**連結呼叫登出動作帳戶控制器中。 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

   反白顯示上述的程式碼顯示 OWIN`AuthenticationManager.SignOut`方法。 這相當於[FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx)所使用的方法[FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)在 Web Form 中的模組。

## <a name="components-of-aspnet-identity"></a>ASP.NET 身分識別的元件

下圖顯示 ASP.NET 身分識別系統的元件 (按一下[這](introduction-to-aspnet-identity/_static/image3.png)或將它放大圖表上)。 以綠色的封裝是由 ASP.NET 身分識別系統所組成。 所有其他套件是使用 ASP.NET 身分識別系統在 ASP.NET 應用程式時所需的相依性。

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

以下是先前未提及的 NuGet 套件的簡短描述：

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 中介軟體，可讓應用程式使用 cookie 型驗證，類似於 ASP。NET 的表單驗證。
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework 是 Microsoft 的關聯式資料庫的建議的資料存取技術。

## <a name="migrating-from-membership-to-aspnet-identity"></a>從成員資格移轉至 ASP.NET Identity

我們希望能很快就提供移轉您現有的應用程式到新的 ASP.NET 身分識別系統中使用 ASP.NET 成員資格或簡單的成員資格的指引。

## <a name="next-steps"></a>後續步驟

- [使用 Facebook 和 Google OAuth2 和 OpenID 登入建立 ASP.NET MVC 5 應用程式](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 教學課程會使用 ASP.NET 身分識別 API，以新增至使用者資料庫中，設定檔資訊及如何使用 Google 和 Facebook 進行驗證。
- [使用驗證和 SQL DB 建立 ASP.NET MVC 應用程式並部署至 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 本教學課程會示範如何使用身分識別 API 來新增使用者和角色。
- [個別使用者帳戶](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#indauth)ASP.NET Web 專案在 Visual Studio 2013 中建立
- [組織帳戶](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth)ASP.NET Web 專案在 Visual Studio 2013 中建立
- [在 ASP.NET 身分識別自訂的設定檔資訊，在 VS 2013 範本](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)
- [從 VS 2013 專案範本中所使用的社交提供者取得詳細資訊](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 示範如何新增基本的角色和使用者支援，以及如何進行角色和使用者管理的範例應用程式。
