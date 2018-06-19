---
uid: web-api/overview/security/individual-accounts-in-web-api
title: 保護使用個別的帳戶和 ASP.NET Web API 2.2 中的本機登入的 Web Api< / |Microsoft 文件
author: MikeWasson
description: 本主題說明如何保護 web API 使用 OAuth2 驗證成員資格資料庫。 教學課程的 Visual Studio 201 中使用的軟體版本...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2014
ms.topic: article
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: e2056e769edf972cba830b31cf37f6418148ca73
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
ms.locfileid: "28038269"
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>保護使用個別的帳戶和 ASP.NET Web API 2.2 中的本機登入的 Web 應用程式開發介面
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載範例應用程式](https://github.com/MikeWasson/LocalAccountsApp)

> 本主題說明如何保護 web API 使用 OAuth2 驗證成員資格資料庫。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Web API 2.2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2.1](../../../identity/index.md)


在 Visual Studio 2013 中，Web API 專案範本可讓您驗證的三個選項：

- **個別的帳戶。** 應用程式會使用成員資格資料庫。
- **組織帳戶。** 使用者登入其 Azure Active Directory、 Office 365 或在內部部署 Active Directory 認證。
- **Windows 驗證。** 此選項僅供內部網路應用程式，並使用 Windows 驗證的 IIS 模組。

如需有關這些選項的詳細資訊，請參閱[Visual Studio 2013 中建立 ASP.NET Web 專案](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth)。

個別帳戶提供登入使用者的兩種方式：

- **本機登入**。 使用者會註冊在網站上，輸入使用者名稱和密碼。 應用程式成員資格資料庫中儲存密碼雜湊。 當使用者登入時，ASP.NET Identity 系統會確認密碼。
- **社交登入**。 使用者登入的外部服務，例如 Facebook、 Microsoft 或 Google。 此應用程式仍在成員資格資料庫中，建立使用者項目，但不會儲存任何認證。 使用者會驗證登入外部服務。

這篇文章探討本機登入案例。 本機和社交登入 Web API 使用 OAuth2 驗證要求。 然而，認證流程有不同的本機和社交登入。

