---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: OWIN OAuth 2.0 授權伺服器 |Microsoft Docs
author: hongyes
description: 本教學課程將引導您如何實作 OAuth 2.0 授權伺服器使用 OWIN OAuth 中介軟體。 這是進階的教學課程，只有 outlin...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: b8451d2d9e346bd5e2f51ba45e48030a5221b549
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667644"
---
# <a name="owin-oauth-20-authorization-server"></a><span data-ttu-id="3ffb7-104">OWIN OAuth 2.0 授權伺服器</span><span class="sxs-lookup"><span data-stu-id="3ffb7-104">OWIN OAuth 2.0 Authorization Server</span></span>

> <span data-ttu-id="3ffb7-105">本教學課程將引導您如何實作 OAuth 2.0 授權伺服器使用 OWIN OAuth 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-105">This tutorial will guide you on how to implement an OAuth 2.0 Authorization Server using OWIN OAuth middleware.</span></span> <span data-ttu-id="3ffb7-106">這是僅概述的步驟來建立 OWIN OAuth 2.0 授權伺服器的進階教學課程。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-106">This is an advanced tutorial that only outlines the steps to create an OWIN OAuth 2.0 Authorization Server.</span></span> <span data-ttu-id="3ffb7-107">這不是逐步教學課程。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-107">This is not a step by step tutorial.</span></span> <span data-ttu-id="3ffb7-108">[下載範例程式碼](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip)。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-108">[Download the sample code](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).</span></span>
>
> > [!NOTE]
> > <span data-ttu-id="3ffb7-109">這個外框，則不應該適合用來建立安全的生產應用程式。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-109">This outline should not be intended to be used for creating a secure production app.</span></span> <span data-ttu-id="3ffb7-110">本教學課程旨在提供有關如何實作 OAuth 2.0 授權伺服器使用 OWIN OAuth 中介軟體的外框。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-110">This tutorial is intended to provide only an outline on how to implement an OAuth 2.0 Authorization Server using OWIN OAuth middleware.</span></span>
>
>
> ## <a name="software-versions"></a><span data-ttu-id="3ffb7-111">軟體版本</span><span class="sxs-lookup"><span data-stu-id="3ffb7-111">Software versions</span></span>
>
> | <span data-ttu-id="3ffb7-112">**在本教學課程中所示**</span><span class="sxs-lookup"><span data-stu-id="3ffb7-112">**Shown in the tutorial**</span></span> | <span data-ttu-id="3ffb7-113">**也可以搭配**</span><span class="sxs-lookup"><span data-stu-id="3ffb7-113">**Also works with**</span></span> |
> | --- | --- |
> | <span data-ttu-id="3ffb7-114">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="3ffb7-114">Windows 8.1</span></span> | <span data-ttu-id="3ffb7-115">Windows 10，8，Windows 7</span><span class="sxs-lookup"><span data-stu-id="3ffb7-115">Windows 10, Windows 8, Windows 7</span></span> |
> | [<span data-ttu-id="3ffb7-116">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="3ffb7-116">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> | <span data-ttu-id="3ffb7-117">.NET 4.7.2</span><span class="sxs-lookup"><span data-stu-id="3ffb7-117">.NET 4.7.2</span></span> |  |
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="3ffb7-118">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="3ffb7-118">Questions and comments</span></span>
>
> <span data-ttu-id="3ffb7-119">如果您有不直接相關的教學課程中的問題，您可以將它們在公佈[Katana 專案，GitHub 上](https://github.com/aspnet/AspNetKatana/)。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-119">If you have questions that are not directly related to the tutorial, you can post them at [Katana Project on GitHub](https://github.com/aspnet/AspNetKatana/).</span></span> <span data-ttu-id="3ffb7-120">如需問題和針對本教學課程有關的註解，請參閱在頁面底部的註解區段。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-120">For questions and comments regarding the tutorial itself, see the comments section at the bottom of the page.</span></span>


<span data-ttu-id="3ffb7-121">[OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749)可讓協力廠商應用程式取得的有限的存取權的 HTTP 服務。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-121">The [OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) enables a third-party app to obtain limited access to an HTTP service.</span></span> <span data-ttu-id="3ffb7-122">而不是您可以使用資源擁有者的認證來存取受保護的資源，用戶端取得存取權杖 (這是字串，表示特定的範圍、 存留期和其他存取屬性)。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-122">Instead of using the resource owner's credentials to access a protected resource, the client obtains an access token (which is a string denoting a specific scope, lifetime, and other access attributes).</span></span> <span data-ttu-id="3ffb7-123">存取權杖是協力廠商用戶端中，由授權伺服器發行的資源擁有者核准。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-123">Access tokens are issued to third-party clients by an authorization server with the approval of the resource owner.</span></span>

<span data-ttu-id="3ffb7-124">本教學課程將涵蓋：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-124">This tutorial will cover:</span></span>

- <span data-ttu-id="3ffb7-125">如何建立授權伺服器以支援四種授權授與類型，以及重新整理權杖：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-125">How to create an authorization server to support four authorization grant types and refresh tokens:</span></span>
    - <span data-ttu-id="3ffb7-126">授權碼授與</span><span class="sxs-lookup"><span data-stu-id="3ffb7-126">Authorization code grant</span></span>
    - <span data-ttu-id="3ffb7-127">隱含授與</span><span class="sxs-lookup"><span data-stu-id="3ffb7-127">Implicit Grant</span></span>
    - <span data-ttu-id="3ffb7-128">資源擁有者密碼認證授與</span><span class="sxs-lookup"><span data-stu-id="3ffb7-128">Resource Owner Password Credentials Grant</span></span>
    - <span data-ttu-id="3ffb7-129">用戶端認證授與</span><span class="sxs-lookup"><span data-stu-id="3ffb7-129">Client Credentials Grant</span></span>
- <span data-ttu-id="3ffb7-130">建立受到存取權杖的資源伺服器。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-130">Creating a resource server which is protected by an access token.</span></span>
- <span data-ttu-id="3ffb7-131">建立 OAuth 2.0 用戶端。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-131">Creating OAuth 2.0 clients.</span></span>

<a id="prerequisites"></a>
## <a name="prerequisites"></a><span data-ttu-id="3ffb7-132">必要條件</span><span class="sxs-lookup"><span data-stu-id="3ffb7-132">Prerequisites</span></span>

- <span data-ttu-id="3ffb7-133">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)中所示**軟體版本**頁面的頂端。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-133">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) as indicated in **Software Versions** at the top of the page.</span></span>
- <span data-ttu-id="3ffb7-134">OWIN 熟悉。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-134">Familiarity with OWIN.</span></span> <span data-ttu-id="3ffb7-135">請參閱[Getting Started with Katana 專案](https://msdn.microsoft.com/magazine/dn451439.aspx)並[OWIN 和 Katana 中最新消息](index.md)。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-135">See [Getting Started with the Katana Project](https://msdn.microsoft.com/magazine/dn451439.aspx) and [What's new in OWIN and Katana](index.md).</span></span>
- <span data-ttu-id="3ffb7-136">熟悉[OAuth](http://tools.ietf.org/html/rfc6749)術語，包括[角色](http://tools.ietf.org/html/rfc6749#section-1.1)，[通訊協定流程](http://tools.ietf.org/html/rfc6749#section-1.2)，以及[授權授與](http://tools.ietf.org/html/rfc6749#section-1.3)。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-136">Familiarity with [OAuth](http://tools.ietf.org/html/rfc6749) terminology, including [Roles](http://tools.ietf.org/html/rfc6749#section-1.1), [Protocol Flow](http://tools.ietf.org/html/rfc6749#section-1.2), and [Authorization Grant](http://tools.ietf.org/html/rfc6749#section-1.3).</span></span> <span data-ttu-id="3ffb7-137">[OAuth 2.0 簡介](http://tools.ietf.org/html/rfc6749#section-1)提供了絕佳的簡介。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-137">[OAuth 2.0 introduction](http://tools.ietf.org/html/rfc6749#section-1) provides a good introduction.</span></span>

## <a name="create-an-authorization-server"></a><span data-ttu-id="3ffb7-138">建立授權伺服器</span><span class="sxs-lookup"><span data-stu-id="3ffb7-138">Create an Authorization Server</span></span>

<span data-ttu-id="3ffb7-139">在本教學課程中，我們大致上描繪出如何使用[OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx)和 ASP.NET MVC 建立授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-139">In this tutorial, we will roughly sketch out how to use [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) and ASP.NET MVC to create an authorization server.</span></span> <span data-ttu-id="3ffb7-140">我們希望即將提供下載完整的範例中，如本教學課程中不包含每個步驟所示。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-140">We hope to soon provide a download for the completed sample, as this tutorial does not include each step.</span></span> <span data-ttu-id="3ffb7-141">首先，建立名為空的 web 應用程式*AuthorizationServer*並安裝下列封裝：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-141">First, create an empty web app named *AuthorizationServer* and install the following packages:</span></span>

- <span data-ttu-id="3ffb7-142">Microsoft.AspNet.Mvc</span><span class="sxs-lookup"><span data-stu-id="3ffb7-142">Microsoft.AspNet.Mvc</span></span>
- <span data-ttu-id="3ffb7-143">Microsoft.Owin.Host.SystemWeb</span><span class="sxs-lookup"><span data-stu-id="3ffb7-143">Microsoft.Owin.Host.SystemWeb</span></span>
- <span data-ttu-id="3ffb7-144">Microsoft.Owin.Security.OAuth</span><span class="sxs-lookup"><span data-stu-id="3ffb7-144">Microsoft.Owin.Security.OAuth</span></span>
- <span data-ttu-id="3ffb7-145">Microsoft.Owin.Security.Cookies</span><span class="sxs-lookup"><span data-stu-id="3ffb7-145">Microsoft.Owin.Security.Cookies</span></span>
- <span data-ttu-id="3ffb7-146">Microsoft.Owin.Security.Google （或任何其他社交登入封裝，例如 Microsoft.Owin.Security.Facebook）</span><span class="sxs-lookup"><span data-stu-id="3ffb7-146">Microsoft.Owin.Security.Google (Or any other social login package such as Microsoft.Owin.Security.Facebook)</span></span>

<span data-ttu-id="3ffb7-147">新增[OWIN 啟動類別](owin-startup-class-detection.md)在專案根資料夾中名為*啟動*。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-147">Add an [OWIN Startup class](owin-startup-class-detection.md) under the project root folder named *Startup*.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

<span data-ttu-id="3ffb7-148">建立*應用程式\_啟動*資料夾。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-148">Create an *App\_Start* folder.</span></span> <span data-ttu-id="3ffb7-149">選取 *應用程式\_開始*資料夾，然後使用 Shift + Alt + A 將已下載的版本*AuthorizationServer\App\_Start\Startup.Auth.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-149">Select the *App\_Start* folder and use Shift+Alt+A to add the downloaded version of the *AuthorizationServer\App\_Start\Startup.Auth.cs* file.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

<span data-ttu-id="3ffb7-150">上述程式碼可讓應用程式/外部登入 cookie 和 Google 驗證其本身的授權伺服器會用它來管理帳戶。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-150">The code above enables application/external sign in cookies and Google authentication, which are used by authorization server itself to manage accounts.</span></span>

<span data-ttu-id="3ffb7-151">`UseOAuthAuthorizationServer`擴充方法是設定授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-151">The `UseOAuthAuthorizationServer` extension method is to setup the authorization server.</span></span> <span data-ttu-id="3ffb7-152">安裝程式選項如下：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-152">The setup options are:</span></span>

- <span data-ttu-id="3ffb7-153">`AuthorizeEndpointPath`：要求路徑，而用戶端應用程式重新導向 user-agent 以取得使用者同意發出的權杖或程式碼。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-153">`AuthorizeEndpointPath`: The request path where client applications will redirect the user-agent in order to obtain the users consent to issue a token or code.</span></span> <span data-ttu-id="3ffb7-154">其開頭必須前置斜線，比方說，「`/Authorize`"。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-154">It must begin with a leading slash, for example, "`/Authorize`".</span></span>
- <span data-ttu-id="3ffb7-155">`TokenEndpointPath`：若要取得存取權杖的要求路徑用戶端應用程式直接通訊。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-155">`TokenEndpointPath`: The request path client applications directly communicate to obtain the access token.</span></span> <span data-ttu-id="3ffb7-156">它必須以斜線，例如"/token"開頭。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-156">It must begin with a leading slash, like "/Token".</span></span> <span data-ttu-id="3ffb7-157">如果用戶端會發出[用戶端\_祕密](http://tools.ietf.org/html/rfc6749#appendix-A.2)，它必須提供至此端點。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-157">If the client is issued a [client\_secret](http://tools.ietf.org/html/rfc6749#appendix-A.2), it must be provided to this endpoint.</span></span>
- <span data-ttu-id="3ffb7-158">`ApplicationCanDisplayErrors`：設定為`true`如果想要產生的用戶端驗證錯誤的自訂錯誤頁面上的 web 應用程式`/Authorize`端點。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-158">`ApplicationCanDisplayErrors`: Set to `true` if the web application wants to generate a custom error page for the client validation errors on `/Authorize` endpoint.</span></span> <span data-ttu-id="3ffb7-159">這只需要的情況下，瀏覽器不會重新導向回用戶端應用程式，例如，當`client_id`或`redirect_uri`不正確。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-159">This is only needed for cases where the browser is not redirected back to the client application, for example, when the `client_id` or `redirect_uri` are incorrect.</span></span> <span data-ttu-id="3ffb7-160">`/Authorize`端點應該會看到 「 oauth。錯誤 」、 「 oauth。ErrorDescription"和"oauth。ErrorUri"屬性會新增至 OWIN 環境。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-160">The `/Authorize` endpoint should expect to see the "oauth.Error", "oauth.ErrorDescription", and "oauth.ErrorUri" properties are added to the OWIN environment.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3ffb7-161">如果沒有，則為 true，授權伺服器會傳回預設的錯誤網頁的錯誤詳細資料。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-161">If not true, the authorization server will return a default error page with the error details.</span></span>
- <span data-ttu-id="3ffb7-162">`AllowInsecureHttp`：True 表示允許授權和語彙基元要求抵達 HTTP URI 位址，並允許連入`redirect_uri`授權要求參數具有 HTTP URI 位址。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-162">`AllowInsecureHttp`: True to allow authorize and token requests to arrive on HTTP URI addresses, and to allow incoming `redirect_uri` authorize request parameters to have HTTP URI addresses.</span></span>

    > [!WARNING]
    > <span data-ttu-id="3ffb7-163">安全性-這是只有開發。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-163">Security - This is for development only.</span></span>
- <span data-ttu-id="3ffb7-164">`Provider`：提供應用程式對授權伺服器中介軟體所引發的程序事件的物件。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-164">`Provider`: The object provided by the application to process events raised by the Authorization Server middleware.</span></span> <span data-ttu-id="3ffb7-165">應用程式可能完整，實作介面，或它可能會建立的執行個體`OAuthAuthorizationServerProvider`並指派此伺服器所支援的 OAuth 流程所需的委派。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-165">The application may implement the interface fully, or it may create an instance of `OAuthAuthorizationServerProvider` and assign delegates necessary for the OAuth flows this server supports.</span></span>
- <span data-ttu-id="3ffb7-166">`AuthorizationCodeProvider`：產生單次使用授權碼傳回至用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-166">`AuthorizationCodeProvider`: Produces a single-use authorization code to return to the client application.</span></span> <span data-ttu-id="3ffb7-167">OAuth 伺服器可保護應用程式**必須**提供的執行個體`AuthorizationCodeProvider`語彙基元所產生的程式`OnCreate/OnCreateAsync`事件會被視為適用於只有一個呼叫`OnReceive/OnReceiveAsync`。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-167">For the OAuth server to be secure the application **MUST** provide an instance for `AuthorizationCodeProvider` where the token produced by the `OnCreate/OnCreateAsync` event is considered valid for only one call to `OnReceive/OnReceiveAsync`.</span></span>
- <span data-ttu-id="3ffb7-168">`RefreshTokenProvider`：產生可用來產生新的存取權杖時所需的重新整理權杖。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-168">`RefreshTokenProvider`: Produces a refresh token which may be used to produce a new access token when needed.</span></span> <span data-ttu-id="3ffb7-169">如果未提供授權伺服器不會傳回來自重新整理權杖`/Token`端點。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-169">If not provided the authorization server will not return refresh tokens from the `/Token` endpoint.</span></span>

## <a name="account-management"></a><span data-ttu-id="3ffb7-170">帳戶管理</span><span class="sxs-lookup"><span data-stu-id="3ffb7-170">Account Management</span></span>

<span data-ttu-id="3ffb7-171">OAuth 不在意您何處或如何管理您的使用者帳戶資訊。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-171">OAuth doesn't care where or how you manage your user account information.</span></span> <span data-ttu-id="3ffb7-172">它有[ASP.NET Identity](../../../identity/index.md)即負責。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-172">It's [ASP.NET Identity](../../../identity/index.md) which is responsible for it.</span></span> <span data-ttu-id="3ffb7-173">在本教學課程中，我們將簡化帳戶管理的程式碼，並先確定該使用者可以使用 OWIN cookie 中介軟體登入。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-173">In this tutorial, we will simplify the account management code and just make sure that user can login using OWIN cookie middleware.</span></span> <span data-ttu-id="3ffb7-174">以下是簡化的範例程式碼`AccountController`:</span><span class="sxs-lookup"><span data-stu-id="3ffb7-174">Here is the simplified sample code for the `AccountController`:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

<span data-ttu-id="3ffb7-175">`ValidateClientRedirectUri` 用來驗證用戶端，其已註冊的重新導向 url。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-175">`ValidateClientRedirectUri` is used to validate the client with its registered redirect URL.</span></span> <span data-ttu-id="3ffb7-176">`ValidateClientAuthentication` 檢查基本配置標頭和表單內文，以取得用戶端的認證。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-176">`ValidateClientAuthentication` checks the basic scheme header and form body to get the client's credentials.</span></span>

<span data-ttu-id="3ffb7-177">登入頁面如下所示：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-177">The login page is shown below:</span></span>

![](owin-oauth-20-authorization-server/_static/image1.png)

<span data-ttu-id="3ffb7-178">檢閱 IETF OAuth 2[授權碼授與](http://tools.ietf.org/html/rfc6749#section-4.1)現在區段。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-178">Review the IETF's OAuth 2 [Authorization Code Grant](http://tools.ietf.org/html/rfc6749#section-4.1) section now.</span></span>

<span data-ttu-id="3ffb7-179">**提供者**（在下表中） 是[OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx)。提供者，也就是型別的`OAuthAuthorizationServerProvider`，其中包含所有的 OAuth 伺服器事件。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-179">**Provider** (in the table below) is [OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx).Provider, which is of type `OAuthAuthorizationServerProvider`, which contains all OAuth server events.</span></span>

| <span data-ttu-id="3ffb7-180">從授權碼授與區段的流程步驟</span><span class="sxs-lookup"><span data-stu-id="3ffb7-180">Flow steps from Authorization Code Grant section</span></span> | <span data-ttu-id="3ffb7-181">下載範例會執行這些步驟：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-181">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="3ffb7-182">（A） 用戶端導向資源擁有者的使用者代理程式的授權端點以起始流程。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-182">(A) The client initiates the flow by directing the resource owner's user-agent to the authorization endpoint.</span></span> <span data-ttu-id="3ffb7-183">用戶端會包含其用戶端識別碼、 要求的範圍、 本機狀態和重新導向 URI 的授權伺服器會將 user-agent 回後授與 （或拒絕存取）。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-183">The client includes its client identifier, requested scope, local state, and a redirection URI to which the authorization server will send the user-agent back once access is granted (or denied).</span></span> | <span data-ttu-id="3ffb7-184">Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint</span><span class="sxs-lookup"><span data-stu-id="3ffb7-184">Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint</span></span> |
|  |  |
| <span data-ttu-id="3ffb7-185">（B） 授權伺服器會驗證資源擁有者 （透過使用者-代理程式），然後表明資源擁有者授與或拒絕用戶端的存取要求。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-185">(B) The authorization server authenticates the resource owner (via the user-agent) and establishes whether the resource owner grants or denies the client's access request.</span></span> | <span data-ttu-id="3ffb7-186">**&lt;If user grants access&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="3ffb7-186">**&lt;If user grants access&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="3ffb7-187">（C） 假設資源擁有者授與存取，授權伺服器會將使用者代理程式重新導向回用戶端會使用重新導向 URI 提供先前 （在要求中或在用戶端註冊）。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-187">(C) Assuming the resource owner grants access, the authorization server redirects the user-agent back to the client using the redirection URI provided earlier (in the request or during client registration).</span></span> <span data-ttu-id="3ffb7-188">...</span><span class="sxs-lookup"><span data-stu-id="3ffb7-188">...</span></span> |  |
|  |  |
| <span data-ttu-id="3ffb7-189">（D) 用戶端向授權伺服器的權杖端點要求存取權杖，加上一個步驟中收到的授權碼。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-189">(D) The client requests an access token from the authorization server's token endpoint by including the authorization code received in the previous step.</span></span> <span data-ttu-id="3ffb7-190">當提出要求，用戶端會向授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-190">When making the request, the client authenticates with the authorization server.</span></span> <span data-ttu-id="3ffb7-191">用戶端會包含重新導向 URI 用來取得授權碼，以便驗證。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-191">The client includes the redirection URI used to obtain the authorization code for verification.</span></span> | <span data-ttu-id="3ffb7-192">Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="3ffb7-192">Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync</span></span> |

<span data-ttu-id="3ffb7-193">如需範例實作`AuthorizationCodeProvider.CreateAsync`和`ReceiveAsync`來控制建立和驗證授權碼如下所示。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-193">A sample implementation for `AuthorizationCodeProvider.CreateAsync` and `ReceiveAsync` to control the creation and validation of authorization code is shown below.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

<span data-ttu-id="3ffb7-194">上述程式碼會使用記憶體中的並行字典來儲存程式碼和身分識別的票證和還原之後接收程式碼的身分識別。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-194">The code above uses an in-memory concurrent dictionary to store the code and identity ticket and restore the identity after receiving the code.</span></span> <span data-ttu-id="3ffb7-195">在實際的應用程式，它會由持續性資料存放區將其取代。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-195">In a real application, it would be replaced by a persistent data store.</span></span> <span data-ttu-id="3ffb7-196">授權端點做為資源擁有者授與存取權的用戶端。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-196">The authorization endpoint is for the resource owner to grant access to the client.</span></span> <span data-ttu-id="3ffb7-197">通常，它需要的使用者介面，以便使用者按一下按鈕，並確認授與。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-197">Usually, it needs a user interface to allow the user to click a button and confirm the grant.</span></span> <span data-ttu-id="3ffb7-198">OWIN OAuth 中介軟體可讓應用程式程式碼來處理授權端點。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-198">OWIN OAuth middleware allows application code to handle the authorization endpoint.</span></span> <span data-ttu-id="3ffb7-199">在我們的範例應用程式，我們會使用 MVC 控制器呼叫`OAuthController`來加以處理。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-199">In our sample app, we use an MVC controller called `OAuthController` to handle it.</span></span> <span data-ttu-id="3ffb7-200">以下是範例實作：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-200">Here is the sample implementation:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

<span data-ttu-id="3ffb7-201">`Authorize`動作會先檢查使用者是否已登入授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-201">The `Authorize` action will first check if the user has logged in to the authorization server.</span></span> <span data-ttu-id="3ffb7-202">如果沒有，驗證中介軟體挑戰使用 「 應用程式 」 cookie 來驗證呼叫端，並將重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-202">If not, the authentication middleware challenges the caller to authenticate using the "Application" cookie and redirects to the login page.</span></span> <span data-ttu-id="3ffb7-203">（請參閱上述的反白顯示程式碼）。如果使用者已登入，它會轉譯 [Authorize] 檢視中，如下所示：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-203">(See highlighted code above.) If user has logged in, it will render the Authorize view, as shown below:</span></span>

![](owin-oauth-20-authorization-server/_static/image2.png)

<span data-ttu-id="3ffb7-204">如果**Grant**  按鈕已選取，`Authorize`動作將會建立新的"Bearer"身分識別和使用它登入。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-204">If the **Grant** button is selected, the `Authorize` action will create a new "Bearer" identity and sign in with it.</span></span> <span data-ttu-id="3ffb7-205">它會觸發產生持有人權杖，並將它傳送回 JSON 裝載的用戶端的授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-205">It will trigger the authorization server to generate a bearer token and send it back to the client with JSON payload.</span></span>

### <a name="implicit-grant"></a><span data-ttu-id="3ffb7-206">隱含授與</span><span class="sxs-lookup"><span data-stu-id="3ffb7-206">Implicit Grant</span></span>

<span data-ttu-id="3ffb7-207">請參閱 IETF OAuth 2[隱含授與](http://tools.ietf.org/html/rfc6749#section-4.2)現在區段。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-207">Refer to the IETF's OAuth 2 [Implicit Grant](http://tools.ietf.org/html/rfc6749#section-4.2) section now.</span></span>

 <span data-ttu-id="3ffb7-208">[隱含授與](http://tools.ietf.org/html/rfc6749#section-4.2)[圖 4] 所示的 flow，流程，以及對應的 OWIN OAuth 則中介軟體會遵循。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-208">The [Implicit Grant](http://tools.ietf.org/html/rfc6749#section-4.2) flow shown in Figure 4 is the flow and mapping which the OWIN OAuth middleware follows.</span></span>

| <span data-ttu-id="3ffb7-209">隱含授與一節中的流程步驟</span><span class="sxs-lookup"><span data-stu-id="3ffb7-209">Flow steps from Implicit Grant section</span></span> | <span data-ttu-id="3ffb7-210">下載範例會執行這些步驟：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-210">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="3ffb7-211">（A） 用戶端導向資源擁有者的使用者代理程式的授權端點以起始流程。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-211">(A) The client initiates the flow by directing the resource owner's user-agent to the authorization endpoint.</span></span> <span data-ttu-id="3ffb7-212">用戶端會包含其用戶端識別碼、 要求的範圍、 本機狀態和重新導向 URI 的授權伺服器會將 user-agent 回後授與 （或拒絕存取）。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-212">The client includes its client identifier, requested scope, local state, and a redirection URI to which the authorization server will send the user-agent back once access is granted (or denied).</span></span> | <span data-ttu-id="3ffb7-213">Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint</span><span class="sxs-lookup"><span data-stu-id="3ffb7-213">Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint</span></span> |
|  |  |
| <span data-ttu-id="3ffb7-214">（B） 授權伺服器會驗證資源擁有者 （透過使用者-代理程式），然後表明資源擁有者授與或拒絕用戶端的存取要求。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-214">(B) The authorization server authenticates the resource owner (via the user-agent) and establishes whether the resource owner grants or denies the client's access request.</span></span> | <span data-ttu-id="3ffb7-215">**&lt;If user grants access&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="3ffb7-215">**&lt;If user grants access&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="3ffb7-216">（C） 假設資源擁有者授與存取，授權伺服器會將使用者代理程式重新導向回用戶端會使用重新導向 URI 提供先前 （在要求中或在用戶端註冊）。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-216">(C) Assuming the resource owner grants access, the authorization server redirects the user-agent back to the client using the redirection URI provided earlier (in the request or during client registration).</span></span> <span data-ttu-id="3ffb7-217">...</span><span class="sxs-lookup"><span data-stu-id="3ffb7-217">...</span></span> |  |
|  |  |
| <span data-ttu-id="3ffb7-218">（D) 用戶端向授權伺服器的權杖端點要求存取權杖，加上一個步驟中收到的授權碼。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-218">(D) The client requests an access token from the authorization server's token endpoint by including the authorization code received in the previous step.</span></span> <span data-ttu-id="3ffb7-219">當提出要求，用戶端會向授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-219">When making the request, the client authenticates with the authorization server.</span></span> <span data-ttu-id="3ffb7-220">用戶端會包含重新導向 URI 用來取得授權碼，以便驗證。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-220">The client includes the redirection URI used to obtain the authorization code for verification.</span></span> |  |

<span data-ttu-id="3ffb7-221">因為我們已實作授權端點 (`OAuthController.Authorize`動作) 的授權碼授與，因此它會自動啟用也隱含流程。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-221">Since we already implemented the authorization endpoint (`OAuthController.Authorize` action) for authorization code grant, it automatically enables implicit flow as well.</span></span> <span data-ttu-id="3ffb7-222">注意︰`Provider.ValidateClientRedirectUri`用來驗證具有其傳送存取權杖來惡意用戶端時，防止隱含授與流程的重新導向 URL 的用戶端識別碼 ([攔截攻擊](https://www.owasp.org/index.php/Man-in-the-middle_attack))。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-222">Note: `Provider.ValidateClientRedirectUri` is used to validate the client ID with its redirection URL, which protects the implicit grant flow from sending the access token to malicious clients ([Man-in-the-middle attack](https://www.owasp.org/index.php/Man-in-the-middle_attack)).</span></span>

### <a name="resource-owner-password-credentials-grant"></a><span data-ttu-id="3ffb7-223">資源擁有者密碼認證授與</span><span class="sxs-lookup"><span data-stu-id="3ffb7-223">Resource Owner Password Credentials Grant</span></span>

<span data-ttu-id="3ffb7-224">請參閱 IETF OAuth 2[資源擁有者密碼認證授與](http://tools.ietf.org/html/rfc6749#section-4.3)現在區段。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-224">Refer to the IETF's OAuth 2 [Resource Owner Password Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.3) section now.</span></span>

 <span data-ttu-id="3ffb7-225">[資源擁有者密碼認證授與](http://tools.ietf.org/html/rfc6749#section-4.3)[圖 5] 所示的 flow，流程，以及對應的 OWIN OAuth 則中介軟體會遵循。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-225">The [Resource Owner Password Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.3) flow shown in Figure 5 is the flow and mapping which the OWIN OAuth middleware follows.</span></span>

| <span data-ttu-id="3ffb7-226">從資源擁有者密碼認證授與區段的流程步驟</span><span class="sxs-lookup"><span data-stu-id="3ffb7-226">Flow steps from Resource Owner Password Credentials Grant section</span></span> | <span data-ttu-id="3ffb7-227">下載範例會執行這些步驟：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-227">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="3ffb7-228">（A） 資源擁有者會為用戶端提供自己的使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-228">(A) The resource owner provides the client with its username and password.</span></span> |  |
|  |  |
| <span data-ttu-id="3ffb7-229">（B） 用戶端包含來自資源擁有者的認證，從授權伺服器的權杖端點要求存取權杖。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-229">(B) The client requests an access token from the authorization server's token endpoint by including the credentials received from the resource owner.</span></span> <span data-ttu-id="3ffb7-230">當提出要求，用戶端會向授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-230">When making the request, the client authenticates with the authorization server.</span></span> | <span data-ttu-id="3ffb7-231">Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="3ffb7-231">Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="3ffb7-232">（C） 授權伺服器會驗證用戶端和驗證資源擁有者認證，然後如果有效，就會發出存取權杖。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-232">(C) The authorization server authenticates the client and validates the resource owner credentials, and if valid, issues an access token.</span></span> |  |

<span data-ttu-id="3ffb7-233">以下是範例實作`Provider.GrantResourceOwnerCredentials`:</span><span class="sxs-lookup"><span data-stu-id="3ffb7-233">Here is the sample implementation for `Provider.GrantResourceOwnerCredentials`:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> <span data-ttu-id="3ffb7-234">上述程式碼用來說明本教學課程的這一節，不應在安全或生產環境應用程式。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-234">The code above is intended to explain this section of the tutorial and should not be used in secure or production apps.</span></span> <span data-ttu-id="3ffb7-235">它不會檢查資源擁有者認證。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-235">It does not check the resource owners credentials.</span></span> <span data-ttu-id="3ffb7-236">它會假設每個認證有效，並為其建立新的身分識別。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-236">It assumes every credential is valid and creates a new identity for it.</span></span> <span data-ttu-id="3ffb7-237">新的身分識別將用來產生存取權杖和重新整理權杖中。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-237">The new identity will be used to generate the access token and refresh token.</span></span> <span data-ttu-id="3ffb7-238">程式碼取代您自己的安全帳戶管理程式碼。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-238">Please replace the code with your own secure account management code.</span></span>


### <a name="client-credentials-grant"></a><span data-ttu-id="3ffb7-239">用戶端認證授與</span><span class="sxs-lookup"><span data-stu-id="3ffb7-239">Client Credentials Grant</span></span>

<span data-ttu-id="3ffb7-240">請參閱 IETF OAuth 2[用戶端認證授與](http://tools.ietf.org/html/rfc6749#section-4.4)現在區段。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-240">Refer to the IETF's OAuth 2 [Client Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.4) section now.</span></span>

 <span data-ttu-id="3ffb7-241">[用戶端認證授與](http://tools.ietf.org/html/rfc6749#section-4.4)[圖 6] 所示的 flow，流程，以及對應的 OWIN OAuth 則中介軟體會遵循。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-241">The [Client Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.4) flow shown in Figure 6 is the flow and mapping which the OWIN OAuth middleware follows.</span></span>

| <span data-ttu-id="3ffb7-242">從用戶端認證授與區段的流程步驟</span><span class="sxs-lookup"><span data-stu-id="3ffb7-242">Flow steps from Client Credentials Grant section</span></span> | <span data-ttu-id="3ffb7-243">下載範例會執行這些步驟：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-243">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="3ffb7-244">（A) 用戶端向授權伺服器，並向權杖端點要求存取權杖。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-244">(A) The client authenticates with the authorization server and requests an access token from the token endpoint.</span></span> | <span data-ttu-id="3ffb7-245">Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="3ffb7-245">Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="3ffb7-246">（B） 授權伺服器會驗證用戶端，然後如果有效，就會發出存取權杖。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-246">(B) The authorization server authenticates the client, and if valid, issues an access token.</span></span> |  |

<span data-ttu-id="3ffb7-247">以下是範例實作`Provider.GrantClientCredentials`:</span><span class="sxs-lookup"><span data-stu-id="3ffb7-247">Here is the sample implementation for `Provider.GrantClientCredentials`:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="3ffb7-248">上述程式碼用來說明本教學課程的這一節，不應在安全或生產環境應用程式。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-248">The code above is intended to explain this section of the tutorial and should not be used in secure or production apps.</span></span> <span data-ttu-id="3ffb7-249">程式碼取代您自己的安全用戶端管理程式碼。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-249">Please replace the code with your own secure client management code.</span></span>


### <a name="refresh-token"></a><span data-ttu-id="3ffb7-250">重新整理權杖</span><span class="sxs-lookup"><span data-stu-id="3ffb7-250">Refresh Token</span></span>

<span data-ttu-id="3ffb7-251">請參閱 IETF OAuth 2[重新整理權杖](http://tools.ietf.org/html/rfc6749#section-1.5)現在區段。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-251">Refer to the IETF's OAuth 2 [Refresh Token](http://tools.ietf.org/html/rfc6749#section-1.5) section now.</span></span>

 <span data-ttu-id="3ffb7-252">[重新整理權杖](http://tools.ietf.org/html/rfc6749#section-1.5)[圖 2] 所示的 flow，流程，以及對應的 OWIN OAuth 則中介軟體會遵循。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-252">The [Refresh Token](http://tools.ietf.org/html/rfc6749#section-1.5) flow shown in Figure 2 is the flow and mapping which the OWIN OAuth middleware follows.</span></span>

| <span data-ttu-id="3ffb7-253">從用戶端認證授與區段的流程步驟</span><span class="sxs-lookup"><span data-stu-id="3ffb7-253">Flow steps from Client Credentials Grant section</span></span> | <span data-ttu-id="3ffb7-254">下載範例會執行這些步驟：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-254">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="3ffb7-255">（G） 用戶端會要求新的存取權杖，向授權伺服器，並呈現重新整理權杖。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-255">(G) The client requests a new access token by authenticating with the authorization server and presenting the refresh token.</span></span> <span data-ttu-id="3ffb7-256">用戶端驗證需求會根據用戶端類型和授權伺服器原則。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-256">The client authentication requirements are based on the client type and on the authorization server policies.</span></span> | <span data-ttu-id="3ffb7-257">Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsyncRefreshTokenProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="3ffb7-257">Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="3ffb7-258">（H） 授權伺服器會驗證用戶端和驗證重新整理權杖，然後如果有效，就會發出新的存取權杖 （以及選擇性地為新的重新整理語彙基元）。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-258">(H) The authorization server authenticates the client and validates the refresh token, and if valid, issues a new access token (and, optionally, a new refresh token).</span></span> |  |

<span data-ttu-id="3ffb7-259">以下是範例實作`Provider.GrantRefreshToken`:</span><span class="sxs-lookup"><span data-stu-id="3ffb7-259">Here is the sample implementation for `Provider.GrantRefreshToken`:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a><span data-ttu-id="3ffb7-260">建立資源的伺服器保護的存取權杖</span><span class="sxs-lookup"><span data-stu-id="3ffb7-260">Create a Resource Server which is protected by Access Token</span></span>

<span data-ttu-id="3ffb7-261">建立空的 web 應用程式專案並安裝下列專案中的封裝：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-261">Create an empty web app project and install following packages in the project:</span></span>

- <span data-ttu-id="3ffb7-262">Microsoft.AspNet.WebApi.Owin</span><span class="sxs-lookup"><span data-stu-id="3ffb7-262">Microsoft.AspNet.WebApi.Owin</span></span>
- <span data-ttu-id="3ffb7-263">Microsoft.Owin.Host.SystemWeb</span><span class="sxs-lookup"><span data-stu-id="3ffb7-263">Microsoft.Owin.Host.SystemWeb</span></span>
- <span data-ttu-id="3ffb7-264">Microsoft.Owin.Security.OAuth</span><span class="sxs-lookup"><span data-stu-id="3ffb7-264">Microsoft.Owin.Security.OAuth</span></span>

<span data-ttu-id="3ffb7-265">建立啟動類別，並設定驗證和 Web API。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-265">Create a startup class and configure authentication and Web API.</span></span> <span data-ttu-id="3ffb7-266">請參閱*AuthorizationServer\ResourceServer\Startup.cs*下載範例中。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-266">See *AuthorizationServer\ResourceServer\Startup.cs* in the sample download.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

<span data-ttu-id="3ffb7-267">請參閱*AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs*下載範例中。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-267">See *AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs* in the sample download.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

<span data-ttu-id="3ffb7-268">請參閱*AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs*下載範例中。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-268">See *AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs* in the sample download.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- <span data-ttu-id="3ffb7-269">`UseCors` 方法會允許所有網域的 CORS。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-269">`UseCors` method allows CORS for all domains.</span></span>
- <span data-ttu-id="3ffb7-270">`UseOAuthBearerAuthentication` 方法可讓 OAuth 持有人權杖驗證中介軟體會接收及驗證從要求中的授權標頭的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-270">`UseOAuthBearerAuthentication` method enables OAuth bearer token authentication middleware which will receive and validate bearer token from authorization header in the request.</span></span>
- <span data-ttu-id="3ffb7-271">`Config.SuppressDefaultHostAuthenticaiton` 隱藏預設主機驗證的主體，從應用程式，因此所有的要求將會是匿名的這個呼叫之後。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-271">`Config.SuppressDefaultHostAuthenticaiton` suppresses default host authenticated principal from the app, therefore all requests will be anonymous after this call.</span></span>
- <span data-ttu-id="3ffb7-272">`HostAuthenticationFilter` 啟用驗證，只針對指定的驗證類型。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-272">`HostAuthenticationFilter` enables authentication just for the specified authentication type.</span></span> <span data-ttu-id="3ffb7-273">在此情況下，它會是持有人驗證類型。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-273">In this case, it's bearer authentication type.</span></span>

<span data-ttu-id="3ffb7-274">為了示範已驗證的身分識別，我們會建立 ApiController 輸出目前的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-274">In order to demonstrate the authenticated identity, we create an ApiController to output current user's claims.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

<span data-ttu-id="3ffb7-275">如果授權伺服器和資源伺服器不在相同電腦上，OAuth 的中介軟體會使用不同的機器金鑰來加密和解密的持有人存取權杖。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-275">If the authorization server and the resource server are not on the same computer, the OAuth middleware will use the different machine keys to encrypt and decrypt bearer access token.</span></span> <span data-ttu-id="3ffb7-276">若要共用相同的私人金鑰，這兩個專案之間，我們將新增相同`machinekey`設定中都*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-276">In order to share the same private key between both projects, we add the same `machinekey` setting in both *web.config* files.</span></span>

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a><span data-ttu-id="3ffb7-277">建立 OAuth 2.0 用戶端</span><span class="sxs-lookup"><span data-stu-id="3ffb7-277">Create OAuth 2.0 Clients</span></span>

 <span data-ttu-id="3ffb7-278">我們會使用[DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) NuGet 封裝來簡化用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-278">We use the [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) NuGet package to simplify the client code.</span></span>

### <a name="authorization-code-grant-client"></a><span data-ttu-id="3ffb7-279">授權碼授與用戶端</span><span class="sxs-lookup"><span data-stu-id="3ffb7-279">Authorization Code Grant Client</span></span>

 <span data-ttu-id="3ffb7-280">此用戶端是在 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-280">This client is an MVC application.</span></span> <span data-ttu-id="3ffb7-281">它會觸發從後端取得存取權杖的授權碼授與流程。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-281">It will trigger an authorization code grant flow to get the access token from backend.</span></span> <span data-ttu-id="3ffb7-282">它會有單一頁面，如下所示：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-282">It has a single page as shown below:</span></span>

![](owin-oauth-20-authorization-server/_static/image3.png)

- <span data-ttu-id="3ffb7-283">**授權**按鈕將瀏覽器重新導向至授權伺服器，以通知資源擁有者存取權授與此用戶端。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-283">The **Authorize** button will redirect browser to the authorization server to notify the resource owner to grant access to this client.</span></span>
- <span data-ttu-id="3ffb7-284">**重新整理**按鈕會取得新存取權杖和重新整理權杖，使用目前的重新整理權杖。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-284">The **Refresh** button will get a new access token and refresh token using the current refresh token.</span></span>
- <span data-ttu-id="3ffb7-285">**存取受保護的資源 API**按鈕會呼叫資源伺服器，以取得目前使用者的宣告資料，並在頁面上顯示它們。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-285">The **Access Protected Resource API** button will call the resource server to get current user's claims data and show them on the page.</span></span>

<span data-ttu-id="3ffb7-286">以下是範例程式碼的`HomeController`的用戶端。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-286">Here is the sample code of the `HomeController` of the client.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

<span data-ttu-id="3ffb7-287">`DotNetOpenAuth` 根據預設，需要 SSL。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-287">`DotNetOpenAuth` requires SSL by default.</span></span> <span data-ttu-id="3ffb7-288">由於我們的示範，使用 HTTP，您必須新增下列組態檔中的設定：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-288">Since our demo is using HTTP, you need to add following setting in the config file:</span></span>

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> <span data-ttu-id="3ffb7-289">安全性-永遠不會停用 SSL 生產應用程式中。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-289">Security - Never disable SSL in a production app.</span></span> <span data-ttu-id="3ffb7-290">現在在純文字透過網路傳送登入認證。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-290">Your login credentials are now being sent in clear-text across the wire.</span></span> <span data-ttu-id="3ffb7-291">上述程式碼只適用於本機範例偵錯和探索。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-291">The code above is just for local sample debugging and exploration.</span></span>


### <a name="implicit-grant-client"></a><span data-ttu-id="3ffb7-292">隱含授與用戶端</span><span class="sxs-lookup"><span data-stu-id="3ffb7-292">Implicit Grant Client</span></span>

<span data-ttu-id="3ffb7-293">此用戶端使用 JavaScript 來：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-293">This client is using JavaScript to:</span></span>

1. <span data-ttu-id="3ffb7-294">開啟新視窗，並將重新導向至授權端點的授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-294">Open a new window and redirect to the authorize endpoint of the Authorization Server.</span></span>
2. <span data-ttu-id="3ffb7-295">從取得存取權杖 URL 片段時將重新導向回。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-295">Get the access token from URL fragments when it redirects back.</span></span>

<span data-ttu-id="3ffb7-296">下圖顯示此程序：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-296">The following image shows this process:</span></span>

![](owin-oauth-20-authorization-server/_static/image4.png)

<span data-ttu-id="3ffb7-297">用戶端應該有兩個頁面： 一個用於首頁上，另一個則用於回呼。以下是範例程式碼中找到的 JavaScript *Index.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-297">The client should have two pages: one for home page and the other for callback.Here is the sample JavaScript code found in the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

<span data-ttu-id="3ffb7-298">以下是處理程式碼中的回呼*SignIn.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-298">Here is the callback handling code in *SignIn.cshtml* file:</span></span>

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> <span data-ttu-id="3ffb7-299">最佳的作法是將 JavaScript 移至外部檔案，並不將它內嵌加上 Razor 標記。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-299">A best practice is to move the JavaScript to an external file and not embed it with the Razor markup.</span></span> <span data-ttu-id="3ffb7-300">為了簡化此範例中，在經過組合。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-300">To keep this sample simple, they have been combined.</span></span>


### <a name="resource-owner-password-credentials-grant-client"></a><span data-ttu-id="3ffb7-301">資源擁有者密碼認證授與用戶端</span><span class="sxs-lookup"><span data-stu-id="3ffb7-301">Resource Owner Password Credentials Grant Client</span></span>

<span data-ttu-id="3ffb7-302">我們可以使用主控台應用程式來示範此用戶端。</span><span class="sxs-lookup"><span data-stu-id="3ffb7-302">We uses a console app to demo this client.</span></span> <span data-ttu-id="3ffb7-303">程式碼如下：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-303">Here is the code:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a><span data-ttu-id="3ffb7-304">用戶端認證授與用戶端</span><span class="sxs-lookup"><span data-stu-id="3ffb7-304">Client Credentials Grant Client</span></span>

<span data-ttu-id="3ffb7-305">類似於資源擁有者密碼認證授與，以下是主控台應用程式程式碼：</span><span class="sxs-lookup"><span data-stu-id="3ffb7-305">Similar to the Resource Owner Password Credentials Grant, here is console app code:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
