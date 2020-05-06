---
title: ASP.NET Core 中的 Facebook 外部登入設定
author: rick-anderson
description: 教學課程中的程式碼範例示範如何將 Facebook 帳戶使用者驗證整合到現有的 ASP.NET Core 應用程式中。
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/facebook-logins
ms.openlocfilehash: 11ddc7314a694446d488da6ef1b2e3423bf7241a
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82777029"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="1ae30-103">ASP.NET Core 中的 Facebook 外部登入設定</span><span class="sxs-lookup"><span data-stu-id="1ae30-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="1ae30-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1ae30-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<!-- per @rick-anderson and scott addie, don't update images. Remove images and point the customer to the FB set up page. FB needs to maintain  instructions to get key and secret.
-->

<span data-ttu-id="1ae30-105">本教學課程包含程式碼範例，說明如何讓使用者使用在[上一頁](xref:security/authentication/social/index)建立的範例 ASP.NET Core 3.0 專案，以其 Facebook 帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="1ae30-105">This tutorial with code examples shows how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="1ae30-106">我們一開始會遵循[官方步驟](https://developers.facebook.com)來建立 Facebook 應用程式識別碼。</span><span class="sxs-lookup"><span data-stu-id="1ae30-106">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="1ae30-107">在 Facebook 中建立應用程式</span><span class="sxs-lookup"><span data-stu-id="1ae30-107">Create the app in Facebook</span></span>

* <span data-ttu-id="1ae30-108">將[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet 套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="1ae30-108">Add the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package to the project.</span></span>

* <span data-ttu-id="1ae30-109">流覽至[Facebook 開發人員應用程式](https://developers.facebook.com/apps/)頁面並登入。</span><span class="sxs-lookup"><span data-stu-id="1ae30-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="1ae30-110">如果您還沒有 Facebook 帳戶，請使用登入頁面上的 [**註冊 Facebook** ] 連結來建立一個。</span><span class="sxs-lookup"><span data-stu-id="1ae30-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>  <span data-ttu-id="1ae30-111">擁有 Facebook 帳戶後，請遵循指示註冊為 Facebook 開發人員。</span><span class="sxs-lookup"><span data-stu-id="1ae30-111">Once you have a Facebook account, follow the instructions to register as a Facebook Developer.</span></span>

* <span data-ttu-id="1ae30-112">從 [**我的應用程式**] 功能表中，選取 [**建立應用程式**] 以建立新的應用程式識別碼。</span><span class="sxs-lookup"><span data-stu-id="1ae30-112">From the **My Apps** menu select **Create App** to create a new App ID.</span></span>

   ![適用于開發人員的 Facebook 入口網站在 Microsoft Edge 中開啟](index/_static/FBMyApps.png)

* <span data-ttu-id="1ae30-114">填寫表單，並按一下 [**建立應用程式識別碼**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="1ae30-114">Fill out the form and tap the **Create App ID** button.</span></span>

  ![建立新的應用程式識別碼表單](index/_static/FBNewAppId.png)

* <span data-ttu-id="1ae30-116">在 [新的應用程式] 卡片上，選取 [**新增產品**]。</span><span class="sxs-lookup"><span data-stu-id="1ae30-116">On the new App card, select **Add a Product**.</span></span>  <span data-ttu-id="1ae30-117">在**Facebook 登**入卡上，按一下 [**設定**]</span><span class="sxs-lookup"><span data-stu-id="1ae30-117">On the **Facebook Login** card, click **Set Up**</span></span> 

  ![產品設定頁面](index/_static/FBProductSetup.png)

* <span data-ttu-id="1ae30-119">[**快速入門**] 嚮導會以 **[選擇平臺**] 作為第一頁來啟動。</span><span class="sxs-lookup"><span data-stu-id="1ae30-119">The **Quickstart** wizard launches with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="1ae30-120">立即略過嚮導，方法是按一下左下方功能表中的 [ **FaceBook 登**入**設定**] 連結：</span><span class="sxs-lookup"><span data-stu-id="1ae30-120">Bypass the wizard for now by clicking the **FaceBook Login** **Settings** link in the menu on the lower left:</span></span>

  ![略過快速入門](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="1ae30-122">您會看到 [**用戶端 OAuth 設定**] 頁面：</span><span class="sxs-lookup"><span data-stu-id="1ae30-122">You are presented with the **Client OAuth Settings** page:</span></span>

  ![[用戶端 OAuth 設定] 頁面](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="1ae30-124">輸入您的開發 URI，並將 */signin-facebook*附加至 [**有效的 OAuth 重新導向 uri** ] 欄位（例如： `https://localhost:44320/signin-facebook`）。</span><span class="sxs-lookup"><span data-stu-id="1ae30-124">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="1ae30-125">本教學課程稍後設定的 Facebook 驗證會在 */signin-facebook*路由上自動處理要求，以執行 OAuth 流程。</span><span class="sxs-lookup"><span data-stu-id="1ae30-125">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="1ae30-126">URI */signin-facebook*會設定為 facebook 驗證提供者的預設回呼。</span><span class="sxs-lookup"><span data-stu-id="1ae30-126">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="1ae30-127">您可以在設定 Facebook 驗證中介軟體時，透過[FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions)類別的繼承[RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性來變更預設的回呼 URI。</span><span class="sxs-lookup"><span data-stu-id="1ae30-127">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="1ae30-128">按一下 **[儲存變更]** 。</span><span class="sxs-lookup"><span data-stu-id="1ae30-128">Click **Save Changes**.</span></span>

* <span data-ttu-id="1ae30-129">按一下左側導覽列中的 [**設定** > ] [**基本**] 連結。</span><span class="sxs-lookup"><span data-stu-id="1ae30-129">Click **Settings** > **Basic** link in the left navigation.</span></span>

  <span data-ttu-id="1ae30-130">在此頁面上，記下您`App ID`的和。 `App Secret`</span><span class="sxs-lookup"><span data-stu-id="1ae30-130">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="1ae30-131">您會在下一節中，將這兩個新增至您的 ASP.NET Core 應用程式：</span><span class="sxs-lookup"><span data-stu-id="1ae30-131">You will add both into your ASP.NET Core application in the next section:</span></span>

* <span data-ttu-id="1ae30-132">部署網站時，您必須重新流覽**Facebook 登**入設定頁面，並註冊新的公用 URI。</span><span class="sxs-lookup"><span data-stu-id="1ae30-132">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-the-facebook-app-id-and-secret"></a><span data-ttu-id="1ae30-133">儲存 Facebook 應用程式識別碼和密碼</span><span class="sxs-lookup"><span data-stu-id="1ae30-133">Store the Facebook app ID and secret</span></span>

<span data-ttu-id="1ae30-134">使用[秘密管理員](xref:security/app-secrets)來儲存機密設定（例如 Facebook 應用程式識別碼和秘密值）。</span><span class="sxs-lookup"><span data-stu-id="1ae30-134">Store sensitive settings such as the Facebook app ID and secret values with [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="1ae30-135">針對此範例，請使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="1ae30-135">For this sample, use the following steps:</span></span>

1. <span data-ttu-id="1ae30-136">根據[啟用秘密儲存](xref:security/app-secrets#enable-secret-storage)中的指示，初始化秘密儲存的專案。</span><span class="sxs-lookup"><span data-stu-id="1ae30-136">Initialize the project for secret storage per the instructions at [Enable secret storage](xref:security/app-secrets#enable-secret-storage).</span></span>
1. <span data-ttu-id="1ae30-137">使用秘密金鑰`Authentication:Facebook:AppId`和， `Authentication:Facebook:AppSecret`將敏感性設定儲存在本機密碼存放區中：</span><span class="sxs-lookup"><span data-stu-id="1ae30-137">Store the sensitive settings in the local secret store with the secret keys `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`:</span></span>

    ```dotnetcli
    dotnet user-secrets set "Authentication:Facebook:AppId" "<app-id>"
    dotnet user-secrets set "Authentication:Facebook:AppSecret" "<app-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-facebook-authentication"></a><span data-ttu-id="1ae30-138">設定 Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="1ae30-138">Configure Facebook Authentication</span></span>

<span data-ttu-id="1ae30-139">在`ConfigureServices` *Startup.cs*檔案的方法中新增 Facebook 服務：</span><span class="sxs-lookup"><span data-stu-id="1ae30-139">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

## <a name="sign-in-with-facebook"></a><span data-ttu-id="1ae30-140">使用 Facebook 登入</span><span class="sxs-lookup"><span data-stu-id="1ae30-140">Sign in with Facebook</span></span>

* <span data-ttu-id="1ae30-141">執行應用程式，然後選取 [**登入**]。</span><span class="sxs-lookup"><span data-stu-id="1ae30-141">Run the app and select **Log in**.</span></span> 
* <span data-ttu-id="1ae30-142">在 [**使用其他服務登入**] 底下，選取 [Facebook]。</span><span class="sxs-lookup"><span data-stu-id="1ae30-142">Under **Use another service to log in.**, select Facebook.</span></span>
* <span data-ttu-id="1ae30-143">系統會將您重新導向至**Facebook**以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="1ae30-143">You are redirected to **Facebook** for authentication.</span></span>
* <span data-ttu-id="1ae30-144">輸入您的 Facebook 認證。</span><span class="sxs-lookup"><span data-stu-id="1ae30-144">Enter your Facebook credentials.</span></span>
* <span data-ttu-id="1ae30-145">系統會將您重新導向回到您的網站，您可以在其中設定您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="1ae30-145">You are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="1ae30-146">您現在已使用 Facebook 認證登入：</span><span class="sxs-lookup"><span data-stu-id="1ae30-146">You are now logged in using your Facebook credentials:</span></span>

<a name="react"></a>

## <a name="react-to-cancel-authorize-external-sign-in"></a><span data-ttu-id="1ae30-147">回應取消授權外部登入</span><span class="sxs-lookup"><span data-stu-id="1ae30-147">React to cancel authorize external sign-in</span></span>

<span data-ttu-id="1ae30-148"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.AccessDeniedPath>當使用者未核准要求的授權需求時，可以提供使用者代理程式的重新導向路徑。</span><span class="sxs-lookup"><span data-stu-id="1ae30-148"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.AccessDeniedPath> can provide a redirect path to the user agent when the user doesn't approve the requested authorization demand.</span></span>

<span data-ttu-id="1ae30-149">下列程式碼會將`AccessDeniedPath`設定`"/AccessDeniedPathInfo"`為：</span><span class="sxs-lookup"><span data-stu-id="1ae30-149">The following code sets the `AccessDeniedPath` to `"/AccessDeniedPathInfo"`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupAccessDeniedPath.cs?name=snippetFB)]

<span data-ttu-id="1ae30-150">我們建議`AccessDeniedPath`頁面包含下列資訊：</span><span class="sxs-lookup"><span data-stu-id="1ae30-150">We recommend the `AccessDeniedPath` page contain the following information:</span></span>

*  <span data-ttu-id="1ae30-151">遠端驗證已取消。</span><span class="sxs-lookup"><span data-stu-id="1ae30-151">Remote authentication was canceled.</span></span>
* <span data-ttu-id="1ae30-152">此應用程式需要驗證。</span><span class="sxs-lookup"><span data-stu-id="1ae30-152">This app requires authentication.</span></span>
* <span data-ttu-id="1ae30-153">若要再次嘗試登入，請選取 [登入] 連結。</span><span class="sxs-lookup"><span data-stu-id="1ae30-153">To try sign-in again, select the Login link.</span></span>

### <a name="test-accessdeniedpath"></a><span data-ttu-id="1ae30-154">測試 AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="1ae30-154">Test AccessDeniedPath</span></span>

* <span data-ttu-id="1ae30-155">流覽至[facebook.com](https://www.facebook.com/)</span><span class="sxs-lookup"><span data-stu-id="1ae30-155">Navigate to [facebook.com](https://www.facebook.com/)</span></span>
* <span data-ttu-id="1ae30-156">如果您已登入，則必須登出。</span><span class="sxs-lookup"><span data-stu-id="1ae30-156">If you are signed in, you must sign out.</span></span>
* <span data-ttu-id="1ae30-157">執行應用程式，然後選取 [Facebook 登入]。</span><span class="sxs-lookup"><span data-stu-id="1ae30-157">Run the app and select Facebook sign-in.</span></span>
* <span data-ttu-id="1ae30-158">選取 [**不是現在**]。</span><span class="sxs-lookup"><span data-stu-id="1ae30-158">Select **Not now**.</span></span> <span data-ttu-id="1ae30-159">系統會將您重新導向`AccessDeniedPath`至指定的頁面。</span><span class="sxs-lookup"><span data-stu-id="1ae30-159">You are redirected to the specified `AccessDeniedPath` page.</span></span>

<!-- End of React  -->
[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="1ae30-160">如需 Facebook 驗證所支援之設定選項的詳細資訊，請參閱[FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API 參考。</span><span class="sxs-lookup"><span data-stu-id="1ae30-160">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="1ae30-161">設定選項可用於：</span><span class="sxs-lookup"><span data-stu-id="1ae30-161">Configuration options can be used to:</span></span>

* <span data-ttu-id="1ae30-162">要求使用者的不同資訊。</span><span class="sxs-lookup"><span data-stu-id="1ae30-162">Request different information about the user.</span></span>
* <span data-ttu-id="1ae30-163">加入查詢字串引數來自訂登入體驗。</span><span class="sxs-lookup"><span data-stu-id="1ae30-163">Add query string arguments to customize the login experience.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="1ae30-164">疑難排解</span><span class="sxs-lookup"><span data-stu-id="1ae30-164">Troubleshooting</span></span>

* <span data-ttu-id="1ae30-165">**僅限 ASP.NET Core 2.x：** 如果Identity未透過在中`services.AddIdentity` `ConfigureServices`呼叫來設定，則嘗試驗證會導致*ArgumentException：必須提供 ' SignInScheme ' 選項*。</span><span class="sxs-lookup"><span data-stu-id="1ae30-165">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="1ae30-166">本教學課程中使用的專案範本可確保這項作業已完成。</span><span class="sxs-lookup"><span data-stu-id="1ae30-166">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="1ae30-167">如果尚未藉由套用初始遷移來建立網站資料庫，則在*處理要求錯誤時*，您會取得資料庫作業失敗。</span><span class="sxs-lookup"><span data-stu-id="1ae30-167">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="1ae30-168">請按 [套用**遷移**] 來建立資料庫，並重新整理以繼續發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="1ae30-168">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1ae30-169">後續步驟</span><span class="sxs-lookup"><span data-stu-id="1ae30-169">Next steps</span></span>

* <span data-ttu-id="1ae30-170">本文說明了您可以如何使用 Facebook 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="1ae30-170">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="1ae30-171">您可以遵循類似的方法，向[先前頁面](xref:security/authentication/social/index)上所列的其他提供者進行驗證。</span><span class="sxs-lookup"><span data-stu-id="1ae30-171">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="1ae30-172">將網站發佈至 Azure web 應用程式之後，您應該`AppSecret`在 Facebook 開發人員入口網站中重設。</span><span class="sxs-lookup"><span data-stu-id="1ae30-172">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="1ae30-173">將`Authentication:Facebook:AppId`和`Authentication:Facebook:AppSecret`設定為 Azure 入口網站中的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="1ae30-173">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="1ae30-174">設定系統已設定為從環境變數讀取金鑰。</span><span class="sxs-lookup"><span data-stu-id="1ae30-174">The configuration system is set up to read keys from environment variables.</span></span>
