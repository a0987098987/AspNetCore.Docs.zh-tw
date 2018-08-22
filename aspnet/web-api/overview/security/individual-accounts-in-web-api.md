---
uid: web-api/overview/security/individual-accounts-in-web-api
title: 保護 Web API 使用個別帳戶和 ASP.NET Web API 2.2 中的本機登入 |Microsoft Docs
author: MikeWasson
description: 本主題說明如何保護 web API 使用 OAuth2 驗證的成員資格資料庫。 教學課程的 Visual Studio 201 中所使用的軟體版本...
ms.author: riande
ms.date: 10/15/2014
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 01d117260ef458453bee79285a37a8977221998c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830129"
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>保護 Web API 使用個別帳戶和 ASP.NET Web API 2.2 中的本機登入
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載範例應用程式](https://github.com/MikeWasson/LocalAccountsApp)

> 本主題說明如何保護 web API 使用 OAuth2 驗證的成員資格資料庫。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Web API 2.2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2.1](../../../identity/index.md)


在 Visual Studio 2013 中，Web API 專案範本可讓您進行驗證的三個選項：

- **個別的帳戶。** 應用程式使用的成員資格資料庫。
- **組織帳戶。** 使用者使用他們的 Azure Active Directory、 Office 365 或內部部署 Active Directory 認證登入。
- **Windows 驗證。** 此選項適用於內部網路應用程式，並使用 Windows 驗證的 IIS 模組。

如需有關這些選項的詳細資訊，請參閱 < [Visual Studio 2013 中建立 ASP.NET Web 專案](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth)。

個別的帳戶會提供兩種方法讓使用者登入：

- **本機登入**。 使用者註冊站台上，輸入使用者名稱和密碼。 應用程式會在成員資格資料庫中儲存密碼雜湊。 當使用者登入時，ASP.NET 身分識別系統會驗證密碼。
- **社交登入**。 使用者登入外部服務，例如 Facebook、 Microsoft 或 Google。 此應用程式仍會在成員資格資料庫中，建立使用者的項目，但不會儲存任何認證。 使用者驗證登入外部服務。

這篇文章探討本機登入案例。 本機和社交登入，Web API 會使用 OAuth2 驗證的要求。 不過，認證流程是不同的本機和社交登入。

