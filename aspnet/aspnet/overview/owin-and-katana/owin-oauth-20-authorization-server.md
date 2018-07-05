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
<a name="owin-oauth-20-authorization-server"></a><span data-ttu-id="2655b-104">OWIN OAuth 2.0 授權伺服器</span><span class="sxs-lookup"><span data-stu-id="2655b-104">OWIN OAuth 2.0 Authorization Server</span></span>
====================
<span data-ttu-id="2655b-105">藉由[Hongye Sun](https://github.com/hongyes)， [Praburaj Thiagarajan](https://github.com/Praburaj)， [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="2655b-105">by [Hongye Sun](https://github.com/hongyes), [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="2655b-106">本教學課程將引導您如何實作 OAuth 2.0 授權伺服器使用 OWIN OAuth 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2655b-106">This tutorial will guide you on how to implement an OAuth 2.0 Authorization Server using OWIN OAuth middleware.</span></span> <span data-ttu-id="2655b-107">這是僅概述的步驟來建立 OWIN OAuth 2.0 授權伺服器的進階教學課程。</span><span class="sxs-lookup"><span data-stu-id="2655b-107">This is an advanced tutorial that only outlines the steps to create an OWIN OAuth 2.0 Authorization Server.</span></span> <span data-ttu-id="2655b-108">這不是逐步教學課程。</span><span class="sxs-lookup"><span data-stu-id="2655b-108">This is not a step by step tutorial.</span></span> <span data-ttu-id="2655b-109">[下載範例程式碼](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip)。</span><span class="sxs-lookup"><span data-stu-id="2655b-109">[Download the sample code](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="2655b-110">這個外框，則不應該適合用來建立安全的生產應用程式。</span><span class="sxs-lookup"><span data-stu-id="2655b-110">This outline should not be intended to be used for creating a secure production app.</span></span> <span data-ttu-id="2655b-111">本教學課程旨在提供有關如何實作 OAuth 2.0 授權伺服器使用 OWIN OAuth 中介軟體的外框。</span><span class="sxs-lookup"><span data-stu-id="2655b-111">This tutorial is intended to provide only an outline on how to implement an OAuth 2.0 Authorization Server using OWIN OAuth middleware.</span></span>
> 
> 
> ## <a name="software-versions"></a><span data-ttu-id="2655b-112">軟體版本</span><span class="sxs-lookup"><span data-stu-id="2655b-112">Software versions</span></span>
> 
> | <span data-ttu-id="2655b-113">**在本教學課程中所示**</span><span class="sxs-lookup"><span data-stu-id="2655b-113">**Shown in the tutorial**</span></span> | <span data-ttu-id="2655b-114">**也可以搭配**</span><span class="sxs-lookup"><span data-stu-id="2655b-114">**Also works with**</span></span> |
> | --- | --- |
> | <span data-ttu-id="2655b-115">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="2655b-115">Windows 8.1</span></span> | <span data-ttu-id="2655b-116">Windows 8，Windows 7</span><span class="sxs-lookup"><span data-stu-id="2655b-116">Windows 8, Windows 7</span></span> |
> | [<span data-ttu-id="2655b-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2655b-117">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads) | <span data-ttu-id="2655b-118">[Visual Studio 2013 Express for Desktop](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express)。</span><span class="sxs-lookup"><span data-stu-id="2655b-118">[Visual Studio 2013 Express for Desktop](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express).</span></span> <span data-ttu-id="2655b-119">Visual Studio 2012 最新的更新應該可行，但本教學課程尚未經過測試，且某些功能表選取項目和對話方塊都不同。</span><span class="sxs-lookup"><span data-stu-id="2655b-119">Visual Studio 2012 with the latest update should work, but the tutorial has not been tested with it, and some menu selections and dialog boxes are different.</span></span> |
> | <span data-ttu-id="2655b-120">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="2655b-120">.NET 4.5</span></span> |  |
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="2655b-121">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="2655b-121">Questions and Comments</span></span>
> 
> <span data-ttu-id="2655b-122">如果您有不直接相關的教學課程中的問題，您可以將它們在公佈[Katana 專案，GitHub 上](https://github.com/aspnet/AspNetKatana/)。</span><span class="sxs-lookup"><span data-stu-id="2655b-122">If you have questions that are not directly related to the tutorial, you can post them at [Katana Project on GitHub](https://github.com/aspnet/AspNetKatana/).</span></span> <span data-ttu-id="2655b-123">如需問題和針對本教學課程有關的註解，請參閱在頁面底部的註解區段。</span><span class="sxs-lookup"><span data-stu-id="2655b-123">For questions and comments regarding the tutorial itself, see the comments section at the bottom of the page.</span></span>


<span data-ttu-id="2655b-124">[OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749)可讓協力廠商應用程式取得的有限的存取權的 HTTP 服務。</span><span class="sxs-lookup"><span data-stu-id="2655b-124">The [OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) enables a third-party app to obtain limited access to an HTTP service.</span></span> <span data-ttu-id="2655b-125">而不是您可以使用資源擁有者的認證來存取受保護的資源，用戶端取得存取權杖 (這是字串，表示特定的範圍、 存留期和其他存取屬性)。</span><span class="sxs-lookup"><span data-stu-id="2655b-125">Instead of using the resource owner's credentials to access a protected resource, the client obtains an access token (which is a string denoting a specific scope, lifetime, and other access attributes).</span></span> <span data-ttu-id="2655b-126">存取權杖是協力廠商用戶端中，由授權伺服器發行的資源擁有者核准。</span><span class="sxs-lookup"><span data-stu-id="2655b-126">Access tokens are issued to third-party clients by an authorization server with the approval of the resource owner.</span></span>

<span data-ttu-id="2655b-127">本教學課程將涵蓋：</span><span class="sxs-lookup"><span data-stu-id="2655b-127">This tutorial will cover:</span></span>

- <span data-ttu-id="2655b-128">如何建立授權伺服器以支援四種授權授與類型，以及重新整理權杖：</span><span class="sxs-lookup"><span data-stu-id="2655b-128">How to create an authorization server to support four authorization grant types and refresh tokens:</span></span>
    - <span data-ttu-id="2655b-129">授權碼授與</span><span class="sxs-lookup"><span data-stu-id="2655b-129">Authorization code grant</span></span>
    - <span data-ttu-id="2655b-130">隱含授與</span><span class="sxs-lookup"><span data-stu-id="2655b-130">Implicit Grant</span></span>
    - <span data-ttu-id="2655b-131">資源擁有者密碼認證授與</span><span class="sxs-lookup"><span data-stu-id="2655b-131">Resource Owner Password Credentials Grant</span></span>
    - <span data-ttu-id="2655b-132">用戶端認證授與</span><span class="sxs-lookup"><span data-stu-id="2655b-132">Client Credentials Grant</span></span>
- <span data-ttu-id="2655b-133">建立受到存取權杖的資源伺服器。</span><span class="sxs-lookup"><span data-stu-id="2655b-133">Creating a resource server which is protected by an access token.</span></span>
- <span data-ttu-id="2655b-134">建立 OAuth 2.0 用戶端。</span><span class="sxs-lookup"><span data-stu-id="2655b-134">Creating OAuth 2.0 clients.</span></span>

<a id="prerequisites"></a>
## <a name="prerequisites"></a><span data-ttu-id="2655b-135">必要條件</span><span class="sxs-lookup"><span data-stu-id="2655b-135">Prerequisites</span></span>

- <span data-ttu-id="2655b-136">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions)或免費[Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express)，如下所示**軟體版本**頁面的頂端。</span><span class="sxs-lookup"><span data-stu-id="2655b-136">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions) or the free [Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express), as indicated in **Software Versions** at the top of the page.</span></span>
- <span data-ttu-id="2655b-137">OWIN 熟悉。</span><span class="sxs-lookup"><span data-stu-id="2655b-137">Familiarity with OWIN.</span></span> <span data-ttu-id="2655b-138">請參閱[Getting Started with Katana 專案](https://msdn.microsoft.com/magazine/dn451439.aspx)並[OWIN 和 Katana 中最新消息](index.md)。</span><span class="sxs-lookup"><span data-stu-id="2655b-138">See [Getting Started with the Katana Project](https://msdn.microsoft.com/magazine/dn451439.aspx) and [What's new in OWIN and Katana](index.md).</span></span>
- <span data-ttu-id="2655b-139">熟悉[OAuth](http://tools.ietf.org/html/rfc6749)術語，包括[角色](http://tools.ietf.org/html/rfc6749#section-1.1)，[通訊協定流程](http://tools.ietf.org/html/rfc6749#section-1.2)，以及[授權授與](http://tools.ietf.org/html/rfc6749#section-1.3)。</span><span class="sxs-lookup"><span data-stu-id="2655b-139">Familiarity with [OAuth](http://tools.ietf.org/html/rfc6749) terminology, including [Roles](http://tools.ietf.org/html/rfc6749#section-1.1), [Protocol Flow](http://tools.ietf.org/html/rfc6749#section-1.2), and [Authorization Grant](http://tools.ietf.org/html/rfc6749#section-1.3).</span></span> <span data-ttu-id="2655b-140">[OAuth 2.0 簡介](http://tools.ietf.org/html/rfc6749#section-1)提供了絕佳的簡介。</span><span class="sxs-lookup"><span data-stu-id="2655b-140">[OAuth 2.0 introduction](http://tools.ietf.org/html/rfc6749#section-1) provides a good introduction.</span></span>

## <a name="create-an-authorization-server"></a><span data-ttu-id="2655b-141">建立授權伺服器</span><span class="sxs-lookup"><span data-stu-id="2655b-141">Create an Authorization Server</span></span>

<span data-ttu-id="2655b-142">在本教學課程中，我們大致上描繪出如何使用[OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx)和 ASP.NET MVC 建立授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="2655b-142">In this tutorial, we will roughly sketch out how to use [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) and ASP.NET MVC to create an authorization server.</span></span> <span data-ttu-id="2655b-143">我們希望即將提供下載完整的範例中，如本教學課程中不包含每個步驟所示。</span><span class="sxs-lookup"><span data-stu-id="2655b-143">We hope to soon provide a download for the completed sample, as this tutorial does not include each step.</span></span> <span data-ttu-id="2655b-144">首先，建立名為空的 web 應用程式*AuthorizationServer*並安裝下列封裝：</span><span class="sxs-lookup"><span data-stu-id="2655b-144">First, create an empty web app named *AuthorizationServer* and install the following packages:</span></span>

- <span data-ttu-id="2655b-145">Microsoft.AspNet.Mvc</span><span class="sxs-lookup"><span data-stu-id="2655b-145">Microsoft.AspNet.Mvc</span></span>
- <span data-ttu-id="2655b-146">Microsoft.Owin.Host.SystemWeb</span><span class="sxs-lookup"><span data-stu-id="2655b-146">Microsoft.Owin.Host.SystemWeb</span></span>
- <span data-ttu-id="2655b-147">Microsoft.Owin.Security.OAuth</span><span class="sxs-lookup"><span data-stu-id="2655b-147">Microsoft.Owin.Security.OAuth</span></span>
- <span data-ttu-id="2655b-148">Microsoft.Owin.Security.Cookies</span><span class="sxs-lookup"><span data-stu-id="2655b-148">Microsoft.Owin.Security.Cookies</span></span>
- <span data-ttu-id="2655b-149">Microsoft.Owin.Security.Google （或任何其他社交登入封裝，例如 Microsoft.Owin.Security.Facebook）</span><span class="sxs-lookup"><span data-stu-id="2655b-149">Microsoft.Owin.Security.Google (Or any other social login package such as Microsoft.Owin.Security.Facebook)</span></span>

<span data-ttu-id="2655b-150">新增[OWIN 啟動類別](owin-startup-class-detection.md)在專案根資料夾中名為*啟動*。</span><span class="sxs-lookup"><span data-stu-id="2655b-150">Add an [OWIN Startup class](owin-startup-class-detection.md) under the project root folder named *Startup*.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

<span data-ttu-id="2655b-151">建立*應用程式\_啟動*資料夾。</span><span class="sxs-lookup"><span data-stu-id="2655b-151">Create an *App\_Start* folder.</span></span> <span data-ttu-id="2655b-152">選取 *應用程式\_開始*資料夾，然後使用 Shift + Alt + A 將已下載的版本*AuthorizationServer\App\_Start\Startup.Auth.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="2655b-152">Select the *App\_Start* folder and use Shift+Alt+A to add the downloaded version of the *AuthorizationServer\App\_Start\Startup.Auth.cs* file.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

<span data-ttu-id="2655b-153">上述程式碼可讓應用程式/外部登入 cookie 和 Google 驗證其本身的授權伺服器會用它來管理帳戶。</span><span class="sxs-lookup"><span data-stu-id="2655b-153">The code above enables application/external sign in cookies and Google authentication, which are used by authorization server itself to manage accounts.</span></span>

<span data-ttu-id="2655b-154">`UseOAuthAuthorizationServer`擴充方法是設定授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="2655b-154">The `UseOAuthAuthorizationServer` extension method is to setup the authorization server.</span></span> <span data-ttu-id="2655b-155">安裝程式選項如下：</span><span class="sxs-lookup"><span data-stu-id="2655b-155">The setup options are:</span></span>

- <span data-ttu-id="2655b-156">`AuthorizeEndpointPath`: 要求路徑，而用戶端應用程式重新導向 user-agent 以取得使用者同意發出的權杖或程式碼。</span><span class="sxs-lookup"><span data-stu-id="2655b-156">`AuthorizeEndpointPath`: The request path where client applications will redirect the user-agent in order to obtain the users consent to issue a token or code.</span></span> <span data-ttu-id="2655b-157">其開頭必須前置斜線，比方說，「`/Authorize`"。</span><span class="sxs-lookup"><span data-stu-id="2655b-157">It must begin with a leading slash, for example, "`/Authorize`".</span></span>
- <span data-ttu-id="2655b-158">`TokenEndpointPath`: 要求路徑用戶端應用程式直接進行通訊來取得存取權杖。</span><span class="sxs-lookup"><span data-stu-id="2655b-158">`TokenEndpointPath`: The request path client applications directly communicate to obtain the access token.</span></span> <span data-ttu-id="2655b-159">它必須以斜線，例如"/token"開頭。</span><span class="sxs-lookup"><span data-stu-id="2655b-159">It must begin with a leading slash, like "/Token".</span></span> <span data-ttu-id="2655b-160">如果用戶端會發出[用戶端\_祕密](http://tools.ietf.org/html/rfc6749#appendix-A.2)，它必須提供至此端點。</span><span class="sxs-lookup"><span data-stu-id="2655b-160">If the client is issued a [client\_secret](http://tools.ietf.org/html/rfc6749#appendix-A.2), it must be provided to this endpoint.</span></span>
- <span data-ttu-id="2655b-161">`ApplicationCanDisplayErrors`： 設定為`true`如果想要產生的用戶端驗證錯誤的自訂錯誤頁面上的 web 應用程式`/Authorize`端點。</span><span class="sxs-lookup"><span data-stu-id="2655b-161">`ApplicationCanDisplayErrors`: Set to `true` if the web application wants to generate a custom error page for the client validation errors on `/Authorize` endpoint.</span></span> <span data-ttu-id="2655b-162">這只需要的情況下，瀏覽器不會重新導向回用戶端應用程式，例如，當`client_id`或`redirect_uri`不正確。</span><span class="sxs-lookup"><span data-stu-id="2655b-162">This is only needed for cases where the browser is not redirected back to the client application, for example, when the `client_id` or `redirect_uri` are incorrect.</span></span> <span data-ttu-id="2655b-163">`/Authorize`端點應該會看到 「 oauth。錯誤 」、 「 oauth。ErrorDescription"和"oauth。ErrorUri"屬性會新增至 OWIN 環境。</span><span class="sxs-lookup"><span data-stu-id="2655b-163">The `/Authorize` endpoint should expect to see the "oauth.Error", "oauth.ErrorDescription", and "oauth.ErrorUri" properties are added to the OWIN environment.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="2655b-164">如果沒有，則為 true，授權伺服器會傳回預設的錯誤網頁的錯誤詳細資料。</span><span class="sxs-lookup"><span data-stu-id="2655b-164">If not true, the authorization server will return a default error page with the error details.</span></span>
- <span data-ttu-id="2655b-165">`AllowInsecureHttp`: True，以允許授權和語彙基元要求抵達 HTTP URI 位址，並允許連入`redirect_uri`授權要求參數具有 HTTP URI 位址。</span><span class="sxs-lookup"><span data-stu-id="2655b-165">`AllowInsecureHttp`: True to allow authorize and token requests to arrive on HTTP URI addresses, and to allow incoming `redirect_uri` authorize request parameters to have HTTP URI addresses.</span></span> 

    > [!WARNING]
    > <span data-ttu-id="2655b-166">安全性-這是只有開發。</span><span class="sxs-lookup"><span data-stu-id="2655b-166">Security - This is for development only.</span></span>
- <span data-ttu-id="2655b-167">`Provider`: 應用程式提供給授權伺服器中介軟體所引發的程序事件的物件。</span><span class="sxs-lookup"><span data-stu-id="2655b-167">`Provider`: The object provided by the application to process events raised by the Authorization Server middleware.</span></span> <span data-ttu-id="2655b-168">應用程式可能完整，實作介面，或它可能會建立的執行個體`OAuthAuthorizationServerProvider`並指派此伺服器所支援的 OAuth 流程所需的委派。</span><span class="sxs-lookup"><span data-stu-id="2655b-168">The application may implement the interface fully, or it may create an instance of `OAuthAuthorizationServerProvider` and assign delegates necessary for the OAuth flows this server supports.</span></span>
- <span data-ttu-id="2655b-169">`AuthorizationCodeProvider`： 產生單次使用授權碼傳回至用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="2655b-169">`AuthorizationCodeProvider`: Produces a single-use authorization code to return to the client application.</span></span> <span data-ttu-id="2655b-170">OAuth 伺服器可保護應用程式**必須**提供的執行個體`AuthorizationCodeProvider`語彙基元所產生的程式`OnCreate/OnCreateAsync`事件會被視為適用於只有一個呼叫`OnReceive/OnReceiveAsync`。</span><span class="sxs-lookup"><span data-stu-id="2655b-170">For the OAuth server to be secure the application **MUST** provide an instance for `AuthorizationCodeProvider` where the token produced by the `OnCreate/OnCreateAsync` event is considered valid for only one call to `OnReceive/OnReceiveAsync`.</span></span>
- <span data-ttu-id="2655b-171">`RefreshTokenProvider`： 產生可用來產生新的存取權杖時所需的重新整理權杖。</span><span class="sxs-lookup"><span data-stu-id="2655b-171">`RefreshTokenProvider`: Produces a refresh token which may be used to produce a new access token when needed.</span></span> <span data-ttu-id="2655b-172">如果未提供授權伺服器不會傳回來自重新整理權杖`/Token`端點。</span><span class="sxs-lookup"><span data-stu-id="2655b-172">If not provided the authorization server will not return refresh tokens from the `/Token` endpoint.</span></span>

## <a name="account-management"></a><span data-ttu-id="2655b-173">帳戶管理</span><span class="sxs-lookup"><span data-stu-id="2655b-173">Account Management</span></span>

<span data-ttu-id="2655b-174">OAuth 不在意您何處或如何管理您的使用者帳戶資訊。</span><span class="sxs-lookup"><span data-stu-id="2655b-174">OAuth doesn't care where or how you manage your user account information.</span></span> <span data-ttu-id="2655b-175">它有[ASP.NET Identity](../../../identity/index.md)即負責。</span><span class="sxs-lookup"><span data-stu-id="2655b-175">It's [ASP.NET Identity](../../../identity/index.md) which is responsible for it.</span></span> <span data-ttu-id="2655b-176">在本教學課程中，我們將簡化帳戶管理的程式碼，並先確定該使用者可以使用 OWIN cookie 中介軟體登入。</span><span class="sxs-lookup"><span data-stu-id="2655b-176">In this tutorial, we will simplify the account management code and just make sure that user can login using OWIN cookie middleware.</span></span> <span data-ttu-id="2655b-177">以下是簡化的範例程式碼`AccountController`:</span><span class="sxs-lookup"><span data-stu-id="2655b-177">Here is the simplified sample code for the `AccountController`:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

<span data-ttu-id="2655b-178">`ValidateClientRedirectUri` 用來驗證用戶端，其已註冊的重新導向 url。</span><span class="sxs-lookup"><span data-stu-id="2655b-178">`ValidateClientRedirectUri` is used to validate the client with its registered redirect URL.</span></span> <span data-ttu-id="2655b-179">`ValidateClientAuthentication` 檢查基本配置標頭和表單內文，以取得用戶端的認證。</span><span class="sxs-lookup"><span data-stu-id="2655b-179">`ValidateClientAuthentication` checks the basic scheme header and form body to get the client's credentials.</span></span>

<span data-ttu-id="2655b-180">登入頁面如下所示：</span><span class="sxs-lookup"><span data-stu-id="2655b-180">The login page is shown below:</span></span>

![](owin-oauth-20-authorization-server/_static/image1.png)

<span data-ttu-id="2655b-181">檢閱 IETF OAuth 2[授權碼授與](http://tools.ietf.org/html/rfc6749#section-4.1)現在區段。</span><span class="sxs-lookup"><span data-stu-id="2655b-181">Review the IETF's OAuth 2 [Authorization Code Grant](http://tools.ietf.org/html/rfc6749#section-4.1) section now.</span></span> 

<span data-ttu-id="2655b-182">**提供者**（在下表中） 是[OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx)。提供者，也就是型別的`OAuthAuthorizationServerProvider`，其中包含所有的 OAuth 伺服器事件。</span><span class="sxs-lookup"><span data-stu-id="2655b-182">**Provider** (in the table below) is [OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx).Provider, which is of type `OAuthAuthorizationServerProvider`, which contains all OAuth server events.</span></span> 

| <span data-ttu-id="2655b-183">從授權碼授與區段的流程步驟</span><span class="sxs-lookup"><span data-stu-id="2655b-183">Flow steps from Authorization Code Grant section</span></span> | <span data-ttu-id="2655b-184">下載範例會執行這些步驟：</span><span class="sxs-lookup"><span data-stu-id="2655b-184">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="2655b-185">（A） 用戶端導向資源擁有者的使用者代理程式的授權端點以起始流程。</span><span class="sxs-lookup"><span data-stu-id="2655b-185">(A) The client initiates the flow by directing the resource owner's user-agent to the authorization endpoint.</span></span> <span data-ttu-id="2655b-186">用戶端會包含其用戶端識別碼、 要求的範圍、 本機狀態和重新導向 URI 的授權伺服器會將 user-agent 回後授與 （或拒絕存取）。</span><span class="sxs-lookup"><span data-stu-id="2655b-186">The client includes its client identifier, requested scope, local state, and a redirection URI to which the authorization server will send the user-agent back once access is granted (or denied).</span></span> | <span data-ttu-id="2655b-187">Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint</span><span class="sxs-lookup"><span data-stu-id="2655b-187">Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint</span></span> |
|  |  |
| <span data-ttu-id="2655b-188">（B） 授權伺服器會驗證資源擁有者 （透過使用者-代理程式），然後表明資源擁有者授與或拒絕用戶端的存取要求。</span><span class="sxs-lookup"><span data-stu-id="2655b-188">(B) The authorization server authenticates the resource owner (via the user-agent) and establishes whether the resource owner grants or denies the client's access request.</span></span> | <span data-ttu-id="2655b-189">**&lt;如果使用者授與存取權&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="2655b-189">**&lt;If user grants access&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="2655b-190">（C） 假設資源擁有者授與存取，授權伺服器會將使用者代理程式重新導向回用戶端會使用重新導向 URI 提供先前 （在要求中或在用戶端註冊）。</span><span class="sxs-lookup"><span data-stu-id="2655b-190">(C) Assuming the resource owner grants access, the authorization server redirects the user-agent back to the client using the redirection URI provided earlier (in the request or during client registration).</span></span> <span data-ttu-id="2655b-191">...</span><span class="sxs-lookup"><span data-stu-id="2655b-191">...</span></span> |  |
|  |  |
| <span data-ttu-id="2655b-192">（D) 用戶端向授權伺服器的權杖端點要求存取權杖，加上一個步驟中收到的授權碼。</span><span class="sxs-lookup"><span data-stu-id="2655b-192">(D) The client requests an access token from the authorization server's token endpoint by including the authorization code received in the previous step.</span></span> <span data-ttu-id="2655b-193">當提出要求，用戶端會向授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="2655b-193">When making the request, the client authenticates with the authorization server.</span></span> <span data-ttu-id="2655b-194">用戶端會包含重新導向 URI 用來取得授權碼，以便驗證。</span><span class="sxs-lookup"><span data-stu-id="2655b-194">The client includes the redirection URI used to obtain the authorization code for verification.</span></span> | <span data-ttu-id="2655b-195">Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="2655b-195">Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync</span></span> |

<span data-ttu-id="2655b-196">如需範例實作`AuthorizationCodeProvider.CreateAsync`和`ReceiveAsync`來控制建立和驗證授權碼如下所示。</span><span class="sxs-lookup"><span data-stu-id="2655b-196">A sample implementation for `AuthorizationCodeProvider.CreateAsync` and `ReceiveAsync` to control the creation and validation of authorization code is shown below.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

<span data-ttu-id="2655b-197">上述程式碼會使用記憶體中的並行字典來儲存程式碼和身分識別的票證和還原之後接收程式碼的身分識別。</span><span class="sxs-lookup"><span data-stu-id="2655b-197">The code above uses an in-memory concurrent dictionary to store the code and identity ticket and restore the identity after receiving the code.</span></span> <span data-ttu-id="2655b-198">在實際的應用程式，它會由持續性資料存放區將其取代。</span><span class="sxs-lookup"><span data-stu-id="2655b-198">In a real application, it would be replaced by a persistent data store.</span></span> <span data-ttu-id="2655b-199">授權端點做為資源擁有者授與存取權的用戶端。</span><span class="sxs-lookup"><span data-stu-id="2655b-199">The authorization endpoint is for the resource owner to grant access to the client.</span></span> <span data-ttu-id="2655b-200">通常，它需要的使用者介面，以便使用者按一下按鈕，並確認授與。</span><span class="sxs-lookup"><span data-stu-id="2655b-200">Usually, it needs a user interface to allow the user to click a button and confirm the grant.</span></span> <span data-ttu-id="2655b-201">OWIN OAuth 中介軟體可讓應用程式程式碼來處理授權端點。</span><span class="sxs-lookup"><span data-stu-id="2655b-201">OWIN OAuth middleware allows application code to handle the authorization endpoint.</span></span> <span data-ttu-id="2655b-202">在我們的範例應用程式，我們會使用 MVC 控制器呼叫`OAuthController`來加以處理。</span><span class="sxs-lookup"><span data-stu-id="2655b-202">In our sample app, we use an MVC controller called `OAuthController` to handle it.</span></span> <span data-ttu-id="2655b-203">以下是範例實作：</span><span class="sxs-lookup"><span data-stu-id="2655b-203">Here is the sample implementation:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

<span data-ttu-id="2655b-204">`Authorize`動作會先檢查使用者是否已登入授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="2655b-204">The `Authorize` action will first check if the user has logged in to the authorization server.</span></span> <span data-ttu-id="2655b-205">如果沒有，驗證中介軟體挑戰使用 「 應用程式 」 cookie 來驗證呼叫端，並將重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="2655b-205">If not, the authentication middleware challenges the caller to authenticate using the "Application" cookie and redirects to the login page.</span></span> <span data-ttu-id="2655b-206">（請參閱上述的反白顯示程式碼）。如果使用者已登入，它會轉譯 [Authorize] 檢視中，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2655b-206">(See highlighted code above.) If user has logged in, it will render the Authorize view, as shown below:</span></span>

![](owin-oauth-20-authorization-server/_static/image2.png)

<span data-ttu-id="2655b-207">如果**Grant**  按鈕已選取，`Authorize`動作將會建立新的"Bearer"身分識別和使用它登入。</span><span class="sxs-lookup"><span data-stu-id="2655b-207">If the **Grant** button is selected, the `Authorize` action will create a new "Bearer" identity and sign in with it.</span></span> <span data-ttu-id="2655b-208">它會觸發產生持有人權杖，並將它傳送回 JSON 裝載的用戶端的授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="2655b-208">It will trigger the authorization server to generate a bearer token and send it back to the client with JSON payload.</span></span> 

### <a name="implicit-grant"></a><span data-ttu-id="2655b-209">隱含授與</span><span class="sxs-lookup"><span data-stu-id="2655b-209">Implicit Grant</span></span>

<span data-ttu-id="2655b-210">請參閱 IETF OAuth 2[隱含授與](http://tools.ietf.org/html/rfc6749#section-4.2)現在區段。</span><span class="sxs-lookup"><span data-stu-id="2655b-210">Refer to the IETF's OAuth 2 [Implicit Grant](http://tools.ietf.org/html/rfc6749#section-4.2) section now.</span></span>

 <span data-ttu-id="2655b-211">[隱含授與](http://tools.ietf.org/html/rfc6749#section-4.2)[圖 4] 所示的 flow，流程，以及對應的 OWIN OAuth 則中介軟體會遵循。</span><span class="sxs-lookup"><span data-stu-id="2655b-211">The [Implicit Grant](http://tools.ietf.org/html/rfc6749#section-4.2) flow shown in Figure 4 is the flow and mapping which the OWIN OAuth middleware follows.</span></span>  

| <span data-ttu-id="2655b-212">隱含授與一節中的流程步驟</span><span class="sxs-lookup"><span data-stu-id="2655b-212">Flow steps from Implicit Grant section</span></span> | <span data-ttu-id="2655b-213">下載範例會執行這些步驟：</span><span class="sxs-lookup"><span data-stu-id="2655b-213">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="2655b-214">（A） 用戶端導向資源擁有者的使用者代理程式的授權端點以起始流程。</span><span class="sxs-lookup"><span data-stu-id="2655b-214">(A) The client initiates the flow by directing the resource owner's user-agent to the authorization endpoint.</span></span> <span data-ttu-id="2655b-215">用戶端會包含其用戶端識別碼、 要求的範圍、 本機狀態和重新導向 URI 的授權伺服器會將 user-agent 回後授與 （或拒絕存取）。</span><span class="sxs-lookup"><span data-stu-id="2655b-215">The client includes its client identifier, requested scope, local state, and a redirection URI to which the authorization server will send the user-agent back once access is granted (or denied).</span></span> | <span data-ttu-id="2655b-216">Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint</span><span class="sxs-lookup"><span data-stu-id="2655b-216">Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint</span></span> |
|  |  |
| <span data-ttu-id="2655b-217">（B） 授權伺服器會驗證資源擁有者 （透過使用者-代理程式），然後表明資源擁有者授與或拒絕用戶端的存取要求。</span><span class="sxs-lookup"><span data-stu-id="2655b-217">(B) The authorization server authenticates the resource owner (via the user-agent) and establishes whether the resource owner grants or denies the client's access request.</span></span> | <span data-ttu-id="2655b-218">**&lt;如果使用者授與存取權&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="2655b-218">**&lt;If user grants access&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="2655b-219">（C） 假設資源擁有者授與存取，授權伺服器會將使用者代理程式重新導向回用戶端會使用重新導向 URI 提供先前 （在要求中或在用戶端註冊）。</span><span class="sxs-lookup"><span data-stu-id="2655b-219">(C) Assuming the resource owner grants access, the authorization server redirects the user-agent back to the client using the redirection URI provided earlier (in the request or during client registration).</span></span> <span data-ttu-id="2655b-220">...</span><span class="sxs-lookup"><span data-stu-id="2655b-220">...</span></span> |  |
|  |  |
| <span data-ttu-id="2655b-221">（D) 用戶端向授權伺服器的權杖端點要求存取權杖，加上一個步驟中收到的授權碼。</span><span class="sxs-lookup"><span data-stu-id="2655b-221">(D) The client requests an access token from the authorization server's token endpoint by including the authorization code received in the previous step.</span></span> <span data-ttu-id="2655b-222">當提出要求，用戶端會向授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="2655b-222">When making the request, the client authenticates with the authorization server.</span></span> <span data-ttu-id="2655b-223">用戶端會包含重新導向 URI 用來取得授權碼，以便驗證。</span><span class="sxs-lookup"><span data-stu-id="2655b-223">The client includes the redirection URI used to obtain the authorization code for verification.</span></span> |  |

<span data-ttu-id="2655b-224">因為我們已實作授權端點 (`OAuthController.Authorize`動作) 的授權碼授與，因此它會自動啟用也隱含流程。</span><span class="sxs-lookup"><span data-stu-id="2655b-224">Since we already implemented the authorization endpoint (`OAuthController.Authorize` action) for authorization code grant, it automatically enables implicit flow as well.</span></span> <span data-ttu-id="2655b-225">注意︰`Provider.ValidateClientRedirectUri`用來驗證具有其傳送存取權杖來惡意用戶端時，防止隱含授與流程的重新導向 URL 的用戶端識別碼 ([攔截攻擊](https://www.owasp.org/index.php/Man-in-the-middle_attack))。</span><span class="sxs-lookup"><span data-stu-id="2655b-225">Note: `Provider.ValidateClientRedirectUri` is used to validate the client ID with its redirection URL, which protects the implicit grant flow from sending the access token to malicious clients ([Man-in-the-middle attack](https://www.owasp.org/index.php/Man-in-the-middle_attack)).</span></span>

### <a name="resource-owner-password-credentials-grant"></a><span data-ttu-id="2655b-226">資源擁有者密碼認證授與</span><span class="sxs-lookup"><span data-stu-id="2655b-226">Resource Owner Password Credentials Grant</span></span>

<span data-ttu-id="2655b-227">請參閱 IETF OAuth 2[資源擁有者密碼認證授與](http://tools.ietf.org/html/rfc6749#section-4.3)現在區段。</span><span class="sxs-lookup"><span data-stu-id="2655b-227">Refer to the IETF's OAuth 2 [Resource Owner Password Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.3) section now.</span></span>

 <span data-ttu-id="2655b-228">[資源擁有者密碼認證授與](http://tools.ietf.org/html/rfc6749#section-4.3)[圖 5] 所示的 flow，流程，以及對應的 OWIN OAuth 則中介軟體會遵循。</span><span class="sxs-lookup"><span data-stu-id="2655b-228">The [Resource Owner Password Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.3) flow shown in Figure 5 is the flow and mapping which the OWIN OAuth middleware follows.</span></span>  

| <span data-ttu-id="2655b-229">從資源擁有者密碼認證授與區段的流程步驟</span><span class="sxs-lookup"><span data-stu-id="2655b-229">Flow steps from Resource Owner Password Credentials Grant section</span></span> | <span data-ttu-id="2655b-230">下載範例會執行這些步驟：</span><span class="sxs-lookup"><span data-stu-id="2655b-230">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="2655b-231">（A） 資源擁有者會為用戶端提供自己的使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="2655b-231">(A) The resource owner provides the client with its username and password.</span></span> |  |
|  |  |
| <span data-ttu-id="2655b-232">（B） 用戶端包含來自資源擁有者的認證，從授權伺服器的權杖端點要求存取權杖。</span><span class="sxs-lookup"><span data-stu-id="2655b-232">(B) The client requests an access token from the authorization server's token endpoint by including the credentials received from the resource owner.</span></span> <span data-ttu-id="2655b-233">當提出要求，用戶端會向授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="2655b-233">When making the request, the client authenticates with the authorization server.</span></span> | <span data-ttu-id="2655b-234">Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="2655b-234">Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="2655b-235">（C） 授權伺服器會驗證用戶端和驗證資源擁有者認證，然後如果有效，就會發出存取權杖。</span><span class="sxs-lookup"><span data-stu-id="2655b-235">(C) The authorization server authenticates the client and validates the resource owner credentials, and if valid, issues an access token.</span></span> |  |

<span data-ttu-id="2655b-236">以下是範例實作`Provider.GrantResourceOwnerCredentials`:</span><span class="sxs-lookup"><span data-stu-id="2655b-236">Here is the sample implementation for `Provider.GrantResourceOwnerCredentials`:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> <span data-ttu-id="2655b-237">上述程式碼用來說明本教學課程的這一節，不應在安全或生產環境應用程式。</span><span class="sxs-lookup"><span data-stu-id="2655b-237">The code above is intended to explain this section of the tutorial and should not be used in secure or production apps.</span></span> <span data-ttu-id="2655b-238">它不會檢查資源擁有者認證。</span><span class="sxs-lookup"><span data-stu-id="2655b-238">It does not check the resource owners credentials.</span></span> <span data-ttu-id="2655b-239">它會假設每個認證有效，並為其建立新的身分識別。</span><span class="sxs-lookup"><span data-stu-id="2655b-239">It assumes every credential is valid and creates a new identity for it.</span></span> <span data-ttu-id="2655b-240">新的身分識別將用來產生存取權杖和重新整理權杖中。</span><span class="sxs-lookup"><span data-stu-id="2655b-240">The new identity will be used to generate the access token and refresh token.</span></span> <span data-ttu-id="2655b-241">程式碼取代您自己的安全帳戶管理程式碼。</span><span class="sxs-lookup"><span data-stu-id="2655b-241">Please replace the code with your own secure account management code.</span></span>


### <a name="client-credentials-grant"></a><span data-ttu-id="2655b-242">用戶端認證授與</span><span class="sxs-lookup"><span data-stu-id="2655b-242">Client Credentials Grant</span></span>

<span data-ttu-id="2655b-243">請參閱 IETF OAuth 2[用戶端認證授與](http://tools.ietf.org/html/rfc6749#section-4.4)現在區段。</span><span class="sxs-lookup"><span data-stu-id="2655b-243">Refer to the IETF's OAuth 2 [Client Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.4) section now.</span></span>

 <span data-ttu-id="2655b-244">[用戶端認證授與](http://tools.ietf.org/html/rfc6749#section-4.4)[圖 6] 所示的 flow，流程，以及對應的 OWIN OAuth 則中介軟體會遵循。</span><span class="sxs-lookup"><span data-stu-id="2655b-244">The [Client Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.4) flow shown in Figure 6 is the flow and mapping which the OWIN OAuth middleware follows.</span></span>  

| <span data-ttu-id="2655b-245">從用戶端認證授與區段的流程步驟</span><span class="sxs-lookup"><span data-stu-id="2655b-245">Flow steps from Client Credentials Grant section</span></span> | <span data-ttu-id="2655b-246">下載範例會執行這些步驟：</span><span class="sxs-lookup"><span data-stu-id="2655b-246">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="2655b-247">（A) 用戶端向授權伺服器，並向權杖端點要求存取權杖。</span><span class="sxs-lookup"><span data-stu-id="2655b-247">(A) The client authenticates with the authorization server and requests an access token from the token endpoint.</span></span> | <span data-ttu-id="2655b-248">Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="2655b-248">Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="2655b-249">（B） 授權伺服器會驗證用戶端，然後如果有效，就會發出存取權杖。</span><span class="sxs-lookup"><span data-stu-id="2655b-249">(B) The authorization server authenticates the client, and if valid, issues an access token.</span></span> |  |

<span data-ttu-id="2655b-250">以下是範例實作`Provider.GrantClientCredentials`:</span><span class="sxs-lookup"><span data-stu-id="2655b-250">Here is the sample implementation for `Provider.GrantClientCredentials`:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="2655b-251">上述程式碼用來說明本教學課程的這一節，不應在安全或生產環境應用程式。</span><span class="sxs-lookup"><span data-stu-id="2655b-251">The code above is intended to explain this section of the tutorial and should not be used in secure or production apps.</span></span> <span data-ttu-id="2655b-252">程式碼取代您自己的安全用戶端管理程式碼。</span><span class="sxs-lookup"><span data-stu-id="2655b-252">Please replace the code with your own secure client management code.</span></span>


### <a name="refresh-token"></a><span data-ttu-id="2655b-253">重新整理權杖</span><span class="sxs-lookup"><span data-stu-id="2655b-253">Refresh Token</span></span>

<span data-ttu-id="2655b-254">請參閱 IETF OAuth 2[重新整理權杖](http://tools.ietf.org/html/rfc6749#section-1.5)現在區段。</span><span class="sxs-lookup"><span data-stu-id="2655b-254">Refer to the IETF's OAuth 2 [Refresh Token](http://tools.ietf.org/html/rfc6749#section-1.5) section now.</span></span>

 <span data-ttu-id="2655b-255">[重新整理權杖](http://tools.ietf.org/html/rfc6749#section-1.5)[圖 2] 所示的 flow，流程，以及對應的 OWIN OAuth 則中介軟體會遵循。</span><span class="sxs-lookup"><span data-stu-id="2655b-255">The [Refresh Token](http://tools.ietf.org/html/rfc6749#section-1.5) flow shown in Figure 2 is the flow and mapping which the OWIN OAuth middleware follows.</span></span>  

| <span data-ttu-id="2655b-256">從用戶端認證授與區段的流程步驟</span><span class="sxs-lookup"><span data-stu-id="2655b-256">Flow steps from Client Credentials Grant section</span></span> | <span data-ttu-id="2655b-257">下載範例會執行這些步驟：</span><span class="sxs-lookup"><span data-stu-id="2655b-257">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="2655b-258">（G） 用戶端會要求新的存取權杖，向授權伺服器，並呈現重新整理權杖。</span><span class="sxs-lookup"><span data-stu-id="2655b-258">(G) The client requests a new access token by authenticating with the authorization server and presenting the refresh token.</span></span> <span data-ttu-id="2655b-259">用戶端驗證需求會根據用戶端類型和授權伺服器原則。</span><span class="sxs-lookup"><span data-stu-id="2655b-259">The client authentication requirements are based on the client type and on the authorization server policies.</span></span> | <span data-ttu-id="2655b-260">Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsyncRefreshTokenProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="2655b-260">Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="2655b-261">（H） 授權伺服器會驗證用戶端和驗證重新整理權杖，然後如果有效，就會發出新的存取權杖 （以及選擇性地為新的重新整理語彙基元）。</span><span class="sxs-lookup"><span data-stu-id="2655b-261">(H) The authorization server authenticates the client and validates the refresh token, and if valid, issues a new access token (and, optionally, a new refresh token).</span></span> |  |

<span data-ttu-id="2655b-262">以下是範例實作`Provider.GrantRefreshToken`:</span><span class="sxs-lookup"><span data-stu-id="2655b-262">Here is the sample implementation for `Provider.GrantRefreshToken`:</span></span> 

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a><span data-ttu-id="2655b-263">建立資源的伺服器保護的存取權杖</span><span class="sxs-lookup"><span data-stu-id="2655b-263">Create a Resource Server which is protected by Access Token</span></span>

<span data-ttu-id="2655b-264">建立空的 web 應用程式專案並安裝下列專案中的封裝：</span><span class="sxs-lookup"><span data-stu-id="2655b-264">Create an empty web app project and install following packages in the project:</span></span>

- <span data-ttu-id="2655b-265">Microsoft.AspNet.WebApi.Owin</span><span class="sxs-lookup"><span data-stu-id="2655b-265">Microsoft.AspNet.WebApi.Owin</span></span>
- <span data-ttu-id="2655b-266">Microsoft.Owin.Host.SystemWeb</span><span class="sxs-lookup"><span data-stu-id="2655b-266">Microsoft.Owin.Host.SystemWeb</span></span>
- <span data-ttu-id="2655b-267">Microsoft.Owin.Security.OAuth</span><span class="sxs-lookup"><span data-stu-id="2655b-267">Microsoft.Owin.Security.OAuth</span></span>

<span data-ttu-id="2655b-268">建立啟動類別，並設定驗證和 Web API。</span><span class="sxs-lookup"><span data-stu-id="2655b-268">Create a startup class and configure authentication and Web API.</span></span> <span data-ttu-id="2655b-269">請參閱*AuthorizationServer\ResourceServer\Startup.cs*下載範例中。</span><span class="sxs-lookup"><span data-stu-id="2655b-269">See *AuthorizationServer\ResourceServer\Startup.cs* in the sample download.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

<span data-ttu-id="2655b-270">請參閱*AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs*下載範例中。</span><span class="sxs-lookup"><span data-stu-id="2655b-270">See *AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs* in the sample download.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

<span data-ttu-id="2655b-271">請參閱*AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs*下載範例中。</span><span class="sxs-lookup"><span data-stu-id="2655b-271">See *AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs* in the sample download.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- <span data-ttu-id="2655b-272">`UseCors` 方法會允許所有網域的 CORS。</span><span class="sxs-lookup"><span data-stu-id="2655b-272">`UseCors` method allows CORS for all domains.</span></span>
- <span data-ttu-id="2655b-273">`UseOAuthBearerAuthentication` 方法可讓 OAuth 持有人權杖驗證中介軟體會接收及驗證從要求中的授權標頭的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="2655b-273">`UseOAuthBearerAuthentication` method enables OAuth bearer token authentication middleware which will receive and validate bearer token from authorization header in the request.</span></span>
- <span data-ttu-id="2655b-274">`Config.SuppressDefaultHostAuthenticaiton` 隱藏預設主機驗證的主體，從應用程式，因此所有的要求將會是匿名的這個呼叫之後。</span><span class="sxs-lookup"><span data-stu-id="2655b-274">`Config.SuppressDefaultHostAuthenticaiton` suppresses default host authenticated principal from the app, therefore all requests will be anonymous after this call.</span></span>
- <span data-ttu-id="2655b-275">`HostAuthenticationFilter` 啟用驗證，只針對指定的驗證類型。</span><span class="sxs-lookup"><span data-stu-id="2655b-275">`HostAuthenticationFilter` enables authentication just for the specified authentication type.</span></span> <span data-ttu-id="2655b-276">在此情況下，它會是持有人驗證類型。</span><span class="sxs-lookup"><span data-stu-id="2655b-276">In this case, it's bearer authentication type.</span></span>

<span data-ttu-id="2655b-277">為了示範已驗證的身分識別，我們會建立 ApiController 輸出目前的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="2655b-277">In order to demonstrate the authenticated identity, we create an ApiController to output current user's claims.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

<span data-ttu-id="2655b-278">如果授權伺服器和資源伺服器不在相同電腦上，OAuth 的中介軟體會使用不同的機器金鑰來加密和解密的持有人存取權杖。</span><span class="sxs-lookup"><span data-stu-id="2655b-278">If the authorization server and the resource server are not on the same computer, the OAuth middleware will use the different machine keys to encrypt and decrypt bearer access token.</span></span> <span data-ttu-id="2655b-279">若要共用相同的私人金鑰，這兩個專案之間，我們將新增相同`machinekey`設定中都*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="2655b-279">In order to share the same private key between both projects, we add the same `machinekey` setting in both *web.config* files.</span></span>

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a><span data-ttu-id="2655b-280">建立 OAuth 2.0 用戶端</span><span class="sxs-lookup"><span data-stu-id="2655b-280">Create OAuth 2.0 Clients</span></span>

 <span data-ttu-id="2655b-281">我們會使用[DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) NuGet 封裝來簡化用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="2655b-281">We use the [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) NuGet package to simplify the client code.</span></span>

### <a name="authorization-code-grant-client"></a><span data-ttu-id="2655b-282">授權碼授與用戶端</span><span class="sxs-lookup"><span data-stu-id="2655b-282">Authorization Code Grant Client</span></span>

 <span data-ttu-id="2655b-283">此用戶端是在 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2655b-283">This client is an MVC application.</span></span> <span data-ttu-id="2655b-284">它會觸發從後端取得存取權杖的授權碼授與流程。</span><span class="sxs-lookup"><span data-stu-id="2655b-284">It will trigger an authorization code grant flow to get the access token from backend.</span></span> <span data-ttu-id="2655b-285">它會有單一頁面，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2655b-285">It has a single page as shown below:</span></span>

![](owin-oauth-20-authorization-server/_static/image3.png)

- <span data-ttu-id="2655b-286">**授權**按鈕將瀏覽器重新導向至授權伺服器，以通知資源擁有者存取權授與此用戶端。</span><span class="sxs-lookup"><span data-stu-id="2655b-286">The **Authorize** button will redirect browser to the authorization server to notify the resource owner to grant access to this client.</span></span>
- <span data-ttu-id="2655b-287">**重新整理**按鈕會取得新存取權杖和重新整理權杖，使用目前的重新整理權杖。</span><span class="sxs-lookup"><span data-stu-id="2655b-287">The **Refresh** button will get a new access token and refresh token using the current refresh token.</span></span>
- <span data-ttu-id="2655b-288">**存取受保護的資源 API**按鈕會呼叫資源伺服器，以取得目前使用者的宣告資料，並在頁面上顯示它們。</span><span class="sxs-lookup"><span data-stu-id="2655b-288">The **Access Protected Resource API** button will call the resource server to get current user's claims data and show them on the page.</span></span>

<span data-ttu-id="2655b-289">以下是範例程式碼的`HomeController`的用戶端。</span><span class="sxs-lookup"><span data-stu-id="2655b-289">Here is the sample code of the `HomeController` of the client.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

<span data-ttu-id="2655b-290">`DotNetOpenAuth` 根據預設，需要 SSL。</span><span class="sxs-lookup"><span data-stu-id="2655b-290">`DotNetOpenAuth` requires SSL by default.</span></span> <span data-ttu-id="2655b-291">由於我們的示範，使用 HTTP，您必須新增下列組態檔中的設定：</span><span class="sxs-lookup"><span data-stu-id="2655b-291">Since our demo is using HTTP, you need to add following setting in the config file:</span></span>

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> <span data-ttu-id="2655b-292">安全性-永遠不會停用 SSL 生產應用程式中。</span><span class="sxs-lookup"><span data-stu-id="2655b-292">Security - Never disable SSL in a production app.</span></span> <span data-ttu-id="2655b-293">現在在純文字透過網路傳送登入認證。</span><span class="sxs-lookup"><span data-stu-id="2655b-293">Your login credentials are now being sent in clear-text across the wire.</span></span> <span data-ttu-id="2655b-294">上述程式碼只適用於本機範例偵錯和探索。</span><span class="sxs-lookup"><span data-stu-id="2655b-294">The code above is just for local sample debugging and exploration.</span></span>


### <a name="implicit-grant-client"></a><span data-ttu-id="2655b-295">隱含授與用戶端</span><span class="sxs-lookup"><span data-stu-id="2655b-295">Implicit Grant Client</span></span>

<span data-ttu-id="2655b-296">此用戶端使用 JavaScript 來：</span><span class="sxs-lookup"><span data-stu-id="2655b-296">This client is using JavaScript to:</span></span>

1. <span data-ttu-id="2655b-297">開啟新視窗，並將重新導向至授權端點的授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="2655b-297">Open a new window and redirect to the authorize endpoint of the Authorization Server.</span></span>
2. <span data-ttu-id="2655b-298">從取得存取權杖 URL 片段時將重新導向回。</span><span class="sxs-lookup"><span data-stu-id="2655b-298">Get the access token from URL fragments when it redirects back.</span></span>

<span data-ttu-id="2655b-299">下圖顯示此程序：</span><span class="sxs-lookup"><span data-stu-id="2655b-299">The following image shows this process:</span></span>

![](owin-oauth-20-authorization-server/_static/image4.png)

<span data-ttu-id="2655b-300">用戶端應該有兩個頁面： 一個用於首頁上，另一個則用於回呼。以下是範例程式碼中找到的 JavaScript *Index.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="2655b-300">The client should have two pages: one for home page and the other for callback.Here is the sample JavaScript code found in the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

<span data-ttu-id="2655b-301">以下是處理程式碼中的回呼*SignIn.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="2655b-301">Here is the callback handling code in *SignIn.cshtml* file:</span></span>

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> <span data-ttu-id="2655b-302">最佳的作法是將 JavaScript 移至外部檔案，並不將它內嵌加上 Razor 標記。</span><span class="sxs-lookup"><span data-stu-id="2655b-302">A best practice is to move the JavaScript to an external file and not embed it with the Razor markup.</span></span> <span data-ttu-id="2655b-303">為了簡化此範例中，在經過組合。</span><span class="sxs-lookup"><span data-stu-id="2655b-303">To keep this sample simple, they have been combined.</span></span>


### <a name="resource-owner-password-credentials-grant-client"></a><span data-ttu-id="2655b-304">資源擁有者密碼認證授與用戶端</span><span class="sxs-lookup"><span data-stu-id="2655b-304">Resource Owner Password Credentials Grant Client</span></span>

<span data-ttu-id="2655b-305">我們可以使用主控台應用程式來示範此用戶端。</span><span class="sxs-lookup"><span data-stu-id="2655b-305">We uses a console app to demo this client.</span></span> <span data-ttu-id="2655b-306">程式碼如下：</span><span class="sxs-lookup"><span data-stu-id="2655b-306">Here is the code:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a><span data-ttu-id="2655b-307">用戶端認證授與用戶端</span><span class="sxs-lookup"><span data-stu-id="2655b-307">Client Credentials Grant Client</span></span>

<span data-ttu-id="2655b-308">類似於資源擁有者密碼認證授與，以下是主控台應用程式程式碼：</span><span class="sxs-lookup"><span data-stu-id="2655b-308">Similar to the Resource Owner Password Credentials Grant, here is console app code:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
