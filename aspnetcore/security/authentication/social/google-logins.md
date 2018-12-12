---
title: ASP.NET Core 中的 Google 外部登入設定
author: rick-anderson
description: 本教學課程示範如何整合到現有的 ASP.NET Core 應用程式的 Google 帳戶使用者驗證。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/google-logins
ms.openlocfilehash: 372504eb4f6fea412b5b160e0d5e9251dafe0d56
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284483"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="38521-103">ASP.NET Core 中的 Google 外部登入設定</span><span class="sxs-lookup"><span data-stu-id="38521-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="38521-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="38521-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="38521-105">本教學課程會示範如何讓使用者使用 Google + 帳戶使用範例 ASP.NET Core 2.0 專案上建立登入[上一頁](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="38521-105">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="38521-106">我們首先請遵循[官方步驟](https://developers.google.com/identity/sign-in/web/devconsole-project)在 Google API 主控台中建立新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="38521-106">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="38521-107">在 Google API 主控台中建立應用程式</span><span class="sxs-lookup"><span data-stu-id="38521-107">Create the app in Google API Console</span></span>

* <span data-ttu-id="38521-108">瀏覽至[ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library)並登入。</span><span class="sxs-lookup"><span data-stu-id="38521-108">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="38521-109">如果您還沒有 Google 帳戶，使用**更多選項** > **[建立帳戶](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** 連結建立一個：</span><span class="sxs-lookup"><span data-stu-id="38521-109">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Google API 主控台](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="38521-111">將您重新導向**API 管理員程式庫**頁面：</span><span class="sxs-lookup"><span data-stu-id="38521-111">You are redirected to **API Manager Library** page:</span></span>

![在 [API Manager 程式庫] 頁面上的平台](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="38521-113">點選**Create**並輸入您**專案名稱**:</span><span class="sxs-lookup"><span data-stu-id="38521-113">Tap **Create** and enter your **Project name**:</span></span>

![[新增專案] 對話](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="38521-115">接受之後的對話方塊，將您重新導向回媒體櫃 頁面，讓您可以選擇新的應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="38521-115">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="38521-116">尋找**Google + API**清單中，按一下 新增 API 功能的連結：</span><span class="sxs-lookup"><span data-stu-id="38521-116">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

![在 [API Manager 程式庫] 頁面中搜尋 [Google + API]](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="38521-118">新加入的 API 頁面會隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="38521-118">The page for the newly added API is displayed.</span></span> <span data-ttu-id="38521-119">點選**啟用**將 Google + 號功能新增至您的應用程式：</span><span class="sxs-lookup"><span data-stu-id="38521-119">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![在 [API 管理員 Google + API] 頁面上的平台](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="38521-121">啟用之後，API，請點選**建立認證**設定祕密：</span><span class="sxs-lookup"><span data-stu-id="38521-121">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![在 API 管理員 Google + API 頁面上建立認證 按鈕](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="38521-123">選擇：</span><span class="sxs-lookup"><span data-stu-id="38521-123">Choose:</span></span>
  * <span data-ttu-id="38521-124">**Google + API**</span><span class="sxs-lookup"><span data-stu-id="38521-124">**Google+ API**</span></span>
  * <span data-ttu-id="38521-125">**網頁伺服器 (例如 node.js，Tomcat)**，及</span><span class="sxs-lookup"><span data-stu-id="38521-125">**Web server (e.g. node.js, Tomcat)**, and</span></span>
  * <span data-ttu-id="38521-126">**使用者資料**:</span><span class="sxs-lookup"><span data-stu-id="38521-126">**User data**:</span></span>

![API 管理員認證 頁面中：了解哪些種類的認證需要面板](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="38521-128">點選**我需要哪些認證？** 這會引領您的應用程式設定的第二個步驟**建立 OAuth 2.0 用戶端識別碼**:</span><span class="sxs-lookup"><span data-stu-id="38521-128">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![API 管理員認證 頁面中：建立 OAuth 2.0 用戶端識別碼](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="38521-130">因為我們會建立 Google + 專案，與只是一項特徵 （登入），我們可以輸入相同**名稱**OAuth 2.0 用戶端識別碼，為我們所使用的專案。</span><span class="sxs-lookup"><span data-stu-id="38521-130">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="38521-131">輸入您的開發 URI 與`/signin-google`附加至**授權重新導向 Uri**欄位 (例如： `https://localhost:44320/signin-google`)。</span><span class="sxs-lookup"><span data-stu-id="38521-131">Enter your development URI with `/signin-google` appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="38521-132">稍後在本教學課程中設定 Google 驗證會自動處理在要求`/signin-google`實作 OAuth 流程的路由。</span><span class="sxs-lookup"><span data-stu-id="38521-132">The Google authentication configured later in this tutorial will automatically handle requests at `/signin-google` route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="38521-133">URI 區段`/signin-google`會設定為 Google 驗證提供者的預設回呼。</span><span class="sxs-lookup"><span data-stu-id="38521-133">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="38521-134">您可以變更預設的回呼 URI 時設定 Google 驗證中的介軟體，透過繼承[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性[GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions)類別。</span><span class="sxs-lookup"><span data-stu-id="38521-134">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

* <span data-ttu-id="38521-135">按 TAB 鍵以新增**授權重新導向 Uri**項目。</span><span class="sxs-lookup"><span data-stu-id="38521-135">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="38521-136">點選**建立用戶端識別碼**，這會帶您到第三個步驟中，**設定 OAuth 2.0 同意畫面**:</span><span class="sxs-lookup"><span data-stu-id="38521-136">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![API 管理員認證 頁面中：設定 OAuth 2.0 同意畫面](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="38521-138">輸入您的對外**電子郵件地址**並**產品名稱**當 Google + 提示使用者登入您的應用程式所示。</span><span class="sxs-lookup"><span data-stu-id="38521-138">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="38521-139">在有其他選項**更多的自訂選項**。</span><span class="sxs-lookup"><span data-stu-id="38521-139">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="38521-140">點選**繼續**繼續進行到最後一個步驟中，**下載認證**:</span><span class="sxs-lookup"><span data-stu-id="38521-140">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![API 管理員認證 頁面中：下載認證](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="38521-142">點選**下載**儲存 JSON 檔案包含應用程式祕密，以及**完成**以完成建立新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="38521-142">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="38521-143">部署站台時您將必須再次瀏覽**Google 主控台**並註冊新的公用 url。</span><span class="sxs-lookup"><span data-stu-id="38521-143">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="38521-144">存放區 Google ClientID 和 ClientSecret</span><span class="sxs-lookup"><span data-stu-id="38521-144">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="38521-145">連結機密設定，例如 Google`Client ID`並`Client Secret`至您的應用程式的組態使用[Secret Manager](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="38521-145">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="38521-146">基於本教學課程的目的，名稱語彙基元`Authentication:Google:ClientId`和`Authentication:Google:ClientSecret`。</span><span class="sxs-lookup"><span data-stu-id="38521-146">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="38521-147">這些語彙基元的值可以在下一個步驟中下載的 JSON 檔案中找到`web.client_id`和`web.client_secret`。</span><span class="sxs-lookup"><span data-stu-id="38521-147">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="38521-148">設定 Google 驗證</span><span class="sxs-lookup"><span data-stu-id="38521-148">Configure Google Authentication</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="38521-149">將 Google 服務中的新增`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="38521-149">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

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

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="38521-150">在本教學課程中使用的專案範本可確保[Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google)安裝套件。</span><span class="sxs-lookup"><span data-stu-id="38521-150">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

* <span data-ttu-id="38521-151">若要使用 Visual Studio 2017 安裝此套件，以滑鼠右鍵按一下專案，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="38521-151">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="38521-152">若要使用.NET Core CLI 安裝，請執行下列命令在您的專案目錄中：</span><span class="sxs-lookup"><span data-stu-id="38521-152">To install with .NET Core CLI, execute the following in your project directory:</span></span>

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

<span data-ttu-id="38521-153">新增在 Google 中的介軟體`Configure`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="38521-153">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

::: moniker-end

<span data-ttu-id="38521-154">請參閱[GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API 參考，如需有關 Google 驗證所支援的組態選項。</span><span class="sxs-lookup"><span data-stu-id="38521-154">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="38521-155">這可用來要求不同使用者的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="38521-155">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="38521-156">使用 Google 登入</span><span class="sxs-lookup"><span data-stu-id="38521-156">Sign in with Google</span></span>

<span data-ttu-id="38521-157">執行您的應用程式，然後按一下**登入**。</span><span class="sxs-lookup"><span data-stu-id="38521-157">Run your application and click **Log in**.</span></span> <span data-ttu-id="38521-158">此時會出現 使用 Google 登入選項：</span><span class="sxs-lookup"><span data-stu-id="38521-158">An option to sign in with Google appears:</span></span>

![在 Microsoft Edge 中執行的 web 應用程式：未經驗證的使用者](index/_static/DoneGoogle.png)

<span data-ttu-id="38521-160">當您按一下 Google 時，您會重新導向至 Google 進行驗證：</span><span class="sxs-lookup"><span data-stu-id="38521-160">When you click on Google, you are redirected to Google for authentication:</span></span>

![Google 驗證對話方塊](index/_static/GoogleLogin.png)

<span data-ttu-id="38521-162">輸入您的 Google 認證之後, 再將您重新導向回您可以在其中設定您的電子郵件的網站。</span><span class="sxs-lookup"><span data-stu-id="38521-162">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="38521-163">您現在用來登入您的 Google 認證：</span><span class="sxs-lookup"><span data-stu-id="38521-163">You are now logged in using your Google credentials:</span></span>

![在 Microsoft Edge 中執行的 web 應用程式：已驗證的使用者](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="38521-165">疑難排解</span><span class="sxs-lookup"><span data-stu-id="38521-165">Troubleshooting</span></span>

* <span data-ttu-id="38521-166">如果您收到`403 (Forbidden)`錯誤頁面，從您自己的應用程式時執行，在開發模式 （或中斷偵錯工具相同的錯誤），確保**Google + API**中已啟用**API Manager 程式庫**依照所列的步驟[早在此頁面上](#create-the-app-in-google-api-console)。</span><span class="sxs-lookup"><span data-stu-id="38521-166">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="38521-167">如果登入無法運作，而且您未收到任何錯誤，切換到開發模式，以便讓您更輕鬆地偵錯問題。</span><span class="sxs-lookup"><span data-stu-id="38521-167">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="38521-168">**ASP.NET Core 2.x 只有：** 如果身分識別未設定藉由呼叫`services.AddIdentity`中`ConfigureServices`，來驗證嘗試會導致*ArgumentException:必須提供 'SignInScheme' 選項*。</span><span class="sxs-lookup"><span data-stu-id="38521-168">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="38521-169">在本教學課程中使用的專案範本可確保，這麼做。</span><span class="sxs-lookup"><span data-stu-id="38521-169">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="38521-170">如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時資料庫作業失敗*時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="38521-170">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="38521-171">點選**套用移轉**建立資料庫，並重新整理 以忽略錯誤繼續執行。</span><span class="sxs-lookup"><span data-stu-id="38521-171">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="38521-172">後續步驟</span><span class="sxs-lookup"><span data-stu-id="38521-172">Next steps</span></span>

* <span data-ttu-id="38521-173">本文說明如何您可以使用 Google 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="38521-173">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="38521-174">您可以依照類似的方法，來驗證與列在其他提供者[上一頁](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="38521-174">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="38521-175">一旦您發行您的網站至 Azure web 應用程式時，您應該重設`ClientSecret`Google API 主控台中。</span><span class="sxs-lookup"><span data-stu-id="38521-175">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="38521-176">設定`Authentication:Google:ClientId`和`Authentication:Google:ClientSecret`當做 Azure 入口網站中的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="38521-176">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="38521-177">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="38521-177">The configuration system is set up to read keys from environment variables.</span></span>
