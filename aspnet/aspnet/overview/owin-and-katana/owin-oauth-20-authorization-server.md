---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: OWIN OAuth 2.0 授權伺服器 |Microsoft Docs
author: hongyes
description: 本教學課程將引導您如何實作 OAuth 2.0 授權伺服器使用 OWIN OAuth 中介軟體。 這是進階的教學課程，只有 outlin...
ms.author: aspnetcontent
ms.date: 03/20/2014
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: e3b5b37b4f22f3c59d3c1f4043e9b52e46a8926b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828415"
---
<a name="owin-oauth-20-authorization-server"></a>OWIN OAuth 2.0 授權伺服器
====================
藉由[Hongye Sun](https://github.com/hongyes)， [Praburaj Thiagarajan](https://github.com/Praburaj)， [Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將引導您如何實作 OAuth 2.0 授權伺服器使用 OWIN OAuth 中介軟體。 這是僅概述的步驟來建立 OWIN OAuth 2.0 授權伺服器的進階教學課程。 這不是逐步教學課程。 [下載範例程式碼](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip)。
> 
> > [!NOTE]
> > 這個外框，則不應該適合用來建立安全的生產應用程式。 本教學課程旨在提供有關如何實作 OAuth 2.0 授權伺服器使用 OWIN OAuth 中介軟體的外框。
> 
> 
> ## <a name="software-versions"></a>軟體版本
> 
> | **在本教學課程中所示** | **也可以搭配** |
> | --- | --- |
> | Windows 8.1 | Windows 8，Windows 7 |
> | [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) | [Visual Studio 2013 Express for Desktop](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express)。 Visual Studio 2012 最新的更新應該可行，但本教學課程尚未經過測試，且某些功能表選取項目和對話方塊都不同。 |
> | .NET 4.5 |  |
> 
> ## <a name="questions-and-comments"></a>提出問題或意見
> 
> 如果您有不直接相關的教學課程中的問題，您可以將它們在公佈[Katana 專案，GitHub 上](https://github.com/aspnet/AspNetKatana/)。 如需問題和針對本教學課程有關的註解，請參閱在頁面底部的註解區段。


[OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749)可讓協力廠商應用程式取得的有限的存取權的 HTTP 服務。 而不是您可以使用資源擁有者的認證來存取受保護的資源，用戶端取得存取權杖 (這是字串，表示特定的範圍、 存留期和其他存取屬性)。 存取權杖是協力廠商用戶端中，由授權伺服器發行的資源擁有者核准。

本教學課程將涵蓋：

- 如何建立授權伺服器以支援四種授權授與類型，以及重新整理權杖：
    - 授權碼授與
    - 隱含授與
    - 資源擁有者密碼認證授與
    - 用戶端認證授與
- 建立受到存取權杖的資源伺服器。
- 建立 OAuth 2.0 用戶端。

<a id="prerequisites"></a>
## <a name="prerequisites"></a>必要條件

- [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions)或免費[Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express)，如下所示**軟體版本**頁面的頂端。
- OWIN 熟悉。 請參閱[Getting Started with Katana 專案](https://msdn.microsoft.com/magazine/dn451439.aspx)並[OWIN 和 Katana 中最新消息](index.md)。
- 熟悉[OAuth](http://tools.ietf.org/html/rfc6749)術語，包括[角色](http://tools.ietf.org/html/rfc6749#section-1.1)，[通訊協定流程](http://tools.ietf.org/html/rfc6749#section-1.2)，以及[授權授與](http://tools.ietf.org/html/rfc6749#section-1.3)。 [OAuth 2.0 簡介](http://tools.ietf.org/html/rfc6749#section-1)提供了絕佳的簡介。

## <a name="create-an-authorization-server"></a>建立授權伺服器

在本教學課程中，我們大致上描繪出如何使用[OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx)和 ASP.NET MVC 建立授權伺服器。 我們希望即將提供下載完整的範例中，如本教學課程中不包含每個步驟所示。 首先，建立名為空的 web 應用程式*AuthorizationServer*並安裝下列封裝：

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Microsoft.Owin.Security.Google （或任何其他社交登入封裝，例如 Microsoft.Owin.Security.Facebook）

新增[OWIN 啟動類別](owin-startup-class-detection.md)在專案根資料夾中名為*啟動*。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

建立*應用程式\_啟動*資料夾。 選取 *應用程式\_開始*資料夾，然後使用 Shift + Alt + A 將已下載的版本*AuthorizationServer\App\_Start\Startup.Auth.cs*檔案。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

上述程式碼可讓應用程式/外部登入 cookie 和 Google 驗證其本身的授權伺服器會用它來管理帳戶。

`UseOAuthAuthorizationServer`擴充方法是設定授權伺服器。 安裝程式選項如下：

- `AuthorizeEndpointPath`: 要求路徑，而用戶端應用程式重新導向 user-agent 以取得使用者同意發出的權杖或程式碼。 其開頭必須前置斜線，比方說，「`/Authorize`"。
- `TokenEndpointPath`: 要求路徑用戶端應用程式直接進行通訊來取得存取權杖。 它必須以斜線，例如"/token"開頭。 如果用戶端會發出[用戶端\_祕密](http://tools.ietf.org/html/rfc6749#appendix-A.2)，它必須提供至此端點。
- `ApplicationCanDisplayErrors`： 設定為`true`如果想要產生的用戶端驗證錯誤的自訂錯誤頁面上的 web 應用程式`/Authorize`端點。 這只需要的情況下，瀏覽器不會重新導向回用戶端應用程式，例如，當`client_id`或`redirect_uri`不正確。 `/Authorize`端點應該會看到 「 oauth。錯誤 」、 「 oauth。ErrorDescription"和"oauth。ErrorUri"屬性會新增至 OWIN 環境。 

    > [!NOTE]
    > 如果沒有，則為 true，授權伺服器會傳回預設的錯誤網頁的錯誤詳細資料。
- `AllowInsecureHttp`: True，以允許授權和語彙基元要求抵達 HTTP URI 位址，並允許連入`redirect_uri`授權要求參數具有 HTTP URI 位址。 

    > [!WARNING]
    > 安全性-這是只有開發。
- `Provider`: 應用程式提供給授權伺服器中介軟體所引發的程序事件的物件。 應用程式可能完整，實作介面，或它可能會建立的執行個體`OAuthAuthorizationServerProvider`並指派此伺服器所支援的 OAuth 流程所需的委派。
- `AuthorizationCodeProvider`： 產生單次使用授權碼傳回至用戶端應用程式。 OAuth 伺服器可保護應用程式**必須**提供的執行個體`AuthorizationCodeProvider`語彙基元所產生的程式`OnCreate/OnCreateAsync`事件會被視為適用於只有一個呼叫`OnReceive/OnReceiveAsync`。
- `RefreshTokenProvider`： 產生可用來產生新的存取權杖時所需的重新整理權杖。 如果未提供授權伺服器不會傳回來自重新整理權杖`/Token`端點。

## <a name="account-management"></a>帳戶管理

OAuth 不在意您何處或如何管理您的使用者帳戶資訊。 它有[ASP.NET Identity](../../../identity/index.md)即負責。 在本教學課程中，我們將簡化帳戶管理的程式碼，並先確定該使用者可以使用 OWIN cookie 中介軟體登入。 以下是簡化的範例程式碼`AccountController`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri` 用來驗證用戶端，其已註冊的重新導向 url。 `ValidateClientAuthentication` 檢查基本配置標頭和表單內文，以取得用戶端的認證。

登入頁面如下所示：

![](owin-oauth-20-authorization-server/_static/image1.png)

檢閱 IETF OAuth 2[授權碼授與](http://tools.ietf.org/html/rfc6749#section-4.1)現在區段。 

**提供者**（在下表中） 是[OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx)。提供者，也就是型別的`OAuthAuthorizationServerProvider`，其中包含所有的 OAuth 伺服器事件。 

| 從授權碼授與區段的流程步驟 | 下載範例會執行這些步驟： |
| --- | --- |
|  |  |
| （A） 用戶端導向資源擁有者的使用者代理程式的授權端點以起始流程。 用戶端會包含其用戶端識別碼、 要求的範圍、 本機狀態和重新導向 URI 的授權伺服器會將 user-agent 回後授與 （或拒絕存取）。 | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| （B） 授權伺服器會驗證資源擁有者 （透過使用者-代理程式），然後表明資源擁有者授與或拒絕用戶端的存取要求。 | **&lt;如果使用者授與存取權&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| （C） 假設資源擁有者授與存取，授權伺服器會將使用者代理程式重新導向回用戶端會使用重新導向 URI 提供先前 （在要求中或在用戶端註冊）。 ... |  |
|  |  |
| （D) 用戶端向授權伺服器的權杖端點要求存取權杖，加上一個步驟中收到的授權碼。 當提出要求，用戶端會向授權伺服器。 用戶端會包含重新導向 URI 用來取得授權碼，以便驗證。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |

如需範例實作`AuthorizationCodeProvider.CreateAsync`和`ReceiveAsync`來控制建立和驗證授權碼如下所示。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

上述程式碼會使用記憶體中的並行字典來儲存程式碼和身分識別的票證和還原之後接收程式碼的身分識別。 在實際的應用程式，它會由持續性資料存放區將其取代。 授權端點做為資源擁有者授與存取權的用戶端。 通常，它需要的使用者介面，以便使用者按一下按鈕，並確認授與。 OWIN OAuth 中介軟體可讓應用程式程式碼來處理授權端點。 在我們的範例應用程式，我們會使用 MVC 控制器呼叫`OAuthController`來加以處理。 以下是範例實作：

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

`Authorize`動作會先檢查使用者是否已登入授權伺服器。 如果沒有，驗證中介軟體挑戰使用 「 應用程式 」 cookie 來驗證呼叫端，並將重新導向至登入頁面。 （請參閱上述的反白顯示程式碼）。如果使用者已登入，它會轉譯 [Authorize] 檢視中，如下所示：

![](owin-oauth-20-authorization-server/_static/image2.png)

如果**Grant**  按鈕已選取，`Authorize`動作將會建立新的"Bearer"身分識別和使用它登入。 它會觸發產生持有人權杖，並將它傳送回 JSON 裝載的用戶端的授權伺服器。 

### <a name="implicit-grant"></a>隱含授與

請參閱 IETF OAuth 2[隱含授與](http://tools.ietf.org/html/rfc6749#section-4.2)現在區段。

 [隱含授與](http://tools.ietf.org/html/rfc6749#section-4.2)[圖 4] 所示的 flow，流程，以及對應的 OWIN OAuth 則中介軟體會遵循。  

| 隱含授與一節中的流程步驟 | 下載範例會執行這些步驟： |
| --- | --- |
|  |  |
| （A） 用戶端導向資源擁有者的使用者代理程式的授權端點以起始流程。 用戶端會包含其用戶端識別碼、 要求的範圍、 本機狀態和重新導向 URI 的授權伺服器會將 user-agent 回後授與 （或拒絕存取）。 | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| （B） 授權伺服器會驗證資源擁有者 （透過使用者-代理程式），然後表明資源擁有者授與或拒絕用戶端的存取要求。 | **&lt;如果使用者授與存取權&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| （C） 假設資源擁有者授與存取，授權伺服器會將使用者代理程式重新導向回用戶端會使用重新導向 URI 提供先前 （在要求中或在用戶端註冊）。 ... |  |
|  |  |
| （D) 用戶端向授權伺服器的權杖端點要求存取權杖，加上一個步驟中收到的授權碼。 當提出要求，用戶端會向授權伺服器。 用戶端會包含重新導向 URI 用來取得授權碼，以便驗證。 |  |

因為我們已實作授權端點 (`OAuthController.Authorize`動作) 的授權碼授與，因此它會自動啟用也隱含流程。 注意︰`Provider.ValidateClientRedirectUri`用來驗證具有其傳送存取權杖來惡意用戶端時，防止隱含授與流程的重新導向 URL 的用戶端識別碼 ([攔截攻擊](https://www.owasp.org/index.php/Man-in-the-middle_attack))。

### <a name="resource-owner-password-credentials-grant"></a>資源擁有者密碼認證授與

請參閱 IETF OAuth 2[資源擁有者密碼認證授與](http://tools.ietf.org/html/rfc6749#section-4.3)現在區段。

 [資源擁有者密碼認證授與](http://tools.ietf.org/html/rfc6749#section-4.3)[圖 5] 所示的 flow，流程，以及對應的 OWIN OAuth 則中介軟體會遵循。  

| 從資源擁有者密碼認證授與區段的流程步驟 | 下載範例會執行這些步驟： |
| --- | --- |
|  |  |
| （A） 資源擁有者會為用戶端提供自己的使用者名稱和密碼。 |  |
|  |  |
| （B） 用戶端包含來自資源擁有者的認證，從授權伺服器的權杖端點要求存取權杖。 當提出要求，用戶端會向授權伺服器。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| （C） 授權伺服器會驗證用戶端和驗證資源擁有者認證，然後如果有效，就會發出存取權杖。 |  |

以下是範例實作`Provider.GrantResourceOwnerCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> 上述程式碼用來說明本教學課程的這一節，不應在安全或生產環境應用程式。 它不會檢查資源擁有者認證。 它會假設每個認證有效，並為其建立新的身分識別。 新的身分識別將用來產生存取權杖和重新整理權杖中。 程式碼取代您自己的安全帳戶管理程式碼。


### <a name="client-credentials-grant"></a>用戶端認證授與

請參閱 IETF OAuth 2[用戶端認證授與](http://tools.ietf.org/html/rfc6749#section-4.4)現在區段。

 [用戶端認證授與](http://tools.ietf.org/html/rfc6749#section-4.4)[圖 6] 所示的 flow，流程，以及對應的 OWIN OAuth 則中介軟體會遵循。  

| 從用戶端認證授與區段的流程步驟 | 下載範例會執行這些步驟： |
| --- | --- |
|  |  |
| （A) 用戶端向授權伺服器，並向權杖端點要求存取權杖。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| （B） 授權伺服器會驗證用戶端，然後如果有效，就會發出存取權杖。 |  |

以下是範例實作`Provider.GrantClientCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> 上述程式碼用來說明本教學課程的這一節，不應在安全或生產環境應用程式。 程式碼取代您自己的安全用戶端管理程式碼。


### <a name="refresh-token"></a>重新整理權杖

請參閱 IETF OAuth 2[重新整理權杖](http://tools.ietf.org/html/rfc6749#section-1.5)現在區段。

 [重新整理權杖](http://tools.ietf.org/html/rfc6749#section-1.5)[圖 2] 所示的 flow，流程，以及對應的 OWIN OAuth 則中介軟體會遵循。  

| 從用戶端認證授與區段的流程步驟 | 下載範例會執行這些步驟： |
| --- | --- |
|  |  |
| （G） 用戶端會要求新的存取權杖，向授權伺服器，並呈現重新整理權杖。 用戶端驗證需求會根據用戶端類型和授權伺服器原則。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsyncRefreshTokenProvider.CreateAsync |
|  |  |
| （H） 授權伺服器會驗證用戶端和驗證重新整理權杖，然後如果有效，就會發出新的存取權杖 （以及選擇性地為新的重新整理語彙基元）。 |  |

以下是範例實作`Provider.GrantRefreshToken`: 

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>建立資源的伺服器保護的存取權杖

建立空的 web 應用程式專案並安裝下列專案中的封裝：

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

建立啟動類別，並設定驗證和 Web API。 請參閱*AuthorizationServer\ResourceServer\Startup.cs*下載範例中。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

請參閱*AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs*下載範例中。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

請參閱*AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs*下載範例中。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors` 方法會允許所有網域的 CORS。
- `UseOAuthBearerAuthentication` 方法可讓 OAuth 持有人權杖驗證中介軟體會接收及驗證從要求中的授權標頭的持有人權杖。
- `Config.SuppressDefaultHostAuthenticaiton` 隱藏預設主機驗證的主體，從應用程式，因此所有的要求將會是匿名的這個呼叫之後。
- `HostAuthenticationFilter` 啟用驗證，只針對指定的驗證類型。 在此情況下，它會是持有人驗證類型。

為了示範已驗證的身分識別，我們會建立 ApiController 輸出目前的使用者宣告。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

如果授權伺服器和資源伺服器不在相同電腦上，OAuth 的中介軟體會使用不同的機器金鑰來加密和解密的持有人存取權杖。 若要共用相同的私人金鑰，這兩個專案之間，我們將新增相同`machinekey`設定中都*web.config*檔案。

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>建立 OAuth 2.0 用戶端

 我們會使用[DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) NuGet 封裝來簡化用戶端程式碼。

### <a name="authorization-code-grant-client"></a>授權碼授與用戶端

 此用戶端是在 MVC 應用程式。 它會觸發從後端取得存取權杖的授權碼授與流程。 它會有單一頁面，如下所示：

![](owin-oauth-20-authorization-server/_static/image3.png)

- **授權**按鈕將瀏覽器重新導向至授權伺服器，以通知資源擁有者存取權授與此用戶端。
- **重新整理**按鈕會取得新存取權杖和重新整理權杖，使用目前的重新整理權杖。
- **存取受保護的資源 API**按鈕會呼叫資源伺服器，以取得目前使用者的宣告資料，並在頁面上顯示它們。

以下是範例程式碼的`HomeController`的用戶端。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth` 根據預設，需要 SSL。 由於我們的示範，使用 HTTP，您必須新增下列組態檔中的設定：

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> 安全性-永遠不會停用 SSL 生產應用程式中。 現在在純文字透過網路傳送登入認證。 上述程式碼只適用於本機範例偵錯和探索。


### <a name="implicit-grant-client"></a>隱含授與用戶端

此用戶端使用 JavaScript 來：

1. 開啟新視窗，並將重新導向至授權端點的授權伺服器。
2. 從取得存取權杖 URL 片段時將重新導向回。

下圖顯示此程序：

![](owin-oauth-20-authorization-server/_static/image4.png)

用戶端應該有兩個頁面： 一個用於首頁上，另一個則用於回呼。以下是範例程式碼中找到的 JavaScript *Index.cshtml*檔案：

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

以下是處理程式碼中的回呼*SignIn.cshtml*檔案：

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> 最佳的作法是將 JavaScript 移至外部檔案，並不將它內嵌加上 Razor 標記。 為了簡化此範例中，在經過組合。


### <a name="resource-owner-password-credentials-grant-client"></a>資源擁有者密碼認證授與用戶端

我們可以使用主控台應用程式來示範此用戶端。 程式碼如下：

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>用戶端認證授與用戶端

類似於資源擁有者密碼認證授與，以下是主控台應用程式程式碼：

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
