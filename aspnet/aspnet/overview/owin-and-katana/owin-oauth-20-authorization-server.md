---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: OWIN OAuth 2.0 授權伺服器 |Microsoft 文件
author: hongyes
description: 本教學課程將引導您如何實作 OAuth 2.0 授權伺服器使用 OWIN OAuth 中介軟體。 這是進階的教學課程的唯一 outlin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/20/2014
ms.topic: article
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: e5968f8d19191c3f44e9bd58f8e22a39d8d8faff
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="owin-oauth-20-authorization-server"></a>OWIN OAuth 2.0 授權伺服器
====================
由[Hongye Sun](https://github.com/hongyes)， [Praburaj Thiagarajan](https://github.com/Praburaj)， [Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將引導您如何實作 OAuth 2.0 授權伺服器使用 OWIN OAuth 中介軟體。 這是進階的教學課程，僅概述建立 OWIN OAuth 2.0 授權伺服器的步驟。 這不是逐步教學課程。 [下載範例程式碼](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip)。
> 
> > [!NOTE]
> > 此外框不被適合用來建立安全的生產應用程式。 本教學課程旨在提供如何實作 OAuth 2.0 授權伺服器使用 OWIN OAuth 中介軟體的外框。
> 
> 
> ## <a name="software-versions"></a>軟體版本
> 
> | **教學課程中所示** | **也可以搭配** |
> | --- | --- |
> | Windows 8.1 | Windows 8, Windows 7 |
> | [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) | [Visual Studio 2013 Express for Desktop](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express)。 Visual Studio 2012 最新的更新應該會運作，但本教學課程尚未經過測試，和某些功能表選取項目和對話方塊不同。 |
> | .NET 4.5 |  |
> 
> ## <a name="questions-and-comments"></a>問題和註解
> 
> 如果您有與本教學課程不直接相關的問題，您可以張貼在[GitHub 上的 Katana 專案](https://github.com/aspnet/AspNetKatana/)。 問題及本身的教學課程有關的註解，請參閱在頁面底部的註解區段。


[OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749)可讓協力廠商應用程式取得的有限的存取權的 HTTP 服務。 除了使用資源擁有者的認證存取受保護的資源，用戶端取得存取權杖 (這是字串，代表特定範圍、 存留期和其他存取屬性)。 存取權杖給第三方用戶端由發行授權伺服器的資源擁有者核准。

本教學課程將說明如何：

- 如何建立授權伺服器以支援四種授權授與類型，以及重新整理權杖：
    - 授權碼授與
    - 隱含授予
    - 認證授與的資源擁有者密碼
    - 用戶端認證授與
- 建立保護的存取權杖的資源伺服器。
- 建立 OAuth 2.0 用戶端。

<a id="prerequisites"></a>
## <a name="prerequisites"></a>必要條件

- [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions)或免費[Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express)，如下所示**軟體版本**頁面的頂端。
- OWIN 的認識。 請參閱[入門 Katana 專案](https://msdn.microsoft.com/magazine/dn451439.aspx)和[OWIN 和 Katana 中最新消息](index.md)。
- 熟悉[OAuth](http://tools.ietf.org/html/rfc6749)術語，包括[角色](http://tools.ietf.org/html/rfc6749#section-1.1)，[通訊協定流程](http://tools.ietf.org/html/rfc6749#section-1.2)，和[授權授與](http://tools.ietf.org/html/rfc6749#section-1.3)。 [OAuth 2.0 簡介](http://tools.ietf.org/html/rfc6749#section-1)提供了絕佳的簡介。

## <a name="create-an-authorization-server"></a>建立授權伺服器

在本教學課程中，我們大致勾勒出如何使用[OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx)和 ASP.NET MVC 建立授權伺服器。 我們希望很快就完成的範例中，提供下載，因為本教學課程中不包含每個步驟。 首先，建立名為空 web 應用程式*AuthorizationServer*並安裝下列封裝：

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Microsoft.Owin.Security.Google （或例如 Microsoft.Owin.Security.Facebook 套件的任何其他社交登入）

新增[OWIN 啟動 「 類別](owin-startup-class-detection.md)在專案根資料夾中名為*啟動*。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

建立*應用程式\_啟動*資料夾。 選取*應用程式\_啟動*資料夾，然後使用 Shift + Alt + A 將下載的版本*AuthorizationServer\App\_Start\Startup.Auth.cs*檔案。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

上述程式碼可讓應用程式/外部登入 cookie 和 Google 驗證授權伺服器本身會用它來管理帳戶。

`UseOAuthAuthorizationServer`設定授權伺服器是擴充方法。 安裝程式選項如下：

- `AuthorizeEndpointPath`： 要求路徑而用戶端應用程式重新導向使用者代理程式為了取得使用者同意發出的權杖或程式碼。 它必須以開頭前置斜線，例如，"`/Authorize`"。
- `TokenEndpointPath`: 要求路徑用戶端應用程式直接通訊以取得存取權杖。 它必須以斜線，例如"/token"開頭。 如果用戶端發出[用戶端\_密碼](http://tools.ietf.org/html/rfc6749#appendix-A.2)，它必須提供至此端點。
- `ApplicationCanDisplayErrors`： 設定為`true`如果 web 應用程式想要在產生的用戶端驗證錯誤的自訂錯誤頁面`/Authorize`端點。 這只必要的情況下，瀏覽器不會重新導向回用戶端應用程式，例如，當`client_id`或`redirect_uri`不正確。 `/Authorize`端點應該會看到 「 oauth。錯誤 」，「 oauth。ErrorDescription"和"oauth。ErrorUri"屬性會新增至 OWIN 環境。 

    > [!NOTE]
    > 如果不為 true，則授權伺服器會傳回預設錯誤網頁包含錯誤詳細資料。
- `AllowInsecureHttp`： 若要允許授權和語彙基元要求抵達 HTTP URI 位址，並允許連入`redirect_uri`授權要求參數具有 HTTP URI 位址。 

    > [!WARNING]
    > 安全性-這是只有開發。
- `Provider`: 提供可處理由授權伺服器中介軟體引發的事件的應用程式的物件。 應用程式可能完整，實作介面，或它可能會建立的執行個體`OAuthAuthorizationServerProvider`並指派此伺服器所支援的 OAuth 流程所需的委派。
- `AuthorizationCodeProvider`： 會產生單次使用授權碼，以傳回用戶端應用程式。 OAuth 伺服器可保護應用程式**必須**提供的執行個體`AuthorizationCodeProvider`權杖會由`OnCreate/OnCreateAsync`事件會被視為有效只有一個呼叫`OnReceive/OnReceiveAsync`。
- `RefreshTokenProvider`： 產生重新整理語彙基元，可能會用來產生新的存取權杖時所需。 如果未提供授權伺服器不會傳回重新整理權杖從`/Token`端點。

## <a name="account-management"></a>帳戶管理

OAuth 不在意您何處或如何管理您的使用者帳戶資訊。 它有[ASP.NET Identity](../../../identity/index.md)負責它。 在本教學課程中，我們將簡化帳戶管理程式碼，並請確定使用者可以登入使用 OWIN cookie 中介軟體。 以下是簡化的範例程式碼`AccountController`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri` 用來驗證用戶端，其已註冊的重新導向 url。 `ValidateClientAuthentication` 檢查基本配置標頭並取得用戶端認證的表單內容。

登入頁面如下所示：

![](owin-oauth-20-authorization-server/_static/image1.png)

檢閱 IETF OAuth 2[授權碼授與](http://tools.ietf.org/html/rfc6749#section-4.1)現在區段。 

**提供者**（在下表） 是[OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx)。型別提供者`OAuthAuthorizationServerProvider`，其中包含所有的 OAuth 伺服器事件。 

| 從授權碼授與區段的流程步驟 | 下載範例會執行這些步驟： |
| --- | --- |
|  |  |
| （A） 用戶端會起始流程，以控制程式資源擁有者的使用者代理程式的授權端點。 用戶端中包含用戶端識別碼、 要求的範圍，區域狀態中和重新導向 URI 的授權伺服器會傳送使用者代理程式上一步要等到授與 （或拒絕存取）。 | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| （B） 授權伺服器會驗證資源擁有者 （透過使用者-代理程式），並建立資源擁有者是否會授與或拒絕用戶端的存取要求。 | **&lt;如果使用者授與存取&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| （C） 假設資源擁有者授與存取權，授權伺服器將使用者代理程式重新導向回到用戶端會使用重新導向 URI 提供舊版 （或在要求中用戶端註冊期間）。 ... |  |
|  |  |
| （D） 用戶端向授權伺服器的權杖端點要求存取權杖，加上一個步驟中收到的授權碼。 當提出要求，用戶端驗證與授權伺服器。 用戶端中包含重新導向 URI 用來取得授權碼，以便驗證。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |

如需範例實作`AuthorizationCodeProvider.CreateAsync`和`ReceiveAsync`來控制建立和驗證授權碼如下所示。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

上述程式碼會使用記憶體中並行字典來儲存程式碼和身分識別的票證，還原之後收到程式碼的身分識別。 在實際應用中，它會取代永續性資料存放區。 資源擁有者授與存取權的用戶端，則授權端點。 通常，它需要使用者介面，以讓使用者按一下按鈕時，確認授權。 OWIN OAuth 中介軟體可讓應用程式程式碼來處理授權端點。 在我們的範例應用程式，我們會使用呼叫 MVC 控制器`OAuthController`來處理它。 以下是範例實作：

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

`Authorize`動作會先檢查使用者是否已登入授權伺服器。 如果沒有，驗證中介軟體挑戰使用 「 應用程式 」 cookie 進行驗證呼叫者，並將重新導向至登入頁面。 （請參閱上述的反白顯示程式碼）。如果使用者已登入，它會轉譯 [Authorize] 檢視，如下所示：

![](owin-oauth-20-authorization-server/_static/image2.png)

如果**Grant**選取按鈕時，`Authorize`動作將會建立新的"Bearer"身分識別和使用它登入。 它將會觸發要產生承載語彙基元，並將它傳送回用戶端，JSON 裝載之授權伺服器。 

### <a name="implicit-grant"></a>隱含授予

請參閱 IETF OAuth 2[隱含授予](http://tools.ietf.org/html/rfc6749#section-4.2)現在區段。

 [隱含授予](http://tools.ietf.org/html/rfc6749#section-4.2)圖 4 所示的流程是流程和對應的 OWIN OAuth 的中介軟體之後。  

| 資料流程從隱含授予 > 一節的步驟 | 下載範例會執行這些步驟： |
| --- | --- |
|  |  |
| （A） 用戶端會起始流程，以控制程式資源擁有者的使用者代理程式的授權端點。 用戶端中包含用戶端識別碼、 要求的範圍，區域狀態中和重新導向 URI 的授權伺服器會傳送使用者代理程式上一步要等到授與 （或拒絕存取）。 | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| （B） 授權伺服器會驗證資源擁有者 （透過使用者-代理程式），並建立資源擁有者是否會授與或拒絕用戶端的存取要求。 | **&lt;如果使用者授與存取&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| （C） 假設資源擁有者授與存取權，授權伺服器將使用者代理程式重新導向回到用戶端會使用重新導向 URI 提供舊版 （或在要求中用戶端註冊期間）。 ... |  |
|  |  |
| （D） 用戶端向授權伺服器的權杖端點要求存取權杖，加上一個步驟中收到的授權碼。 當提出要求，用戶端驗證與授權伺服器。 用戶端中包含重新導向 URI 用來取得授權碼，以便驗證。 |  |

因為我們已實作授權端點 (`OAuthController.Authorize`動作) 的授權碼授與，它會自動啟用也隱含流程。 注意：`Provider.ValidateClientRedirectUri`用來驗證其重新導向 URL，防止隱含授與流程傳送存取權杖來惡意用戶端的用戶端識別碼 ([攔截攻擊](https://www.owasp.org/index.php/Man-in-the-middle_attack))。

### <a name="resource-owner-password-credentials-grant"></a>認證授與的資源擁有者密碼

請參閱 IETF OAuth 2[資源擁有者密碼認證授與](http://tools.ietf.org/html/rfc6749#section-4.3)現在區段。

 [資源擁有者密碼認證授與](http://tools.ietf.org/html/rfc6749#section-4.3)圖 5 所示的流程是流程和對應的 OWIN OAuth 的中介軟體之後。  

| 從資源擁有者密碼認證授與區段的流程步驟 | 下載範例會執行這些步驟： |
| --- | --- |
|  |  |
| （A） 資源擁有者會為用戶端提供其使用者名稱和密碼。 |  |
|  |  |
| （B） 用戶端包括資源擁有者從收到的認證，向授權伺服器的權杖端點要求存取權杖。 當提出要求，用戶端驗證與授權伺服器。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| （C） 授權伺服器上驗證用戶端會驗證資源的擁有者認證，和如果有效，就會發出存取權杖。 |  |

以下是範例實作`Provider.GrantResourceOwnerCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> 上述程式碼用來說明本章節的教學課程，不應該使用安全或實際執行應用程式。 它不會檢查資源擁有者認證。 它會假設每個認證有效，並為其建立新的識別。 新的身分識別將用來產生存取權杖和重新整理權杖中。 程式碼取代您自己的安全帳戶管理程式碼。


### <a name="client-credentials-grant"></a>用戶端認證授與

請參閱 IETF OAuth 2[用戶端認證授與](http://tools.ietf.org/html/rfc6749#section-4.4)現在區段。

 [用戶端認證授與](http://tools.ietf.org/html/rfc6749#section-4.4)流程  濆迶 6 是流程和對應的 OWIN OAuth 的中介軟體之後。  

| 從用戶端認證授與區段的流程步驟 | 下載範例會執行這些步驟： |
| --- | --- |
|  |  |
| （A） 用戶端驗證與授權伺服器，並從權杖端點要求存取權杖。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| （B） 授權伺服器上驗證用戶端，以及如果有效，就會發出存取權杖。 |  |

以下是範例實作`Provider.GrantClientCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> 上述程式碼用來說明本章節的教學課程，不應該使用安全或實際執行應用程式。 程式碼取代您自己的安全用戶端管理程式碼。


### <a name="refresh-token"></a>重新整理權杖

請參閱 IETF OAuth 2[重新整理權杖](http://tools.ietf.org/html/rfc6749#section-1.5)現在區段。

 [重新整理權杖](http://tools.ietf.org/html/rfc6749#section-1.5)流程顯示在圖 2 是流程和對應的 OWIN OAuth 的中介軟體之後。  

| 從用戶端認證授與區段的流程步驟 | 下載範例會執行這些步驟： |
| --- | --- |
|  |  |
| （G） 用戶端會要求新的存取權杖，向授權伺服器，並提供給重新整理權杖。 用戶端驗證需求會根據用戶端類型和授權伺服器原則。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| （H） 授權伺服器會驗證用戶端和驗證重新整理權杖，以及如果有效，就會發出新的存取權杖 （並選擇性地將新的重新整理權杖）。 |  |

以下是範例實作`Provider.GrantRefreshToken`: 

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>建立資源伺服器受到由存取權杖

建立空 web 應用程式專案，並安裝下列專案中的封裝：

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

建立啟動類別，並設定驗證和 Web API。 請參閱*AuthorizationServer\ResourceServer\Startup.cs*下載範例中。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

請參閱*AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs*下載範例中。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

請參閱*AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs*下載範例中。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors` 方法允許所有網域的 CORS。
- `UseOAuthBearerAuthentication` 方法可讓 OAuth 承載權杖驗證中介軟體將會接收及驗證來自授權要求標頭的持有人權杖。
- `Config.SuppressDefaultHostAuthenticaiton` 預設會隱藏主機驗證的主體，從應用程式，因此所有要求將會都是匿名的這個呼叫之後。
- `HostAuthenticationFilter` 啟用驗證，只針對指定的驗證類型。 在此情況下，它是承載驗證類型。

為了示範驗證的識別，我們建立 ApiController 輸出目前使用者的宣告。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

如果授權伺服器的資源伺服器，並不在同一部電腦上，OAuth 的中介軟體會使用不同的電腦金鑰來加密和解密持有人的存取權杖。 若要共用相同的私用金鑰，這兩個專案之間，我們加入相同`machinekey`中同時設定*web.config*檔案。

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>建立 OAuth 2.0 用戶端

 我們使用[DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) NuGet 封裝，以簡化用戶端程式碼。

### <a name="authorization-code-grant-client"></a>授權碼授與用戶端

 此用戶端是 MVC 應用程式。 它將會觸發從後端，取得存取權杖的授權碼授與流程。 它具有的單一頁面，如下所示：

![](owin-oauth-20-authorization-server/_static/image3.png)

- **授權**按鈕會瀏覽器重新導向至授權伺服器，以通知資源擁有者授與此用戶端存取。
- **重新整理**按鈕將會取得新存取權杖和重新整理權杖使用目前的重新整理權杖。
- **存取受保護資源 API**按鈕會呼叫以取得目前使用者的宣告資料，並在頁面上顯示它們的資源伺服器。

以下是範例程式碼的`HomeController`的用戶端。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth` 依預設需要 SSL。 因為我們的示範，使用 HTTP，您需要加入下列組態檔中的設定：

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> 安全性-永遠不會停用 SSL 生產環境應用程式中。 現在純文字方式透過網路傳送登入認證。 上述程式碼只適用於本機範例偵錯和瀏覽。


### <a name="implicit-grant-client"></a>隱含授與用戶端

此用戶端會使用 JavaScript 來：

1. 開啟新視窗，並重新導向至授權伺服器的授權端點。
2. 從取得存取權杖 URL 片段時將重新導向回。

下圖顯示此程序：

![](owin-oauth-20-authorization-server/_static/image4.png)

用戶端應該有兩個頁面： 一個用於首頁上，另一個則用於回呼。以下是範例程式碼中找到的 JavaScript *Index.cshtml*檔案：

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

以下是處理程式碼中的回呼*SignIn.cshtml*檔案：

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> 最佳作法是將 JavaScript 移至外部檔案，並不將它內嵌 Razor 標記。 為了簡化這個範例中，在經過組合。


### <a name="resource-owner-password-credentials-grant-client"></a>資源擁有者密碼認證授與用戶端

我們要示範此用戶端使用的主控台應用程式。 下列程式碼：

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>用戶端認證授與用戶端

類似資源的擁有者密碼認證授與，以下是主控台應用程式程式碼：

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
