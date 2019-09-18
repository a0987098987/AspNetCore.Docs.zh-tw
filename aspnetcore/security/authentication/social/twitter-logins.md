---
title: 使用 ASP.NET Core 的 Twitter 外部登錄設定
author: rick-anderson
description: 本教學課程示範如何將 Twitter 帳戶使用者驗證整合到現有的 ASP.NET Core 應用程式中。
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: 5182f1647acb664bf35f086fcddbe909559a62f7
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082296"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="72015-103">使用 ASP.NET Core 的 Twitter 外部登錄設定</span><span class="sxs-lookup"><span data-stu-id="72015-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="72015-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="72015-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="72015-105">這個範例會示範如何使用在[上一頁](xref:security/authentication/social/index)建立的範例 ASP.NET Core 2.2 專案，讓使用者能夠以[Twitter 帳戶登入](https://dev.twitter.com/web/sign-in/desktop-browser)。</span><span class="sxs-lookup"><span data-stu-id="72015-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="72015-106">在 Twitter 中建立應用程式</span><span class="sxs-lookup"><span data-stu-id="72015-106">Create the app in Twitter</span></span>

* <span data-ttu-id="72015-107">瀏覽至[ https://apps.twitter.com/ ](https://apps.twitter.com/)並登入。</span><span class="sxs-lookup"><span data-stu-id="72015-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="72015-108">如果您還沒有 Twitter 帳戶，使用 **[歡迎現在註冊](https://twitter.com/signup)** 連結建立一個。</span><span class="sxs-lookup"><span data-stu-id="72015-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="72015-109">按一下 [**建立新的應用**程式]，並填寫應用程式**名稱**、**描述**和公用**網站**URI （這可以是暫時性的，直到您註冊功能變數名稱）：</span><span class="sxs-lookup"><span data-stu-id="72015-109">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="72015-110">輸入您的開發 URI `/signin-twitter` ，並附加至 [**有效的 OAuth 重新導向 uri** ] `https://webapp128.azurewebsites.net/signin-twitter`欄位（例如：）。</span><span class="sxs-lookup"><span data-stu-id="72015-110">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="72015-111">稍後在此範例中設定的 Twitter 驗證配置會自動處理路由`/signin-twitter`上的要求，以執行 OAuth 流程。</span><span class="sxs-lookup"><span data-stu-id="72015-111">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="72015-112">URI 區段`/signin-twitter`會設定為 Twitter 驗證提供者的預設回呼。</span><span class="sxs-lookup"><span data-stu-id="72015-112">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="72015-113">您可以在設定 Twitter 驗證中介軟體時，透過[TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions)類別的繼承[RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性來變更預設的回呼 URI。</span><span class="sxs-lookup"><span data-stu-id="72015-113">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="72015-114">填寫表單的其餘部分，並按一下 [**建立您的 Twitter 應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="72015-114">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="72015-115">隨即顯示新的應用程式詳細資料：</span><span class="sxs-lookup"><span data-stu-id="72015-115">New application details are displayed:</span></span>

## <a name="storing-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="72015-116">儲存 Twitter 取用者 API 金鑰和密碼</span><span class="sxs-lookup"><span data-stu-id="72015-116">Storing Twitter Consumer API key and secret</span></span>

<span data-ttu-id="72015-117">執行下列命令以安全地儲存`ClientId`和`ClientSecret`使用「[秘密管理員](xref:security/app-secrets)」：</span><span class="sxs-lookup"><span data-stu-id="72015-117">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

<span data-ttu-id="72015-118">使用[秘密管理員](xref:security/app-secrets)，將`Consumer Key` Twitter `Consumer Secret`和等機密設定連結至您的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="72015-118">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="72015-119">基於此範例的目的，請將權杖`Authentication:Twitter:ConsumerKey`命名為和。 `Authentication:Twitter:ConsumerSecret`</span><span class="sxs-lookup"><span data-stu-id="72015-119">For the purposes of this sample, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="72015-120">在建立新的 Twitter 應用程式之後，您可以在 [**金鑰和存取權杖**] 索引標籤上找到這些權杖：</span><span class="sxs-lookup"><span data-stu-id="72015-120">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="72015-121">設定 Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="72015-121">Configure Twitter Authentication</span></span>

<span data-ttu-id="72015-122">在`ConfigureServices` *Startup.cs*檔案的方法中新增 Twitter 服務：</span><span class="sxs-lookup"><span data-stu-id="72015-122">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="72015-123">如需 Twitter 驗證所支援之設定選項的詳細資訊，請參閱[TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API 參考。</span><span class="sxs-lookup"><span data-stu-id="72015-123">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="72015-124">這可用來要求不同使用者的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="72015-124">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="72015-125">使用 Twitter 登入</span><span class="sxs-lookup"><span data-stu-id="72015-125">Sign in with Twitter</span></span>

<span data-ttu-id="72015-126">執行應用程式，然後選取 [**登入**]。</span><span class="sxs-lookup"><span data-stu-id="72015-126">Run the app and select **Log in**.</span></span> <span data-ttu-id="72015-127">隨即會出現使用 Twitter 登入的選項：</span><span class="sxs-lookup"><span data-stu-id="72015-127">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="72015-128">按一下 [ **twitter**重新導向至 twitter 以進行驗證]：</span><span class="sxs-lookup"><span data-stu-id="72015-128">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="72015-129">輸入您的 Twitter 認證之後，系統會將您重新導向回到網站，您可以在其中設定您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72015-129">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="72015-130">您現在已使用 Twitter 認證登入：</span><span class="sxs-lookup"><span data-stu-id="72015-130">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="72015-131">疑難排解</span><span class="sxs-lookup"><span data-stu-id="72015-131">Troubleshooting</span></span>

* <span data-ttu-id="72015-132">**僅限 ASP.NET Core 2.x：** 如果未透過在中`services.AddIdentity` `ConfigureServices`呼叫*來設定身分識別，則嘗試進行驗證會導致 ArgumentException：必須提供*' SignInScheme ' 選項。</span><span class="sxs-lookup"><span data-stu-id="72015-132">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="72015-133">此範例中使用的專案範本可確保完成此作業。</span><span class="sxs-lookup"><span data-stu-id="72015-133">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="72015-134">如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時資料庫作業失敗*時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="72015-134">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="72015-135">點選**套用移轉**建立資料庫，並重新整理 以忽略錯誤繼續執行。</span><span class="sxs-lookup"><span data-stu-id="72015-135">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="72015-136">後續步驟</span><span class="sxs-lookup"><span data-stu-id="72015-136">Next steps</span></span>

* <span data-ttu-id="72015-137">本文說明如何使用 Twitter 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="72015-137">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="72015-138">您可以依照類似的方法，來驗證與列在其他提供者[上一頁](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="72015-138">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="72015-139">將網站發佈至 Azure web 應用程式之後，您應該`ConsumerSecret`在 Twitter 開發人員入口網站中重設。</span><span class="sxs-lookup"><span data-stu-id="72015-139">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="72015-140">設定`Authentication:Twitter:ConsumerKey`和`Authentication:Twitter:ConsumerSecret`當做 Azure 入口網站中的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="72015-140">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="72015-141">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="72015-141">The configuration system is set up to read keys from environment variables.</span></span>
