---
title: 使用 ASP.NET Core 的 twitter 外部登入設定
author: rick-anderson
description: 本教學課程示範如何整合到現有的 ASP.NET Core 應用程式的 Twitter 帳戶使用者驗證。
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/twitter-logins
ms.openlocfilehash: 43a5ea59d8853d297ae2c1ec3f4b1c0c14ec80c3
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708422"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a><span data-ttu-id="1f5bf-103">使用 ASP.NET Core 的 twitter 外部登入設定</span><span class="sxs-lookup"><span data-stu-id="1f5bf-103">Twitter external login setup with ASP.NET Core</span></span>

<span data-ttu-id="1f5bf-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1f5bf-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1f5bf-105">本教學課程會示範如何讓使用者[使用他們的 Twitter 帳戶登入](https://dev.twitter.com/web/sign-in/desktop-browser)上使用範例 ASP.NET Core 2.0 專案建立[上一頁](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="1f5bf-106">在 Twitter 中建立應用程式</span><span class="sxs-lookup"><span data-stu-id="1f5bf-106">Create the app in Twitter</span></span>

* <span data-ttu-id="1f5bf-107">瀏覽至[ https://apps.twitter.com/ ](https://apps.twitter.com/)並登入。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="1f5bf-108">如果您還沒有 Twitter 帳戶，使用**[歡迎現在註冊](https://twitter.com/signup)** 連結建立一個。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="1f5bf-109">登入之後，**應用程式管理**頁面會顯示：</span><span class="sxs-lookup"><span data-stu-id="1f5bf-109">After signing in, the **Application Management** page is shown:</span></span>

  ![在 Microsoft Edge 中開啟的 twitter 應用程式管理](index/_static/TwitterAppManage.png)

* <span data-ttu-id="1f5bf-111">點選**建立新的應用程式**並填妥應用程式**名稱**，**描述**和公用**網站**（這可以是暫時直到您的 URI註冊的網域名稱）：</span><span class="sxs-lookup"><span data-stu-id="1f5bf-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

  ![建立應用程式頁面](index/_static/TwitterCreate.png)

* <span data-ttu-id="1f5bf-113">輸入您的開發 URI 與`/signin-twitter`附加至**有效的 OAuth 重新導向 Uri**欄位 (例如： `https://localhost:44320/signin-twitter`)。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-113">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="1f5bf-114">本教學課程稍後設定 Twitter 驗證配置會自動處理在要求`/signin-twitter`實作 OAuth 流程的路由。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="1f5bf-115">URI 區段`/signin-twitter`設為 Twitter 驗證提供者的預設回呼。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-115">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="1f5bf-116">您可以變更預設的回呼 URI 時設定 Twitter 驗證中的介軟體，透過繼承[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性[TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions)類別。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-116">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="1f5bf-117">填寫表單的其餘部分，然後點選**建立 Twitter 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-117">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="1f5bf-118">會顯示新的應用程式詳細資料：</span><span class="sxs-lookup"><span data-stu-id="1f5bf-118">New application details are displayed:</span></span>

  ![應用程式頁面的詳細資料 索引標籤](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="1f5bf-120">部署站台時您將必須再次瀏覽**應用程式管理**頁面上，並註冊新的公用 URI。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-120">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="1f5bf-121">儲存 Twitter ConsumerKey 和 ConsumerSecret</span><span class="sxs-lookup"><span data-stu-id="1f5bf-121">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="1f5bf-122">連結機密設定，例如 Twitter`Consumer Key`並`Consumer Secret`至您的應用程式的組態使用[Secret Manager](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-122">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="1f5bf-123">基於本教學課程的目的，名稱語彙基元`Authentication:Twitter:ConsumerKey`和`Authentication:Twitter:ConsumerSecret`。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-123">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="1f5bf-124">這些權杖可於**金鑰和存取權杖**之後建立新的 Twitter 應用程式 索引標籤：</span><span class="sxs-lookup"><span data-stu-id="1f5bf-124">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![金鑰和存取權杖 索引標籤](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="1f5bf-126">設定 Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="1f5bf-126">Configure Twitter Authentication</span></span>

<span data-ttu-id="1f5bf-127">在本教學課程中使用的專案範本可確保[Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter)已經安裝套件。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-127">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="1f5bf-128">若要使用 Visual Studio 2017 安裝此套件，以滑鼠右鍵按一下專案，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-128">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="1f5bf-129">若要使用.NET Core CLI 安裝，請執行下列命令在您的專案目錄中：</span><span class="sxs-lookup"><span data-stu-id="1f5bf-129">To install with .NET Core CLI, execute the following in your project directory:</span></span>

  `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1f5bf-130">將 Twitter 服務中的新增`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="1f5bf-130">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1f5bf-131">新增在 Twitter 中的介軟體`Configure`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="1f5bf-131">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

::: moniker-end

<span data-ttu-id="1f5bf-132">請參閱[TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API 參考，如 Twitter 驗證所支援的組態選項的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-132">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="1f5bf-133">這可用來要求不同使用者的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="1f5bf-134">使用 Twitter 登入</span><span class="sxs-lookup"><span data-stu-id="1f5bf-134">Sign in with Twitter</span></span>

<span data-ttu-id="1f5bf-135">執行您的應用程式，然後按一下**登入**。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-135">Run your application and click **Log in**.</span></span> <span data-ttu-id="1f5bf-136">此時會出現以 Twitter 登入選項：</span><span class="sxs-lookup"><span data-stu-id="1f5bf-136">An option to sign in with Twitter appears:</span></span>

![Web 應用程式： 未驗證的使用者](index/_static/DoneTwitter.png)

<span data-ttu-id="1f5bf-138">按一下**Twitter**將重新導向至 Twitter 進行驗證：</span><span class="sxs-lookup"><span data-stu-id="1f5bf-138">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Twitter 驗證頁面](index/_static/TwitterLogin.png)

<span data-ttu-id="1f5bf-140">輸入您的 Twitter 認證之後, 您將被重新導向回到網站，您可以在其中設定您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-140">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="1f5bf-141">您現在用來登入您的 Twitter 認證：</span><span class="sxs-lookup"><span data-stu-id="1f5bf-141">You are now logged in using your Twitter credentials:</span></span>

![Web 應用程式： 驗證使用者](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="1f5bf-143">疑難排解</span><span class="sxs-lookup"><span data-stu-id="1f5bf-143">Troubleshooting</span></span>

* <span data-ttu-id="1f5bf-144">**ASP.NET Core 2.x 僅：** 如果身分識別未設定藉由呼叫`services.AddIdentity`中`ConfigureServices`，來驗證嘗試會導致*ArgumentException： 必須提供 'SignInScheme' 選項*。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-144">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="1f5bf-145">在本教學課程中使用的專案範本可確保，這麼做。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-145">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="1f5bf-146">如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時資料庫作業失敗*時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="1f5bf-147">點選**套用移轉**建立資料庫，並重新整理 以忽略錯誤繼續執行。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1f5bf-148">後續步驟</span><span class="sxs-lookup"><span data-stu-id="1f5bf-148">Next steps</span></span>

* <span data-ttu-id="1f5bf-149">本文說明如何您可以使用 Twitter 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-149">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="1f5bf-150">您可以依照類似的方法，來驗證與列在其他提供者[上一頁](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="1f5bf-151">一旦您發行您的網站至 Azure web 應用程式時，您應該重設`ConsumerSecret`Twitter 開發人員入口網站中。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-151">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="1f5bf-152">設定`Authentication:Twitter:ConsumerKey`和`Authentication:Twitter:ConsumerSecret`當做 Azure 入口網站中的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-152">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="1f5bf-153">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="1f5bf-153">The configuration system is set up to read keys from environment variables.</span></span>