在本文中，我將示範簡單的應用程式，可讓使用者登入，並傳送至 web API 驗證的 AJAX 呼叫。 您可以下載範例程式碼[此處](https://github.com/MikeWasson/LocalAccountsApp)。 讀我檔案說明如何在 Visual Studio 中從頭開始建立範例。

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

範例應用程式中使用 Knockout.js 資料繫結和 jQuery 進行傳送 AJAX 要求。 我將專注於 AJAX 呼叫，因此您不需要知道 Knockout.js 這篇文章。

過程中，我將說明：

- 在用戶端上進行的哪些應用程式。
- 什麼在伺服器上發生。
- 在中間的 HTTP 流量。

首先，我們需要定義 OAuth2 的一些術語。

- *資源*。 某些可以保護的資料片段。
- *資源伺服器*。 裝載資源的伺服器。
- *資源擁有者*。 可以授與權限來存取資源的實體。 （通常是使用者。）
- *用戶端*： 想要存取之資源的應用程式。 在本文中，用戶端會將網頁瀏覽器。
- *存取權杖*。 授與對資源的存取語彙基元。
- *持有人權杖*。 特定類型的存取權杖，任何人都可以使用語彙基元的屬性。 換句話說，用戶端不需要的密碼編譯金鑰或使用持有人權杖的其他祕密。 基於這個理由，應該只用於透過 HTTPS，持有人權杖，並應該會有相對較短的到期時間。
- *授權伺服器*。 一種伺服器可存取權杖。

應用程式可以做為授權伺服器與資源伺服器。 Web API 專案範本會遵循此模式。

## <a name="local-login-credential-flow"></a>本機登入認證流程

Web API 所使用的本機登入[資源擁有者密碼流程](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html)OAuth2 中定義。

1. 使用者會在用戶端輸入的名稱和密碼。
2. 用戶端會傳送到授權伺服器的這些認證。
3. 授權伺服器驗證的認證，並傳回存取權杖。
4. 若要存取受保護的資源，用戶端會包含在 HTTP 要求的 Authorization 標頭中的存取權杖。

![](individual-accounts-in-web-api/_static/image3.png)

當您選取**個別帳戶**在 Web API 專案範本中，專案會包含授權伺服器會驗證使用者的認證，並簽發權杖。 下圖顯示相同的認證流程，以 Web API 的元件。

![](individual-accounts-in-web-api/_static/image4.png)

在此案例中，Web API 控制器會作為資源伺服器。 驗證篩選條件會驗證存取權杖，而 **[Authorize]** 屬性用來保護資源。 當控制器或動作具有 **[Authorize]** 屬性，該控制站的所有要求，或必須驗證動作。 否則授權會遭到拒絕，而且 Web API 會傳回 401 （未經授權） 時發生錯誤。

授權伺服器，並同時呼叫的驗證篩選[OWIN 中介軟體](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)元件，可處理 OAuth2 的詳細資料。 本教學課程稍後，我將說明更多詳細資料中的設計。

## <a name="sending-an-unauthorized-request"></a>傳送未經授權的要求

若要開始，執行應用程式，然後按一下**呼叫 API**  按鈕。 要求完成時，您應該會看到錯誤訊息中的**結果** 方塊中。 這是因為要求不包含存取權杖，因此要求未獲授權。

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

**呼叫 API**按鈕 AJAX 將要求傳送至 ~/api/值，會叫用 Web API 控制器動作。 以下是傳送 AJAX 要求的 JavaScript 程式碼區段。 範例應用程式中的所有 JavaScript 應用程式程式碼位於 Scripts\app.js 檔案中。

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

在使用者登入，直到沒有任何持有人權杖，並因此在要求中的沒有授權標頭。 這會導致傳回 401 錯誤的要求。

以下是 HTTP 要求。 (我使用了[Fiddler](http://www.telerik.com/fiddler)擷取 HTTP 流量。)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

HTTP 回應：

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

請注意，回應就會包含 Www-authenticate 標頭設定為持有人的挑戰。 指出伺服器所預期的持有人權杖。

## <a name="register-a-user"></a>註冊的使用者

在 [**註冊**一節的應用程式，請輸入電子郵件和密碼，然後按一下**註冊**] 按鈕。

您不需要針對此範例中，使用有效的電子郵件地址，但實際的應用程式會確認該位址。 (請參閱[登入、 電子郵件確認和密碼重設建立安全的 ASP.NET MVC 5 web 應用程式](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)。)輸入密碼，使用類似"Password1 ！ 」，與大寫字母、 小寫字母、 數字和非英數字元。 為了簡化應用程式，我省略了用戶端驗證，因此如果沒有密碼格式問題，您會得到 400 （不正確的要求） 錯誤。

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

**註冊**按鈕會將 POST 要求傳送至 ~/api/Account/Register /。 要求主體是 JSON 物件保留的名稱和密碼。 以下是傳送要求的 JavaScript 程式碼：

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

HTTP 要求：

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

HTTP 回應：

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

此要求由`AccountController`類別。 就內部而言，`AccountController`使用 ASP.NET 身分識別管理成員資格資料庫。

從 Visual Studio 中本機執行應用程式時，如果使用者帳戶會在 LocalDB 中，儲存在 AspNetUsers 資料表中。 若要檢視的資料表，在 Visual Studio 中，按一下**檢視**功能表上，選取**伺服器總管**，然後展開**資料連接**。

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>取得存取權杖

到目前為止我們不做任何 OAuth 中，但是現在我們先看 OAuth 授權伺服器，在動作中，當我們要求存取權杖。 在 **登入**區域中的範例應用程式中，輸入電子郵件和密碼，然後按一下**登入**。

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

**登入**按鈕會將要求傳送至權杖端點。 在要求主體包含下列的表單 url 編碼資料：

- 授與\_類型: 「 密碼 」
- 使用者名稱：&lt;使用者的電子郵件&gt;
- 密碼：&lt;密碼&gt;

以下是傳送 AJAX 要求的 JavaScript 程式碼：

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

如果要求成功，則授權伺服器會傳回存取權杖回應主體中。 請注意，我們將權杖儲存在工作階段存放區，以供日後使用，將要求傳送至 API 時。 不同於某些形式的驗證 （例如 cookie 型驗證） 中，瀏覽器不會自動包含存取權杖中的後續要求。 應用程式必須明確加以。 這是一件好事，因為它會限制[CSRF 弱點](preventing-cross-site-request-forgery-csrf-attacks.md)。

HTTP 要求：

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

您可以看到此要求包含使用者的認證。 您*必須*使用 HTTPS 來提供傳輸層安全性。

HTTP 回應：

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

為了便於閱讀，我會縮排的 JSON，並截斷的存取權杖，這是相當長。

`access_token`， `token_type`，和`expires_in`OAuth2 規格來定義屬性。其他屬性 (`userName`， `.issued`，和`.expires`) 是僅供資訊之用。 您可以找到加入這些其他的屬性中的程式碼`TokenEndpoint`/Providers/ApplicationOAuthProvider.cs 檔案中的方法。

## <a name="send-an-authenticated-request"></a>傳送已驗證的要求

既然我們已經持有人權杖，就可以在對 API 提出已驗證的要求。 這是由在要求中設定授權標頭。 按一下 **呼叫 API**一次，才能看到此按鈕。

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

HTTP 要求：

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

HTTP 回應：

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>登出

因為認證或存取權杖，並不會快取瀏覽器，進行登出時只是 「 不會忘記 「 語彙基元，方法是移除工作階段存放區：

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>了解個別帳戶專案範本

當您選取**個別帳戶**在 ASP.NET Web 應用程式專案範本中，專案會包含：

- OAuth2 授權伺服器。
- Web API 端點以管理使用者帳戶
- EF 模型來儲存使用者帳戶。

以下是實作這些功能的主要應用程式類別：

- `AccountController`. 提供 Web API 端點以管理使用者帳戶。 `Register`動作是唯一的我們在本教學課程中使用。 在類別上的其他方法可支援密碼重設、 社交登入及其他功能。
- `ApplicationUser`定義於 /Models/IdentityModels.cs。 這個類別是成員資格資料庫中的使用者帳戶的 EF 模型。
- `ApplicationUserManager`定義於 /App\_Start/IdentityConfig.cs 這個類別衍生自[UserManager](https://msdn.microsoft.com/library/dn613290.aspx)和執行在使用者帳戶，例如建立新的使用者，並確認密碼，以及其他等等的作業，並會自動保存資料庫的變更。
- `ApplicationOAuthProvider`. 此物件插入的 OWIN 中介軟體，並處理中介軟體所引發的事件。 它衍生自[OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx)。

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>設定授權伺服器

在 StartupAuth.cs，下列程式碼會設定 OAuth2 授權伺服器。

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

`TokenEndpointPath`屬性是授權伺服器端點的 URL 路徑。 這是 URL 的應用程式用來取得持有人權杖。

`Provider`屬性指定的提供者，可插入的 OWIN 中介軟體，並處理中介軟體所引發的事件。

當應用程式想要取得權杖時，以下是基本流程：

1. 若要取得存取權杖，應用程式傳送要求至 ~ / 語彙基元。
2. OAuth 的中介軟體呼叫`GrantResourceOwnerCredentials`的提供者。
3. 在提供者呼叫`ApplicationUserManager`驗證認證，並建立宣告識別。
4. 如果成功，此提供者會建立驗證票證，用來產生權杖。

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

OAuth 的中介軟體不了解使用者帳戶。 提供者會在的中介軟體和 ASP.NET 身分識別之間進行通訊。 如需有關如何實作授權伺服器的詳細資訊，請參閱 < [OWIN OAuth 2.0 授權伺服器](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md)。

### <a name="configuring-web-api-to-use-bearer-tokens"></a>設定要使用持有人權杖的 Web API

在 `WebApiConfig.Register`方法，下列程式碼會設定 Web API 管線的驗證：

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

**HostAuthenticationFilter**類別可讓您使用持有人權杖進行驗證。

**SuppressDefaultHostAuthentication**方法會告訴要忽略會在要求到達 Web API 管線在 IIS 或 OWIN 中介軟體之前需要驗證的 Web API。 如此一來，我們可以限制只使用持有人權杖進行驗證的 Web API。

> [!NOTE]
> 特別是，您的應用程式的 MVC 部分可能會使用表單驗證，將認證儲存在 cookie 中。 以 cookie 為基礎的驗證需要使用防偽權杖，以防止 CSRF 攻擊。 這是 web Api 的問題，因為 web API 傳送給用戶端的防偽語彙基元的便利方法。 (如需此問題有關的詳細背景，請參閱[Web API 中防止 CSRF 攻擊](preventing-cross-site-request-forgery-csrf-attacks.md)。)呼叫**SuppressDefaultHostAuthentication**可確保 Web API 不容易遭受 CSRF 攻擊從儲存在 cookie 中的認證。


當用戶端要求受保護的資源時，以下是 Web API 管線中發生的動作：

1. **HostAuthentication**篩選器會呼叫來驗證權杖的 OAuth 中介軟體。
2. 中介軟體會將語彙基元轉換成的宣告識別。
3. 此時，已要求*驗證*但不是*授權*。
4. 授權篩選條件會檢查宣告識別。 如果宣告授權該資源的使用者，請將授權要求。 根據預設， **[Authorize]** 屬性將會授權任何已驗證的要求。 不過，您可以授權角色或其他宣告。 如需詳細資訊，請參閱 <<c0> [ 驗證和授權的 Web API](authentication-and-authorization-in-aspnet-web-api.md)。
5. 如果先前的步驟都成功，控制器會傳回受保護的資源。 否則，用戶端會收到 401 （未經授權） 時發生錯誤。

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>其他資源

- [ASP.NET 身分識別](../../../identity/index.md)
- [了解 VS2013 RC SPA 範本中的安全性功能](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)。 MSDN 部落格文章所 Hongye Sun。
- [剖析 Web API 的個別帳戶範本 – 第 2 部分： 本機帳戶](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/)。 Dominick Baier 部落格文章。
- [驗證和 Web API 與 OWIN 主機](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/)。 合理的解釋`SuppressDefaultHostAuthentication`和`HostAuthenticationFilter`由 Brock Allen。
- [自訂 ASP.NET 身分識別中的設定檔資訊，在 VS 2013 範本](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)。 請參閱 Pranav rastogi 的 MSDN 部落格文章。
- [每個要求存留期管理，在 ASP.NET 身分識別的 UserManager 類別](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx)。 MSDN 部落格文章，Suhas Joshi 所使用的詳細說明`UserManager`類別。