在本文中，我將示範簡單的應用程式，可讓使用者登入或傳送已驗證的 AJAX 呼叫 web API。 您可以下載範例程式碼[這裡](https://github.com/MikeWasson/LocalAccountsApp)。 讀我檔案說明如何在 Visual Studio 中從頭開始建立範例。

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

範例應用程式中使用資料繫結和 jQuery 進行傳送 AJAX 要求解 Knockout.js。 我的焦點 AJAX 呼叫時，因此您不需要知道解 Knockout.js 這個發行項。

過程中，我將說明：

- 應用程式做什麼用戶端。
- 在伺服器上發生什麼事。
- 在中間的 HTTP 流量。

首先，我們必須定義一些 OAuth2 術語。

- *資源*。 一些可保護的資料。
- *資源伺服器*。 主控資源的伺服器。
- *資源擁有者*。 可以授與權限來存取資源的實體。 （通常是使用者。）
- *用戶端*： 想要存取之資源的應用程式。 在本文中，用戶端是網頁瀏覽器。
- *存取權杖*。 授與資源的存取語彙基元。
- *承載權杖*。 特定類型的存取權杖，任何人都可以使用語彙基元的屬性。 換句話說，用戶端不需要的密碼編譯金鑰或其他使用 bearer 權杖的密碼。 基於這個原因，承載語彙基元應該只用於透過 HTTPS，並應該有相對較短的到期時間。
- *授權伺服器*。 伺服器可存取權杖。

應用程式可以做為授權伺服器與資源伺服器。 Web API 專案範本會遵循此模式。

## <a name="local-login-credential-flow"></a>本機登入認證流程

Web API 所使用的本機登入，[資源擁有者密碼的流程](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html)OAuth2 中定義。

1. 使用者會輸入到用戶端名稱和密碼。
2. 用戶端傳送至授權伺服器的這些認證。
3. 授權伺服器驗證憑證，並傳回存取權杖。
4. 若要存取受保護的資源，用戶端會在 HTTP 要求的授權標頭中包含存取權杖。

![](individual-accounts-in-web-api/_static/image3.png)

當您選取**個別帳戶**的 Web API 專案範本，專案會包含授權伺服器，它會驗證使用者認證會簽發權杖。 下圖顯示相同的認證流程以 Web 應用程式開發介面的元件。

![](individual-accounts-in-web-api/_static/image4.png)

在此案例中，Web API 控制器會做為資源伺服器。 驗證篩選條件會驗證存取權杖和 **[Authorize]** 屬性用來保護資源。 當控制器或動作具有 **[Authorize]** 屬性，該控制站的所有要求，或必須驗證動作。 否則，授權遭到拒絕，而且 Web API 會傳回 401 （未經授權） 錯誤。

授權伺服器，並同時呼叫的驗證篩選[OWIN 中介軟體](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)元件，可處理的 OAuth2 詳細資料。 我將稍後在本教學課程說明的設計中更多詳細資料。

## <a name="sending-an-unauthorized-request"></a>傳送未經授權的要求

若要開始，執行應用程式，然後按一下**呼叫 API**  按鈕。 當要求完成時，您應該會看到錯誤訊息中的**結果**方塊。 這是因為要求不包含存取權杖，因此是未經授權的要求。

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

**呼叫 API**按鈕時傳送 AJAX 要求至 ~/api/值，這會叫用 Web API 控制器動作。 以下是傳送 AJAX 要求的 JavaScript 程式碼區段。 範例應用程式中的所有 JavaScript 應用程式程式碼位於 Scripts\app.js 檔案中。

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

使用者登入，直到沒有任何持有者權杖，因此在要求中的沒有授權標頭。 這會導致該要求傳回 401 錯誤。

以下是 HTTP 要求。 (用[Fiddler](http://www.telerik.com/fiddler)擷取 HTTP 流量。)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

HTTP 回應：

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

請注意，回應就會包含 Www-authenticate 標頭與設承載挑戰。 表示伺服器所預期的承載權杖。

## <a name="register-a-user"></a>註冊的使用者

在**註冊**區段的應用程式中，輸入電子郵件和密碼，並按一下**註冊** 按鈕。

您不需要使用有效的電子郵件地址，此範例中，但實際的應用程式會確認的位址。 (請參閱[建立安全的 ASP.NET MVC 5 web 應用程式與記錄檔中，電子郵件確認和密碼重設](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)。)輸入密碼，使用類似"Password1 ！"，與大寫字母、 小寫字母、 數字和非英數字元。 為了簡化應用程式，我中省略了用戶端驗證，因此如果沒有密碼格式有問題，您會得到 400 （不正確的要求） 錯誤。

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

**註冊**按鈕將 POST 要求傳送至 ~/api/Account/Register /。 要求主體是保留的名稱和密碼的 JSON 物件。 以下是將要求傳送的 JavaScript 程式碼：

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

HTTP 要求：

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

HTTP 回應：

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

此要求由`AccountController`類別。 就內部而言，`AccountController`會使用 ASP.NET 識別來管理成員資格資料庫。

如果您在本機從 Visual Studio 執行應用程式，使用者帳戶都會儲存在 LocalDB 中，以在 AspNetUsers 資料表。 若要檢視 Visual Studio 中的資料表，請按一下**檢視**功能表上，選取**伺服器總管**，然後展開**資料連接**。

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>取得存取權杖

到目前為止我們不做任何 OAuth，不過現在我們看到 OAuth 授權伺服器，在動作中，要求存取權杖時。 在**登入**區域的範例應用程式中，輸入電子郵件和密碼，然後按一下**登入**。

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

**登入**按鈕將要求傳送至權杖的端點。 在要求主體包含下列表單 url 編碼資料：

- 授與\_類型: 「 密碼 」
- 使用者名稱：&lt;使用者的電子郵件&gt;
- 密碼：&lt;密碼&gt;

以下是傳送 AJAX 要求的 JavaScript 程式碼：

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

如果要求成功，則授權伺服器會傳回存取權杖回應主體中。 請注意，我們會將語彙基元儲存於工作階段儲存體，供日後使用的應用程式開發介面傳送要求時。 不同於某些形式的驗證 （例如 cookie 為基礎的驗證） 中，瀏覽器不會自動包含存取權杖在後續要求中。 應用程式必須明確加以。 這是好的結果，因為它會限制[CSRF 弱點](preventing-cross-site-request-forgery-csrf-attacks.md)。

HTTP 要求：

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

您可以看到此要求包含使用者的認證。 您*必須*使用 HTTPS 來提供傳輸層安全性。

HTTP 回應：

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

可讀性，我要縮排的 JSON，並截斷相當長的存取語彙基元。

`access_token`， `token_type`，和`expires_in`OAuth2 規格所定義的屬性。其他屬性 (`userName`， `.issued`，和`.expires`) 都只供使用者參考。 您可以找到加入這些額外的屬性中的程式碼`TokenEndpoint`/Providers/ApplicationOAuthProvider.cs 檔案中的方法。

## <a name="send-an-authenticated-request"></a>傳送已驗證的要求

現在，我們已經持有人權杖時，我們可以讓已驗證的要求的 api。 這是在要求中設定授權標頭。 按一下**呼叫 API**再次若要查看此按鈕。

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

HTTP 要求：

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

HTTP 回應：

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>登出

因為認證或存取權杖，則無法快取瀏覽器，請登出就是 「 不會忘記"語彙基元，藉由移除從工作階段儲存體：

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>了解個別帳戶專案範本

當您選取**個別帳戶**在 ASP.NET Web 應用程式專案範本中，專案會包含：

- OAuth2 授權伺服器。
- Web API 端點以管理使用者帳戶
- 儲存使用者帳戶的 EF 模型。

以下是實作這些功能的主要應用程式類別：

- `AccountController`. 提供 Web API 端點以管理使用者帳戶。 `Register`動作時，我們在本教學課程中使用。 在類別上的其他方法支援密碼重設、 社交登入，以及其他功能。
- `ApplicationUser`/Models/IdentityModels.cs 中定義。 這個類別是使用者帳戶的成員資格資料庫的 EF 模型。
- `ApplicationUserManager`定義於 /App\_Start/IdentityConfig.cs 此類別衍生自[UserManager](https://msdn.microsoft.com/library/dn613290.aspx)和使用者帳戶，例如建立新的使用者，並確認密碼，以及其他等等上執行作業，並會自動保存資料庫的變更。
- `ApplicationOAuthProvider`. 此物件可插入 OWIN 中介軟體，並由中介軟體引發的事件處理。 它衍生自[OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx)。

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>設定授權伺服器

StartupAuth.cs，在下列程式碼會設定 OAuth2 授權伺服器。

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

`TokenEndpointPath`屬性是授權伺服器端點的 URL 路徑。 這是 URL 的應用程式會使用來取得持有者權杖。

`Provider`屬性會指定可插入的 OWIN 中介軟體，以及由中介軟體引發的事件處理的提供者。

以下是基本流程，當應用程式想要取得權杖：

1. 若要取得存取權杖，應用程式傳送的要求 ~ / t o k。
2. OAuth 中介軟體呼叫`GrantResourceOwnerCredentials`視提供者。
3. 提供者呼叫`ApplicationUserManager`驗證認證，並建立宣告識別。
4. 如果成功，此提供者會建立驗證 ticket，用來產生權杖。

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

OAuth 的中介軟體並不知道的使用者帳戶。 提供者會在的中介軟體和 ASP.NET Identity 之間進行通訊。 如需實作授權伺服器的詳細資訊，請參閱[OWIN OAuth 2.0 授權伺服器](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md)。

### <a name="configuring-web-api-to-use-bearer-tokens"></a>設定 Web 應用程式開發介面使用承載語彙基元

在`WebApiConfig.Register`方法，下列程式碼設定 Web API 管線的驗證：

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

**HostAuthenticationFilter**類別可讓您使用 bearer 權杖驗證。

**SuppressDefaultHostAuthentication**方法會告知 Web API，略過任何後，才能在要求到達 Web API 管線，IIS 或 OWIN 中介軟體的驗證。 這樣一來，我們可以限制僅使用 bearer 權杖進行驗證的 Web API。

> [!NOTE]
> 特別是，您的應用程式的 MVC 部分可能會使用表單驗證，將認證儲存在 cookie 中。 Cookie 為基礎的驗證需要防偽語彙基元，以防止 CSRF 攻擊的使用。 因為 web API 的用戶端傳送的防偽語彙基元的便利方法，這是 web 應用程式開發介面，讓發生問題。 (如需此問題有關的詳細背景，請參閱[Web API 中防止攻擊 CSRF](preventing-cross-site-request-forgery-csrf-attacks.md)。)呼叫**SuppressDefaultHostAuthentication**可確保 Web 應用程式開發介面不會受到影響儲存在 cookie 中的認證從 CSRF 攻擊。


當用戶端要求受保護的資源時，會發生下列情況中 Web API 管線：

1. **HostAuthentication**篩選條件會呼叫驗證權杖的 OAuth 中介軟體。
2. 中介軟體會將語彙基元轉換成宣告身分識別。
3. 此時，要求是*驗證*但不是*授權*。
4. 授權篩選條件會檢查宣告身分識別。 如果宣告授權該資源的使用者，將授權要求。 根據預設， **[Authorize]** 屬性將授權任何已驗證的要求。 不過，您可以授權角色或其他宣告。 如需詳細資訊，請參閱[驗證和授權的 Web API](authentication-and-authorization-in-aspnet-web-api.md)。
5. 如果上一個步驟都成功，控制器會傳回受保護的資源。 否則，用戶端收到 401 （未經授權） 錯誤。

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>其他資源

- [ASP.NET Identity](../../../identity/index.md)
- [了解 VS2013 RC SPA 範本中的安全性功能](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)。 MSDN 部落格文章所 Hongye 太陽。
- [將 Web 應用程式開發介面個別分解帳戶範本 – 組件 2： 本機帳戶](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/)。 Dominick Baier 部落格文章。
- [驗證和 Web API 與 OWIN 裝載](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/)。 良好說明`SuppressDefaultHostAuthentication`和`HostAuthenticationFilter`由 Brock Allen。
- [自訂在 ASP.NET Identity 中在 VS 2013 範本中的設定檔資訊](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)。 Pranav Rastogi MSDN 部落格文章。
- [每個要求存留期管理 UserManager 在 ASP.NET Identity 中的類別](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx)。 MSDN 部落格文章 Suhas Joshi 良好的說明`UserManager`類別。
