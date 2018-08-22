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
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a><span data-ttu-id="f5fb9-104">保護 Web API 使用個別帳戶和 ASP.NET Web API 2.2 中的本機登入</span><span class="sxs-lookup"><span data-stu-id="f5fb9-104">Secure a Web API with Individual Accounts and Local Login in ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="f5fb9-105">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f5fb9-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f5fb9-106">下載範例應用程式</span><span class="sxs-lookup"><span data-stu-id="f5fb9-106">Download Sample App</span></span>](https://github.com/MikeWasson/LocalAccountsApp)

> <span data-ttu-id="f5fb9-107">本主題說明如何保護 web API 使用 OAuth2 驗證的成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-107">This topic shows how to secure a web API using OAuth2 to authenticate against a membership database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f5fb9-108">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="f5fb9-108">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="f5fb9-109">Visual Studio 2013 Update 3</span><span class="sxs-lookup"><span data-stu-id="f5fb9-109">Visual Studio 2013 Update 3</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [<span data-ttu-id="f5fb9-110">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="f5fb9-110">Web API 2.2</span></span>](../releases/whats-new-in-aspnet-web-api-22.md)
> - [<span data-ttu-id="f5fb9-111">ASP.NET Identity 2.1</span><span class="sxs-lookup"><span data-stu-id="f5fb9-111">ASP.NET Identity 2.1</span></span>](../../../identity/index.md)


<span data-ttu-id="f5fb9-112">在 Visual Studio 2013 中，Web API 專案範本可讓您進行驗證的三個選項：</span><span class="sxs-lookup"><span data-stu-id="f5fb9-112">In Visual Studio 2013, the Web API project template gives you three options for authentication:</span></span>

- <span data-ttu-id="f5fb9-113">**個別的帳戶。**</span><span class="sxs-lookup"><span data-stu-id="f5fb9-113">**Individual accounts.**</span></span> <span data-ttu-id="f5fb9-114">應用程式使用的成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-114">The app uses a membership database.</span></span>
- <span data-ttu-id="f5fb9-115">**組織帳戶。**</span><span class="sxs-lookup"><span data-stu-id="f5fb9-115">**Organizational accounts.**</span></span> <span data-ttu-id="f5fb9-116">使用者使用他們的 Azure Active Directory、 Office 365 或內部部署 Active Directory 認證登入。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-116">Users sign in with their Azure Active Directory, Office 365, or on-premise Active Directory credentials.</span></span>
- <span data-ttu-id="f5fb9-117">**Windows 驗證。**</span><span class="sxs-lookup"><span data-stu-id="f5fb9-117">**Windows authentication.**</span></span> <span data-ttu-id="f5fb9-118">此選項適用於內部網路應用程式，並使用 Windows 驗證的 IIS 模組。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-118">This option is intended for Intranet applications, and uses the Windows Authentication IIS module.</span></span>

<span data-ttu-id="f5fb9-119">如需有關這些選項的詳細資訊，請參閱 < [Visual Studio 2013 中建立 ASP.NET Web 專案](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth)。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-119">For more details about these options, see [Creating ASP.NET Web Projects in Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).</span></span>

<span data-ttu-id="f5fb9-120">個別的帳戶會提供兩種方法讓使用者登入：</span><span class="sxs-lookup"><span data-stu-id="f5fb9-120">Individual accounts provide two ways for a user to log in:</span></span>

- <span data-ttu-id="f5fb9-121">**本機登入**。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-121">**Local login**.</span></span> <span data-ttu-id="f5fb9-122">使用者註冊站台上，輸入使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-122">The user registers at the site, entering a username and password.</span></span> <span data-ttu-id="f5fb9-123">應用程式會在成員資格資料庫中儲存密碼雜湊。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-123">The app stores the password hash in the membership database.</span></span> <span data-ttu-id="f5fb9-124">當使用者登入時，ASP.NET 身分識別系統會驗證密碼。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-124">When the user logs in, the ASP.NET Identity system verifies the password.</span></span>
- <span data-ttu-id="f5fb9-125">**社交登入**。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-125">**Social login**.</span></span> <span data-ttu-id="f5fb9-126">使用者登入外部服務，例如 Facebook、 Microsoft 或 Google。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-126">The user signs in with an external service, such as Facebook, Microsoft, or Google.</span></span> <span data-ttu-id="f5fb9-127">此應用程式仍會在成員資格資料庫中，建立使用者的項目，但不會儲存任何認證。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-127">The app still creates an entry for the user in the membership database, but does not store any credentials.</span></span> <span data-ttu-id="f5fb9-128">使用者驗證登入外部服務。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-128">The user authenticates by signing into the external service.</span></span>

<span data-ttu-id="f5fb9-129">這篇文章探討本機登入案例。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-129">This article looks at the local login scenario.</span></span> <span data-ttu-id="f5fb9-130">本機和社交登入，Web API 會使用 OAuth2 驗證的要求。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-130">For both local and social login, Web API uses OAuth2 to authenticate requests.</span></span> <span data-ttu-id="f5fb9-131">不過，認證流程是不同的本機和社交登入。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-131">However, the credential flows are different for local and social login.</span></span>

