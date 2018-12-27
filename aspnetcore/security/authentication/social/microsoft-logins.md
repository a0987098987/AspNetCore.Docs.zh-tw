---
title: 使用 ASP.NET Core 的 Microsoft 帳戶外部登入設定
author: rick-anderson
description: 本教學課程示範如何整合到現有的 ASP.NET Core 應用程式的 Microsoft 帳戶使用者驗證。
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 4909a0084994654777ad7a6ebda866ac727f0528
ms.sourcegitcommit: 3e94d192b2ed9409fe72e3735e158b333354964c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2018
ms.locfileid: "53735748"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="00537-103">使用 ASP.NET Core 的 Microsoft 帳戶外部登入設定</span><span class="sxs-lookup"><span data-stu-id="00537-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="00537-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="00537-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="00537-105">本教學課程會示範如何讓使用者使用其使用範例 ASP.NET Core 2.0 專案上建立的 Microsoft 帳戶登入[上一頁](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="00537-105">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="00537-106">在 Microsoft 開發人員入口網站中建立應用程式</span><span class="sxs-lookup"><span data-stu-id="00537-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="00537-107">瀏覽至[ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com)和建立，或登入 Microsoft 帳戶：</span><span class="sxs-lookup"><span data-stu-id="00537-107">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![登入對話方塊](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="00537-109">如果您還沒有 Microsoft 帳戶，請點選 **[建立一個 ！](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="00537-109">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="00537-110">登入之後將您重新導向 **我的應用程式** 頁面：</span><span class="sxs-lookup"><span data-stu-id="00537-110">After signing in you are redirected to **My applications** page:</span></span>

![開啟 Microsoft Edge 中的 Microsoft 開發人員入口網站](index/_static/MicrosoftDev.png)

* <span data-ttu-id="00537-112">點選**新增應用程式**右上角，然後輸入您**應用程式名稱**並**Contact Email**:</span><span class="sxs-lookup"><span data-stu-id="00537-112">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![新的應用程式註冊 對話方塊](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="00537-114">基於本教學課程的目的，請清除**引導式設定**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="00537-114">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="00537-115">點選**Create**繼續**註冊**頁面。</span><span class="sxs-lookup"><span data-stu-id="00537-115">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="00537-116">提供**名稱**，並記下的值**應用程式識別碼**，使用即`ClientId`稍後的教學課程：</span><span class="sxs-lookup"><span data-stu-id="00537-116">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![註冊頁面](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="00537-118">點選**新增平台**中**平台**區段，然後選取**Web**平台：</span><span class="sxs-lookup"><span data-stu-id="00537-118">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![新增平台 對話方塊](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="00537-120">在新**Web**平台區段中，輸入與您開發的 URL`/signin-microsoft`附加到**重新導向 Url**欄位 (例如： `https://localhost:44320/signin-microsoft`)。</span><span class="sxs-lookup"><span data-stu-id="00537-120">In the new **Web** platform section, enter your development URL with `/signin-microsoft` appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="00537-121">稍後在本教學課程中設定的 Microsoft 驗證配置會自動處理在要求`/signin-microsoft`實作 OAuth 流程的路由：</span><span class="sxs-lookup"><span data-stu-id="00537-121">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow:</span></span>

![Web 平台 」 一節](index/_static/MicrosoftRedirectUri.png)

> [!NOTE]
> <span data-ttu-id="00537-123">URI 區段`/signin-microsoft`設為預設回呼的 Microsoft 驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="00537-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="00537-124">您可以變更預設的回呼 URI 時設定 Microsoft 驗證中的介軟體，透過繼承[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性[MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions)類別。</span><span class="sxs-lookup"><span data-stu-id="00537-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

* <span data-ttu-id="00537-125">點選**加入 URL**確定 URL 已新增。</span><span class="sxs-lookup"><span data-stu-id="00537-125">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="00537-126">填寫任何其他應用程式設定，如有必要，點選**儲存**底部的 [將變更儲存至應用程式設定] 頁面。</span><span class="sxs-lookup"><span data-stu-id="00537-126">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="00537-127">部署站台時您將必須再次瀏覽**註冊**頁面，然後將新的公用 URL。</span><span class="sxs-lookup"><span data-stu-id="00537-127">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="00537-128">儲存 Microsoft 應用程式識別碼和密碼</span><span class="sxs-lookup"><span data-stu-id="00537-128">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="00537-129">附註`Application Id`上顯示**註冊**頁面。</span><span class="sxs-lookup"><span data-stu-id="00537-129">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="00537-130">點選**產生新密碼**中**應用程式祕密**一節。</span><span class="sxs-lookup"><span data-stu-id="00537-130">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="00537-131">這會顯示的方塊，您可以在其中複製應用程式密碼：</span><span class="sxs-lookup"><span data-stu-id="00537-131">This displays a box where you can copy the application password:</span></span>

![新產生的密碼 對話方塊](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="00537-133">連結機密設定，例如 Microsoft`Application ID`並`Password`至您的應用程式的組態使用[Secret Manager](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="00537-133">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="00537-134">基於本教學課程的目的，名稱語彙基元`Authentication:Microsoft:ApplicationId`和`Authentication:Microsoft:Password`。</span><span class="sxs-lookup"><span data-stu-id="00537-134">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="00537-135">設定 Microsoft 帳戶驗證</span><span class="sxs-lookup"><span data-stu-id="00537-135">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="00537-136">在本教學課程中使用的專案範本可確保[Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount)已經安裝套件。</span><span class="sxs-lookup"><span data-stu-id="00537-136">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="00537-137">若要使用 Visual Studio 2017 安裝此套件，以滑鼠右鍵按一下專案，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="00537-137">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="00537-138">若要使用.NET Core CLI 安裝，請執行下列命令在您的專案目錄中：</span><span class="sxs-lookup"><span data-stu-id="00537-138">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="00537-139">新增 Microsoft 帳戶服務中的`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="00537-139">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="00537-140">新增在 Microsoft 帳戶中的介軟體`Configure`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="00537-140">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

::: moniker-end

<span data-ttu-id="00537-141">雖然 Microsoft 開發人員入口網站所使用的術語命名這些語彙基元`ApplicationId`並`Password`，它們會公開為`ClientId`和`ClientSecret`組態 API。</span><span class="sxs-lookup"><span data-stu-id="00537-141">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they're exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="00537-142">請參閱[MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API 參考，如需有關 Microsoft 帳戶驗證所支援的組態選項。</span><span class="sxs-lookup"><span data-stu-id="00537-142">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="00537-143">這可用來要求不同使用者的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="00537-143">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="00537-144">使用 Microsoft 帳戶登入</span><span class="sxs-lookup"><span data-stu-id="00537-144">Sign in with Microsoft Account</span></span>

<span data-ttu-id="00537-145">執行您的應用程式，然後按一下**登入**。</span><span class="sxs-lookup"><span data-stu-id="00537-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="00537-146">此時會出現 使用 Microsoft 登入選項：</span><span class="sxs-lookup"><span data-stu-id="00537-146">An option to sign in with Microsoft appears:</span></span>

![Web 應用程式記錄檔 頁面中：未經驗證的使用者](index/_static/DoneMicrosoft.png)

<span data-ttu-id="00537-148">當您按一下 Microsoft 時，您會重新導向到 Microsoft 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="00537-148">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="00537-149">之後您 Microsoft 帳戶登入 （如果您尚未登入） 將會提示您讓應用程式存取您的資訊：</span><span class="sxs-lookup"><span data-stu-id="00537-149">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Microsoft 驗證對話方塊](index/_static/MicrosoftLogin.png)

<span data-ttu-id="00537-151">點選**是**您就會重新導向回 web 站台，您可以在其中設定您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="00537-151">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="00537-152">您現在用來登入您的 Microsoft 認證：</span><span class="sxs-lookup"><span data-stu-id="00537-152">You are now logged in using your Microsoft credentials:</span></span>

![Web 應用程式：已驗證的使用者](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="00537-154">疑難排解</span><span class="sxs-lookup"><span data-stu-id="00537-154">Troubleshooting</span></span>

* <span data-ttu-id="00537-155">如果 Microsoft 帳戶提供者會將您導向登入錯誤頁面，請注意錯誤標題和描述查詢字串參數直接跟`#`（主題標籤） 中的 Uri。</span><span class="sxs-lookup"><span data-stu-id="00537-155">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="00537-156">雖然指出使用 Microsoft 驗證問題似乎出現錯誤訊息，最常見的原因是您的應用程式 Uri 不符合任一**重新導向 Uri**指定**Web**平台.</span><span class="sxs-lookup"><span data-stu-id="00537-156">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="00537-157">**ASP.NET Core 2.x 只有：** 如果身分識別未設定藉由呼叫`services.AddIdentity`中`ConfigureServices`，來驗證嘗試會導致*ArgumentException:必須提供 'SignInScheme' 選項*。</span><span class="sxs-lookup"><span data-stu-id="00537-157">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="00537-158">在本教學課程中使用的專案範本可確保，這麼做。</span><span class="sxs-lookup"><span data-stu-id="00537-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="00537-159">如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時資料庫作業失敗*時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="00537-159">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="00537-160">點選**套用移轉**建立資料庫，並重新整理 以忽略錯誤繼續執行。</span><span class="sxs-lookup"><span data-stu-id="00537-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="00537-161">後續步驟</span><span class="sxs-lookup"><span data-stu-id="00537-161">Next steps</span></span>

* <span data-ttu-id="00537-162">本文章說明您可以向 Microsoft。</span><span class="sxs-lookup"><span data-stu-id="00537-162">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="00537-163">您可以依照類似的方法，來驗證與列在其他提供者[上一頁](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="00537-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="00537-164">一旦您發行您的網站至 Azure web 應用程式時，您應該建立新`Password`Microsoft 開發人員入口網站中。</span><span class="sxs-lookup"><span data-stu-id="00537-164">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="00537-165">設定`Authentication:Microsoft:ApplicationId`和`Authentication:Microsoft:Password`當做 Azure 入口網站中的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="00537-165">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="00537-166">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="00537-166">The configuration system is set up to read keys from environment variables.</span></span>
