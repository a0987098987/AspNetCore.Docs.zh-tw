---
uid: web-api/overview/security/individual-accounts-in-web-api
title: "保護使用個別的帳戶和 ASP.NET Web API 2.2 中的本機登入的 Web Api< / |Microsoft 文件"
author: MikeWasson
description: "本主題說明如何保護 web API 使用 OAuth2 驗證成員資格資料庫。 教學課程的 Visual Studio 201 中使用的軟體版本..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2014
ms.topic: article
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 8207df79c1e915b33a0ba095d917a6dc69550173
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a><span data-ttu-id="b5a82-104">保護使用個別的帳戶和 ASP.NET Web API 2.2 中的本機登入的 Web 應用程式開發介面</span><span class="sxs-lookup"><span data-stu-id="b5a82-104">Secure a Web API with Individual Accounts and Local Login in ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="b5a82-105">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b5a82-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b5a82-106">下載範例應用程式</span><span class="sxs-lookup"><span data-stu-id="b5a82-106">Download Sample App</span></span>](https://github.com/MikeWasson/LocalAccountsApp)

> <span data-ttu-id="b5a82-107">本主題說明如何保護 web API 使用 OAuth2 驗證成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="b5a82-107">This topic shows how to secure a web API using OAuth2 to authenticate against a membership database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b5a82-108">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="b5a82-108">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="b5a82-109">Visual Studio 2013 Update 3</span><span class="sxs-lookup"><span data-stu-id="b5a82-109">Visual Studio 2013 Update 3</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [<span data-ttu-id="b5a82-110">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="b5a82-110">Web API 2.2</span></span>](../releases/whats-new-in-aspnet-web-api-22.md)
> - [<span data-ttu-id="b5a82-111">ASP.NET Identity 2.1</span><span class="sxs-lookup"><span data-stu-id="b5a82-111">ASP.NET Identity 2.1</span></span>](../../../identity/index.md)


<span data-ttu-id="b5a82-112">在 Visual Studio 2013 中，Web API 專案範本可讓您驗證的三個選項：</span><span class="sxs-lookup"><span data-stu-id="b5a82-112">In Visual Studio 2013, the Web API project template gives you three options for authentication:</span></span>

- <span data-ttu-id="b5a82-113">**個別的帳戶。**</span><span class="sxs-lookup"><span data-stu-id="b5a82-113">**Individual accounts.**</span></span> <span data-ttu-id="b5a82-114">應用程式會使用成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="b5a82-114">The app uses a membership database.</span></span>
- <span data-ttu-id="b5a82-115">**組織帳戶。**</span><span class="sxs-lookup"><span data-stu-id="b5a82-115">**Organizational accounts.**</span></span> <span data-ttu-id="b5a82-116">使用者登入其 Azure Active Directory、 Office 365 或在內部部署 Active Directory 認證。</span><span class="sxs-lookup"><span data-stu-id="b5a82-116">Users sign in with their Azure Active Directory, Office 365, or on-premise Active Directory credentials.</span></span>
- <span data-ttu-id="b5a82-117">**Windows 驗證。**</span><span class="sxs-lookup"><span data-stu-id="b5a82-117">**Windows authentication.**</span></span> <span data-ttu-id="b5a82-118">此選項僅供內部網路應用程式，並使用 Windows 驗證的 IIS 模組。</span><span class="sxs-lookup"><span data-stu-id="b5a82-118">This option is intended for Intranet applications, and uses the Windows Authentication IIS module.</span></span>

<span data-ttu-id="b5a82-119">如需有關這些選項的詳細資訊，請參閱[Visual Studio 2013 中建立 ASP.NET Web 專案](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth)。</span><span class="sxs-lookup"><span data-stu-id="b5a82-119">For more details about these options, see [Creating ASP.NET Web Projects in Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).</span></span>

<span data-ttu-id="b5a82-120">個別帳戶提供登入使用者的兩種方式：</span><span class="sxs-lookup"><span data-stu-id="b5a82-120">Individual accounts provide two ways for a user to log in:</span></span>

- <span data-ttu-id="b5a82-121">**本機登入**。</span><span class="sxs-lookup"><span data-stu-id="b5a82-121">**Local login**.</span></span> <span data-ttu-id="b5a82-122">使用者會註冊在網站上，輸入使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="b5a82-122">The user registers at the site, entering a username and password.</span></span> <span data-ttu-id="b5a82-123">應用程式成員資格資料庫中儲存密碼雜湊。</span><span class="sxs-lookup"><span data-stu-id="b5a82-123">The app stores the password hash in the membership database.</span></span> <span data-ttu-id="b5a82-124">當使用者登入時，ASP.NET Identity 系統會確認密碼。</span><span class="sxs-lookup"><span data-stu-id="b5a82-124">When the user logs in, the ASP.NET Identity system verifies the password.</span></span>
- <span data-ttu-id="b5a82-125">**社交登入**。</span><span class="sxs-lookup"><span data-stu-id="b5a82-125">**Social login**.</span></span> <span data-ttu-id="b5a82-126">使用者登入的外部服務，例如 Facebook、 Microsoft 或 Google。</span><span class="sxs-lookup"><span data-stu-id="b5a82-126">The user signs in with an external service, such as Facebook, Microsoft, or Google.</span></span> <span data-ttu-id="b5a82-127">此應用程式仍在成員資格資料庫中，建立使用者項目，但不會儲存任何認證。</span><span class="sxs-lookup"><span data-stu-id="b5a82-127">The app still creates an entry for the user in the membership database, but does not store any credentials.</span></span> <span data-ttu-id="b5a82-128">使用者會驗證登入外部服務。</span><span class="sxs-lookup"><span data-stu-id="b5a82-128">The user authenticates by signing into the external service.</span></span>

