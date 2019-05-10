---
title: Twitter 與 ASP.NET Core 的外部登入設定
author: rick-anderson
description: 本教學課程示範如何整合到現有的 ASP.NET Core 應用程式的 Twitter 帳戶使用者驗證。
ms.author: riande
ms.custom: mvc
ms.date: 5/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: 486d58b600ca5326a0728de40bb386fbb9440f67
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/10/2019
ms.locfileid: "65516876"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="9442f-103">Twitter 與 ASP.NET Core 的外部登入設定</span><span class="sxs-lookup"><span data-stu-id="9442f-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="9442f-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9442f-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9442f-105">此範例示範如何讓使用者能夠[使用他們的 Twitter 帳戶登入](https://dev.twitter.com/web/sign-in/desktop-browser)上使用範例 ASP.NET Core 2.2 專案建立[上一頁](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="9442f-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="9442f-106">在 Twitter 中建立應用程式</span><span class="sxs-lookup"><span data-stu-id="9442f-106">Create the app in Twitter</span></span>

* <span data-ttu-id="9442f-107">瀏覽至[ https://apps.twitter.com/ ](https://apps.twitter.com/)並登入。</span><span class="sxs-lookup"><span data-stu-id="9442f-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="9442f-108">如果您還沒有 Twitter 帳戶，使用 **[歡迎現在註冊](https://twitter.com/signup)** 連結建立一個。</span><span class="sxs-lookup"><span data-stu-id="9442f-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="9442f-109">點選**建立新的應用程式**並填妥應用程式**名稱**，**描述**和公用**網站**（這可以是暫時直到您的 URI註冊的網域名稱）：</span><span class="sxs-lookup"><span data-stu-id="9442f-109">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="9442f-110">輸入您的開發 URI 與`/signin-twitter`附加至**有效的 OAuth 重新導向 Uri**欄位 (例如： `https://webapp128.azurewebsites.net/signin-twitter`)。</span><span class="sxs-lookup"><span data-stu-id="9442f-110">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="9442f-111">稍後在此範例中所設定的 Twitter 驗證配置會自動處理在要求`/signin-twitter`實作 OAuth 流程的路由。</span><span class="sxs-lookup"><span data-stu-id="9442f-111">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="9442f-112">URI 區段`/signin-twitter`設為 Twitter 驗證提供者的預設回呼。</span><span class="sxs-lookup"><span data-stu-id="9442f-112">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="9442f-113">您可以變更預設的回呼 URI 時設定 Twitter 驗證中的介軟體，透過繼承[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性[TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions)類別。</span><span class="sxs-lookup"><span data-stu-id="9442f-113">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="9442f-114">填寫表單的其餘部分，然後點選**建立 Twitter 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="9442f-114">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="9442f-115">會顯示新的應用程式詳細資料：</span><span class="sxs-lookup"><span data-stu-id="9442f-115">New application details are displayed:</span></span>

## <a name="storing-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="9442f-116">儲存的 Twitter 取用者 API 金鑰和祕密</span><span class="sxs-lookup"><span data-stu-id="9442f-116">Storing Twitter Consumer API key and secret</span></span>

<span data-ttu-id="9442f-117">執行下列命令，以安全地儲存`ClientId`並`ClientSecret`使用[Secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="9442f-117">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```console
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerAPISecret <Secret>
```

<span data-ttu-id="9442f-118">連結機密設定，例如 Twitter`Consumer Key`並`Consumer Secret`至您的應用程式的組態使用[Secret Manager](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="9442f-118">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="9442f-119">基於此範例的目的，名稱語彙基元`Authentication:Twitter:ConsumerKey`和`Authentication:Twitter:ConsumerSecret`。</span><span class="sxs-lookup"><span data-stu-id="9442f-119">For the purposes of this sample, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="9442f-120">這些權杖可於**金鑰和存取權杖**之後建立新的 Twitter 應用程式 索引標籤：</span><span class="sxs-lookup"><span data-stu-id="9442f-120">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="9442f-121">設定 Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="9442f-121">Configure Twitter Authentication</span></span>

<span data-ttu-id="9442f-122">將 Twitter 服務中的新增`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="9442f-122">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="9442f-123">請參閱[TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API 參考，如 Twitter 驗證所支援的組態選項的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="9442f-123">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="9442f-124">這可用來要求不同使用者的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="9442f-124">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="9442f-125">使用 Twitter 登入</span><span class="sxs-lookup"><span data-stu-id="9442f-125">Sign in with Twitter</span></span>

<span data-ttu-id="9442f-126">執行應用程式，然後選取**登入**。</span><span class="sxs-lookup"><span data-stu-id="9442f-126">Run the app and select **Log in**.</span></span> <span data-ttu-id="9442f-127">此時會出現以 Twitter 登入選項：</span><span class="sxs-lookup"><span data-stu-id="9442f-127">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="9442f-128">按一下**Twitter**將重新導向至 Twitter 進行驗證：</span><span class="sxs-lookup"><span data-stu-id="9442f-128">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="9442f-129">輸入您的 Twitter 認證之後, 您將被重新導向回到網站，您可以在其中設定您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="9442f-129">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="9442f-130">您現在用來登入您的 Twitter 認證：</span><span class="sxs-lookup"><span data-stu-id="9442f-130">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="9442f-131">疑難排解</span><span class="sxs-lookup"><span data-stu-id="9442f-131">Troubleshooting</span></span>

* <span data-ttu-id="9442f-132">**ASP.NET Core 2.x 只有：** 如果身分識別未設定藉由呼叫`services.AddIdentity`中`ConfigureServices`，來驗證嘗試會導致*ArgumentException:必須提供 'SignInScheme' 選項*。</span><span class="sxs-lookup"><span data-stu-id="9442f-132">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="9442f-133">此範例中使用的專案範本可確保，這麼做。</span><span class="sxs-lookup"><span data-stu-id="9442f-133">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="9442f-134">如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時資料庫作業失敗*時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="9442f-134">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="9442f-135">點選**套用移轉**建立資料庫，並重新整理 以忽略錯誤繼續執行。</span><span class="sxs-lookup"><span data-stu-id="9442f-135">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9442f-136">後續步驟</span><span class="sxs-lookup"><span data-stu-id="9442f-136">Next steps</span></span>

* <span data-ttu-id="9442f-137">本文說明如何您可以使用 Twitter 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="9442f-137">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="9442f-138">您可以依照類似的方法，來驗證與列在其他提供者[上一頁](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="9442f-138">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="9442f-139">一旦您發行您的網站至 Azure web 應用程式時，您應該重設`ConsumerSecret`Twitter 開發人員入口網站中。</span><span class="sxs-lookup"><span data-stu-id="9442f-139">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="9442f-140">設定`Authentication:Twitter:ConsumerKey`和`Authentication:Twitter:ConsumerSecret`當做 Azure 入口網站中的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="9442f-140">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="9442f-141">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="9442f-141">The configuration system is set up to read keys from environment variables.</span></span>