<span data-ttu-id="f5fb9-132">在本文中，我將示範簡單的應用程式，可讓使用者登入，並傳送至 web API 驗證的 AJAX 呼叫。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-132">In this article, I'll demonstrate a simple app that lets the user log in and send authenticated AJAX calls to a web API.</span></span> <span data-ttu-id="f5fb9-133">您可以下載範例程式碼[此處](https://github.com/MikeWasson/LocalAccountsApp)。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-133">You can download the sample code [here](https://github.com/MikeWasson/LocalAccountsApp).</span></span> <span data-ttu-id="f5fb9-134">讀我檔案說明如何在 Visual Studio 中從頭開始建立範例。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-134">The readme describes how to create the sample from scratch in Visual Studio.</span></span>

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

<span data-ttu-id="f5fb9-135">範例應用程式中使用 Knockout.js 資料繫結和 jQuery 進行傳送 AJAX 要求。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-135">The sample app uses Knockout.js for data-binding and jQuery for sending AJAX requests.</span></span> <span data-ttu-id="f5fb9-136">我將專注於 AJAX 呼叫，因此您不需要知道 Knockout.js 這篇文章。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-136">I'll be focusing on the AJAX calls, so you don't need to know Knockout.js for this article.</span></span>

<span data-ttu-id="f5fb9-137">過程中，我將說明：</span><span class="sxs-lookup"><span data-stu-id="f5fb9-137">Along the way, I'll describe:</span></span>

- <span data-ttu-id="f5fb9-138">在用戶端上進行的哪些應用程式。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-138">What the app is doing on the client side.</span></span>
- <span data-ttu-id="f5fb9-139">什麼在伺服器上發生。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-139">What's happening on the server.</span></span>
- <span data-ttu-id="f5fb9-140">在中間的 HTTP 流量。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-140">The HTTP traffic in the middle.</span></span>

<span data-ttu-id="f5fb9-141">首先，我們需要定義 OAuth2 的一些術語。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-141">First, we need to define some OAuth2 terminology.</span></span>

- <span data-ttu-id="f5fb9-142">*資源*。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-142">*Resource*.</span></span> <span data-ttu-id="f5fb9-143">某些可以保護的資料片段。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-143">Some piece of data that can be protected.</span></span>
- <span data-ttu-id="f5fb9-144">*資源伺服器*。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-144">*Resource server*.</span></span> <span data-ttu-id="f5fb9-145">裝載資源的伺服器。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-145">The server that hosts the resource.</span></span>
- <span data-ttu-id="f5fb9-146">*資源擁有者*。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-146">*Resource owner*.</span></span> <span data-ttu-id="f5fb9-147">可以授與權限來存取資源的實體。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-147">The entity that can grant permission to access a resource.</span></span> <span data-ttu-id="f5fb9-148">（通常是使用者。）</span><span class="sxs-lookup"><span data-stu-id="f5fb9-148">(Typically the user.)</span></span>
- <span data-ttu-id="f5fb9-149">*用戶端*： 想要存取之資源的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-149">*Client*: The app that wants access to the resource.</span></span> <span data-ttu-id="f5fb9-150">在本文中，用戶端會將網頁瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-150">In this article, the client is a web browser.</span></span>
- <span data-ttu-id="f5fb9-151">*存取權杖*。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-151">*Access token*.</span></span> <span data-ttu-id="f5fb9-152">授與對資源的存取語彙基元。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-152">A token that grants access to a resource.</span></span>
- <span data-ttu-id="f5fb9-153">*持有人權杖*。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-153">*Bearer token*.</span></span> <span data-ttu-id="f5fb9-154">特定類型的存取權杖，任何人都可以使用語彙基元的屬性。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-154">A particular type of access token, with the property that anyone can use the token.</span></span> <span data-ttu-id="f5fb9-155">換句話說，用戶端不需要的密碼編譯金鑰或使用持有人權杖的其他祕密。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-155">In other words, a client doesn't need a cryptographic key or other secret to use a bearer token.</span></span> <span data-ttu-id="f5fb9-156">基於這個理由，應該只用於透過 HTTPS，持有人權杖，並應該會有相對較短的到期時間。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-156">For that reason, bearer tokens should only be used over a HTTPS, and should have relatively short expiration times.</span></span>
- <span data-ttu-id="f5fb9-157">*授權伺服器*。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-157">*Authorization server*.</span></span> <span data-ttu-id="f5fb9-158">一種伺服器可存取權杖。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-158">A server that gives out access tokens.</span></span>

<span data-ttu-id="f5fb9-159">應用程式可以做為授權伺服器與資源伺服器。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-159">An application can act as both authorization server and resource server.</span></span> <span data-ttu-id="f5fb9-160">Web API 專案範本會遵循此模式。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-160">The Web API project template follows this pattern.</span></span>

## <a name="local-login-credential-flow"></a><span data-ttu-id="f5fb9-161">本機登入認證流程</span><span class="sxs-lookup"><span data-stu-id="f5fb9-161">Local Login Credential Flow</span></span>

<span data-ttu-id="f5fb9-162">Web API 所使用的本機登入[資源擁有者密碼流程](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html)OAuth2 中定義。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-162">For local login, Web API uses the [resource owner password flow](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) defined in OAuth2.</span></span>

1. <span data-ttu-id="f5fb9-163">使用者會在用戶端輸入的名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-163">The user enters a name and password into the client.</span></span>
2. <span data-ttu-id="f5fb9-164">用戶端會傳送到授權伺服器的這些認證。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-164">The client sends these credentials to the authorization server.</span></span>
3. <span data-ttu-id="f5fb9-165">授權伺服器驗證的認證，並傳回存取權杖。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-165">The authorization server authenticates the credentials and returns an access token.</span></span>
4. <span data-ttu-id="f5fb9-166">若要存取受保護的資源，用戶端會包含在 HTTP 要求的 Authorization 標頭中的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-166">To access a protected resource, the client includes the access token in the Authorization header of the HTTP request.</span></span>

![](individual-accounts-in-web-api/_static/image3.png)

<span data-ttu-id="f5fb9-167">當您選取**個別帳戶**在 Web API 專案範本中，專案會包含授權伺服器會驗證使用者的認證，並簽發權杖。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-167">When you select **Individual accounts** in the Web API project template, the project includes an authorization server that validates user credentials and issues tokens.</span></span> <span data-ttu-id="f5fb9-168">下圖顯示相同的認證流程，以 Web API 的元件。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-168">The following diagram shows the same credential flow in terms of Web API components.</span></span>

![](individual-accounts-in-web-api/_static/image4.png)

<span data-ttu-id="f5fb9-169">在此案例中，Web API 控制器會作為資源伺服器。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-169">In this scenario, Web API controllers act as resource servers.</span></span> <span data-ttu-id="f5fb9-170">驗證篩選條件會驗證存取權杖，而 **[Authorize]** 屬性用來保護資源。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-170">An authentication filter validates access tokens, and the **[Authorize]** attribute is used to protect a resource.</span></span> <span data-ttu-id="f5fb9-171">當控制器或動作具有 **[Authorize]** 屬性，該控制站的所有要求，或必須驗證動作。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-171">When a controller or action has the **[Authorize]** attribute, all requests to that controller or action must be authenticated.</span></span> <span data-ttu-id="f5fb9-172">否則授權會遭到拒絕，而且 Web API 會傳回 401 （未經授權） 時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-172">Otherwise, authorization is denied, and Web API returns a 401 (Unauthorized) error.</span></span>

<span data-ttu-id="f5fb9-173">授權伺服器，並同時呼叫的驗證篩選[OWIN 中介軟體](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)元件，可處理 OAuth2 的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-173">The authorization server and the authentication filter both call into an [OWIN middleware](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) component that handles the details of OAuth2.</span></span> <span data-ttu-id="f5fb9-174">本教學課程稍後，我將說明更多詳細資料中的設計。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-174">I'll describe the design in more detail later in this tutorial.</span></span>

## <a name="sending-an-unauthorized-request"></a><span data-ttu-id="f5fb9-175">傳送未經授權的要求</span><span class="sxs-lookup"><span data-stu-id="f5fb9-175">Sending an Unauthorized Request</span></span>

<span data-ttu-id="f5fb9-176">若要開始，執行應用程式，然後按一下**呼叫 API**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-176">To get started, run the app and click the **Call API** button.</span></span> <span data-ttu-id="f5fb9-177">要求完成時，您應該會看到錯誤訊息中的**結果** 方塊中。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-177">When the request completes, you should see an error message in the **Result** box.</span></span> <span data-ttu-id="f5fb9-178">這是因為要求不包含存取權杖，因此要求未獲授權。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-178">That's because the request does not contain an access token, so the request is unauthorized.</span></span>

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

<span data-ttu-id="f5fb9-179">**呼叫 API**按鈕 AJAX 將要求傳送至 ~/api/值，會叫用 Web API 控制器動作。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-179">The **Call API** button sends an AJAX request to ~/api/values, which invokes a Web API controller action.</span></span> <span data-ttu-id="f5fb9-180">以下是傳送 AJAX 要求的 JavaScript 程式碼區段。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-180">Here is the section of JavaScript code that sends the AJAX request.</span></span> <span data-ttu-id="f5fb9-181">範例應用程式中的所有 JavaScript 應用程式程式碼位於 Scripts\app.js 檔案中。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-181">In the sample app, all of the JavaScript app code is located in the Scripts\app.js file.</span></span>

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

<span data-ttu-id="f5fb9-182">在使用者登入，直到沒有任何持有人權杖，並因此在要求中的沒有授權標頭。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-182">Until the user logs in, there is no bearer token, and therefore no Authorization header in the request.</span></span> <span data-ttu-id="f5fb9-183">這會導致傳回 401 錯誤的要求。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-183">This causes the request to return a 401 error.</span></span>

<span data-ttu-id="f5fb9-184">以下是 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-184">Here is the HTTP request.</span></span> <span data-ttu-id="f5fb9-185">(我使用了[Fiddler](http://www.telerik.com/fiddler)擷取 HTTP 流量。)</span><span class="sxs-lookup"><span data-stu-id="f5fb9-185">(I used [Fiddler](http://www.telerik.com/fiddler) to capture the HTTP traffic.)</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

<span data-ttu-id="f5fb9-186">HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="f5fb9-186">HTTP response:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

<span data-ttu-id="f5fb9-187">請注意，回應就會包含 Www-authenticate 標頭設定為持有人的挑戰。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-187">Notice that the response includes a Www-Authenticate header with the challenge set to Bearer.</span></span> <span data-ttu-id="f5fb9-188">指出伺服器所預期的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-188">That indicates the server expects a bearer token.</span></span>

## <a name="register-a-user"></a><span data-ttu-id="f5fb9-189">註冊的使用者</span><span class="sxs-lookup"><span data-stu-id="f5fb9-189">Register a User</span></span>

<span data-ttu-id="f5fb9-190">在 [**註冊**一節的應用程式，請輸入電子郵件和密碼，然後按一下**註冊**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-190">In the **Register** section of the app, enter an email and password, and click the **Register** button.</span></span>

<span data-ttu-id="f5fb9-191">您不需要針對此範例中，使用有效的電子郵件地址，但實際的應用程式會確認該位址。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-191">You don't need to use a valid email address for this sample, but a real app would confirm the address.</span></span> <span data-ttu-id="f5fb9-192">(請參閱[登入、 電子郵件確認和密碼重設建立安全的 ASP.NET MVC 5 web 應用程式](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)。)輸入密碼，使用類似"Password1 ！ 」，與大寫字母、 小寫字母、 數字和非英數字元。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-192">(See [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) For the password, use something like "Password1!", with an upper case letter, lower case letter, number, and non-alpha-numeric character.</span></span> <span data-ttu-id="f5fb9-193">為了簡化應用程式，我省略了用戶端驗證，因此如果沒有密碼格式問題，您會得到 400 （不正確的要求） 錯誤。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-193">To keep the app simple, I left out client-side validation, so if there is a problem with the password format, you'll get a 400 (Bad Request) error.</span></span>

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

<span data-ttu-id="f5fb9-194">**註冊**按鈕會將 POST 要求傳送至 ~/api/Account/Register /。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-194">The **Register** button sends a POST request to ~/api/Account/Register/.</span></span> <span data-ttu-id="f5fb9-195">要求主體是 JSON 物件保留的名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-195">The request body is a JSON object that holds the name and password.</span></span> <span data-ttu-id="f5fb9-196">以下是傳送要求的 JavaScript 程式碼：</span><span class="sxs-lookup"><span data-stu-id="f5fb9-196">Here is the JavaScript code that sends the request:</span></span>

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

<span data-ttu-id="f5fb9-197">HTTP 要求：</span><span class="sxs-lookup"><span data-stu-id="f5fb9-197">HTTP request:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

<span data-ttu-id="f5fb9-198">HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="f5fb9-198">HTTP response:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

<span data-ttu-id="f5fb9-199">此要求由`AccountController`類別。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-199">This request is handled by the `AccountController` class.</span></span> <span data-ttu-id="f5fb9-200">就內部而言，`AccountController`使用 ASP.NET 身分識別管理成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-200">Internally, `AccountController` uses ASP.NET Identity to manage the membership database.</span></span>

<span data-ttu-id="f5fb9-201">從 Visual Studio 中本機執行應用程式時，如果使用者帳戶會在 LocalDB 中，儲存在 AspNetUsers 資料表中。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-201">If you run the app locally from Visual Studio, user accounts are stored in LocalDB, in the AspNetUsers table.</span></span> <span data-ttu-id="f5fb9-202">若要檢視的資料表，在 Visual Studio 中，按一下**檢視**功能表上，選取**伺服器總管**，然後展開**資料連接**。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-202">To view the tables in Visual Studio, click the **View** menu, select **Server Explorer**, then expand **Data Connections**.</span></span>

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a><span data-ttu-id="f5fb9-203">取得存取權杖</span><span class="sxs-lookup"><span data-stu-id="f5fb9-203">Get an Access Token</span></span>

<span data-ttu-id="f5fb9-204">到目前為止我們不做任何 OAuth 中，但是現在我們先看 OAuth 授權伺服器，在動作中，當我們要求存取權杖。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-204">So far we have not done any OAuth, but now we'll see the OAuth authorization server in action, when we request an access token.</span></span> <span data-ttu-id="f5fb9-205">在 **登入**區域中的範例應用程式中，輸入電子郵件和密碼，然後按一下**登入**。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-205">In the **Log In** area of the sample app, enter the email and password and click **Log In**.</span></span>

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

<span data-ttu-id="f5fb9-206">**登入**按鈕會將要求傳送至權杖端點。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-206">The **Log In** button sends a request to the token endpoint.</span></span> <span data-ttu-id="f5fb9-207">在要求主體包含下列的表單 url 編碼資料：</span><span class="sxs-lookup"><span data-stu-id="f5fb9-207">The body of the request contains the following form-url-encoded data:</span></span>

- <span data-ttu-id="f5fb9-208">授與\_類型: 「 密碼 」</span><span class="sxs-lookup"><span data-stu-id="f5fb9-208">grant\_type: "password"</span></span>
- <span data-ttu-id="f5fb9-209">使用者名稱：&lt;使用者的電子郵件&gt;</span><span class="sxs-lookup"><span data-stu-id="f5fb9-209">username: &lt;the user's email&gt;</span></span>
- <span data-ttu-id="f5fb9-210">密碼：&lt;密碼&gt;</span><span class="sxs-lookup"><span data-stu-id="f5fb9-210">password: &lt;password&gt;</span></span>

<span data-ttu-id="f5fb9-211">以下是傳送 AJAX 要求的 JavaScript 程式碼：</span><span class="sxs-lookup"><span data-stu-id="f5fb9-211">Here is the JavaScript code that sends the AJAX request:</span></span>

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

<span data-ttu-id="f5fb9-212">如果要求成功，則授權伺服器會傳回存取權杖回應主體中。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-212">If the request succeeds, the authorization server returns an access token in the response body.</span></span> <span data-ttu-id="f5fb9-213">請注意，我們將權杖儲存在工作階段存放區，以供日後使用，將要求傳送至 API 時。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-213">Notice that we store the token in session storage, to use later when sending requests to the API.</span></span> <span data-ttu-id="f5fb9-214">不同於某些形式的驗證 （例如 cookie 型驗證） 中，瀏覽器不會自動包含存取權杖中的後續要求。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-214">Unlike some forms of authentication (such as cookie-based authentication), the browser will not automatically include the access token in subsequent requests.</span></span> <span data-ttu-id="f5fb9-215">應用程式必須明確加以。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-215">The application must do so explicitly.</span></span> <span data-ttu-id="f5fb9-216">這是一件好事，因為它會限制[CSRF 弱點](preventing-cross-site-request-forgery-csrf-attacks.md)。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-216">That's a good thing, because it limits [CSRF vulnerabilities](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

<span data-ttu-id="f5fb9-217">HTTP 要求：</span><span class="sxs-lookup"><span data-stu-id="f5fb9-217">HTTP request:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

<span data-ttu-id="f5fb9-218">您可以看到此要求包含使用者的認證。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-218">You can see that the request contains the user's credentials.</span></span> <span data-ttu-id="f5fb9-219">您*必須*使用 HTTPS 來提供傳輸層安全性。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-219">You *must* use HTTPS to provide transport layer security.</span></span>

<span data-ttu-id="f5fb9-220">HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="f5fb9-220">HTTP response:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

<span data-ttu-id="f5fb9-221">為了便於閱讀，我會縮排的 JSON，並截斷的存取權杖，這是相當長。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-221">For readability, I indented the JSON and truncated the access token, which is a quite long.</span></span>

<span data-ttu-id="f5fb9-222">`access_token`， `token_type`，和`expires_in`OAuth2 規格來定義屬性。其他屬性 (`userName`， `.issued`，和`.expires`) 是僅供資訊之用。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-222">The `access_token`, `token_type`, and `expires_in` properties are defined by the OAuth2 spec. The other properties (`userName`, `.issued`, and `.expires`) are just for informational purposes.</span></span> <span data-ttu-id="f5fb9-223">您可以找到加入這些其他的屬性中的程式碼`TokenEndpoint`/Providers/ApplicationOAuthProvider.cs 檔案中的方法。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-223">You can find the code that adds those additional properties in the `TokenEndpoint` method, in the /Providers/ApplicationOAuthProvider.cs file.</span></span>

## <a name="send-an-authenticated-request"></a><span data-ttu-id="f5fb9-224">傳送已驗證的要求</span><span class="sxs-lookup"><span data-stu-id="f5fb9-224">Send an Authenticated Request</span></span>

<span data-ttu-id="f5fb9-225">既然我們已經持有人權杖，就可以在對 API 提出已驗證的要求。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-225">Now that we have a bearer token, we can make an authenticated request to the API.</span></span> <span data-ttu-id="f5fb9-226">這是由在要求中設定授權標頭。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-226">This is done by setting the Authorization header in the request.</span></span> <span data-ttu-id="f5fb9-227">按一下 **呼叫 API**一次，才能看到此按鈕。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-227">Click the **Call API** button again to see this.</span></span>

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

<span data-ttu-id="f5fb9-228">HTTP 要求：</span><span class="sxs-lookup"><span data-stu-id="f5fb9-228">HTTP request:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

<span data-ttu-id="f5fb9-229">HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="f5fb9-229">HTTP response:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a><span data-ttu-id="f5fb9-230">登出</span><span class="sxs-lookup"><span data-stu-id="f5fb9-230">Log Out</span></span>

<span data-ttu-id="f5fb9-231">因為認證或存取權杖，並不會快取瀏覽器，進行登出時只是 「 不會忘記 「 語彙基元，方法是移除工作階段存放區：</span><span class="sxs-lookup"><span data-stu-id="f5fb9-231">Because the browser does not cache the credentials or the access token, logging out is simply a matter of "forgetting" the token, by removing it from session storage:</span></span>

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a><span data-ttu-id="f5fb9-232">了解個別帳戶專案範本</span><span class="sxs-lookup"><span data-stu-id="f5fb9-232">Understanding the Individual Accounts Project Template</span></span>

<span data-ttu-id="f5fb9-233">當您選取**個別帳戶**在 ASP.NET Web 應用程式專案範本中，專案會包含：</span><span class="sxs-lookup"><span data-stu-id="f5fb9-233">When you select **Individual Accounts** in the ASP.NET Web Application project template, the project includes:</span></span>

- <span data-ttu-id="f5fb9-234">OAuth2 授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-234">An OAuth2 authorization server.</span></span>
- <span data-ttu-id="f5fb9-235">Web API 端點以管理使用者帳戶</span><span class="sxs-lookup"><span data-stu-id="f5fb9-235">A Web API endpoint for managing user accounts</span></span>
- <span data-ttu-id="f5fb9-236">EF 模型來儲存使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-236">An EF model for storing user accounts.</span></span>

<span data-ttu-id="f5fb9-237">以下是實作這些功能的主要應用程式類別：</span><span class="sxs-lookup"><span data-stu-id="f5fb9-237">Here are the main application classes that implement these features:</span></span>

- <span data-ttu-id="f5fb9-238">`AccountController`.</span><span class="sxs-lookup"><span data-stu-id="f5fb9-238">`AccountController`.</span></span> <span data-ttu-id="f5fb9-239">提供 Web API 端點以管理使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-239">Provides a Web API endpoint for managing user accounts.</span></span> <span data-ttu-id="f5fb9-240">`Register`動作是唯一的我們在本教學課程中使用。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-240">The `Register` action is the only one that we used in this tutorial.</span></span> <span data-ttu-id="f5fb9-241">在類別上的其他方法可支援密碼重設、 社交登入及其他功能。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-241">Other methods on the class support password reset, social logins, and other functionality.</span></span>
- <span data-ttu-id="f5fb9-242">`ApplicationUser`定義於 /Models/IdentityModels.cs。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-242">`ApplicationUser`, defined in /Models/IdentityModels.cs.</span></span> <span data-ttu-id="f5fb9-243">這個類別是成員資格資料庫中的使用者帳戶的 EF 模型。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-243">This class is the EF model for user accounts in the membership database.</span></span>
- <span data-ttu-id="f5fb9-244">`ApplicationUserManager`定義於 /App\_Start/IdentityConfig.cs 這個類別衍生自[UserManager](https://msdn.microsoft.com/library/dn613290.aspx)和執行在使用者帳戶，例如建立新的使用者，並確認密碼，以及其他等等的作業，並會自動保存資料庫的變更。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-244">`ApplicationUserManager`, defined in /App\_Start/IdentityConfig.cs This class derives from [UserManager](https://msdn.microsoft.com/library/dn613290.aspx) and performs operations on user accounts, such as creating a new user, verifying passwords, and so forth, and automatically persists changes to the database.</span></span>
- <span data-ttu-id="f5fb9-245">`ApplicationOAuthProvider`.</span><span class="sxs-lookup"><span data-stu-id="f5fb9-245">`ApplicationOAuthProvider`.</span></span> <span data-ttu-id="f5fb9-246">此物件插入的 OWIN 中介軟體，並處理中介軟體所引發的事件。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-246">This object plugs into the OWIN middleware, and processes events raised by the middleware.</span></span> <span data-ttu-id="f5fb9-247">它衍生自[OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx)。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-247">It derives from [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).</span></span>

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a><span data-ttu-id="f5fb9-248">設定授權伺服器</span><span class="sxs-lookup"><span data-stu-id="f5fb9-248">Configuring the Authorization Server</span></span>

<span data-ttu-id="f5fb9-249">在 StartupAuth.cs，下列程式碼會設定 OAuth2 授權伺服器。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-249">In StartupAuth.cs, the following code configures the OAuth2 authorization server.</span></span>

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

<span data-ttu-id="f5fb9-250">`TokenEndpointPath`屬性是授權伺服器端點的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-250">The `TokenEndpointPath` property is the URL path to the authorization server endpoint.</span></span> <span data-ttu-id="f5fb9-251">這是 URL 的應用程式用來取得持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-251">That's the URL that app uses to get the bearer tokens.</span></span>

<span data-ttu-id="f5fb9-252">`Provider`屬性指定的提供者，可插入的 OWIN 中介軟體，並處理中介軟體所引發的事件。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-252">The `Provider` property specifies a provider that plugs into the OWIN middleware, and processes events raised by the middleware.</span></span>

<span data-ttu-id="f5fb9-253">當應用程式想要取得權杖時，以下是基本流程：</span><span class="sxs-lookup"><span data-stu-id="f5fb9-253">Here is the basic flow when the app wants to get a token:</span></span>

1. <span data-ttu-id="f5fb9-254">若要取得存取權杖，應用程式傳送要求至 ~ / 語彙基元。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-254">To get an access token, the app sends a request to ~/Token.</span></span>
2. <span data-ttu-id="f5fb9-255">OAuth 的中介軟體呼叫`GrantResourceOwnerCredentials`的提供者。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-255">The OAuth middleware calls `GrantResourceOwnerCredentials` on the provider.</span></span>
3. <span data-ttu-id="f5fb9-256">在提供者呼叫`ApplicationUserManager`驗證認證，並建立宣告識別。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-256">The provider calls the `ApplicationUserManager` to validate the credentials and create a claims identity.</span></span>
4. <span data-ttu-id="f5fb9-257">如果成功，此提供者會建立驗證票證，用來產生權杖。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-257">If that succeeds, the provider creates an authentication ticket, which is used to generate the token.</span></span>

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

<span data-ttu-id="f5fb9-258">OAuth 的中介軟體不了解使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-258">The OAuth middleware doesn't know anything about the user accounts.</span></span> <span data-ttu-id="f5fb9-259">提供者會在的中介軟體和 ASP.NET 身分識別之間進行通訊。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-259">The provider communicates between the middleware and ASP.NET Identity.</span></span> <span data-ttu-id="f5fb9-260">如需有關如何實作授權伺服器的詳細資訊，請參閱 < [OWIN OAuth 2.0 授權伺服器](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md)。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-260">For more information about implementing the authorization server, see [OWIN OAuth 2.0 Authorization Server](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).</span></span>

### <a name="configuring-web-api-to-use-bearer-tokens"></a><span data-ttu-id="f5fb9-261">設定要使用持有人權杖的 Web API</span><span class="sxs-lookup"><span data-stu-id="f5fb9-261">Configuring Web API to use Bearer Tokens</span></span>

<span data-ttu-id="f5fb9-262">在 `WebApiConfig.Register`方法，下列程式碼會設定 Web API 管線的驗證：</span><span class="sxs-lookup"><span data-stu-id="f5fb9-262">In the `WebApiConfig.Register` method, the following code sets up authentication for the Web API pipeline:</span></span>

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

<span data-ttu-id="f5fb9-263">**HostAuthenticationFilter**類別可讓您使用持有人權杖進行驗證。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-263">The **HostAuthenticationFilter** class enables authentication using bearer tokens.</span></span>

<span data-ttu-id="f5fb9-264">**SuppressDefaultHostAuthentication**方法會告訴要忽略會在要求到達 Web API 管線在 IIS 或 OWIN 中介軟體之前需要驗證的 Web API。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-264">The **SuppressDefaultHostAuthentication** method tells Web API to ignore any authentication that happens before the request reaches the Web API pipeline, either by IIS or by OWIN middleware.</span></span> <span data-ttu-id="f5fb9-265">如此一來，我們可以限制只使用持有人權杖進行驗證的 Web API。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-265">That way, we can restrict Web API to authenticate only using bearer tokens.</span></span>

> [!NOTE]
> <span data-ttu-id="f5fb9-266">特別是，您的應用程式的 MVC 部分可能會使用表單驗證，將認證儲存在 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-266">In particular, the MVC portion of your app might use forms authentication, which stores credentials in a cookie.</span></span> <span data-ttu-id="f5fb9-267">以 cookie 為基礎的驗證需要使用防偽權杖，以防止 CSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-267">Cookie-based authentication requires the use of anti-forgery tokens, to prevent CSRF attacks.</span></span> <span data-ttu-id="f5fb9-268">這是 web Api 的問題，因為 web API 傳送給用戶端的防偽語彙基元的便利方法。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-268">That's a problem for web APIs, because there is no convenient way for the web API to send the anti-forgery token to the client.</span></span> <span data-ttu-id="f5fb9-269">(如需此問題有關的詳細背景，請參閱[Web API 中防止 CSRF 攻擊](preventing-cross-site-request-forgery-csrf-attacks.md)。)呼叫**SuppressDefaultHostAuthentication**可確保 Web API 不容易遭受 CSRF 攻擊從儲存在 cookie 中的認證。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-269">(For more background on this issue, see [Preventing CSRF Attacks in Web API](preventing-cross-site-request-forgery-csrf-attacks.md).) Calling **SuppressDefaultHostAuthentication** ensures that Web API is not vulnerable to CSRF attacks from credentials stored in cookies.</span></span>


<span data-ttu-id="f5fb9-270">當用戶端要求受保護的資源時，以下是 Web API 管線中發生的動作：</span><span class="sxs-lookup"><span data-stu-id="f5fb9-270">When the client requests a protected resource, here is what happens in the Web API pipeline:</span></span>

1. <span data-ttu-id="f5fb9-271">**HostAuthentication**篩選器會呼叫來驗證權杖的 OAuth 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-271">The **HostAuthentication** filter calls the OAuth middleware to validate the token.</span></span>
2. <span data-ttu-id="f5fb9-272">中介軟體會將語彙基元轉換成的宣告識別。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-272">The middleware converts the token into a claims identity.</span></span>
3. <span data-ttu-id="f5fb9-273">此時，已要求*驗證*但不是*授權*。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-273">At this point, the request is *authenticated* but not *authorized*.</span></span>
4. <span data-ttu-id="f5fb9-274">授權篩選條件會檢查宣告識別。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-274">The authorization filter examines the claims identity.</span></span> <span data-ttu-id="f5fb9-275">如果宣告授權該資源的使用者，請將授權要求。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-275">If the claims authorize the user for that resource, the request is authorized.</span></span> <span data-ttu-id="f5fb9-276">根據預設， **[Authorize]** 屬性將會授權任何已驗證的要求。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-276">By default, the **[Authorize]** attribute will authorize any request that is authenticated.</span></span> <span data-ttu-id="f5fb9-277">不過，您可以授權角色或其他宣告。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-277">However, you can authorize by role or by other claims.</span></span> <span data-ttu-id="f5fb9-278">如需詳細資訊，請參閱 <<c0> [ 驗證和授權的 Web API](authentication-and-authorization-in-aspnet-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-278">For more information, see [Authentication and Authorization in Web API](authentication-and-authorization-in-aspnet-web-api.md).</span></span>
5. <span data-ttu-id="f5fb9-279">如果先前的步驟都成功，控制器會傳回受保護的資源。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-279">If the previous steps are successful, the controller returns the protected resource.</span></span> <span data-ttu-id="f5fb9-280">否則，用戶端會收到 401 （未經授權） 時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-280">Otherwise, the client receives a 401 (Unauthorized) error.</span></span>

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a><span data-ttu-id="f5fb9-281">其他資源</span><span class="sxs-lookup"><span data-stu-id="f5fb9-281">Additional Resources</span></span>

- [<span data-ttu-id="f5fb9-282">ASP.NET 身分識別</span><span class="sxs-lookup"><span data-stu-id="f5fb9-282">ASP.NET Identity</span></span>](../../../identity/index.md)
- <span data-ttu-id="f5fb9-283">[了解 VS2013 RC SPA 範本中的安全性功能](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-283">[Understanding Security Features in the SPA Template for VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx).</span></span> <span data-ttu-id="f5fb9-284">MSDN 部落格文章所 Hongye Sun。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-284">MSDN blog post by Hongye Sun.</span></span>
- <span data-ttu-id="f5fb9-285">[剖析 Web API 的個別帳戶範本 – 第 2 部分： 本機帳戶](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/)。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-285">[Dissecting the Web API Individual Accounts Template–Part 2: Local Accounts](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/).</span></span> <span data-ttu-id="f5fb9-286">Dominick Baier 部落格文章。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-286">Blog post by Dominick Baier.</span></span>
- <span data-ttu-id="f5fb9-287">[驗證和 Web API 與 OWIN 主機](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/)。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-287">[Host authentication and Web API with OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/).</span></span> <span data-ttu-id="f5fb9-288">合理的解釋`SuppressDefaultHostAuthentication`和`HostAuthenticationFilter`由 Brock Allen。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-288">A good explanation of `SuppressDefaultHostAuthentication` and `HostAuthenticationFilter` by Brock Allen.</span></span>
- <span data-ttu-id="f5fb9-289">[自訂 ASP.NET 身分識別中的設定檔資訊，在 VS 2013 範本](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-289">[Customizing profile information in ASP.NET Identity in VS 2013 templates](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx).</span></span> <span data-ttu-id="f5fb9-290">請參閱 Pranav rastogi 的 MSDN 部落格文章。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-290">MSDN blog post by Pranav Rastogi.</span></span>
- <span data-ttu-id="f5fb9-291">[每個要求存留期管理，在 ASP.NET 身分識別的 UserManager 類別](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx)。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-291">[Per request lifetime management for UserManager class in ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).</span></span> <span data-ttu-id="f5fb9-292">MSDN 部落格文章，Suhas Joshi 所使用的詳細說明`UserManager`類別。</span><span class="sxs-lookup"><span data-stu-id="f5fb9-292">MSDN blog post by Suhas Joshi, with a good explanation of the `UserManager` class.</span></span>