<span data-ttu-id="b5a82-129">這篇文章探討本機登入案例。</span><span class="sxs-lookup"><span data-stu-id="b5a82-129">This article looks at the local login scenario.</span></span> <span data-ttu-id="b5a82-130">本機和社交登入 Web API 使用 OAuth2 驗證要求。</span><span class="sxs-lookup"><span data-stu-id="b5a82-130">For both local and social login, Web API uses OAuth2 to authenticate requests.</span></span> <span data-ttu-id="b5a82-131">然而，認證流程有不同的本機和社交登入。</span><span class="sxs-lookup"><span data-stu-id="b5a82-131">However, the credential flows are different for local and social login.</span></span>

<span data-ttu-id="b5a82-132">在本文中，我將示範簡單的應用程式，可讓使用者登入或傳送已驗證的 AJAX 呼叫 web API。</span><span class="sxs-lookup"><span data-stu-id="b5a82-132">In this article, I'll demonstrate a simple app that lets the user log in and send authenticated AJAX calls to a web API.</span></span> <span data-ttu-id="b5a82-133">您可以下載範例程式碼[這裡](https://github.com/MikeWasson/LocalAccountsApp)。</span><span class="sxs-lookup"><span data-stu-id="b5a82-133">You can download the sample code [here](https://github.com/MikeWasson/LocalAccountsApp).</span></span> <span data-ttu-id="b5a82-134">讀我檔案說明如何在 Visual Studio 中從頭開始建立範例。</span><span class="sxs-lookup"><span data-stu-id="b5a82-134">The readme describes how to create the sample from scratch in Visual Studio.</span></span>

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

<span data-ttu-id="b5a82-135">範例應用程式中使用資料繫結和 jQuery 進行傳送 AJAX 要求解 Knockout.js。</span><span class="sxs-lookup"><span data-stu-id="b5a82-135">The sample app uses Knockout.js for data-binding and jQuery for sending AJAX requests.</span></span> <span data-ttu-id="b5a82-136">我的焦點 AJAX 呼叫時，因此您不需要知道解 Knockout.js 這個發行項。</span><span class="sxs-lookup"><span data-stu-id="b5a82-136">I'll be focusing on the AJAX calls, so you don't need to know Knockout.js for this article.</span></span>

<span data-ttu-id="b5a82-137">過程中，我將說明：</span><span class="sxs-lookup"><span data-stu-id="b5a82-137">Along the way, I'll describe:</span></span>

- <span data-ttu-id="b5a82-138">應用程式做什麼用戶端。</span><span class="sxs-lookup"><span data-stu-id="b5a82-138">What the app is doing on the client side.</span></span>
- <span data-ttu-id="b5a82-139">在伺服器上發生什麼事。</span><span class="sxs-lookup"><span data-stu-id="b5a82-139">What's happening on the server.</span></span>
- <span data-ttu-id="b5a82-140">在中間的 HTTP 流量。</span><span class="sxs-lookup"><span data-stu-id="b5a82-140">The HTTP traffic in the middle.</span></span>

<span data-ttu-id="b5a82-141">首先，我們必須定義一些 OAuth2 術語。</span><span class="sxs-lookup"><span data-stu-id="b5a82-141">First, we need to define some OAuth2 terminology.</span></span>

- <span data-ttu-id="b5a82-142">*資源*。</span><span class="sxs-lookup"><span data-stu-id="b5a82-142">*Resource*.</span></span> <span data-ttu-id="b5a82-143">一些可保護的資料。</span><span class="sxs-lookup"><span data-stu-id="b5a82-143">Some piece of data that can be protected.</span></span>
- <span data-ttu-id="b5a82-144">*資源伺服器*。</span><span class="sxs-lookup"><span data-stu-id="b5a82-144">*Resource server*.</span></span> <span data-ttu-id="b5a82-145">主控資源的伺服器。</span><span class="sxs-lookup"><span data-stu-id="b5a82-145">The server that hosts the resource.</span></span>
- <span data-ttu-id="b5a82-146">*資源擁有者*。</span><span class="sxs-lookup"><span data-stu-id="b5a82-146">*Resource owner*.</span></span> <span data-ttu-id="b5a82-147">可以授與權限來存取資源的實體。</span><span class="sxs-lookup"><span data-stu-id="b5a82-147">The entity that can grant permission to access a resource.</span></span> <span data-ttu-id="b5a82-148">（通常是使用者。）</span><span class="sxs-lookup"><span data-stu-id="b5a82-148">(Typically the user.)</span></span>
- <span data-ttu-id="b5a82-149">*用戶端*： 想要存取之資源的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5a82-149">*Client*: The app that wants access to the resource.</span></span> <span data-ttu-id="b5a82-150">在本文中，用戶端是網頁瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="b5a82-150">In this article, the client is a web browser.</span></span>
- <span data-ttu-id="b5a82-151">*存取權杖*。</span><span class="sxs-lookup"><span data-stu-id="b5a82-151">*Access token*.</span></span> <span data-ttu-id="b5a82-152">授與資源的存取語彙基元。</span><span class="sxs-lookup"><span data-stu-id="b5a82-152">A token that grants access to a resource.</span></span>
- <span data-ttu-id="b5a82-153">*承載權杖*。</span><span class="sxs-lookup"><span data-stu-id="b5a82-153">*Bearer token*.</span></span> <span data-ttu-id="b5a82-154">特定類型的存取權杖，任何人都可以使用語彙基元的屬性。</span><span class="sxs-lookup"><span data-stu-id="b5a82-154">A particular type of access token, with the property that anyone can use the token.</span></span> <span data-ttu-id="b5a82-155">換句話說，用戶端不需要的密碼編譯金鑰或其他使用 bearer 權杖的密碼。</span><span class="sxs-lookup"><span data-stu-id="b5a82-155">In other words, a client doesn't need a cryptographic key or other secret to use a bearer token.</span></span> <span data-ttu-id="b5a82-156">基於這個原因，承載語彙基元應該只用於透過 HTTPS，並應該有相對較短的到期時間。</span><span class="sxs-lookup"><span data-stu-id="b5a82-156">For that reason, bearer tokens should only be used over a HTTPS, and should have relatively short expiration times.</span></span>
- <span data-ttu-id="b5a82-157">*授權伺服器*。</span><span class="sxs-lookup"><span data-stu-id="b5a82-157">*Authorization server*.</span></span> <span data-ttu-id="b5a82-158">伺服器可存取權杖。</span><span class="sxs-lookup"><span data-stu-id="b5a82-158">A server that gives out access tokens.</span></span>

