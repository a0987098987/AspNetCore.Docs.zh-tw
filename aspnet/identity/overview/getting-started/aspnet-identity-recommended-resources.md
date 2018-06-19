---
uid: identity/overview/getting-started/aspnet-identity-recommended-resources
title: ASP.NET Identity 建議資源 |Microsoft 文件
author: Rick-Anderson
description: 本主題提供有關如何使用 ASP.NET Identity 的文件資源的連結。 如果您知道的絕佳的部落格文章、 stackoverflow 執行緒或任何其他連結...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2015
ms.topic: article
ms.assetid: 0f78aec2-f509-46fa-b20f-d5208425d8ec
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-recommended-resources
msc.type: authoredcontent
ms.openlocfilehash: f2e1693a32fce6956ddb1e095e6f208b9f0faab6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876068"
---
<a name="aspnet-identity-recommended-resources"></a>ASP.NET 識別建議使用的資源
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 本主題提供有關如何使用 ASP.NET Identity 的文件資源的連結。
> 
> 如果您知道好的部落格文章， [stackoverflow](http://stackoverflow.com)執行緒或任何其他連結，會很有用，[傳送一封電子郵件給我們](mailto:aspnetue@microsoft.com?subject=Identity recommended resources)與連結，或者只是離開此頁面底部的訊息。


- [開始使用 ASP.NET Identity](#gettingstarted)
- [新精選必須讀取文件](#feat)
- [中繼 ASP.NET Identity](#adv)
- [影片](#video)
- [詢問的問題、 要求的功能、 bug 和夜間組建報告位置](#samp)
- [在 身分識別部落格文章](#blog)
- [ASP.NET 識別的自訂儲存體提供者](#cust)
- [其他身分識別的資源](#additional)
- [問： &amp; （問題/答案）](#qand)

<a id="gettingstarted"></a>
## <a name="getting-started-with-aspnet-identity"></a>開始使用 ASP.NET Identity

- [使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入的 MVC 5 應用程式](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)本教學課程會示範如何撰寫 ASP.NET MVC 5 應用程式使用 Facebook 和 Google OAuth 2 」 授權。 它也會示範如何加入識別資料庫中的其他資料。
- [將成員資格、 OAuth、 與 SQL Database 的安全的 ASP.NET MVC 應用程式部署至 Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 本教學課程將加入 Azure 部署時，如何保護您的應用程式角色、 如何使用成員資格應用程式開發介面來新增使用者和角色，以及其他安全性功能。
- [ASP.NET Identity 簡介](introduction-to-aspnet-identity.md)
- [使用登入電子郵件確認和密碼重設建立安全的 ASP.NET MVC 5 web 應用程式](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)
- [使用 SMS 和電子郵件雙因素驗證的 ASP.NET MVC 5 應用程式](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)

<a id="feat"></a>
## <a name="new-featured-must-read-articles"></a>新精選必須讀取文件

- [逐步解說： 使用 Microsoft 帳戶驗證的 ASP.NET MVC 識別](http://www.benday.com/2014/02/25/walkthrough-asp-net-mvc-identity-with-microsoft-account-authentication/)由[Benjamin 一天](http://www.benday.com/about/)
- [ASP.NET Identity 2.0 擴充身分識別模型和使用整數索引鍵而不是字串](http://typecastexception.com/post/2014/07/13/ASPNET-Identity-20-Extending-Identity-Models-and-Using-Integer-Keys-Instead-of-Strings.aspx)
- [使用 ASP.NET Web API 2、 Owin，並識別 AngularJS 權杖驗證](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
- [做為替代 WSAT Thinktecture.IdentityManager](http://www.hanselman.com/blog/ThinktectureIdentityManagerAsAReplacementForTheASPNETWebSiteAdministrationTool.aspx)
- [ASP.NET Identity 2.0： 自訂使用者和角色](http://typecastexception.com/post/2014/06/22/ASPNET-Identity-20-Customizing-Users-and-Roles.aspx)

<a id="adv"></a>
## <a name="intermediate-aspnet-identity"></a>中繼 ASP.NET Identity

- [帳戶確認和 ASP.NET 識別的密碼復原](../features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [使用 SMS 的雙因素驗證和使用 ASP.NET Identity 的電子郵件](../features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [將現有的網站從 SQL 成員資格移轉至 ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [將 ASP.NET Identity 新增至空的或現有的 Web Form 專案](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md)
- MSDN Magazine[外部驗證與 ASP.NET Identity](https://msdn.microsoft.com/magazine/dn745860.aspx) Dino Esposito 由
- MSDN Magazine[初探 ASP.NET Identity](https://msdn.microsoft.com/magazine/dn605872.aspx) Dino Esposito 由
- [ASP.NET Identity – 使用者鎖定](http://tech.trailmax.info/2014/06/asp-net-identity-user-lockout/)

<a id="samp"></a>
## <a name="where-to-ask-questions-request-features-report-a-bug-and-nightly-builds"></a>詢問的問題、 要求的功能、 bug 和夜間組建報告位置

- StackOverflow，使用標記[aspnet 識別](http://stackoverflow.com/questions/tagged/asp.net-identity)
- 適用於 ASP.NET 的論壇張貼到[安全性論壇](https://forums.asp.net/25.aspx)並加入**ASP.NET Identity**標題。
- [GitHub 上的 ASP.NET Identity](https://github.com/aspnet/AspNetIdentity) Get 夜間組建，要求功能，開啟的 bug。

<a id="blog"></a>
## <a name="blog-posts-on-identity"></a>在 身分識別部落格文章

- [在 ASP.NET Identity 中的 SecurityStamp 是什麼？](http://stackoverflow.com/questions/19487322/what-is-asp-net-identitys-iusersecuritystampstoretuser-interface/19505060#19505060)
- 由[John Atten](https://twitter.com/xivSolutions)

    - [ASP.NET Identity 2.0 擴充身分識別模型和使用整數索引鍵而不是字串](http://typecastexception.com/post/2014/07/13/ASPNET-Identity-20-Extending-Identity-Models-and-Using-Integer-Keys-Instead-of-Strings.aspx)
    - [ASP.NET Identity 2.0： 自訂使用者和角色](http://typecastexception.com/post/2014/06/22/ASPNET-Identity-20-Customizing-Users-and-Roles.aspx)
    - [ASP.NET MVC 和身分識別 2.0： 了解基本概念](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)
    - [設定帳戶驗證與授權雙因素](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)
    - [設定資料庫連線和 ASP.NET MVC 5 中識別帳戶的程式碼優先移轉和 Visual Studio 2013](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx)
- 由[Taiseer Joudeh](http://bitoftech.net/taiseer-joudeh-blog/)

    - [使用 ASP.NET Web API 2，ASP.NET Identity Owin 中介軟體，權杖型驗證](http://bitoftech.net/2014/06/01/token-based-authentication-asp-net-web-api-2-owin-asp-net-identity/)
    - [使用 ASP.NET Web API 2、 Owin，並識別 AngularJS 權杖驗證](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
    - [啟用使用 ASP.NET Web API 2 和 Owin – 第 3 篇 AngularJS 應用程式中的 OAuth 重新整理語彙基元。](http://bitoftech.net/2014/07/16/enable-oauth-refresh-tokens-angularjs-app-using-asp-net-web-api-2-owin/)
- 由[Anders Abel](https://twitter.com/anders_abel)

    - [了解 Owin 外部驗證管線](http://coding.abel.nu/2014/06/understanding-the-owin-external-authentication-pipeline/)
    - [ASP.NET Identity 和 Owin 概觀](http://coding.abel.nu/2014/06/asp-net-identity-and-owin-overview/)

  由[K.Scott Allen](https://twitter.com/OdeToCode)上 beethoven 的 Ode 程式碼

    - [ASP.NET Core 識別](http://odetocode.com/blogs/scott/archive/2013/11/25/asp-net-core-identity.aspx)這篇部落格會檢查的核心抽象概念，包括 IUser、 IUserStore 和 I\*存放區介面。
    - [有了 Entity Framework 的 ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/03/asp-net-identity-with-the-entity-framework.aspx) MVC 5、 Web API 和 SPA 應用程式、 連接字串和管理內容中的個別使用者帳戶
    - [使用 ASP.NET 識別的自訂選項](http://odetocode.com/blogs/scott/archive/2014/01/09/customization-options-with-asp-net-identity.aspx)
    - [實作 ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- [Benjamin 天](http://www.benday.com/about/)[逐步解說： 使用 Microsoft 帳戶驗證的 ASP.NET MVC 身分識別](http://www.benday.com/2014/02/25/walkthrough-asp-net-mvc-identity-with-microsoft-account-authentication/)
- [Brock Allen](https://twitter.com/BrockLAllen)

    - [在與 OWIN/Katana 驗證中介軟體的外部登入提供者 （社交登入） 的入門](http://brockallen.com/2014/01/09/a-primer-on-external-login-providers-social-logins-with-owinkatana-authentication-middleware/)
    - [簡介 IdentityReboot](http://brockallen.com/2014/02/11/introducing-identityreboot/)： 一組 ASP.NET 識別實作我了解抱怨主要遺漏功能的延伸模組。
- [Pranav Rastogi](https://twitter.com/rustd)

    - [從社交提供者取得詳細資訊](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [@beabigrockstar](https://twitter.com/beabigrockstar) (Jerrie Pelser)

    - [2 因素驗證](http://www.beabigrockstar.com/blog/2-factor-authentication-with-asp-net-identity-2-0-beta-1/)
    - [使用 ASP.NET Identity 的 Google 驗證器](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)
    - [ASP.NET MVC 5 驗證輔助線](http://www.beabigrockstar.com/)
- [從 VS 2013 專案範本中所使用的社交提供者取得詳細資訊](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [建置含有 ASP.NET 識別的簡單 ToDo 應用程式並將使用者與 ToDoes](https://blogs.msdn.com/b/webdev/archive/2013/10/20/building-a-simple-todo-application-with-asp-net-identity-and-associating-users-with-todoes.aspx)
- [Google OpenId 整合問題與 ASP.NET Identity](http://blog.technovert.com/2014/01/google-openid-integration-issues-asp-net-identity/)如果您收到錯誤： HTTP 錯誤 404.15-找不到要求篩選模組設定為拒絕的要求，其中的查詢字串過長
- [做為替代 WSAT Thinktecture.IdentityManager](http://www.hanselman.com/blog/ThinktectureIdentityManagerAsAReplacementForTheASPNETWebSiteAdministrationTool.aspx)
- [使用 ASP.NET Web API 2、 Owin，並識別 AngularJS 權杖驗證](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
- [Entity Framework 沒有簡單的 Asp.net Identity Core](https://code.msdn.microsoft.com/Simple-Aspnet-Identiy-Core-7475a961)
- [使用 ASP.NET mvc 的身分識別中的角色](http://www.dotnetfunda.com/articles/show/2898/working-with-roles-in-aspnet-identity-for-mvc)由[Sheo Narayan](http://www.dotnetfunda.com/profile/sheonarayan.aspx)
- [從 ASP.NET 成員資格將移至 ASP.NET Identity](http://webdojo.sharepoint.com/ajmatthews/_layouts/15/start.aspx#/Lists/Posts/Post.aspx?ID=2) Alistair 騏宇由

<a id="video"></a>
## <a name="videos"></a>視訊

- Channel 9[保護 ASP.NET 應用程式和服務： 安全性現代應用程式的外觀](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B421#fbid=PhVT9E1WRtr?hashlink=fbid)謝 Flatow 由
- Channel 9 [ASP.NET Identity 簡介](https://channel9.msdn.com/Events/dotnetConf/2014/ASP-NET-Identity-Security)Pranav Rastogi 由
- Channel 9 [ASP.NET 驗證使用 ASP.NET Identity](https://channel9.msdn.com/Shows/Web+Camps+TV/Special-Movember-Episode-ASPNET-Authentication-Provider) Cory Fowler 由
- Channel 9[建置現代化 Web 應用程式： ASP.NET Identity](https://channel9.msdn.com/Series/Building-Modern-Web-Apps/03)由 Jeff Koch
- Channel 9[保護您的網站與 ASP.NET Identity](https://channel9.msdn.com/Events/TechDays/Techdays-2014-the-Netherlands/Securing-your-website-with-ASP-NET-Identity)由 Alex Thissen
- [使用現有的資料庫模型上的 ASP.NET Identity](https://www.youtube.com/watch?v=elfqejow5hM) Alexander Schmidt 由
- [一個 ASP.NET Identity](https://www.youtube.com/watch?v=w8GD-QIusKk)由 Ivaylo Kenov Telerik 的
- [捷克文 ASP.NET Identity](https://www.youtube.com/watch?v=tVbZp5brcpY)在此課程中，我們將示範如何部署基本驗證、 如何加入外部身分識別提供者，例如 Twitter 或 Facebook、 支援和如何使用單次密碼 (OTP)。 [ASP.NET Identity je nástupce 角色提供者的成員資格&#367;v ASP.NET、 tedy knihovna pro zajišt&#283;ní autentizace uživatel&#367;。 V této p&#345;ednášce si ukážeme 的賈克 nasad]

<a id="cust"></a>
## <a name="custom-storage-providers-for-aspnet-identity"></a>ASP.NET 識別的自訂儲存體提供者

如果您想要撰寫您自己的提供者，請參閱[概觀的自訂儲存提供者，ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)和[實作 ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx) ，然後檢查其中一個列出的 OSS 專案的來源下面。

- 教學課程： [ASP.NET 識別的自訂儲存體提供者概觀](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)Tom FitzMacken 由
- 部落格：[實作 ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- 教學課程：[設定基本的身分識別帳戶，並指向外部的 DB](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx)。 由[ @xivSolutions ](https://twitter.com/xivSolutions)。
- 教學課程[： 實作自訂 MySQL ASP.NET Identity 的存放裝置提供者](../extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [CodeFluent 實體](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/)由[SoftFluent](http://www.softfluent.com/)
- [Azure 資料表儲存體](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/)由 James Randall。
- Azure 資料表儲存體： [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage)由[ @stuartleeks ](https://twitter.com/stuartleeks)。
- [CouchDB / Cloudant 由奧 Wertheim。](https://github.com/danielwertheim/mycouch.aspnet.identity)
- 彈性的搜尋[h:%m 彈性識別](https://github.com/bmbsqd/elastic-identity)Bombsquad AB.由
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/)由 Jonathan Sheely Jonathan Sheely。
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity)由 Antônio Milesi Bastos。
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0)由[ @tourismgeek ](https://twitter.com/tourismgeek)。
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity)由[ILMServices](http://www.ilmservice.com/)。
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- 「 資料庫第一次 」 的使用者存放區產生 EF T4 範本程式碼： [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)

<a id="additional"></a>
## <a name="additional-aspnet-identity-resources"></a>其他 ASP.NET Identity 資源

- [導入的 Yahoo 和 LinkedIn OAuth 安全性提供者設定 owin](http://blog.beabigrockstar.com/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/)由 Jerrie Pelser Yahoo 和 LinkedIn 的指示。

<a id="qand"></a>
## <a name="qampa-questionanswer"></a>問：&amp;（問題/答案）

- 問： 鎖定已啟用 [記住我] （因此不一定要在該電腦/瀏覽器通過 2FA） 的使用者時不會鎖定。為何及如何避免，？ 回應[這裡](http://stackoverflow.com/questions/24312247/locked-out-users-can-login-if-they-have-auth-cookie)。
- **問：**： 如何儲存自訂宣告，例如使用者的實際名稱，以避免不必要的資料庫查詢，每個要求的 ASP.NET Identity cookie 中。 回應[這裡](http://stackoverflow.com/questions/23622047/identity-cookie-loses-custom-claim-information-after-a-period-of-time)。
- **問： 更新 AspNetUser 密碼雜湊**： 我有 2 個專案。 其中一個方法使用 ASP.NET 驗證、 其他使用 Windows 驗證，也就是管理側邊。 我想要能夠管理其他使用者管理專案。 我可以修改密碼以外的所有內容。 [回答以下](http://stackoverflow.com/questions/23880666/updating-aspnetuser-password-hash)。
- **問：**： 如何重密碼以其他使用者的系統管理員？ 回應[這裡](http://stackoverflow.com/questions/23783249/identity-2-0-reset-password-by-admin/24211766#24211766)。
- **問：**： 我可以變更 ASP.NET MVC IdentityUser 的使用者名稱欄位的顯示的名稱嗎？ 回應[這裡](http://stackoverflow.com/questions/23256650/can-i-change-the-displayed-name-of-the-username-field-in-asp-net-mvc-identityuse)。
- **問：**： 如何將其他使用者加入至特定角色 gran 使用者權限嗎？ 回應[這裡](http://stackoverflow.com/questions/23695373/allow-users-to-grant-permissions-to-other-users-for-their-account-in-asp-net-ide)。
- **問：**: AspNetUsers 資料表與 AspNetUserClaims 資料表中儲存設定檔資訊。 回應[這裡](http://stackoverflow.com/questions/23215727/is-there-any-benefit-to-storing-user-information-in-aspnetuserclaims-with-asp-ne)。
- **問：**： 使用外部驗證提供者時，請記住我。 回應[這裡](http://stackoverflow.com/questions/23180896/how-to-remember-the-login-in-mvc5-when-an-external-provider-is-used)。
- **問：**： 為什麼每項要求需要才能 ApplicationDBContext，不會太多的額外負荷？。 問題的答案，否，額外負荷很低。
- 問： 如何取得已登入使用者的清單？ 回應[這裡](http://stackoverflow.com/questions/22995653/getting-a-list-of-logged-in-users-in-asp-net-identity/)。
- 問： 如何偵測當使用者登入時 Microsoft.AspNet.Identity？ 回應[這裡](http://stackoverflow.com/questions/22956486/how-can-i-detect-when-a-user-logs-in-with-microsoft-aspnet-identity/22970698#22970698)。
- 問： 如何取得識別當地語系化的錯誤訊息？ 回應[這裡](http://stackoverflow.com/questions/22835981/asp-net-identity-localization-publickeytoken/22845864#22845864)。
- 問： 如何設定以取得全新的宣告，每隔 30 分鐘 CookieMiddleware？ 回應[這裡](http://stackoverflow.com/questions/22682663/how-to-hold-the-cookies-claims-updated-with-mcv5-owin/22796932#22796932)。
- 問： 如何修改使用者的宣告之後他們登入？ 回應[這裡](http://stackoverflow.com/questions/22768836/can-i-modify-claims-in-asp-net-identity-with-owin-after-calling-signin/22769963#22769963)。
- 問： 如何使安全性權杖的失效？ 回應[這裡](http://stackoverflow.com/questions/22755700/revoke-token-generated-by-usertokenprovider-in-asp-net-identity-2-0/22767286#22767286)。
- 問： 如何執行會在 cookie 中介軟體中的存放區宣告？ 回應[這裡](http://stackoverflow.com/questions/22320632/storing-retrieving-user-data-without-database-when-using-owin-cookie-authenticat/22541856#22541856)。
- 問： 我想要有 PIN 或我的 MVC 應用程式中的每個動作方法上檢查安全性，但我想要儲存使用者成功，因此它們不需要在每次要求輸入 pin 碼，該動作方法。 回應[這裡](http://stackoverflow.com/questions/22479958/security-check-an-user-to-a-access-controller-action/22486075#22486075)。
- 問： 我想要儲存從社交提供者傳回電子郵件地址至 DB，我該怎麼嗎？ 回應[這裡](http://stackoverflow.com/questions/22888397/save-claims-to-db-on-login/22970969#22970969):
- 問： 如何偵測時在使用者登入這兩個具有/與-out [記住我] cookie？ 回應[這裡](http://stackoverflow.com/questions/22956486/how-can-i-detect-when-a-user-logs-in-with-microsoft-aspnet-identity/22970698#22970698)。
- 問： 是否可以修改在 ASP.NET Identity OWIN 與中的宣告之後呼叫登入？ 回答： 呼叫 SignIn 正是您應該執行您想要修改使用者的宣告時。 基本上，它會導致要序列化成 cookie，這就是為什麼您查看顯示在後續要求上的新宣告身分識別。
