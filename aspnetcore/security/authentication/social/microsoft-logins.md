---
title: "外部登入的 Microsoft 帳戶設定"
author: rick-anderson
description: "本教學課程示範的整合到現有的 ASP.NET Core 應用程式的 Microsoft 帳戶使用者驗證。"
keywords: "ASP.NET Core 的 Microsoft 帳戶、 登入、 驗證"
ms.author: riande
manager: wpickett
ms.date: 08/24/2017
ms.topic: article
ms.assetid: 66DB4B94-C78C-4005-BA03-3D982B87C268
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 77c16e3ae93c9bfe1f569d0a5888c5b765d04241
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="configuring-microsoft-account-authentication"></a><span data-ttu-id="2bb6e-104">設定 Microsoft 帳戶驗證</span><span class="sxs-lookup"><span data-stu-id="2bb6e-104">Configuring Microsoft Account authentication</span></span>

<span data-ttu-id="2bb6e-105">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2bb6e-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2bb6e-106">本教學課程會示範如何讓使用者使用範例 ASP.NET Core 2.0 專案上建立其 Microsoft 帳戶登入[上一頁](index.md)。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-106">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="2bb6e-107">在 Microsoft 開發人員入口網站中建立應用程式</span><span class="sxs-lookup"><span data-stu-id="2bb6e-107">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="2bb6e-108">瀏覽至[https://apps.dev.microsoft.com](https://apps.dev.microsoft.com)以及建立或登入 Microsoft 帳戶：</span><span class="sxs-lookup"><span data-stu-id="2bb6e-108">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![登入對話方塊](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="2bb6e-110">如果您還沒有 Microsoft 帳戶，請點選**[建立一個 ！](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="2bb6e-110">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="2bb6e-111">在登入後將您重新導向**我的應用程式**頁面：</span><span class="sxs-lookup"><span data-stu-id="2bb6e-111">After signing in you are redirected to **My applications** page:</span></span>

![在 Microsoft Edge 中開啟的 Microsoft 開發人員入口網站](index/_static/MicrosoftDev.png)

* <span data-ttu-id="2bb6e-113">點選**新增應用程式**在右上角，然後輸入您**應用程式名稱**和**Contact Email**:</span><span class="sxs-lookup"><span data-stu-id="2bb6e-113">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![新的應用程式註冊 對話方塊](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="2bb6e-115">基於本教學課程的目的，清除**指引安裝**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-115">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="2bb6e-116">點選**建立**繼續**註冊**頁面。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-116">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="2bb6e-117">提供**名稱**並記下的值**應用程式識別碼**，作為使用`ClientId`稍後在本教學課程：</span><span class="sxs-lookup"><span data-stu-id="2bb6e-117">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![註冊頁面](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="2bb6e-119">點選**加入平台**中**平台**區段，然後選取**Web**平台：</span><span class="sxs-lookup"><span data-stu-id="2bb6e-119">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![加入平台 對話方塊](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="2bb6e-121">在新**Web**平台區段中，輸入您開發的 URL 與*/signin-microsoft*附加到**重新導向 Url**欄位 (例如： `https://localhost:44320/signin-microsoft`)。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-121">In the new **Web** platform section, enter your development URL with */signin-microsoft* appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="2bb6e-122">稍後在本教學課程中設定的 Microsoft 驗證配置將會自動處理在要求*/signin-microsoft*實作 OAuth 流程路由：</span><span class="sxs-lookup"><span data-stu-id="2bb6e-122">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at */signin-microsoft* route to implement the OAuth flow:</span></span>

![Web 平台 > 一節](index/_static/MicrosoftRedirectUri.png)

* <span data-ttu-id="2bb6e-124">點選**新增 URL**確定 URL 已新增。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-124">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="2bb6e-125">填寫任何其他應用程式設定，如有必要，點選 **儲存**將變更儲存到應用程式組態頁面底部。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-125">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="2bb6e-126">部署站台時必須再次瀏覽**註冊**頁面，然後設定新的公用 URL。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-126">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="2bb6e-127">儲存 Microsoft 應用程式識別碼和密碼</span><span class="sxs-lookup"><span data-stu-id="2bb6e-127">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="2bb6e-128">請注意`Application Id`上顯示**註冊**頁面。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-128">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="2bb6e-129">點選**產生新的密碼**中**應用程式密碼**> 一節。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-129">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="2bb6e-130">這會顯示的方塊，您可以在其中複製應用程式密碼：</span><span class="sxs-lookup"><span data-stu-id="2bb6e-130">This displays a box where you can copy the application password:</span></span>

![新產生的密碼對話方塊](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="2bb6e-132">連結機密設定，例如 Microsoft`Application ID`和`Password`您應用程式組態使用[密碼管理員](../../app-secrets.md)。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-132">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="2bb6e-133">基於本教學課程的目的，名稱語彙基元`Authentication:Microsoft:ApplicationId`和`Authentication:Microsoft:Password`。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-133">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="2bb6e-134">設定 Microsoft 帳戶驗證</span><span class="sxs-lookup"><span data-stu-id="2bb6e-134">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="2bb6e-135">在本教學課程中使用的專案範本，可確保[Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount)封裝是否已經安裝。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-135">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="2bb6e-136">若要安裝此套件與 Visual Studio 2017，以滑鼠右鍵按一下專案，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-136">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="2bb6e-137">若要安裝的.NET Core CLI，請在您的專案目錄中執行下列：</span><span class="sxs-lookup"><span data-stu-id="2bb6e-137">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2bb6e-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2bb6e-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2bb6e-139">新增 Microsoft 帳戶服務中的`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="2bb6e-139">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2bb6e-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2bb6e-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2bb6e-141">新增 Microsoft 帳戶中的介軟體中`Configure`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="2bb6e-141">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

<span data-ttu-id="2bb6e-142">雖然 Microsoft 開發人員入口網站上所使用的術語命名這些語彙基元`ApplicationId`和`Password`，它們會公開為`ClientId`和`ClientSecret`組態 API。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-142">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they are exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="2bb6e-143">請參閱[MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API 參考，如需有關 Microsoft 帳戶驗證所支援的組態選項。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-143">See the [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="2bb6e-144">這可以用於要求的使用者不同的資訊。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-144">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="2bb6e-145">使用 Microsoft 帳戶登入</span><span class="sxs-lookup"><span data-stu-id="2bb6e-145">Sign in with Microsoft Account</span></span>

<span data-ttu-id="2bb6e-146">執行您的應用程式，然後按一下**登入**。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-146">Run your application and click **Log in**.</span></span> <span data-ttu-id="2bb6e-147">使用 Microsoft 登入選項會出現：</span><span class="sxs-lookup"><span data-stu-id="2bb6e-147">An option to sign in with Microsoft appears:</span></span>

![Web 應用程式記錄檔中頁面： 未驗證的使用者](index/_static/DoneMicrosoft.png)

<span data-ttu-id="2bb6e-149">當您按一下 on Microsoft 時，您會重新導向到 Microsoft 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-149">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="2bb6e-150">之後您 Microsoft 帳戶登入 （如果尚未登入） 系統會提示您讓應用程式存取您的資訊：</span><span class="sxs-lookup"><span data-stu-id="2bb6e-150">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Microsoft 驗證對話方塊](index/_static/MicrosoftLogin.png)

<span data-ttu-id="2bb6e-152">點選**是**您就會重新導向回 web 站台，您可以設定您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-152">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="2bb6e-153">您現在會記錄在使用您的 Microsoft 認證：</span><span class="sxs-lookup"><span data-stu-id="2bb6e-153">You are now logged in using your Microsoft credentials:</span></span>

![Web 應用程式： 已驗證的使用者](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="2bb6e-155">疑難排解</span><span class="sxs-lookup"><span data-stu-id="2bb6e-155">Troubleshooting</span></span>

* <span data-ttu-id="2bb6e-156">如果 Microsoft 帳戶提供者將您重新導向至登入錯誤頁面，請注意錯誤標題和描述查詢字串參數緊接`#`（雜湊標記） 之 Uri 中。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-156">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="2bb6e-157">雖然指出 Microsoft 驗證問題，看起來的錯誤訊息，但最常見的原因是您的應用程式 Uri 不符合任何的**重新導向 Uri**指定**Web**平台.</span><span class="sxs-lookup"><span data-stu-id="2bb6e-157">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="2bb6e-158">**ASP.NET Core 2.x 僅：**如果身分識別未設定藉由呼叫`services.AddIdentity`中`ConfigureServices`，嘗試驗證將會導致*ArgumentException： 必須提供 'SignInScheme' 選項*。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-158">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="2bb6e-159">在本教學課程中使用的專案範本可確保，這已完成。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-159">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="2bb6e-160">如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時，資料庫作業失敗*錯誤。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-160">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="2bb6e-161">點選**套用移轉**來建立資料庫，並重新整理 以忽略錯誤繼續執行。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-161">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2bb6e-162">後續步驟</span><span class="sxs-lookup"><span data-stu-id="2bb6e-162">Next steps</span></span>

* <span data-ttu-id="2bb6e-163">這篇文章會示範您可以向 Microsoft。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-163">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="2bb6e-164">您可以依照類似的方法，來向其他提供者列在[上一頁](index.md)。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-164">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="2bb6e-165">一旦您將您的網站發行至 Azure web 應用程式時，您應該建立新`Password`Microsoft 開發人員入口網站中。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-165">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="2bb6e-166">設定`Authentication:Microsoft:ApplicationId`和`Authentication:Microsoft:Password`為在 Azure 入口網站應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-166">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="2bb6e-167">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="2bb6e-167">The configuration system is set up to read keys from environment variables.</span></span>