<span data-ttu-id="b5a82-159">應用程式可以做為授權伺服器與資源伺服器。</span><span class="sxs-lookup"><span data-stu-id="b5a82-159">An application can act as both authorization server and resource server.</span></span> <span data-ttu-id="b5a82-160">Web API 專案範本會遵循此模式。</span><span class="sxs-lookup"><span data-stu-id="b5a82-160">The Web API project template follows this pattern.</span></span>

## <a name="local-login-credential-flow"></a><span data-ttu-id="b5a82-161">本機登入認證流程</span><span class="sxs-lookup"><span data-stu-id="b5a82-161">Local Login Credential Flow</span></span>

<span data-ttu-id="b5a82-162">Web API 所使用的本機登入，[資源擁有者密碼的流程](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html)OAuth2 中定義。</span><span class="sxs-lookup"><span data-stu-id="b5a82-162">For local login, Web API uses the [resource owner password flow](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) defined in OAuth2.</span></span>

1. <span data-ttu-id="b5a82-163">使用者會輸入到用戶端名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="b5a82-163">The user enters a name and password into the client.</span></span>
2. <span data-ttu-id="b5a82-164">用戶端傳送至授權伺服器的這些認證。</span><span class="sxs-lookup"><span data-stu-id="b5a82-164">The client sends these credentials to the authorization server.</span></span>
3. <span data-ttu-id="b5a82-165">授權伺服器驗證憑證，並傳回存取權杖。</span><span class="sxs-lookup"><span data-stu-id="b5a82-165">The authorization server authenticates the credentials and returns an access token.</span></span>
4. <span data-ttu-id="b5a82-166">若要存取受保護的資源，用戶端會在 HTTP 要求的授權標頭中包含存取權杖。</span><span class="sxs-lookup"><span data-stu-id="b5a82-166">To access a protected resource, the client includes the access token in the Authorization header of the HTTP request.</span></span>

![](individual-accounts-in-web-api/_static/image3.png)

<span data-ttu-id="b5a82-167">當您選取**個別帳戶**的 Web API 專案範本，專案會包含授權伺服器，它會驗證使用者認證會簽發權杖。</span><span class="sxs-lookup"><span data-stu-id="b5a82-167">When you select **Individual accounts** in the Web API project template, the project includes an authorization server that validates user credentials and issues tokens.</span></span> <span data-ttu-id="b5a82-168">下圖顯示相同的認證流程以 Web 應用程式開發介面的元件。</span><span class="sxs-lookup"><span data-stu-id="b5a82-168">The following diagram shows the same credential flow in terms of Web API components.</span></span>

![](individual-accounts-in-web-api/_static/image4.png)

<span data-ttu-id="b5a82-169">在此案例中，Web API 控制器會做為資源伺服器。</span><span class="sxs-lookup"><span data-stu-id="b5a82-169">In this scenario, Web API controllers act as resource servers.</span></span> <span data-ttu-id="b5a82-170">驗證篩選條件會驗證存取權杖和**[Authorize]**屬性用來保護資源。</span><span class="sxs-lookup"><span data-stu-id="b5a82-170">An authentication filter validates access tokens, and the **[Authorize]** attribute is used to protect a resource.</span></span> <span data-ttu-id="b5a82-171">當控制器或動作具有**[Authorize]**屬性，該控制站的所有要求，或必須驗證動作。</span><span class="sxs-lookup"><span data-stu-id="b5a82-171">When a controller or action has the **[Authorize]** attribute, all requests to that controller or action must be authenticated.</span></span> <span data-ttu-id="b5a82-172">否則，授權遭到拒絕，而且 Web API 會傳回 401 （未經授權） 錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5a82-172">Otherwise, authorization is denied, and Web API returns a 401 (Unauthorized) error.</span></span>

<span data-ttu-id="b5a82-173">授權伺服器，並同時呼叫的驗證篩選[OWIN 中介軟體](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)元件，可處理的 OAuth2 詳細資料。</span><span class="sxs-lookup"><span data-stu-id="b5a82-173">The authorization server and the authentication filter both call into an [OWIN middleware](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) component that handles the details of OAuth2.</span></span> <span data-ttu-id="b5a82-174">我將稍後在本教學課程說明的設計中更多詳細資料。</span><span class="sxs-lookup"><span data-stu-id="b5a82-174">I'll describe the design in more detail later in this tutorial.</span></span>

## <a name="sending-an-unauthorized-request"></a><span data-ttu-id="b5a82-175">傳送未經授權的要求</span><span class="sxs-lookup"><span data-stu-id="b5a82-175">Sending an Unauthorized Request</span></span>

<span data-ttu-id="b5a82-176">若要開始，執行應用程式，然後按一下**呼叫 API**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5a82-176">To get started, run the app and click the **Call API** button.</span></span> <span data-ttu-id="b5a82-177">當要求完成時，您應該會看到錯誤訊息中的**結果**方塊。</span><span class="sxs-lookup"><span data-stu-id="b5a82-177">When the request completes, you should see an error message in the **Result** box.</span></span> <span data-ttu-id="b5a82-178">這是因為要求不包含存取權杖，因此是未經授權的要求。</span><span class="sxs-lookup"><span data-stu-id="b5a82-178">That's because the request does not contain an access token, so the request is unauthorized.</span></span>

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

