---
title: Twitter 外部登入與 ASP.NET Core 的安裝程式
author: rick-anderson
description: 本教學課程示範的整合到現有的 ASP.NET Core 應用程式的 Twitter 帳戶使用者驗證。
manager: wpickett
ms.author: riande
ms.date: 11/01/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/twitter-logins
ms.openlocfilehash: e0bf0084f8e46f3774fa070602404840aa803661
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a><span data-ttu-id="73a55-103">Twitter 外部登入與 ASP.NET Core 的安裝程式</span><span class="sxs-lookup"><span data-stu-id="73a55-103">Twitter external login setup with ASP.NET Core</span></span>

<span data-ttu-id="73a55-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="73a55-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="73a55-105">本教學課程會示範如何讓使用者[使用 Twitter 帳戶登入](https://dev.twitter.com/web/sign-in/desktop-browser)上使用範例 ASP.NET Core 2.0 專案建立[上一頁](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="73a55-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="73a55-106">在 Twitter 中建立應用程式</span><span class="sxs-lookup"><span data-stu-id="73a55-106">Create the app in Twitter</span></span>

* <span data-ttu-id="73a55-107">瀏覽至[ https://apps.twitter.com/ ](https://apps.twitter.com/)並登入。</span><span class="sxs-lookup"><span data-stu-id="73a55-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="73a55-108">如果您還沒有 Twitter 帳戶，使用**[立即註冊](https://twitter.com/signup)** 建立一個連結。</span><span class="sxs-lookup"><span data-stu-id="73a55-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="73a55-109">登入之後,**應用程式管理**頁面會顯示：</span><span class="sxs-lookup"><span data-stu-id="73a55-109">After signing in, the **Application Management** page is shown:</span></span>

![Twitter 應用程式管理的 Microsoft Edge 開啟](index/_static/TwitterAppManage.png)

* <span data-ttu-id="73a55-111">點選**建立新的應用程式**填妥應用程式和**名稱**，**描述**和公用**網站**（這可能是暫時性直到您的 URI註冊的網域名稱）：</span><span class="sxs-lookup"><span data-stu-id="73a55-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

![建立應用程式頁面](index/_static/TwitterCreate.png)

* <span data-ttu-id="73a55-113">輸入您的開發 URI 與 */signin-twitter*附加到**有效的 OAuth 重新導向 Uri**欄位 (例如： `https://localhost:44320/signin-twitter`)。</span><span class="sxs-lookup"><span data-stu-id="73a55-113">Enter your development URI with */signin-twitter* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="73a55-114">稍後在本教學課程中設定的 Twitter 驗證配置將會自動處理在要求 */signin-twitter*實作 OAuth 流程的路由。</span><span class="sxs-lookup"><span data-stu-id="73a55-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at */signin-twitter* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="73a55-115">填寫表單的其餘部分，並點選**建立應用程式 Twitter**。</span><span class="sxs-lookup"><span data-stu-id="73a55-115">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="73a55-116">會顯示新的應用程式詳細資料：</span><span class="sxs-lookup"><span data-stu-id="73a55-116">New application details are displayed:</span></span>

![應用程式頁面的詳細資料 索引標籤](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="73a55-118">部署站台時必須再次瀏覽**應用程式管理**頁面上，並註冊新的公用 URI。</span><span class="sxs-lookup"><span data-stu-id="73a55-118">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="73a55-119">儲存 Twitter ConsumerKey 和 ConsumerSecret</span><span class="sxs-lookup"><span data-stu-id="73a55-119">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="73a55-120">連結機密設定，例如 Twitter`Consumer Key`和`Consumer Secret`您應用程式組態使用[密碼管理員](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="73a55-120">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="73a55-121">基於本教學課程的目的，名稱語彙基元`Authentication:Twitter:ConsumerKey`和`Authentication:Twitter:ConsumerSecret`。</span><span class="sxs-lookup"><span data-stu-id="73a55-121">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="73a55-122">這些語彙基元可以找到上**機碼和存取權杖**之後建立新的 Twitter 應用程式 索引標籤：</span><span class="sxs-lookup"><span data-stu-id="73a55-122">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![索引鍵和存取權杖 索引標籤](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="73a55-124">設定 Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="73a55-124">Configure Twitter Authentication</span></span>

<span data-ttu-id="73a55-125">在本教學課程中使用的專案範本，可確保[Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter)封裝是否已經安裝。</span><span class="sxs-lookup"><span data-stu-id="73a55-125">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="73a55-126">若要安裝此套件與 Visual Studio 2017，以滑鼠右鍵按一下專案，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="73a55-126">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="73a55-127">若要安裝的.NET Core CLI，請在您的專案目錄中執行下列：</span><span class="sxs-lookup"><span data-stu-id="73a55-127">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="73a55-128">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="73a55-128">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="73a55-129">Twitter 服務中的新增`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="73a55-129">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

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

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="73a55-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="73a55-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="73a55-131">加入在 Twitter 中的介軟體`Configure`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="73a55-131">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

* * *
<span data-ttu-id="73a55-132">請參閱[TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API 參考，如需有關 Twitter 驗證所支援的組態選項。</span><span class="sxs-lookup"><span data-stu-id="73a55-132">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="73a55-133">這可以用於要求的使用者不同的資訊。</span><span class="sxs-lookup"><span data-stu-id="73a55-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="73a55-134">使用 Twitter 登入</span><span class="sxs-lookup"><span data-stu-id="73a55-134">Sign in with Twitter</span></span>

<span data-ttu-id="73a55-135">執行您的應用程式，然後按一下**登入**。</span><span class="sxs-lookup"><span data-stu-id="73a55-135">Run your application and click **Log in**.</span></span> <span data-ttu-id="73a55-136">使用 Twitter 登入選項會出現：</span><span class="sxs-lookup"><span data-stu-id="73a55-136">An option to sign in with Twitter appears:</span></span>

![Web 應用程式： 未驗證的使用者](index/_static/DoneTwitter.png)

<span data-ttu-id="73a55-138">按一下**Twitter**重新導向至 Twitter 驗證：</span><span class="sxs-lookup"><span data-stu-id="73a55-138">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Twitter 驗證頁面](index/_static/TwitterLogin.png)

<span data-ttu-id="73a55-140">輸入您的 Twitter 認證之後, 您將會重新導向回 web 站台，您可以設定您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="73a55-140">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="73a55-141">您現在會記錄在使用您的 Twitter 認證：</span><span class="sxs-lookup"><span data-stu-id="73a55-141">You are now logged in using your Twitter credentials:</span></span>

![Web 應用程式： 已驗證的使用者](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="73a55-143">疑難排解</span><span class="sxs-lookup"><span data-stu-id="73a55-143">Troubleshooting</span></span>

* <span data-ttu-id="73a55-144">**ASP.NET Core 2.x 僅：**如果身分識別不藉由呼叫設定`services.AddIdentity`中`ConfigureServices`，嘗試驗證將會導致*ArgumentException： 必須提供 'SignInScheme' 選項*。</span><span class="sxs-lookup"><span data-stu-id="73a55-144">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="73a55-145">在本教學課程中使用的專案範本可確保，這已完成。</span><span class="sxs-lookup"><span data-stu-id="73a55-145">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="73a55-146">如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時，資料庫作業失敗*錯誤。</span><span class="sxs-lookup"><span data-stu-id="73a55-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="73a55-147">點選**套用移轉**來建立資料庫，並重新整理 以忽略錯誤繼續執行。</span><span class="sxs-lookup"><span data-stu-id="73a55-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="73a55-148">後續步驟</span><span class="sxs-lookup"><span data-stu-id="73a55-148">Next steps</span></span>

* <span data-ttu-id="73a55-149">這篇文章會示範您可以向 Twitter。</span><span class="sxs-lookup"><span data-stu-id="73a55-149">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="73a55-150">您可以依照類似的方法，來向其他提供者列在[上一頁](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="73a55-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="73a55-151">一旦您將您的網站發行至 Azure web 應用程式時，您應該重設`ConsumerSecret`Twitter 開發人員入口網站中。</span><span class="sxs-lookup"><span data-stu-id="73a55-151">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="73a55-152">設定`Authentication:Twitter:ConsumerKey`和`Authentication:Twitter:ConsumerSecret`為在 Azure 入口網站應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="73a55-152">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="73a55-153">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="73a55-153">The configuration system is set up to read keys from environment variables.</span></span>
