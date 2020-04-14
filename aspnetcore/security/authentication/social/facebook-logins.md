---
title: ASP.NET核心中的 Facebook 外部登入設定
author: rick-anderson
description: 包含代碼示例的教程,演示 Facebook 帳戶使用者身份驗證集成到現有ASP.NET核心應用。
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/facebook-logins
ms.openlocfilehash: 9b3128addafb41ad6ec44af5cb12e89607e1ae59
ms.sourcegitcommit: 5af16166977da598953f82da3ed3b7712d38f6cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/14/2020
ms.locfileid: "81277297"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="23a6a-103">ASP.NET核心中的 Facebook 外部登入設定</span><span class="sxs-lookup"><span data-stu-id="23a6a-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="23a6a-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="23a6a-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<!-- per @rick-anderson and scott addie, don't update images. Remove images and point the customer to the FB set up page. FB needs to maintain  instructions to get key and secret.
-->

<span data-ttu-id="23a6a-105">本教程包含代碼示例展示如何允許使用者使用[上一頁上](xref:security/authentication/social/index)創建的範例ASP.NET Core 3.0 專案使用 Facebook 帳戶登錄。</span><span class="sxs-lookup"><span data-stu-id="23a6a-105">This tutorial with code examples shows how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="23a6a-106">我們首先按照[官方步驟](https://developers.facebook.com)創建 Facebook 應用程式 ID。</span><span class="sxs-lookup"><span data-stu-id="23a6a-106">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="23a6a-107">在 Facebook 中建立應用</span><span class="sxs-lookup"><span data-stu-id="23a6a-107">Create the app in Facebook</span></span>

* <span data-ttu-id="23a6a-108">將[Microsoft.AspNetCore.身份驗證.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet 包添加到專案中。</span><span class="sxs-lookup"><span data-stu-id="23a6a-108">Add the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package to the project.</span></span>

* <span data-ttu-id="23a6a-109">導航到[Facebook 開發人員應用](https://developers.facebook.com/apps/)頁面並登錄。</span><span class="sxs-lookup"><span data-stu-id="23a6a-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="23a6a-110">如果您還沒有 Facebook 帳戶,請使用登入頁面上**的「註冊 Facebook」** 連結創建一個帳戶。</span><span class="sxs-lookup"><span data-stu-id="23a6a-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>  <span data-ttu-id="23a6a-111">擁有 Facebook 帳戶後,請按照說明註冊為 Facebook 開發人員。</span><span class="sxs-lookup"><span data-stu-id="23a6a-111">Once you have a Facebook account, follow the instructions to register as a Facebook Developer.</span></span>

* <span data-ttu-id="23a6a-112">從「**我的應用」** 選單中選擇 **「創建應用**」以建立新的應用 ID。</span><span class="sxs-lookup"><span data-stu-id="23a6a-112">From the **My Apps** menu select **Create App** to create a new App ID.</span></span>

   ![Facebook 面向開發人員門戶在微軟邊緣打開](index/_static/FBMyApps.png)

* <span data-ttu-id="23a6a-114">填寫表單並點擊「**創建應用 ID」** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="23a6a-114">Fill out the form and tap the **Create App ID** button.</span></span>

  ![建立新的應用程式識別碼表單](index/_static/FBNewAppId.png)

* <span data-ttu-id="23a6a-116">在新應用卡上,選擇 **「添加產品**」。。</span><span class="sxs-lookup"><span data-stu-id="23a6a-116">On the new App card, select **Add a Product**.</span></span>  <span data-ttu-id="23a6a-117">在**Facebook 登錄**卡上,單擊"**設置"**</span><span class="sxs-lookup"><span data-stu-id="23a6a-117">On the **Facebook Login** card, click **Set Up**</span></span> 

  ![產品設定頁面](index/_static/FBProductSetup.png)

* <span data-ttu-id="23a6a-119">**"快速入門**"嚮導以 **"選擇平台**作為第一頁"啟動。</span><span class="sxs-lookup"><span data-stu-id="23a6a-119">The **Quickstart** wizard launches with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="23a6a-120">按下左下角選單中的**FaceBook 登錄\*\*\*\*設定**連結,立即繞過嚮導:</span><span class="sxs-lookup"><span data-stu-id="23a6a-120">Bypass the wizard for now by clicking the **FaceBook Login** **Settings** link in the menu on the lower left:</span></span>

  ![跳過快速入門](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="23a6a-122">將顯示 **「 客戶端設定」** 頁面:</span><span class="sxs-lookup"><span data-stu-id="23a6a-122">You are presented with the **Client OAuth Settings** page:</span></span>

  ![用戶端 Auth 設定頁面](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="23a6a-124">輸入您的開發*URI,將 /signin-facebook*新增到**有效 OAuth 重定向 URI**欄位`https://localhost:44320/signin-facebook`(例如:</span><span class="sxs-lookup"><span data-stu-id="23a6a-124">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="23a6a-125">本教程稍後配置的 Facebook 身份驗證將自動處理 */signin-facebook*路由上的請求,以實現 OAuth 流。</span><span class="sxs-lookup"><span data-stu-id="23a6a-125">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="23a6a-126">URI */signin-facebook*設置為 Facebook 身份驗證供應商的預設回調。</span><span class="sxs-lookup"><span data-stu-id="23a6a-126">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="23a6a-127">您可以通過繼承的[「遠端身份驗證選項.CallbackPath」](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性配置 Facebook 身份驗證中間件,同時[FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions)更改預設回檔 URI。</span><span class="sxs-lookup"><span data-stu-id="23a6a-127">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="23a6a-128">按一下 [儲存變更]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="23a6a-128">Click **Save Changes**.</span></span>

* <span data-ttu-id="23a6a-129">按一下左側導航中的 **「設置** > **基本」** 連結。</span><span class="sxs-lookup"><span data-stu-id="23a6a-129">Click **Settings** > **Basic** link in the left navigation.</span></span>

  <span data-ttu-id="23a6a-130">在此頁上,記下您和您的`App ID``App Secret`。</span><span class="sxs-lookup"><span data-stu-id="23a6a-130">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="23a6a-131">在下一節中,您將兩者加入ASP.NET核心應用程式中:</span><span class="sxs-lookup"><span data-stu-id="23a6a-131">You will add both into your ASP.NET Core application in the next section:</span></span>

* <span data-ttu-id="23a6a-132">部署網站時,您需要重新訪問**Facebook 登錄**設置頁面並註冊新的公共 URI。</span><span class="sxs-lookup"><span data-stu-id="23a6a-132">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-the-facebook-app-id-and-secret"></a><span data-ttu-id="23a6a-133">儲存 Facebook 應用程式識別碼和機密</span><span class="sxs-lookup"><span data-stu-id="23a6a-133">Store the Facebook app ID and secret</span></span>

<span data-ttu-id="23a6a-134">使用[秘密管理器](xref:security/app-secrets)儲存敏感設置,如 Facebook 應用 ID 和機密值。</span><span class="sxs-lookup"><span data-stu-id="23a6a-134">Store sensitive settings such as the Facebook app ID and secret values with [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="23a6a-135">對於此示例,請使用以下步驟:</span><span class="sxs-lookup"><span data-stu-id="23a6a-135">For this sample, use the following steps:</span></span>

1. <span data-ttu-id="23a6a-136">根據[啟用密鑰存儲](xref:security/app-secrets#enable-secret-storage)的指令初始化了機密存儲的專案。</span><span class="sxs-lookup"><span data-stu-id="23a6a-136">Initialize the project for secret storage per the instructions at [Enable secret storage](xref:security/app-secrets#enable-secret-storage).</span></span>
1. <span data-ttu-id="23a6a-137">使用金鑰`Authentication:Facebook:AppId`與將敏感設定儲存在本地機密儲存中`Authentication:Facebook:AppSecret`:</span><span class="sxs-lookup"><span data-stu-id="23a6a-137">Store the sensitive settings in the local secret store with the secret keys `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`:</span></span>

    ```dotnetcli
    dotnet user-secrets set "Authentication:Facebook:AppId" "<app-id>"
    dotnet user-secrets set "Authentication:Facebook:AppSecret" "<app-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-facebook-authentication"></a><span data-ttu-id="23a6a-138">設定 Facebook 認證</span><span class="sxs-lookup"><span data-stu-id="23a6a-138">Configure Facebook Authentication</span></span>

<span data-ttu-id="23a6a-139">在`ConfigureServices`*Startup.cs*檔案中的方法中新增 Facebook 服務:</span><span class="sxs-lookup"><span data-stu-id="23a6a-139">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

## <a name="sign-in-with-facebook"></a><span data-ttu-id="23a6a-140">使用 Facebook 登入</span><span class="sxs-lookup"><span data-stu-id="23a6a-140">Sign in with Facebook</span></span>

* <span data-ttu-id="23a6a-141">運行應用並選擇 **「登錄」。**</span><span class="sxs-lookup"><span data-stu-id="23a6a-141">Run the app and select **Log in**.</span></span> 
* <span data-ttu-id="23a6a-142">在 **"使用其他服務登錄"** 下,選擇 Facebook。</span><span class="sxs-lookup"><span data-stu-id="23a6a-142">Under **Use another service to log in.**, select Facebook.</span></span>
* <span data-ttu-id="23a6a-143">您將被重定向到**Facebook**進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="23a6a-143">You are redirected to **Facebook** for authentication.</span></span>
* <span data-ttu-id="23a6a-144">輸入您的 Facebook 認證。</span><span class="sxs-lookup"><span data-stu-id="23a6a-144">Enter your Facebook credentials.</span></span>
* <span data-ttu-id="23a6a-145">您將被重定向回您的網站,您可以在其中設定電子郵件。</span><span class="sxs-lookup"><span data-stu-id="23a6a-145">You are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="23a6a-146">您現在使用 Facebook 認證登入:</span><span class="sxs-lookup"><span data-stu-id="23a6a-146">You are now logged in using your Facebook credentials:</span></span>

<a name="react"></a>

## <a name="react-to-cancel-authorize-external-sign-in"></a><span data-ttu-id="23a6a-147">回應取消授權外部登入</span><span class="sxs-lookup"><span data-stu-id="23a6a-147">React to cancel authorize external sign-in</span></span>

<span data-ttu-id="23a6a-148"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.AccessDeniedPath>當使用者不批准請求的授權請求時,可以為使用者代理提供重定向路徑。</span><span class="sxs-lookup"><span data-stu-id="23a6a-148"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.AccessDeniedPath> can provide a redirect path to the user agent when the user doesn't approve the requested authorization demand.</span></span>

<span data-ttu-id="23a6a-149">以下代碼將`AccessDeniedPath``"/AccessDeniedPathInfo"`集 :</span><span class="sxs-lookup"><span data-stu-id="23a6a-149">The following code sets the `AccessDeniedPath` to `"/AccessDeniedPathInfo"`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupAccessDeniedPath.cs?name=snippetFB)]

<span data-ttu-id="23a6a-150">我們建議該`AccessDeniedPath`頁面包含以下資訊:</span><span class="sxs-lookup"><span data-stu-id="23a6a-150">We recommend the `AccessDeniedPath` page contain the following information:</span></span>

*  <span data-ttu-id="23a6a-151">遠端身份驗證已取消。</span><span class="sxs-lookup"><span data-stu-id="23a6a-151">Remote authentication was canceled.</span></span>
* <span data-ttu-id="23a6a-152">此應用程式需要身份驗證。</span><span class="sxs-lookup"><span data-stu-id="23a6a-152">This app requires authentication.</span></span>
* <span data-ttu-id="23a6a-153">要再次嘗試登錄,請選擇"登錄"連結。</span><span class="sxs-lookup"><span data-stu-id="23a6a-153">To try sign-in again, select the Login link.</span></span>

### <a name="test-accessdeniedpath"></a><span data-ttu-id="23a6a-154">測試存取拒絕路徑</span><span class="sxs-lookup"><span data-stu-id="23a6a-154">Test AccessDeniedPath</span></span>

* <span data-ttu-id="23a6a-155">導航到[facebook.com](https://www.facebook.com/)</span><span class="sxs-lookup"><span data-stu-id="23a6a-155">Navigate to [facebook.com](https://www.facebook.com/)</span></span>
* <span data-ttu-id="23a6a-156">如果您已登錄,則必須註銷。</span><span class="sxs-lookup"><span data-stu-id="23a6a-156">If you are signed in, you must sign out.</span></span>
* <span data-ttu-id="23a6a-157">運行應用並選擇 Facebook 登錄。</span><span class="sxs-lookup"><span data-stu-id="23a6a-157">Run the app and select Facebook sign-in.</span></span>
* <span data-ttu-id="23a6a-158">選擇 **「現在不**」。</span><span class="sxs-lookup"><span data-stu-id="23a6a-158">Select **Not now**.</span></span> <span data-ttu-id="23a6a-159">您將重定到指定的`AccessDeniedPath`頁面。</span><span class="sxs-lookup"><span data-stu-id="23a6a-159">You are redirected to the specified `AccessDeniedPath` page.</span></span>

<!-- End of React  -->
[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="23a6a-160">有關 Facebook 身份驗證支援的配置選項的詳細資訊,請參閱[Facebook 選項](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions)API 參考。</span><span class="sxs-lookup"><span data-stu-id="23a6a-160">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="23a6a-161">設定選項可用於:</span><span class="sxs-lookup"><span data-stu-id="23a6a-161">Configuration options can be used to:</span></span>

* <span data-ttu-id="23a6a-162">請求有關使用者的不同資訊。</span><span class="sxs-lookup"><span data-stu-id="23a6a-162">Request different information about the user.</span></span>
* <span data-ttu-id="23a6a-163">添加查詢字串參數以自定義登錄體驗。</span><span class="sxs-lookup"><span data-stu-id="23a6a-163">Add query string arguments to customize the login experience.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="23a6a-164">疑難排解</span><span class="sxs-lookup"><span data-stu-id="23a6a-164">Troubleshooting</span></span>

* <span data-ttu-id="23a6a-165">**只有ASP.NET核心 2.x:** 如果未透過除錯`services.AddIdentity`設定識別`ConfigureServices`,則試著認證會導致*參數異常:必須提供「SignInScheme」選項*。</span><span class="sxs-lookup"><span data-stu-id="23a6a-165">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="23a6a-166">本教學中使用的專案範本可確保完成此操作。</span><span class="sxs-lookup"><span data-stu-id="23a6a-166">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="23a6a-167">如果尚未透過應用程式初始移至建立網站資料庫,則在處理請求錯誤時,將取得*資料庫操作失敗*。</span><span class="sxs-lookup"><span data-stu-id="23a6a-167">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="23a6a-168">點按 **「應用遷移**」以建立資料庫並刷新以繼續結束錯誤。</span><span class="sxs-lookup"><span data-stu-id="23a6a-168">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="23a6a-169">後續步驟</span><span class="sxs-lookup"><span data-stu-id="23a6a-169">Next steps</span></span>

* <span data-ttu-id="23a6a-170">本文展示了如何使用 Facebook 進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="23a6a-170">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="23a6a-171">您可以採用類似的方法對[上一頁](xref:security/authentication/social/index)中列出的其他提供程式進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="23a6a-171">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="23a6a-172">將網站發布到 Azure Web 應用程式後,應重`AppSecret`置 Facebook 開發人員門戶中的 。</span><span class="sxs-lookup"><span data-stu-id="23a6a-172">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="23a6a-173">在`Authentication:Facebook:AppId`Azure`Authentication:Facebook:AppSecret`門戶中將 和 設置為應用程式設置。</span><span class="sxs-lookup"><span data-stu-id="23a6a-173">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="23a6a-174">配置系統設置為從環境變數讀取密鑰。</span><span class="sxs-lookup"><span data-stu-id="23a6a-174">The configuration system is set up to read keys from environment variables.</span></span>