<span data-ttu-id="b5a82-179">**呼叫 API**按鈕時傳送 AJAX 要求至 ~/api/值，這會叫用 Web API 控制器動作。</span><span class="sxs-lookup"><span data-stu-id="b5a82-179">The **Call API** button sends an AJAX request to ~/api/values, which invokes a Web API controller action.</span></span> <span data-ttu-id="b5a82-180">以下是傳送 AJAX 要求的 JavaScript 程式碼區段。</span><span class="sxs-lookup"><span data-stu-id="b5a82-180">Here is the section of JavaScript code that sends the AJAX request.</span></span> <span data-ttu-id="b5a82-181">範例應用程式中的所有 JavaScript 應用程式程式碼位於 Scripts\app.js 檔案中。</span><span class="sxs-lookup"><span data-stu-id="b5a82-181">In the sample app, all of the JavaScript app code is located in the Scripts\app.js file.</span></span>

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

<span data-ttu-id="b5a82-182">使用者登入，直到沒有任何持有者權杖，因此在要求中的沒有授權標頭。</span><span class="sxs-lookup"><span data-stu-id="b5a82-182">Until the user logs in, there is no bearer token, and therefore no Authorization header in the request.</span></span> <span data-ttu-id="b5a82-183">這會導致該要求傳回 401 錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5a82-183">This causes the request to return a 401 error.</span></span>

