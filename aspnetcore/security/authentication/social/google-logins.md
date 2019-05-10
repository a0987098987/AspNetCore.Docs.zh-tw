---
title: ASP.NET Core 中的 Google 外部登入設定
author: rick-anderson
description: 本教學課程示範如何整合到現有的 ASP.NET Core 應用程式的 Google 帳戶使用者驗證。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 1/11/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 44c79b3279db7946b6d89a726bd3f5acfb5f51af
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895535"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="5829c-103">ASP.NET Core 中的 Google 外部登入設定</span><span class="sxs-lookup"><span data-stu-id="5829c-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="5829c-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5829c-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5829c-105">[自於 2019 年 3 月 7 日起已關閉舊版 Google + Api](https://developers.google.com/+/api-shutdown)。</span><span class="sxs-lookup"><span data-stu-id="5829c-105">[Legacy Google+ APIs have been shut down as of March 7, 2019](https://developers.google.com/+/api-shutdown).</span></span> <span data-ttu-id="5829c-106">Google + 登入，並開發人員必須在系統中移動至新的 Google 登。</span><span class="sxs-lookup"><span data-stu-id="5829c-106">Google+ sign in and developers must move to a new Google sign in system.</span></span> <span data-ttu-id="5829c-107">Google 驗證的 ASP.NET Core 2.1 和 2.2 套件已更新的變更。</span><span class="sxs-lookup"><span data-stu-id="5829c-107">The ASP.NET Core 2.1 and 2.2 packages for Google Authentication have be updated to accommodate the changes.</span></span> <span data-ttu-id="5829c-108">如需詳細資訊和 ASP.NET core 的暫存緩和措施，請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore/issues/6486)。</span><span class="sxs-lookup"><span data-stu-id="5829c-108">For more information and temporary mitigations for ASP.NET Core, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/6486).</span></span> <span data-ttu-id="5829c-109">本教學課程已更新新的安裝程序。</span><span class="sxs-lookup"><span data-stu-id="5829c-109">This tutorial has been updated with the new setup process.</span></span>

<span data-ttu-id="5829c-110">本教學課程會示範如何讓使用者使用其使用 ASP.NET Core 2.2 專案上建立的 Google 帳戶登入[上一頁](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="5829c-110">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="5829c-111">建立 Google API 主控台中的專案和用戶端識別碼</span><span class="sxs-lookup"><span data-stu-id="5829c-111">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="5829c-112">瀏覽至[整合 Google 登入到您的 web 應用程式](https://developers.google.com/identity/sign-in/web/devconsole-project)，然後選取**設定專案**。</span><span class="sxs-lookup"><span data-stu-id="5829c-112">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="5829c-113">在 **設定您的 OAuth 用戶端**對話方塊中，選取**Web 伺服器**。</span><span class="sxs-lookup"><span data-stu-id="5829c-113">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="5829c-114">在 **授權重新導向 Uri**文字方塊項目中，設定重新導向 URI。</span><span class="sxs-lookup"><span data-stu-id="5829c-114">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="5829c-115">例如： `https://localhost:5001/signin-google` </span><span class="sxs-lookup"><span data-stu-id="5829c-115">For example, `https://localhost:5001/signin-google`</span></span>
* <span data-ttu-id="5829c-116">儲存**用戶端識別碼**並**用戶端祕密**。</span><span class="sxs-lookup"><span data-stu-id="5829c-116">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="5829c-117">當部署的網站，註冊新的公用 url，從**Google 主控台**。</span><span class="sxs-lookup"><span data-stu-id="5829c-117">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="5829c-118">存放區 Google ClientID 和 ClientSecret</span><span class="sxs-lookup"><span data-stu-id="5829c-118">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="5829c-119">儲存機密設定，例如 Google`Client ID`並`Client Secret`具有[Secret Manager](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="5829c-119">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="5829c-120">基於本教學課程的目的，名稱語彙基元`Authentication:Google:ClientId`和`Authentication:Google:ClientSecret`:</span><span class="sxs-lookup"><span data-stu-id="5829c-120">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```console
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="5829c-121">您可以管理您的 API 認證和使用量[API 主控台](https://console.developers.google.com/apis/dashboard)。</span><span class="sxs-lookup"><span data-stu-id="5829c-121">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="5829c-122">設定 Google 驗證</span><span class="sxs-lookup"><span data-stu-id="5829c-122">Configure Google authentication</span></span>

<span data-ttu-id="5829c-123">新增 Google 服務，以`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="5829c-123">Add the Google service to `Startup.ConfigureServices`.</span></span>

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="5829c-124">使用 Google 登入</span><span class="sxs-lookup"><span data-stu-id="5829c-124">Sign in with Google</span></span>

* <span data-ttu-id="5829c-125">執行應用程式，然後按一下**登入**。</span><span class="sxs-lookup"><span data-stu-id="5829c-125">Run the app and click **Log in**.</span></span> <span data-ttu-id="5829c-126">若要使用 Google 登入的選項會出現。</span><span class="sxs-lookup"><span data-stu-id="5829c-126">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="5829c-127">按一下  **Google**按鈕，將重新導向至 Google 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="5829c-127">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="5829c-128">輸入您的 Google 認證之後, 您將被重新導向回到網站。</span><span class="sxs-lookup"><span data-stu-id="5829c-128">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="5829c-129">請參閱[GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) API 參考，如需有關 Google 驗證所支援的組態選項。</span><span class="sxs-lookup"><span data-stu-id="5829c-129">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="5829c-130">這可用來要求不同使用者的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="5829c-130">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="5829c-131">變更預設的回呼 URI</span><span class="sxs-lookup"><span data-stu-id="5829c-131">Change the default callback URI</span></span>

<span data-ttu-id="5829c-132">URI 區段`/signin-google`會設定為 Google 驗證提供者的預設回呼。</span><span class="sxs-lookup"><span data-stu-id="5829c-132">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="5829c-133">您可以變更預設的回呼 URI 時設定 Google 驗證中的介軟體，透過繼承[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性[GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions)類別。</span><span class="sxs-lookup"><span data-stu-id="5829c-133">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="5829c-134">疑難排解</span><span class="sxs-lookup"><span data-stu-id="5829c-134">Troubleshooting</span></span>

* <span data-ttu-id="5829c-135">如果登入無法運作，而且您未收到任何錯誤，切換到開發模式，以便讓您更輕鬆地偵錯問題。</span><span class="sxs-lookup"><span data-stu-id="5829c-135">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="5829c-136">如果身分識別未設定藉由呼叫`services.AddIdentity`中`ConfigureServices`，嘗試驗證會導致*ArgumentException:必須提供 'SignInScheme' 選項*。</span><span class="sxs-lookup"><span data-stu-id="5829c-136">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="5829c-137">在本教學課程中使用的專案範本可確保，這麼做。</span><span class="sxs-lookup"><span data-stu-id="5829c-137">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="5829c-138">如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時資料庫作業失敗*時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="5829c-138">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="5829c-139">點選**套用移轉**建立資料庫，並重新整理 以忽略錯誤繼續執行。</span><span class="sxs-lookup"><span data-stu-id="5829c-139">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5829c-140">後續步驟</span><span class="sxs-lookup"><span data-stu-id="5829c-140">Next steps</span></span>

* <span data-ttu-id="5829c-141">本文說明如何您可以使用 Google 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="5829c-141">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="5829c-142">您可以依照類似的方法，來驗證與列在其他提供者[上一頁](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="5829c-142">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="5829c-143">一旦您將應用程式發佈至 Azure 時，會重設`ClientSecret`Google API 主控台中。</span><span class="sxs-lookup"><span data-stu-id="5829c-143">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="5829c-144">設定`Authentication:Google:ClientId`和`Authentication:Google:ClientSecret`當做 Azure 入口網站中的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="5829c-144">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="5829c-145">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="5829c-145">The configuration system is set up to read keys from environment variables.</span></span>
