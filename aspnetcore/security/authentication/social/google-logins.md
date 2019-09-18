---
title: ASP.NET Core 中的 Google 外部登入設定
author: rick-anderson
description: 本教學課程示範如何整合到現有的 ASP.NET Core 應用程式的 Google 帳戶使用者驗證。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/19/2019
uid: security/authentication/google-logins
ms.openlocfilehash: e12d831d2e0a5c9acae5ea41fb4187ad4ca6b0ea
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082478"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="ed121-103">ASP.NET Core 中的 Google 外部登入設定</span><span class="sxs-lookup"><span data-stu-id="ed121-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="ed121-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ed121-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ed121-105">[從2019年3月7日起，舊版 Google + api 已關閉](https://developers.google.com/+/api-shutdown)。</span><span class="sxs-lookup"><span data-stu-id="ed121-105">[Legacy Google+ APIs have been shut down as of March 7, 2019](https://developers.google.com/+/api-shutdown).</span></span> <span data-ttu-id="ed121-106">Google + 登入和開發人員必須移至新的 Google 登入系統。</span><span class="sxs-lookup"><span data-stu-id="ed121-106">Google+ sign in and developers must move to a new Google sign in system.</span></span> <span data-ttu-id="ed121-107">適用于 Google 驗證的 ASP.NET Core 2.1 和2.2 封裝已更新，以配合變更。</span><span class="sxs-lookup"><span data-stu-id="ed121-107">The ASP.NET Core 2.1 and 2.2 packages for Google Authentication have be updated to accommodate the changes.</span></span> <span data-ttu-id="ed121-108">如需 ASP.NET Core 的詳細資訊和暫時緩和措施，請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore/issues/6486)。</span><span class="sxs-lookup"><span data-stu-id="ed121-108">For more information and temporary mitigations for ASP.NET Core, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/6486).</span></span> <span data-ttu-id="ed121-109">本教學課程已更新為新的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="ed121-109">This tutorial has been updated with the new setup process.</span></span>

<span data-ttu-id="ed121-110">本教學課程說明如何讓使用者使用在[前一頁](xref:security/authentication/social/index)建立的 ASP.NET Core 2.2 專案，以其 Google 帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="ed121-110">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="ed121-111">建立 Google API 主控台專案和用戶端識別碼</span><span class="sxs-lookup"><span data-stu-id="ed121-111">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="ed121-112">流覽至 [將[Google 登入整合到您的 web 應用程式](https://developers.google.com/identity/sign-in/web/devconsole-project)]，然後選取 [**設定專案**]。</span><span class="sxs-lookup"><span data-stu-id="ed121-112">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="ed121-113">在 [**設定您的 OAuth 用戶端**] 對話方塊中，選取 [ **Web 服務器**]。</span><span class="sxs-lookup"><span data-stu-id="ed121-113">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="ed121-114">在 [**授權重新導向 uri** ] 文字輸入方塊中，設定 [重新導向 uri]。</span><span class="sxs-lookup"><span data-stu-id="ed121-114">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="ed121-115">例如： `https://localhost:5001/signin-google`</span><span class="sxs-lookup"><span data-stu-id="ed121-115">For example, `https://localhost:5001/signin-google`</span></span>
* <span data-ttu-id="ed121-116">儲存**用戶端識別碼**和**用戶端密碼**。</span><span class="sxs-lookup"><span data-stu-id="ed121-116">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="ed121-117">部署網站時，從**Google 主控台**註冊新的公用 url。</span><span class="sxs-lookup"><span data-stu-id="ed121-117">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="ed121-118">存放區 Google ClientID 和 ClientSecret</span><span class="sxs-lookup"><span data-stu-id="ed121-118">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="ed121-119">使用[秘密管理員](xref:security/app-secrets)來儲存機密設定`Client ID` ， `Client Secret`例如 Google 和。</span><span class="sxs-lookup"><span data-stu-id="ed121-119">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="ed121-120">基於本教學課程的目的，請將權杖`Authentication:Google:ClientId`命名`Authentication:Google:ClientSecret`為，並：</span><span class="sxs-lookup"><span data-stu-id="ed121-120">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="ed121-121">您可以在[Api 主控台](https://console.developers.google.com/apis/dashboard)中管理 api 認證和使用方式。</span><span class="sxs-lookup"><span data-stu-id="ed121-121">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="ed121-122">設定 Google 驗證</span><span class="sxs-lookup"><span data-stu-id="ed121-122">Configure Google authentication</span></span>

<span data-ttu-id="ed121-123">將 Google 服務新增至`Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="ed121-123">Add the Google service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupGoogle.cs?name=snippet_ConfigureServices&highlight=10-18)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="ed121-124">使用 Google 登入</span><span class="sxs-lookup"><span data-stu-id="ed121-124">Sign in with Google</span></span>

* <span data-ttu-id="ed121-125">執行應用程式，然後按一下 [**登入**]。</span><span class="sxs-lookup"><span data-stu-id="ed121-125">Run the app and click **Log in**.</span></span> <span data-ttu-id="ed121-126">[使用 Google 登入] 選項隨即出現。</span><span class="sxs-lookup"><span data-stu-id="ed121-126">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="ed121-127">按一下 [ **google** ] 按鈕，以重新導向至 google 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="ed121-127">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="ed121-128">輸入您的 Google 認證之後，系統會將您重新導向回到網站。</span><span class="sxs-lookup"><span data-stu-id="ed121-128">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="ed121-129">如需 Google 驗證所支援之設定選項的詳細資訊，請參閱API參考。<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions></span><span class="sxs-lookup"><span data-stu-id="ed121-129">See the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="ed121-130">這可用來要求不同使用者的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="ed121-130">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="ed121-131">變更預設回呼 URI</span><span class="sxs-lookup"><span data-stu-id="ed121-131">Change the default callback URI</span></span>

<span data-ttu-id="ed121-132">URI 區段`/signin-google`會設定為 Google 驗證提供者的預設回呼。</span><span class="sxs-lookup"><span data-stu-id="ed121-132">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="ed121-133">您可以變更預設的回呼 URI 時設定 Google 驗證中的介軟體，透過繼承[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性[GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions)類別。</span><span class="sxs-lookup"><span data-stu-id="ed121-133">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="ed121-134">疑難排解</span><span class="sxs-lookup"><span data-stu-id="ed121-134">Troubleshooting</span></span>

* <span data-ttu-id="ed121-135">如果登入無法正常執行，而且您沒有收到任何錯誤，請切換到開發模式，讓問題更容易進行調試。</span><span class="sxs-lookup"><span data-stu-id="ed121-135">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="ed121-136">如果未藉由在中`services.AddIdentity` `ConfigureServices`呼叫來設定身分識別，則*嘗試驗證 ArgumentException 中的結果：必須提供*' SignInScheme ' 選項。</span><span class="sxs-lookup"><span data-stu-id="ed121-136">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="ed121-137">在本教學課程中使用的專案範本可確保，這麼做。</span><span class="sxs-lookup"><span data-stu-id="ed121-137">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="ed121-138">如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時資料庫作業失敗*時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="ed121-138">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="ed121-139">選取 [套用**遷移**] 以建立資料庫，然後重新整理頁面以繼續發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="ed121-139">Select **Apply Migrations** to create the database, and refresh the page to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed121-140">後續步驟</span><span class="sxs-lookup"><span data-stu-id="ed121-140">Next steps</span></span>

* <span data-ttu-id="ed121-141">本文說明如何您可以使用 Google 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="ed121-141">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="ed121-142">您可以依照類似的方法，來驗證與列在其他提供者[上一頁](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="ed121-142">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="ed121-143">將應用程式發行至 Azure 之後，請`ClientSecret`在 Google API 主控台中重設。</span><span class="sxs-lookup"><span data-stu-id="ed121-143">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="ed121-144">設定`Authentication:Google:ClientId`和`Authentication:Google:ClientSecret`當做 Azure 入口網站中的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="ed121-144">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="ed121-145">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="ed121-145">The configuration system is set up to read keys from environment variables.</span></span>
