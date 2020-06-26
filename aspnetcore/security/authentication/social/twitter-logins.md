---
title: 使用 ASP.NET Core 的 Twitter 外部登錄設定
author: rick-anderson
description: 本教學課程示範如何將 Twitter 帳戶使用者驗證整合到現有的 ASP.NET Core 應用程式中。
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/twitter-logins
ms.openlocfilehash: 61c983de33b91a16ad207d8a350daf4859c89eaf
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85406091"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="dc9d3-103">使用 ASP.NET Core 的 Twitter 外部登錄設定</span><span class="sxs-lookup"><span data-stu-id="dc9d3-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="dc9d3-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dc9d3-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="dc9d3-105">這個範例會示範如何使用在[上一頁](xref:security/authentication/social/index)建立的範例 ASP.NET Core 3.0 專案，讓使用者能夠以[Twitter 帳戶登入](https://dev.twitter.com/web/sign-in/desktop-browser)。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="dc9d3-106">在 Twitter 中建立應用程式</span><span class="sxs-lookup"><span data-stu-id="dc9d3-106">Create the app in Twitter</span></span>

* <span data-ttu-id="dc9d3-107">將[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0)新增至該專案中。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-107">Add the [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) NuGet package to the project.</span></span>

* <span data-ttu-id="dc9d3-108">流覽至 [https://apps.twitter.com/](https://apps.twitter.com/) 並登入。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-108">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="dc9d3-109">如果您還沒有 Twitter 帳戶，請使用 [**[立即註冊](https://twitter.com/signup)**] 連結來建立一個。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-109">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="dc9d3-110">選取 [**建立應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-110">Select **Create an app**.</span></span> <span data-ttu-id="dc9d3-111">填寫 [應用程式**名稱**]、[**應用程式描述**] 和 [公用**網站**URI] （這可以是暫時性的，直到您註冊功能變數名稱）：</span><span class="sxs-lookup"><span data-stu-id="dc9d3-111">Fill out the **App name**, **Application description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="dc9d3-112">核取 [**啟用使用 Twitter 登入**] 旁的核取方塊</span><span class="sxs-lookup"><span data-stu-id="dc9d3-112">Check the box next to **Enable Sign in with Twitter**</span></span>

* <span data-ttu-id="dc9d3-113">AspNetCore。Identity</span><span class="sxs-lookup"><span data-stu-id="dc9d3-113">Microsoft.AspNetCore.Identity</span></span> <span data-ttu-id="dc9d3-114">根據預設，使用者需要有電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-114">requires users to have an email address by default.</span></span> <span data-ttu-id="dc9d3-115">前往 [**許可權**] 索引標籤，按一下 [**編輯**] 按鈕，然後核取 [**要求使用者的電子郵件地址**] 旁的方塊。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-115">Go to the **Permissions** tab, click the **Edit** button and check the box next to **Request email address from users**.</span></span>

* <span data-ttu-id="dc9d3-116">輸入您的開發 URI，並 `/signin-twitter` 附加至 [**回呼 url** ] 欄位（例如： `https://webapp128.azurewebsites.net/signin-twitter` ）。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-116">Enter your development URI with `/signin-twitter` appended into the **Callback URLs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="dc9d3-117">稍後在此範例中設定的 Twitter 驗證配置會自動處理 `/signin-twitter` 路由上的要求，以執行 OAuth 流程。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-117">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="dc9d3-118">URI 區段 `/signin-twitter` 會設定為 Twitter 驗證提供者的預設回呼。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-118">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="dc9d3-119">您可以在設定 Twitter 驗證中介軟體時，透過[TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions)類別的繼承[RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性來變更預設的回呼 URI。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-119">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="dc9d3-120">填寫表單的其餘部分，然後選取 [**建立**]。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-120">Fill out the rest of the form and select **Create**.</span></span> <span data-ttu-id="dc9d3-121">隨即顯示新的應用程式詳細資料：</span><span class="sxs-lookup"><span data-stu-id="dc9d3-121">New application details are displayed:</span></span>

## <a name="store-the-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="dc9d3-122">儲存 Twitter 取用者 API 金鑰和密碼</span><span class="sxs-lookup"><span data-stu-id="dc9d3-122">Store the Twitter consumer API key and secret</span></span>

<span data-ttu-id="dc9d3-123">使用[秘密管理員](xref:security/app-secrets)來儲存機密設定，例如 Twitter 取用者 API 金鑰和密碼。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-123">Store sensitive settings such as the Twitter consumer API key and secret with [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="dc9d3-124">針對此範例，請使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="dc9d3-124">For this sample, use the following steps:</span></span>

1. <span data-ttu-id="dc9d3-125">根據[啟用秘密儲存](xref:security/app-secrets#enable-secret-storage)中的指示，初始化秘密儲存的專案。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-125">Initialize the project for secret storage per the instructions at [Enable secret storage](xref:security/app-secrets#enable-secret-storage).</span></span>
1. <span data-ttu-id="dc9d3-126">使用秘密金鑰和，將敏感性設定儲存在本機密碼存放區中 `Authentication:Twitter:ConsumerKey` `Authentication:Twitter:ConsumerSecret` ：</span><span class="sxs-lookup"><span data-stu-id="dc9d3-126">Store the sensitive settings in the local secret store with the secrets keys `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`:</span></span>

    ```dotnetcli
    dotnet user-secrets set "Authentication:Twitter:ConsumerAPIKey" "<consumer-api-key>"
    dotnet user-secrets set "Authentication:Twitter:ConsumerSecret" "<consumer-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="dc9d3-127">在建立新的 Twitter 應用程式之後，您可以在 [**金鑰和存取權杖**] 索引標籤上找到這些權杖：</span><span class="sxs-lookup"><span data-stu-id="dc9d3-127">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="dc9d3-128">設定 Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="dc9d3-128">Configure Twitter Authentication</span></span>

<span data-ttu-id="dc9d3-129">在 Startup.cs 檔案的方法中新增 Twitter 服務 `ConfigureServices` ： *Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="dc9d3-129">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupTwitter3x.cs?name=snippet&highlight=10-15)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="dc9d3-130">如需 Twitter 驗證所支援之設定選項的詳細資訊，請參閱[TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API 參考。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-130">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="dc9d3-131">這可以用來要求使用者的不同資訊。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-131">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="dc9d3-132">使用 Twitter 登入</span><span class="sxs-lookup"><span data-stu-id="dc9d3-132">Sign in with Twitter</span></span>

<span data-ttu-id="dc9d3-133">執行應用程式，然後選取 [**登入**]。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-133">Run the app and select **Log in**.</span></span> <span data-ttu-id="dc9d3-134">隨即會出現使用 Twitter 登入的選項：</span><span class="sxs-lookup"><span data-stu-id="dc9d3-134">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="dc9d3-135">按一下 [ **twitter**重新導向至 twitter 以進行驗證]：</span><span class="sxs-lookup"><span data-stu-id="dc9d3-135">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="dc9d3-136">輸入您的 Twitter 認證之後，系統會將您重新導向回到網站，您可以在其中設定您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-136">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="dc9d3-137">您現在已使用 Twitter 認證登入：</span><span class="sxs-lookup"><span data-stu-id="dc9d3-137">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

<!-- 
### React to cancel Authorize External sign-in
Twitter doesn't support AccessDeniedPath
Rather in the twitter setup, you can provide an External sign-in homepage. The external sign-in homepage doesn't support localhost. Tested with https://cors3.azurewebsites.net/ and that works.
-->

## <a name="troubleshooting"></a><span data-ttu-id="dc9d3-138">疑難排解</span><span class="sxs-lookup"><span data-stu-id="dc9d3-138">Troubleshooting</span></span>

* <span data-ttu-id="dc9d3-139">**僅限 ASP.NET Core 2.x：** 如果 Identity 未透過 `services.AddIdentity` 在中呼叫 `ConfigureServices` 來設定，則嘗試驗證會導致*ArgumentException：必須提供 ' SignInScheme ' 選項*。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-139">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="dc9d3-140">此範例中使用的專案範本可確保完成此作業。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-140">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="dc9d3-141">如果尚未藉由套用初始遷移來建立網站資料庫，則在處理要求錯誤時，您將會收到*資料庫作業失敗的*情況。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-141">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="dc9d3-142">請按 [套用**遷移**] 來建立資料庫，並重新整理以繼續發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-142">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dc9d3-143">後續步驟</span><span class="sxs-lookup"><span data-stu-id="dc9d3-143">Next steps</span></span>

* <span data-ttu-id="dc9d3-144">本文說明如何使用 Twitter 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-144">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="dc9d3-145">您可以遵循類似的方法，向[先前頁面](xref:security/authentication/social/index)上所列的其他提供者進行驗證。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-145">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="dc9d3-146">將網站發佈至 Azure web 應用程式之後，您應該 `ConsumerSecret` 在 Twitter 開發人員入口網站中重設。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-146">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="dc9d3-147">將 `Authentication:Twitter:ConsumerKey` 和設定 `Authentication:Twitter:ConsumerSecret` 為 Azure 入口網站中的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-147">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="dc9d3-148">設定系統已設定為從環境變數讀取金鑰。</span><span class="sxs-lookup"><span data-stu-id="dc9d3-148">The configuration system is set up to read keys from environment variables.</span></span>
