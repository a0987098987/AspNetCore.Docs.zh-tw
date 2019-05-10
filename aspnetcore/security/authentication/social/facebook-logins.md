---
title: ASP.NET Core 中的 Facebook 外部登入設定
author: rick-anderson
description: 程式碼範例來示範整合到現有的 ASP.NET Core 應用程式的 Facebook 帳戶使用者驗證的教學課程。
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 03/04/2019
uid: security/authentication/facebook-logins
ms.openlocfilehash: 84b891f6cee71a26726d49a9d42ae45007d2b429
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64893585"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="436e7-103">ASP.NET Core 中的 Facebook 外部登入設定</span><span class="sxs-lookup"><span data-stu-id="436e7-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="436e7-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="436e7-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="436e7-105">本教學課程，程式碼範例示範如何讓使用者使用自己的 Facebook 帳戶使用範例 ASP.NET Core 2.0 專案上建立登入[上一頁](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="436e7-105">This tutorial with code examples shows how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="436e7-106">我們先建立 Facebook 應用程式識別碼[官方步驟](https://developers.facebook.com)。</span><span class="sxs-lookup"><span data-stu-id="436e7-106">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="436e7-107">在 Facebook 中建立應用程式</span><span class="sxs-lookup"><span data-stu-id="436e7-107">Create the app in Facebook</span></span>

* <span data-ttu-id="436e7-108">瀏覽至[Facebook 開發人員應用程式](https://developers.facebook.com/apps/)頁面上，並登入。</span><span class="sxs-lookup"><span data-stu-id="436e7-108">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="436e7-109">如果您還沒有的 Facebook 帳戶，使用**註冊 Facebook**在登入 頁面上，若要建立一個連結。</span><span class="sxs-lookup"><span data-stu-id="436e7-109">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="436e7-110">點選**將新的應用程式新增**來建立新的應用程式識別碼。 右上角的按鈕</span><span class="sxs-lookup"><span data-stu-id="436e7-110">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Facebook 開發人員入口網站的 Microsoft Edge 中開啟](index/_static/FBMyApps.png)

* <span data-ttu-id="436e7-112">填寫表單，然後點選**建立應用程式識別碼** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="436e7-112">Fill out the form and tap the **Create App ID** button.</span></span>

  ![建立新的應用程式識別碼的表單](index/_static/FBNewAppId.png)

* <span data-ttu-id="436e7-114">在 **選取的產品**頁面上，按一下**設定**上**Facebook 登入**卡。</span><span class="sxs-lookup"><span data-stu-id="436e7-114">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

  ![產品 [安裝] 頁面](index/_static/FBProductSetup.png)

* <span data-ttu-id="436e7-116">**快速入門**精靈將會啟動具有**選擇平台**做的第一頁。</span><span class="sxs-lookup"><span data-stu-id="436e7-116">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="436e7-117">現在略過精靈，依序按一下**設定**在左側功能表中的連結：</span><span class="sxs-lookup"><span data-stu-id="436e7-117">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

  ![略過快速入門](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="436e7-119">您會看到**用戶端 OAuth 設定**頁面：</span><span class="sxs-lookup"><span data-stu-id="436e7-119">You are presented with the **Client OAuth Settings** page:</span></span>

  ![用戶端 OAuth 設定 頁面](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="436e7-121">輸入您的開發 URI 與 */signin-facebook*附加至**有效的 OAuth 重新導向 Uri**欄位 (例如： `https://localhost:44320/signin-facebook`)。</span><span class="sxs-lookup"><span data-stu-id="436e7-121">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="436e7-122">稍後在本教學課程中設定 Facebook 驗證會自動處理在要求 */signin-facebook*實作 OAuth 流程的路由。</span><span class="sxs-lookup"><span data-stu-id="436e7-122">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="436e7-123">URI */signin-facebook*設為預設回呼 Facebook 驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="436e7-123">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="436e7-124">您可以變更預設的回呼 URI 時設定 Facebook 驗證中的介軟體，透過繼承[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性[FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions)類別。</span><span class="sxs-lookup"><span data-stu-id="436e7-124">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="436e7-125">按一下 **儲存變更**。</span><span class="sxs-lookup"><span data-stu-id="436e7-125">Click **Save Changes**.</span></span>

* <span data-ttu-id="436e7-126">按一下 **設定** > **基本**在左側導覽中的連結。</span><span class="sxs-lookup"><span data-stu-id="436e7-126">Click **Settings** > **Basic** link in the left navigation.</span></span>

  <span data-ttu-id="436e7-127">在此頁面上，記下您`App ID`和您`App Secret`。</span><span class="sxs-lookup"><span data-stu-id="436e7-127">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="436e7-128">您會新增至 ASP.NET Core 應用程式下一節：</span><span class="sxs-lookup"><span data-stu-id="436e7-128">You will add both into your ASP.NET Core application in the next section:</span></span>

* <span data-ttu-id="436e7-129">部署站台時，您必須先討論一下**Facebook 登入**安裝頁面，並註冊新的公用 URI。</span><span class="sxs-lookup"><span data-stu-id="436e7-129">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="436e7-130">儲存 Facebook 應用程式識別碼和應用程式祕密</span><span class="sxs-lookup"><span data-stu-id="436e7-130">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="436e7-131">連結機密設定，例如 Facebook`App ID`並`App Secret`至您的應用程式的組態使用[Secret Manager](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="436e7-131">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="436e7-132">基於本教學課程的目的，名稱語彙基元`Authentication:Facebook:AppId`和`Authentication:Facebook:AppSecret`。</span><span class="sxs-lookup"><span data-stu-id="436e7-132">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="436e7-133">執行下列命令，以安全地儲存`App ID`和`App Secret`使用 Secret Manager:</span><span class="sxs-lookup"><span data-stu-id="436e7-133">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="436e7-134">設定 Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="436e7-134">Configure Facebook Authentication</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="436e7-135">將 Facebook 服務中的新增`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="436e7-135">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="436e7-136">安裝[Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook)封裝。</span><span class="sxs-lookup"><span data-stu-id="436e7-136">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="436e7-137">若要使用 Visual Studio 2017 安裝此套件，以滑鼠右鍵按一下專案，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="436e7-137">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="436e7-138">若要使用.NET Core CLI 安裝，請執行下列命令在您的專案目錄中：</span><span class="sxs-lookup"><span data-stu-id="436e7-138">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="436e7-139">新增在 Facebook 中的介軟體`Configure`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="436e7-139">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

<span data-ttu-id="436e7-140">請參閱[FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) Facebook 驗證所支援的組態選項的詳細資訊的 API 參考。</span><span class="sxs-lookup"><span data-stu-id="436e7-140">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="436e7-141">組態選項可以用來：</span><span class="sxs-lookup"><span data-stu-id="436e7-141">Configuration options can be used to:</span></span>

* <span data-ttu-id="436e7-142">要求不同使用者的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="436e7-142">Request different information about the user.</span></span>
* <span data-ttu-id="436e7-143">新增查詢字串引數來自訂登入體驗。</span><span class="sxs-lookup"><span data-stu-id="436e7-143">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="436e7-144">使用 Facebook 登入</span><span class="sxs-lookup"><span data-stu-id="436e7-144">Sign in with Facebook</span></span>

<span data-ttu-id="436e7-145">執行您的應用程式，然後按一下**登入**。</span><span class="sxs-lookup"><span data-stu-id="436e7-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="436e7-146">您會看到一個選項來使用 Facebook 登入。</span><span class="sxs-lookup"><span data-stu-id="436e7-146">You see an option to sign in with Facebook.</span></span>

![Web 應用程式：未經驗證的使用者](index/_static/DoneFacebook.png)

<span data-ttu-id="436e7-148">當您按一下**Facebook**，將您重新導向至 Facebook 進行驗證：</span><span class="sxs-lookup"><span data-stu-id="436e7-148">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Facebook 驗證 頁面](index/_static/FBLogin.png)

<span data-ttu-id="436e7-150">Facebook 驗證會要求預設公用設定檔和電子郵件地址：</span><span class="sxs-lookup"><span data-stu-id="436e7-150">Facebook authentication requests public profile and email address by default:</span></span>

![Facebook 驗證頁面同意畫面](index/_static/FBLoginDone.png)

<span data-ttu-id="436e7-152">一旦您輸入您的 Facebook 認證將您重新導向回您的網站，您可以在其中設定您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="436e7-152">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="436e7-153">您現在用來登入您的 Facebook 認證：</span><span class="sxs-lookup"><span data-stu-id="436e7-153">You are now logged in using your Facebook credentials:</span></span>

![Web 應用程式：已驗證的使用者](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="436e7-155">疑難排解</span><span class="sxs-lookup"><span data-stu-id="436e7-155">Troubleshooting</span></span>

* <span data-ttu-id="436e7-156">**ASP.NET Core 2.x 只有：** 如果身分識別未設定藉由呼叫`services.AddIdentity`中`ConfigureServices`，來驗證嘗試會導致*ArgumentException:必須提供 'SignInScheme' 選項*。</span><span class="sxs-lookup"><span data-stu-id="436e7-156">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="436e7-157">在本教學課程中使用的專案範本可確保，這麼做。</span><span class="sxs-lookup"><span data-stu-id="436e7-157">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="436e7-158">如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時資料庫作業失敗*時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="436e7-158">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="436e7-159">點選**套用移轉**建立資料庫，並重新整理 以忽略錯誤繼續執行。</span><span class="sxs-lookup"><span data-stu-id="436e7-159">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="436e7-160">後續步驟</span><span class="sxs-lookup"><span data-stu-id="436e7-160">Next steps</span></span>

* <span data-ttu-id="436e7-161">新增[Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet 封裝加入您的專案進行進階的 Facebook 驗證案例。</span><span class="sxs-lookup"><span data-stu-id="436e7-161">Add the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package to your project for advanced Facebook authentication scenarios.</span></span> <span data-ttu-id="436e7-162">此封裝不一定要將 Facebook 外部登入功能整合您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="436e7-162">This package isn't required to integrate Facebook external login functionality with your app.</span></span> 

* <span data-ttu-id="436e7-163">本文說明如何您可以使用 Facebook 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="436e7-163">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="436e7-164">您可以依照類似的方法，來驗證與列在其他提供者[上一頁](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="436e7-164">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="436e7-165">一旦您發行您的網站至 Azure web 應用程式時，您應該重設`AppSecret`Facebook 開發人員入口網站中。</span><span class="sxs-lookup"><span data-stu-id="436e7-165">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="436e7-166">設定`Authentication:Facebook:AppId`和`Authentication:Facebook:AppSecret`當做 Azure 入口網站中的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="436e7-166">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="436e7-167">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="436e7-167">The configuration system is set up to read keys from environment variables.</span></span>
