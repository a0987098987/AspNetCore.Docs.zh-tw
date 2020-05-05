---
title: ASP.NET Core 中的 Google external 登入設定
author: rick-anderson
description: 本教學課程示範 Google 帳戶使用者驗證與現有 ASP.NET Core 應用程式的整合。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 03/19/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/google-logins
ms.openlocfilehash: 06a5cf77e47d9fc618fddf3a096001a4f31688be
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776990"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="932a9-103">ASP.NET Core 中的 Google external 登入設定</span><span class="sxs-lookup"><span data-stu-id="932a9-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="932a9-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="932a9-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="932a9-105">本教學課程說明如何讓使用者使用在[前一頁](xref:security/authentication/social/index)建立的 ASP.NET Core 3.0 專案，以其 Google 帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="932a9-105">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="932a9-106">建立 Google API 主控台專案和用戶端識別碼</span><span class="sxs-lookup"><span data-stu-id="932a9-106">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="932a9-107">安裝[AspNetCore。 Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google)。</span><span class="sxs-lookup"><span data-stu-id="932a9-107">Install [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google).</span></span>
* <span data-ttu-id="932a9-108">流覽至 [將[Google 登入整合到您的 web 應用程式](https://developers.google.com/identity/sign-in/web/devconsole-project)]，然後選取 [**設定專案**]。</span><span class="sxs-lookup"><span data-stu-id="932a9-108">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="932a9-109">在 [**設定您的 OAuth 用戶端**] 對話方塊中，選取 [ **Web 服務器**]。</span><span class="sxs-lookup"><span data-stu-id="932a9-109">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="932a9-110">在 [**授權重新導向 uri** ] 文字輸入方塊中，設定 [重新導向 uri]。</span><span class="sxs-lookup"><span data-stu-id="932a9-110">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="932a9-111">例如， `https://localhost:44312/signin-google`</span><span class="sxs-lookup"><span data-stu-id="932a9-111">For example, `https://localhost:44312/signin-google`</span></span>
* <span data-ttu-id="932a9-112">儲存**用戶端識別碼**和**用戶端密碼**。</span><span class="sxs-lookup"><span data-stu-id="932a9-112">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="932a9-113">部署網站時，從**Google 主控台**註冊新的公用 url。</span><span class="sxs-lookup"><span data-stu-id="932a9-113">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-the-google-client-id-and-secret"></a><span data-ttu-id="932a9-114">儲存 Google 用戶端識別碼和密碼</span><span class="sxs-lookup"><span data-stu-id="932a9-114">Store the Google client ID and secret</span></span>

<span data-ttu-id="932a9-115">使用[秘密管理員](xref:security/app-secrets)來儲存機密設定（例如 Google 用戶端識別碼和秘密值）。</span><span class="sxs-lookup"><span data-stu-id="932a9-115">Store sensitive settings such as the Google client ID and secret values with [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="932a9-116">針對此範例，請使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="932a9-116">For this sample, use the following steps:</span></span>

1. <span data-ttu-id="932a9-117">根據[啟用秘密儲存](xref:security/app-secrets#enable-secret-storage)中的指示，初始化秘密儲存的專案。</span><span class="sxs-lookup"><span data-stu-id="932a9-117">Initialize the project for secret storage per the instructions at [Enable secret storage](xref:security/app-secrets#enable-secret-storage).</span></span>
1. <span data-ttu-id="932a9-118">使用秘密金鑰`Authentication:Google:ClientId`和， `Authentication:Google:ClientSecret`將敏感性設定儲存在本機密碼存放區中：</span><span class="sxs-lookup"><span data-stu-id="932a9-118">Store the sensitive settings in the local secret store with the secret keys `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

    ```dotnetcli
    dotnet user-secrets set "Authentication:Google:ClientId" "<client-id>"
    dotnet user-secrets set "Authentication:Google:ClientSecret" "<client-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="932a9-119">您可以在[Api 主控台](https://console.developers.google.com/apis/dashboard)中管理 api 認證和使用方式。</span><span class="sxs-lookup"><span data-stu-id="932a9-119">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="932a9-120">設定 Google 驗證</span><span class="sxs-lookup"><span data-stu-id="932a9-120">Configure Google authentication</span></span>

<span data-ttu-id="932a9-121">將 Google 服務新增至`Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="932a9-121">Add the Google service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupGoogle3x.cs?highlight=11-19)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="932a9-122">以 Google 登入</span><span class="sxs-lookup"><span data-stu-id="932a9-122">Sign in with Google</span></span>

* <span data-ttu-id="932a9-123">執行應用程式，然後按一下 [**登入**]。</span><span class="sxs-lookup"><span data-stu-id="932a9-123">Run the app and click **Log in**.</span></span> <span data-ttu-id="932a9-124">[使用 Google 登入] 選項隨即出現。</span><span class="sxs-lookup"><span data-stu-id="932a9-124">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="932a9-125">按一下 [ **google** ] 按鈕，以重新導向至 google 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="932a9-125">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="932a9-126">輸入您的 Google 認證之後，系統會將您重新導向回到網站。</span><span class="sxs-lookup"><span data-stu-id="932a9-126">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="932a9-127">如需<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> Google 驗證所支援之設定選項的詳細資訊，請參閱 API 參考。</span><span class="sxs-lookup"><span data-stu-id="932a9-127">See the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="932a9-128">這可以用來要求使用者的不同資訊。</span><span class="sxs-lookup"><span data-stu-id="932a9-128">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="932a9-129">變更預設回呼 URI</span><span class="sxs-lookup"><span data-stu-id="932a9-129">Change the default callback URI</span></span>

<span data-ttu-id="932a9-130">URI 區段`/signin-google`會設定為 Google 驗證提供者的預設回呼。</span><span class="sxs-lookup"><span data-stu-id="932a9-130">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="932a9-131">設定 Google 驗證中介軟體時，您可以透過[GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions)類別的繼承[RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性來變更預設的回呼 URI。</span><span class="sxs-lookup"><span data-stu-id="932a9-131">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="932a9-132">疑難排解</span><span class="sxs-lookup"><span data-stu-id="932a9-132">Troubleshooting</span></span>

* <span data-ttu-id="932a9-133">如果登入無法正常執行，而且您沒有收到任何錯誤，請切換到開發模式，讓問題更容易進行調試。</span><span class="sxs-lookup"><span data-stu-id="932a9-133">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="932a9-134">如果Identity未透過在中`services.AddIdentity` `ConfigureServices`呼叫來設定，則嘗試在 ArgumentException 中驗證結果 *：必須提供 ' SignInScheme ' 選項*。</span><span class="sxs-lookup"><span data-stu-id="932a9-134">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="932a9-135">本教學課程中使用的專案範本可確保這項作業已完成。</span><span class="sxs-lookup"><span data-stu-id="932a9-135">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="932a9-136">如果尚未藉由套用初始遷移來建立網站資料庫，則在*處理要求錯誤時*，您會取得資料庫作業失敗。</span><span class="sxs-lookup"><span data-stu-id="932a9-136">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="932a9-137">選取 [套用**遷移**] 以建立資料庫，然後重新整理頁面以繼續發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="932a9-137">Select **Apply Migrations** to create the database, and refresh the page to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="932a9-138">後續步驟</span><span class="sxs-lookup"><span data-stu-id="932a9-138">Next steps</span></span>

* <span data-ttu-id="932a9-139">本文說明了您可以如何使用 Google 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="932a9-139">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="932a9-140">您可以遵循類似的方法，向[先前頁面](xref:security/authentication/social/index)上所列的其他提供者進行驗證。</span><span class="sxs-lookup"><span data-stu-id="932a9-140">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="932a9-141">將應用程式發行至 Azure 之後，請`ClientSecret`在 Google API 主控台中重設。</span><span class="sxs-lookup"><span data-stu-id="932a9-141">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="932a9-142">將`Authentication:Google:ClientId`和`Authentication:Google:ClientSecret`設定為 Azure 入口網站中的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="932a9-142">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="932a9-143">設定系統已設定為從環境變數讀取金鑰。</span><span class="sxs-lookup"><span data-stu-id="932a9-143">The configuration system is set up to read keys from environment variables.</span></span>
