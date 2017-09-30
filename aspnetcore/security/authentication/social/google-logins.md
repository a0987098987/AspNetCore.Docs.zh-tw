---
title: "在 ASP.NET Core Google 外部登入安裝程式"
author: rick-anderson
description: "在 ASP.NET Core Google 外部登入安裝程式"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/google-logins
ms.openlocfilehash: 8723a74250ff1b0a63139057bfc17fdd31dd169e
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2017
---
# <a name="configuring-google-authentication-in-aspnet-core"></a><span data-ttu-id="f8c0f-104">在 ASP.NET Core 中設定 Google 驗證</span><span class="sxs-lookup"><span data-stu-id="f8c0f-104">Configuring Google authentication in ASP.NET Core</span></span>

<a name=security-authentication-google-logins></a>

<span data-ttu-id="f8c0f-105">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f8c0f-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f8c0f-106">本教學課程會示範如何讓使用者使用 Google + 帳戶使用範例 ASP.NET Core 2.0 專案上建立登入[上一頁](index.md)。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-106">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span> <span data-ttu-id="f8c0f-107">遵循我們啟動[官方步驟](https://developers.google.com/identity/sign-in/web/devconsole-project)在 Google API 主控台中建立新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-107">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="f8c0f-108">在 Google API 主控台中建立應用程式</span><span class="sxs-lookup"><span data-stu-id="f8c0f-108">Create the app in Google API Console</span></span>

* <span data-ttu-id="f8c0f-109">瀏覽至[https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library)並登入。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-109">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="f8c0f-110">如果您還沒有 Google 帳戶，使用**更多選項** > **[建立帳戶](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**建立一個連結：</span><span class="sxs-lookup"><span data-stu-id="f8c0f-110">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Google API 主控台](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="f8c0f-112">將您重新導向**API Manager 程式庫**頁面：</span><span class="sxs-lookup"><span data-stu-id="f8c0f-112">You are redirected to **API Manager Library** page:</span></span>

![管理員 API 程式庫 頁面](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="f8c0f-114">點選**建立**並輸入您**專案名稱**:</span><span class="sxs-lookup"><span data-stu-id="f8c0f-114">Tap **Create** and enter your **Project name**:</span></span>

![[新增專案] 對話](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="f8c0f-116">之後接受對話方塊，您會重新導向至 [程式庫] 頁面可讓您選擇新的應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-116">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="f8c0f-117">尋找**Google + API**清單中，按一下 新增 API 功能的連結：</span><span class="sxs-lookup"><span data-stu-id="f8c0f-117">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

![管理員 API 程式庫 頁面](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="f8c0f-119">新加入的 API 頁面會隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-119">The page for the newly added API is displayed.</span></span> <span data-ttu-id="f8c0f-120">點選**啟用**Google + 號功能中加入您的應用程式：</span><span class="sxs-lookup"><span data-stu-id="f8c0f-120">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![API Manager Google + API 頁面](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="f8c0f-122">在之後啟用 API，點選**建立認證**設定密碼：</span><span class="sxs-lookup"><span data-stu-id="f8c0f-122">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![API Manager Google + API 頁面](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="f8c0f-124">選擇：</span><span class="sxs-lookup"><span data-stu-id="f8c0f-124">Choose:</span></span>
   * <span data-ttu-id="f8c0f-125">**Google + 應用程式開發介面**</span><span class="sxs-lookup"><span data-stu-id="f8c0f-125">**Google+ API**</span></span>
   * <span data-ttu-id="f8c0f-126">**網頁伺服器 (例如 node.js Tomcat)**，和</span><span class="sxs-lookup"><span data-stu-id="f8c0f-126">**Web server (e.g. node.js, Tomcat)**, and</span></span>
   * <span data-ttu-id="f8c0f-127">**使用者資料**:</span><span class="sxs-lookup"><span data-stu-id="f8c0f-127">**User data**:</span></span>

![API 管理員認證 頁面中： 了解什麼種類的認證需要面板](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="f8c0f-129">點選**我需要哪些認證？**這會引領您的應用程式設定中，第二步**建立 OAuth 2.0 用戶端識別碼**:</span><span class="sxs-lookup"><span data-stu-id="f8c0f-129">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![API 管理員認證 頁面上： 建立 OAuth 2.0 用戶端識別碼](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="f8c0f-131">因為我們建立 Google + 專案的一個功能 （登入），我們可以輸入相同**名稱**OAuth 2.0 用戶端識別碼，我們使用的專案。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-131">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="f8c0f-132">輸入您的開發 URI 與*/signin-google*附加到**授權重新導向 Uri**欄位 (例如： `https://localhost:44320/signin-google`)。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-132">Enter your development URI with */signin-google* appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="f8c0f-133">稍後在本教學課程設定 Google 驗證將會自動處理在要求*/signin-google*實作 OAuth 流程的路由。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-133">The Google authentication configured later in this tutorial will automatically handle requests at */signin-google* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="f8c0f-134">按 TAB 鍵以新增**授權重新導向 Uri**項目。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-134">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="f8c0f-135">點選**建立用戶端識別碼**，這樣會帶您到第三個步驟中，**設定 OAuth 2.0 同意畫面**:</span><span class="sxs-lookup"><span data-stu-id="f8c0f-135">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![API 管理員認證 頁面上： 設定 OAuth 2.0 同意畫面](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="f8c0f-137">輸入您的對外**電子郵件地址**和**產品名稱**時提示使用者登入 Google + 顯示您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-137">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="f8c0f-138">底下的其他選項可用**更多的自訂選項**。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-138">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="f8c0f-139">點選**繼續**繼續進行到最後一個步驟中，**下載認證**:</span><span class="sxs-lookup"><span data-stu-id="f8c0f-139">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![API 管理員認證 頁面上： 下載認證](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="f8c0f-141">點選**下載**儲存 JSON 檔案，應用程式密碼和**完成**，完成建立新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-141">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="f8c0f-142">部署站台時必須再次瀏覽**Google 主控台**並註冊新的公用 url。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-142">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="f8c0f-143">存放區 Google ClientID 和 ClientSecret</span><span class="sxs-lookup"><span data-stu-id="f8c0f-143">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="f8c0f-144">連結機密設定，例如 Google`Client ID`和`Client Secret`您應用程式組態使用[密碼管理員](../../app-secrets.md)。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-144">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="f8c0f-145">基於本教學課程的目的，名稱語彙基元`Authentication:Google:ClientId`和`Authentication:Google:ClientSecret`。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-145">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="f8c0f-146">這些語彙基元的值可以在下一個步驟中下載 JSON 檔案中找到`web.client_id`和`web.client_secret`。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-146">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="f8c0f-147">設定 Google 驗證</span><span class="sxs-lookup"><span data-stu-id="f8c0f-147">Configure Google Authentication</span></span>

<span data-ttu-id="f8c0f-148">在本教學課程中使用的專案範本，可確保[Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google)安裝封裝。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-148">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

 * <span data-ttu-id="f8c0f-149">若要安裝此套件與 Visual Studio 2017，以滑鼠右鍵按一下專案，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-149">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
 * <span data-ttu-id="f8c0f-150">若要安裝的.NET Core CLI，請在您的專案目錄中執行下列：</span><span class="sxs-lookup"><span data-stu-id="f8c0f-150">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Google`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f8c0f-151">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f8c0f-151">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f8c0f-152">加入在 Google 服務`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="f8c0f-152">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f8c0f-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f8c0f-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f8c0f-154">新增 Google 中的介軟體中`Configure`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="f8c0f-154">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

<span data-ttu-id="f8c0f-155">請參閱[GoogleOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.googleoptions) API 參考，如需 Google 驗證所支援的組態選項的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-155">See the [GoogleOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="f8c0f-156">這可以用於要求的使用者不同的資訊。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-156">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="f8c0f-157">使用 Google 登入</span><span class="sxs-lookup"><span data-stu-id="f8c0f-157">Sign in with Google</span></span>

<span data-ttu-id="f8c0f-158">執行您的應用程式，然後按一下**登入**。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-158">Run your application and click **Log in**.</span></span> <span data-ttu-id="f8c0f-159">使用 Google 登入選項會出現：</span><span class="sxs-lookup"><span data-stu-id="f8c0f-159">An option to sign in with Google appears:</span></span>

![在 Microsoft Edge 中執行的 web 應用程式： 未驗證的使用者](index/_static/DoneGoogle.png)

<span data-ttu-id="f8c0f-161">當您按一下 Google 時，您會重新導向至 Google 驗證：</span><span class="sxs-lookup"><span data-stu-id="f8c0f-161">When you click on Google, you are redirected to Google for authentication:</span></span>

![Google 驗證對話方塊](index/_static/GoogleLogin.png)

<span data-ttu-id="f8c0f-163">輸入您的 Google 認證之後, 再將您重新導向回 web 站台，您可以設定您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-163">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="f8c0f-164">您現在會記錄在使用您的 Google 認證：</span><span class="sxs-lookup"><span data-stu-id="f8c0f-164">You are now logged in using your Google credentials:</span></span>

![在 Microsoft Edge 中執行的 web 應用程式： 已驗證的使用者](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="f8c0f-166">疑難排解</span><span class="sxs-lookup"><span data-stu-id="f8c0f-166">Troubleshooting</span></span>

* <span data-ttu-id="f8c0f-167">如果您收到`403 (Forbidden)`錯誤頁面，從您自己的應用程式時執行，在開發模式 （或相同的錯誤與偵錯工具中斷），請確認**Google + API**中已啟用**API Manager 程式庫**遵循所列的步驟[更早版本在此頁面](#create-the-app-in-google-api-console)。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-167">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="f8c0f-168">如果登入無法運作，且您未收到任何錯誤訊息，切換到開發模式來進行問題偵錯更容易。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-168">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="f8c0f-169">**ASP.NET Core 2.x 僅：**如果身分識別未設定藉由呼叫`services.AddIdentity`中`ConfigureServices`，嘗試驗證將會導致*ArgumentException： 必須提供 'SignInScheme' 選項*。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-169">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="f8c0f-170">在本教學課程中使用的專案範本可確保，這已完成。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-170">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="f8c0f-171">如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時，資料庫作業失敗*錯誤。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-171">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="f8c0f-172">點選**套用移轉**來建立資料庫，並重新整理 以忽略錯誤繼續執行。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-172">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f8c0f-173">後續步驟</span><span class="sxs-lookup"><span data-stu-id="f8c0f-173">Next steps</span></span>

* <span data-ttu-id="f8c0f-174">這篇文章會示範您可以向 Google。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-174">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="f8c0f-175">您可以依照類似的方法，來向其他提供者列在[上一頁](index.md)。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-175">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="f8c0f-176">一旦您將您的網站發行至 Azure web 應用程式時，您應該重設`ClientSecret`Google API 主控台中。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-176">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="f8c0f-177">設定`Authentication:Google:ClientId`和`Authentication:Google:ClientSecret`為在 Azure 入口網站應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-177">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="f8c0f-178">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="f8c0f-178">The configuration system is set up to read keys from environment variables.</span></span>