<span data-ttu-id="b5a82-184">以下是 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="b5a82-184">Here is the HTTP request.</span></span> <span data-ttu-id="b5a82-185">(用[Fiddler](http://www.telerik.com/fiddler)擷取 HTTP 流量。)</span><span class="sxs-lookup"><span data-stu-id="b5a82-185">(I used [Fiddler](http://www.telerik.com/fiddler) to capture the HTTP traffic.)</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

<span data-ttu-id="b5a82-186">HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="b5a82-186">HTTP response:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

<span data-ttu-id="b5a82-187">請注意，回應就會包含 Www-authenticate 標頭與設承載挑戰。</span><span class="sxs-lookup"><span data-stu-id="b5a82-187">Notice that the response includes a Www-Authenticate header with the challenge set to Bearer.</span></span> <span data-ttu-id="b5a82-188">表示伺服器所預期的承載權杖。</span><span class="sxs-lookup"><span data-stu-id="b5a82-188">That indicates the server expects a bearer token.</span></span>

## <a name="register-a-user"></a><span data-ttu-id="b5a82-189">註冊的使用者</span><span class="sxs-lookup"><span data-stu-id="b5a82-189">Register a User</span></span>

<span data-ttu-id="b5a82-190">在**註冊**區段的應用程式中，輸入電子郵件和密碼，並按一下**註冊** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5a82-190">In the **Register** section of the app, enter an email and password, and click the **Register** button.</span></span>

<span data-ttu-id="b5a82-191">您不需要使用有效的電子郵件地址，此範例中，但實際的應用程式會確認的位址。</span><span class="sxs-lookup"><span data-stu-id="b5a82-191">You don't need to use a valid email address for this sample, but a real app would confirm the address.</span></span> <span data-ttu-id="b5a82-192">(請參閱[建立安全的 ASP.NET MVC 5 web 應用程式與記錄檔中，電子郵件確認和密碼重設](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)。)輸入密碼，使用類似"Password1 ！"，與大寫字母、 小寫字母、 數字和非英數字元。</span><span class="sxs-lookup"><span data-stu-id="b5a82-192">(See [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) For the password, use something like "Password1!", with an upper case letter, lower case letter, number, and non-alpha-numeric character.</span></span> <span data-ttu-id="b5a82-193">為了簡化應用程式，我中省略了用戶端驗證，因此如果沒有密碼格式有問題，您會得到 400 （不正確的要求） 錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5a82-193">To keep the app simple, I left out client-side validation, so if there is a problem with the password format, you'll get a 400 (Bad Request) error.</span></span>

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

<span data-ttu-id="b5a82-194">**註冊**按鈕將 POST 要求傳送至 ~/api/Account/Register /。</span><span class="sxs-lookup"><span data-stu-id="b5a82-194">The **Register** button sends a POST request to ~/api/Account/Register/.</span></span> <span data-ttu-id="b5a82-195">要求主體是保留的名稱和密碼的 JSON 物件。</span><span class="sxs-lookup"><span data-stu-id="b5a82-195">The request body is a JSON object that holds the name and password.</span></span> <span data-ttu-id="b5a82-196">以下是將要求傳送的 JavaScript 程式碼：</span><span class="sxs-lookup"><span data-stu-id="b5a82-196">Here is the JavaScript code that sends the request:</span></span>

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

<span data-ttu-id="b5a82-197">HTTP 要求：</span><span class="sxs-lookup"><span data-stu-id="b5a82-197">HTTP request:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

<span data-ttu-id="b5a82-198">HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="b5a82-198">HTTP response:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

<span data-ttu-id="b5a82-199">此要求由`AccountController`類別。</span><span class="sxs-lookup"><span data-stu-id="b5a82-199">This request is handled by the `AccountController` class.</span></span> <span data-ttu-id="b5a82-200">就內部而言，`AccountController`會使用 ASP.NET 識別來管理成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="b5a82-200">Internally, `AccountController` uses ASP.NET Identity to manage the membership database.</span></span>

<span data-ttu-id="b5a82-201">如果您在本機從 Visual Studio 執行應用程式，使用者帳戶都會儲存在 LocalDB 中，以在 AspNetUsers 資料表。</span><span class="sxs-lookup"><span data-stu-id="b5a82-201">If you run the app locally from Visual Studio, user accounts are stored in LocalDB, in the AspNetUsers table.</span></span> <span data-ttu-id="b5a82-202">若要檢視 Visual Studio 中的資料表，請按一下**檢視**功能表上，選取**伺服器總管**，然後展開**資料連接**。</span><span class="sxs-lookup"><span data-stu-id="b5a82-202">To view the tables in Visual Studio, click the **View** menu, select **Server Explorer**, then expand **Data Connections**.</span></span>

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a><span data-ttu-id="b5a82-203">取得存取權杖</span><span class="sxs-lookup"><span data-stu-id="b5a82-203">Get an Access Token</span></span>

<span data-ttu-id="b5a82-204">到目前為止我們不做任何 OAuth，不過現在我們看到 OAuth 授權伺服器，在動作中，要求存取權杖時。</span><span class="sxs-lookup"><span data-stu-id="b5a82-204">So far we have not done any OAuth, but now we'll see the OAuth authorization server in action, when we request an access token.</span></span> <span data-ttu-id="b5a82-205">在**登入**區域的範例應用程式中，輸入電子郵件和密碼，然後按一下**登入**。</span><span class="sxs-lookup"><span data-stu-id="b5a82-205">In the **Log In** area of the sample app, enter the email and password and click **Log In**.</span></span>

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

<span data-ttu-id="b5a82-206">**登入**按鈕將要求傳送至權杖的端點。</span><span class="sxs-lookup"><span data-stu-id="b5a82-206">The **Log In** button sends a request to the token endpoint.</span></span> <span data-ttu-id="b5a82-207">在要求主體包含下列表單 url 編碼資料：</span><span class="sxs-lookup"><span data-stu-id="b5a82-207">The body of the request contains the following form-url-encoded data:</span></span>

- <span data-ttu-id="b5a82-208">授與\_類型: 「 密碼 」</span><span class="sxs-lookup"><span data-stu-id="b5a82-208">grant\_type: "password"</span></span>
- <span data-ttu-id="b5a82-209">使用者名稱：&lt;使用者的電子郵件&gt;</span><span class="sxs-lookup"><span data-stu-id="b5a82-209">username: &lt;the user's email&gt;</span></span>
- <span data-ttu-id="b5a82-210">密碼：&lt;密碼&gt;</span><span class="sxs-lookup"><span data-stu-id="b5a82-210">password: &lt;password&gt;</span></span>

<span data-ttu-id="b5a82-211">以下是傳送 AJAX 要求的 JavaScript 程式碼：</span><span class="sxs-lookup"><span data-stu-id="b5a82-211">Here is the JavaScript code that sends the AJAX request:</span></span>

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

<span data-ttu-id="b5a82-212">如果要求成功，則授權伺服器會傳回存取權杖回應主體中。</span><span class="sxs-lookup"><span data-stu-id="b5a82-212">If the request succeeds, the authorization server returns an access token in the response body.</span></span> <span data-ttu-id="b5a82-213">請注意，我們會將語彙基元儲存於工作階段儲存體，供日後使用的應用程式開發介面傳送要求時。</span><span class="sxs-lookup"><span data-stu-id="b5a82-213">Notice that we store the token in session storage, to use later when sending requests to the API.</span></span> <span data-ttu-id="b5a82-214">不同於某些形式的驗證 （例如 cookie 為基礎的驗證） 中，瀏覽器不會自動包含存取權杖在後續要求中。</span><span class="sxs-lookup"><span data-stu-id="b5a82-214">Unlike some forms of authentication (such as cookie-based authentication), the browser will not automatically include the access token in subsequent requests.</span></span> <span data-ttu-id="b5a82-215">應用程式必須明確加以。</span><span class="sxs-lookup"><span data-stu-id="b5a82-215">The application must do so explicitly.</span></span> <span data-ttu-id="b5a82-216">這是好的結果，因為它會限制[CSRF 弱點](preventing-cross-site-request-forgery-csrf-attacks.md)。</span><span class="sxs-lookup"><span data-stu-id="b5a82-216">That's a good thing, because it limits [CSRF vulnerabilities](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

<span data-ttu-id="b5a82-217">HTTP 要求：</span><span class="sxs-lookup"><span data-stu-id="b5a82-217">HTTP request:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

<span data-ttu-id="b5a82-218">您可以看到此要求包含使用者的認證。</span><span class="sxs-lookup"><span data-stu-id="b5a82-218">You can see that the request contains the user's credentials.</span></span> <span data-ttu-id="b5a82-219">您*必須*使用 HTTPS 來提供傳輸層安全性。</span><span class="sxs-lookup"><span data-stu-id="b5a82-219">You *must* use HTTPS to provide transport layer security.</span></span>

<span data-ttu-id="b5a82-220">HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="b5a82-220">HTTP response:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

<span data-ttu-id="b5a82-221">可讀性，我要縮排的 JSON，並截斷相當長的存取語彙基元。</span><span class="sxs-lookup"><span data-stu-id="b5a82-221">For readability, I indented the JSON and truncated the access token, which is a quite long.</span></span>

<span data-ttu-id="b5a82-222">`access_token`， `token_type`，和`expires_in`OAuth2 規格所定義的屬性。其他屬性 (`userName`， `.issued`，和`.expires`) 都只供使用者參考。</span><span class="sxs-lookup"><span data-stu-id="b5a82-222">The `access_token`, `token_type`, and `expires_in` properties are defined by the OAuth2 spec. The other properties (`userName`, `.issued`, and `.expires`) are just for informational purposes.</span></span> <span data-ttu-id="b5a82-223">您可以找到加入這些額外的屬性中的程式碼`TokenEndpoint`/Providers/ApplicationOAuthProvider.cs 檔案中的方法。</span><span class="sxs-lookup"><span data-stu-id="b5a82-223">You can find the code that adds those additional properties in the `TokenEndpoint` method, in the /Providers/ApplicationOAuthProvider.cs file.</span></span>

## <a name="send-an-authenticated-request"></a><span data-ttu-id="b5a82-224">傳送已驗證的要求</span><span class="sxs-lookup"><span data-stu-id="b5a82-224">Send an Authenticated Request</span></span>

<span data-ttu-id="b5a82-225">現在，我們已經持有人權杖時，我們可以讓已驗證的要求的 api。</span><span class="sxs-lookup"><span data-stu-id="b5a82-225">Now that we have a bearer token, we can make an authenticated request to the API.</span></span> <span data-ttu-id="b5a82-226">這是在要求中設定授權標頭。</span><span class="sxs-lookup"><span data-stu-id="b5a82-226">This is done by setting the Authorization header in the request.</span></span> <span data-ttu-id="b5a82-227">按一下**呼叫 API**再次若要查看此按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5a82-227">Click the **Call API** button again to see this.</span></span>

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

<span data-ttu-id="b5a82-228">HTTP 要求：</span><span class="sxs-lookup"><span data-stu-id="b5a82-228">HTTP request:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

<span data-ttu-id="b5a82-229">HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="b5a82-229">HTTP response:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a><span data-ttu-id="b5a82-230">登出</span><span class="sxs-lookup"><span data-stu-id="b5a82-230">Log Out</span></span>

<span data-ttu-id="b5a82-231">因為認證或存取權杖，則無法快取瀏覽器，請登出就是 「 不會忘記"語彙基元，藉由移除從工作階段儲存體：</span><span class="sxs-lookup"><span data-stu-id="b5a82-231">Because the browser does not cache the credentials or the access token, logging out is simply a matter of "forgetting" the token, by removing it from session storage:</span></span>

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a><span data-ttu-id="b5a82-232">了解個別帳戶專案範本</span><span class="sxs-lookup"><span data-stu-id="b5a82-232">Understanding the Individual Accounts Project Template</span></span>

<span data-ttu-id="b5a82-233">當您選取**個別帳戶**在 ASP.NET Web 應用程式專案範本中，專案會包含：</span><span class="sxs-lookup"><span data-stu-id="b5a82-233">When you select **Individual Accounts** in the ASP.NET Web Application project template, the project includes:</span></span>

- <span data-ttu-id="b5a82-234">OAuth2 授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="b5a82-234">An OAuth2 authorization server.</span></span>
- <span data-ttu-id="b5a82-235">Web API 端點以管理使用者帳戶</span><span class="sxs-lookup"><span data-stu-id="b5a82-235">A Web API endpoint for managing user accounts</span></span>
- <span data-ttu-id="b5a82-236">儲存使用者帳戶的 EF 模型。</span><span class="sxs-lookup"><span data-stu-id="b5a82-236">An EF model for storing user accounts.</span></span>

<span data-ttu-id="b5a82-237">以下是實作這些功能的主要應用程式類別：</span><span class="sxs-lookup"><span data-stu-id="b5a82-237">Here are the main application classes that implement these features:</span></span>

- <span data-ttu-id="b5a82-238">`AccountController`.</span><span class="sxs-lookup"><span data-stu-id="b5a82-238">`AccountController`.</span></span> <span data-ttu-id="b5a82-239">提供 Web API 端點以管理使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="b5a82-239">Provides a Web API endpoint for managing user accounts.</span></span> <span data-ttu-id="b5a82-240">`Register`動作時，我們在本教學課程中使用。</span><span class="sxs-lookup"><span data-stu-id="b5a82-240">The `Register` action is the only one that we used in this tutorial.</span></span> <span data-ttu-id="b5a82-241">在類別上的其他方法支援密碼重設、 社交登入，以及其他功能。</span><span class="sxs-lookup"><span data-stu-id="b5a82-241">Other methods on the class support password reset, social logins, and other functionality.</span></span>
- <span data-ttu-id="b5a82-242">`ApplicationUser`/Models/IdentityModels.cs 中定義。</span><span class="sxs-lookup"><span data-stu-id="b5a82-242">`ApplicationUser`, defined in /Models/IdentityModels.cs.</span></span> <span data-ttu-id="b5a82-243">這個類別是使用者帳戶的成員資格資料庫的 EF 模型。</span><span class="sxs-lookup"><span data-stu-id="b5a82-243">This class is the EF model for user accounts in the membership database.</span></span>
- <span data-ttu-id="b5a82-244">`ApplicationUserManager`定義於 /App\_Start/IdentityConfig.cs 此類別衍生自[UserManager](https://msdn.microsoft.com/en-us/library/dn613290.aspx)和使用者帳戶，例如建立新的使用者，並確認密碼，以及其他等等上執行作業，並會自動保存資料庫的變更。</span><span class="sxs-lookup"><span data-stu-id="b5a82-244">`ApplicationUserManager`, defined in /App\_Start/IdentityConfig.cs This class derives from [UserManager](https://msdn.microsoft.com/en-us/library/dn613290.aspx) and performs operations on user accounts, such as creating a new user, verifying passwords, and so forth, and automatically persists changes to the database.</span></span>
- <span data-ttu-id="b5a82-245">`ApplicationOAuthProvider`.</span><span class="sxs-lookup"><span data-stu-id="b5a82-245">`ApplicationOAuthProvider`.</span></span> <span data-ttu-id="b5a82-246">此物件可插入 OWIN 中介軟體，並由中介軟體引發的事件處理。</span><span class="sxs-lookup"><span data-stu-id="b5a82-246">This object plugs into the OWIN middleware, and processes events raised by the middleware.</span></span> <span data-ttu-id="b5a82-247">它衍生自[OAuthAuthorizationServerProvider](https://msdn.microsoft.com/en-us/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx)。</span><span class="sxs-lookup"><span data-stu-id="b5a82-247">It derives from [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/en-us/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).</span></span>

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a><span data-ttu-id="b5a82-248">設定授權伺服器</span><span class="sxs-lookup"><span data-stu-id="b5a82-248">Configuring the Authorization Server</span></span>

<span data-ttu-id="b5a82-249">StartupAuth.cs，在下列程式碼會設定 OAuth2 授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="b5a82-249">In StartupAuth.cs, the following code configures the OAuth2 authorization server.</span></span>

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

<span data-ttu-id="b5a82-250">`TokenEndpointPath`屬性是授權伺服器端點的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="b5a82-250">The `TokenEndpointPath` property is the URL path to the authorization server endpoint.</span></span> <span data-ttu-id="b5a82-251">這是 URL 的應用程式會使用來取得持有者權杖。</span><span class="sxs-lookup"><span data-stu-id="b5a82-251">That's the URL that app uses to get the bearer tokens.</span></span>

<span data-ttu-id="b5a82-252">`Provider`屬性會指定可插入的 OWIN 中介軟體，以及由中介軟體引發的事件處理的提供者。</span><span class="sxs-lookup"><span data-stu-id="b5a82-252">The `Provider` property specifies a provider that plugs into the OWIN middleware, and processes events raised by the middleware.</span></span>

<span data-ttu-id="b5a82-253">以下是基本流程，當應用程式想要取得權杖：</span><span class="sxs-lookup"><span data-stu-id="b5a82-253">Here is the basic flow when the app wants to get a token:</span></span>

1. <span data-ttu-id="b5a82-254">若要取得存取權杖，應用程式傳送的要求 ~ / t o k。</span><span class="sxs-lookup"><span data-stu-id="b5a82-254">To get an access token, the app sends a request to ~/Token.</span></span>
2. <span data-ttu-id="b5a82-255">OAuth 中介軟體呼叫`GrantResourceOwnerCredentials`視提供者。</span><span class="sxs-lookup"><span data-stu-id="b5a82-255">The OAuth middleware calls `GrantResourceOwnerCredentials` on the provider.</span></span>
3. <span data-ttu-id="b5a82-256">提供者呼叫`ApplicationUserManager`驗證認證，並建立宣告識別。</span><span class="sxs-lookup"><span data-stu-id="b5a82-256">The provider calls the `ApplicationUserManager` to validate the credentials and create a claims identity.</span></span>
4. <span data-ttu-id="b5a82-257">如果成功，此提供者會建立驗證 ticket，用來產生權杖。</span><span class="sxs-lookup"><span data-stu-id="b5a82-257">If that succeeds, the provider creates an authentication ticket, which is used to generate the token.</span></span>

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

<span data-ttu-id="b5a82-258">OAuth 的中介軟體並不知道的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="b5a82-258">The OAuth middleware doesn't know anything about the user accounts.</span></span> <span data-ttu-id="b5a82-259">提供者會在的中介軟體和 ASP.NET Identity 之間進行通訊。</span><span class="sxs-lookup"><span data-stu-id="b5a82-259">The provider communicates between the middleware and ASP.NET Identity.</span></span> <span data-ttu-id="b5a82-260">如需實作授權伺服器的詳細資訊，請參閱[OWIN OAuth 2.0 授權伺服器](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md)。</span><span class="sxs-lookup"><span data-stu-id="b5a82-260">For more information about implementing the authorization server, see [OWIN OAuth 2.0 Authorization Server](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).</span></span>

### <a name="configuring-web-api-to-use-bearer-tokens"></a><span data-ttu-id="b5a82-261">設定 Web 應用程式開發介面使用承載語彙基元</span><span class="sxs-lookup"><span data-stu-id="b5a82-261">Configuring Web API to use Bearer Tokens</span></span>

<span data-ttu-id="b5a82-262">在`WebApiConfig.Register`方法，下列程式碼設定 Web API 管線的驗證：</span><span class="sxs-lookup"><span data-stu-id="b5a82-262">In the `WebApiConfig.Register` method, the following code sets up authentication for the Web API pipeline:</span></span>

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

<span data-ttu-id="b5a82-263">**HostAuthenticationFilter**類別可讓您使用 bearer 權杖驗證。</span><span class="sxs-lookup"><span data-stu-id="b5a82-263">The **HostAuthenticationFilter** class enables authentication using bearer tokens.</span></span>

<span data-ttu-id="b5a82-264">**SuppressDefaultHostAuthentication**方法會告知 Web API，略過任何後，才能在要求到達 Web API 管線，IIS 或 OWIN 中介軟體的驗證。</span><span class="sxs-lookup"><span data-stu-id="b5a82-264">The **SuppressDefaultHostAuthentication** method tells Web API to ignore any authentication that happens before the request reaches the Web API pipeline, either by IIS or by OWIN middleware.</span></span> <span data-ttu-id="b5a82-265">這樣一來，我們可以限制僅使用 bearer 權杖進行驗證的 Web API。</span><span class="sxs-lookup"><span data-stu-id="b5a82-265">That way, we can restrict Web API to authenticate only using bearer tokens.</span></span>

> [!NOTE]
> <span data-ttu-id="b5a82-266">特別是，您的應用程式的 MVC 部分可能會使用表單驗證，將認證儲存在 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="b5a82-266">In particular, the MVC portion of your app might use forms authentication, which stores credentials in a cookie.</span></span> <span data-ttu-id="b5a82-267">Cookie 為基礎的驗證需要防偽語彙基元，以防止 CSRF 攻擊的使用。</span><span class="sxs-lookup"><span data-stu-id="b5a82-267">Cookie-based authentication requires the use of anti-forgery tokens, to prevent CSRF attacks.</span></span> <span data-ttu-id="b5a82-268">因為 web API 的用戶端傳送的防偽語彙基元的便利方法，這是 web 應用程式開發介面，讓發生問題。</span><span class="sxs-lookup"><span data-stu-id="b5a82-268">That's a problem for web APIs, because there is no convenient way for the web API to send the anti-forgery token to the client.</span></span> <span data-ttu-id="b5a82-269">(如需此問題有關的詳細背景，請參閱[Web API 中防止攻擊 CSRF](preventing-cross-site-request-forgery-csrf-attacks.md)。)呼叫**SuppressDefaultHostAuthentication**可確保 Web 應用程式開發介面不會受到影響儲存在 cookie 中的認證從 CSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="b5a82-269">(For more background on this issue, see [Preventing CSRF Attacks in Web API](preventing-cross-site-request-forgery-csrf-attacks.md).) Calling **SuppressDefaultHostAuthentication** ensures that Web API is not vulnerable to CSRF attacks from credentials stored in cookies.</span></span>


<span data-ttu-id="b5a82-270">當用戶端要求受保護的資源時，會發生下列情況中 Web API 管線：</span><span class="sxs-lookup"><span data-stu-id="b5a82-270">When the client requests a protected resource, here is what happens in the Web API pipeline:</span></span>

1. <span data-ttu-id="b5a82-271">**HostAuthentication**篩選條件會呼叫驗證權杖的 OAuth 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b5a82-271">The **HostAuthentication** filter calls the OAuth middleware to validate the token.</span></span>
2. <span data-ttu-id="b5a82-272">中介軟體會將語彙基元轉換成宣告身分識別。</span><span class="sxs-lookup"><span data-stu-id="b5a82-272">The middleware converts the token into a claims identity.</span></span>
3. <span data-ttu-id="b5a82-273">此時，要求是*驗證*但不是*授權*。</span><span class="sxs-lookup"><span data-stu-id="b5a82-273">At this point, the request is *authenticated* but not *authorized*.</span></span>
4. <span data-ttu-id="b5a82-274">授權篩選條件會檢查宣告身分識別。</span><span class="sxs-lookup"><span data-stu-id="b5a82-274">The authorization filter examines the claims identity.</span></span> <span data-ttu-id="b5a82-275">如果宣告授權該資源的使用者，將授權要求。</span><span class="sxs-lookup"><span data-stu-id="b5a82-275">If the claims authorize the user for that resource, the request is authorized.</span></span> <span data-ttu-id="b5a82-276">根據預設， **[Authorize]**屬性將授權任何已驗證的要求。</span><span class="sxs-lookup"><span data-stu-id="b5a82-276">By default, the **[Authorize]** attribute will authorize any request that is authenticated.</span></span> <span data-ttu-id="b5a82-277">不過，您可以授權角色或其他宣告。</span><span class="sxs-lookup"><span data-stu-id="b5a82-277">However, you can authorize by role or by other claims.</span></span> <span data-ttu-id="b5a82-278">如需詳細資訊，請參閱[驗證和授權的 Web API](authentication-and-authorization-in-aspnet-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="b5a82-278">For more information, see [Authentication and Authorization in Web API](authentication-and-authorization-in-aspnet-web-api.md).</span></span>
5. <span data-ttu-id="b5a82-279">如果上一個步驟都成功，控制器會傳回受保護的資源。</span><span class="sxs-lookup"><span data-stu-id="b5a82-279">If the previous steps are successful, the controller returns the protected resource.</span></span> <span data-ttu-id="b5a82-280">否則，用戶端收到 401 （未經授權） 錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5a82-280">Otherwise, the client receives a 401 (Unauthorized) error.</span></span>

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a><span data-ttu-id="b5a82-281">其他資源</span><span class="sxs-lookup"><span data-stu-id="b5a82-281">Additional Resources</span></span>

- [<span data-ttu-id="b5a82-282">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="b5a82-282">ASP.NET Identity</span></span>](../../../identity/index.md)
- <span data-ttu-id="b5a82-283">[了解 VS2013 RC SPA 範本中的安全性功能](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)。</span><span class="sxs-lookup"><span data-stu-id="b5a82-283">[Understanding Security Features in the SPA Template for VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx).</span></span> <span data-ttu-id="b5a82-284">MSDN 部落格文章所 Hongye 太陽。</span><span class="sxs-lookup"><span data-stu-id="b5a82-284">MSDN blog post by Hongye Sun.</span></span>
- <span data-ttu-id="b5a82-285">[將 Web 應用程式開發介面個別分解帳戶範本 – 組件 2： 本機帳戶](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/)。</span><span class="sxs-lookup"><span data-stu-id="b5a82-285">[Dissecting the Web API Individual Accounts Template–Part 2: Local Accounts](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/).</span></span> <span data-ttu-id="b5a82-286">Dominick Baier 部落格文章。</span><span class="sxs-lookup"><span data-stu-id="b5a82-286">Blog post by Dominick Baier.</span></span>
- <span data-ttu-id="b5a82-287">[驗證和 Web API 與 OWIN 裝載](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/)。</span><span class="sxs-lookup"><span data-stu-id="b5a82-287">[Host authentication and Web API with OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/).</span></span> <span data-ttu-id="b5a82-288">良好說明`SuppressDefaultHostAuthentication`和`HostAuthenticationFilter`由 Brock Allen。</span><span class="sxs-lookup"><span data-stu-id="b5a82-288">A good explanation of `SuppressDefaultHostAuthentication` and `HostAuthenticationFilter` by Brock Allen.</span></span>
- <span data-ttu-id="b5a82-289">[自訂在 ASP.NET Identity 中在 VS 2013 範本中的設定檔資訊](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)。</span><span class="sxs-lookup"><span data-stu-id="b5a82-289">[Customizing profile information in ASP.NET Identity in VS 2013 templates](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx).</span></span> <span data-ttu-id="b5a82-290">Pranav Rastogi MSDN 部落格文章。</span><span class="sxs-lookup"><span data-stu-id="b5a82-290">MSDN blog post by Pranav Rastogi.</span></span>
- <span data-ttu-id="b5a82-291">[每個要求存留期管理 UserManager 在 ASP.NET Identity 中的類別](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx)。</span><span class="sxs-lookup"><span data-stu-id="b5a82-291">[Per request lifetime management for UserManager class in ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).</span></span> <span data-ttu-id="b5a82-292">MSDN 部落格文章 Suhas Joshi 良好的說明`UserManager`類別。</span><span class="sxs-lookup"><span data-stu-id="b5a82-292">MSDN blog post by Suhas Joshi, with a good explanation of the `UserManager` class.</span></span>
