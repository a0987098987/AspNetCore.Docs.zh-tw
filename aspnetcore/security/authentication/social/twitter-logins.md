---
title: 帶有ASP.NET核心的Twitter外部登入設定
author: rick-anderson
description: 本教程演示了將Twitter帳戶使用者身份驗證整合到現有ASP.NET核心應用。
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/twitter-logins
ms.openlocfilehash: 1f5d667e905e49ae05f5aa31bd5b69ad126f6e28
ms.sourcegitcommit: 5af16166977da598953f82da3ed3b7712d38f6cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/14/2020
ms.locfileid: "81277284"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="01706-103">帶有ASP.NET核心的Twitter外部登入設定</span><span class="sxs-lookup"><span data-stu-id="01706-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="01706-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="01706-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="01706-105">此範例展示如何允許使用者使用[上一頁上](xref:security/authentication/social/index)建立的範例ASP.NET Core 3.0 專案[使用 Twitter 帳戶登入](https://dev.twitter.com/web/sign-in/desktop-browser)。</span><span class="sxs-lookup"><span data-stu-id="01706-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="01706-106">在 Twitter 中建立應用程式</span><span class="sxs-lookup"><span data-stu-id="01706-106">Create the app in Twitter</span></span>

* <span data-ttu-id="01706-107">將[Microsoft.AspNetCore.身份驗證.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) NuGet 包添加到專案中。</span><span class="sxs-lookup"><span data-stu-id="01706-107">Add the [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) NuGet package to the project.</span></span>

* <span data-ttu-id="01706-108">導航到[https://apps.twitter.com/](https://apps.twitter.com/)並登錄。</span><span class="sxs-lookup"><span data-stu-id="01706-108">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="01706-109">如果您還沒有 Twitter 帳戶,請使用**[「立即註冊」](https://twitter.com/signup)** 連結創建一個帳戶。</span><span class="sxs-lookup"><span data-stu-id="01706-109">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="01706-110">選擇 **「創建應用**」。</span><span class="sxs-lookup"><span data-stu-id="01706-110">Select **Create an app**.</span></span> <span data-ttu-id="01706-111">填寫**應用程式與**公共**網站**URI(在註冊網域名稱之前,這可能是暫時的):**Application description**</span><span class="sxs-lookup"><span data-stu-id="01706-111">Fill out the **App name**, **Application description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="01706-112">選取使用 Twitter**啟用登入**旁邊的複選盒</span><span class="sxs-lookup"><span data-stu-id="01706-112">Check the box next to **Enable Sign in with Twitter**</span></span>

* <span data-ttu-id="01706-113">默認情況下,Microsoft.AspNetCore.標識要求用戶擁有電子郵寄地址。</span><span class="sxs-lookup"><span data-stu-id="01706-113">Microsoft.AspNetCore.Identity requires users to have an email address by default.</span></span> <span data-ttu-id="01706-114">轉到 **"權限'** 選項卡,按下 **"編輯"** 按鈕,然後選中 **「向使用者請求電子郵件位址」** 旁邊的複選框。</span><span class="sxs-lookup"><span data-stu-id="01706-114">Go to the **Permissions** tab, click the **Edit** button and check the box next to **Request email address from users**.</span></span>

* <span data-ttu-id="01706-115">輸入您的開發 URI,`/signin-twitter`並 附加到**回檔網址**欄`https://webapp128.azurewebsites.net/signin-twitter`位(例如: 。</span><span class="sxs-lookup"><span data-stu-id="01706-115">Enter your development URI with `/signin-twitter` appended into the **Callback URLs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="01706-116">此範例中稍後設定的 Twitter 身份驗證方案將自動處理路`/signin-twitter`由中 實現 OAuth 流的請求。</span><span class="sxs-lookup"><span data-stu-id="01706-116">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="01706-117">URI`/signin-twitter`段設置為 Twitter 身份驗證提供程式的預設回調。</span><span class="sxs-lookup"><span data-stu-id="01706-117">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="01706-118">您可以透過[TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions)類別的繼承[遠端身份驗證選項.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性設定 Twitter 身份驗證中間件,同時更改預設回檔 URI。</span><span class="sxs-lookup"><span data-stu-id="01706-118">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="01706-119">填寫表單的其餘部分並選擇 **"創建**"。</span><span class="sxs-lookup"><span data-stu-id="01706-119">Fill out the rest of the form and select **Create**.</span></span> <span data-ttu-id="01706-120">將顯示新的應用程式詳細資訊:</span><span class="sxs-lookup"><span data-stu-id="01706-120">New application details are displayed:</span></span>

## <a name="store-the-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="01706-121">儲存 Twitter 消費者 API 金鑰和機密</span><span class="sxs-lookup"><span data-stu-id="01706-121">Store the Twitter consumer API key and secret</span></span>

<span data-ttu-id="01706-122">儲存敏感設定,如Twitter消費者API金鑰和[秘密管理員](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="01706-122">Store sensitive settings such as the Twitter consumer API key and secret with [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="01706-123">對於此示例,請使用以下步驟:</span><span class="sxs-lookup"><span data-stu-id="01706-123">For this sample, use the following steps:</span></span>

1. <span data-ttu-id="01706-124">根據[啟用密鑰存儲](xref:security/app-secrets#enable-secret-storage)的指令初始化了機密存儲的專案。</span><span class="sxs-lookup"><span data-stu-id="01706-124">Initialize the project for secret storage per the instructions at [Enable secret storage](xref:security/app-secrets#enable-secret-storage).</span></span>
1. <span data-ttu-id="01706-125">使用機金鑰`Authentication:Twitter:ConsumerKey`將敏感設定儲存在本地機密儲存中,並`Authentication:Twitter:ConsumerSecret`:</span><span class="sxs-lookup"><span data-stu-id="01706-125">Store the sensitive settings in the local secret store with the secrets keys `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`:</span></span>

    ```dotnetcli
    dotnet user-secrets set "Authentication:Twitter:ConsumerAPIKey" "<consumer-api-key>"
    dotnet user-secrets set "Authentication:Twitter:ConsumerSecret" "<consumer-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="01706-126">建立新的 Twitter 應用程式後,可以在 **「密鑰和存取權杖」** 選項卡上找到這些權杖:</span><span class="sxs-lookup"><span data-stu-id="01706-126">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="01706-127">設定 Twitter 認證</span><span class="sxs-lookup"><span data-stu-id="01706-127">Configure Twitter Authentication</span></span>

<span data-ttu-id="01706-128">在`ConfigureServices`*Startup.cs*檔案中的方法中加入 Twitter 服務:</span><span class="sxs-lookup"><span data-stu-id="01706-128">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupTwitter3x.cs?name=snippet&highlight=10-15)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="01706-129">有關 Twitter 身份驗證支援的配置選項的詳細資訊,請參閱[TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API 參考。</span><span class="sxs-lookup"><span data-stu-id="01706-129">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="01706-130">這可用於請求有關使用者的不同資訊。</span><span class="sxs-lookup"><span data-stu-id="01706-130">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="01706-131">使用 Twitter 登入</span><span class="sxs-lookup"><span data-stu-id="01706-131">Sign in with Twitter</span></span>

<span data-ttu-id="01706-132">運行應用並選擇 **「登錄」。**</span><span class="sxs-lookup"><span data-stu-id="01706-132">Run the app and select **Log in**.</span></span> <span data-ttu-id="01706-133">使用 Twitter 登入的選項:</span><span class="sxs-lookup"><span data-stu-id="01706-133">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="01706-134">點擊**Twitter**重定向到 Twitter 進行身份驗證:</span><span class="sxs-lookup"><span data-stu-id="01706-134">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="01706-135">輸入 Twitter 認證後,您將被重定向回可以設置電子郵件的網站。</span><span class="sxs-lookup"><span data-stu-id="01706-135">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="01706-136">您現在使用 Twitter 認證登入:</span><span class="sxs-lookup"><span data-stu-id="01706-136">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

<!-- 
### React to cancel Authorize External sign-in
Twitter doesn't support AccessDeniedPath
Rather in the twitter setup, you can provide an External sign-in homepage. The external sign-in homepage doesn't support localhost. Tested with https://cors3.azurewebsites.net/ and that works.
-->

## <a name="troubleshooting"></a><span data-ttu-id="01706-137">疑難排解</span><span class="sxs-lookup"><span data-stu-id="01706-137">Troubleshooting</span></span>

* <span data-ttu-id="01706-138">**只有ASP.NET核心 2.x:** 如果未透過除錯`services.AddIdentity`設定識別`ConfigureServices`,則試著認證會導致*參數異常:必須提供「SignInScheme」選項*。</span><span class="sxs-lookup"><span data-stu-id="01706-138">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="01706-139">此示例中使用的專案範本可確保完成此操作。</span><span class="sxs-lookup"><span data-stu-id="01706-139">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="01706-140">如果尚未透過應用程式初始移至建立網站資料庫,則在處理請求錯誤時,將取得*資料庫操作失敗*。</span><span class="sxs-lookup"><span data-stu-id="01706-140">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="01706-141">點按 **「應用遷移**」以建立資料庫並刷新以繼續結束錯誤。</span><span class="sxs-lookup"><span data-stu-id="01706-141">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="01706-142">後續步驟</span><span class="sxs-lookup"><span data-stu-id="01706-142">Next steps</span></span>

* <span data-ttu-id="01706-143">本文展示了如何使用 Twitter 進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="01706-143">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="01706-144">您可以採用類似的方法對[上一頁](xref:security/authentication/social/index)中列出的其他提供程式進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="01706-144">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="01706-145">將網站發布到 Azure Web 應用程式後,應`ConsumerSecret`重設 Twitter 開發人員門戶中的 。</span><span class="sxs-lookup"><span data-stu-id="01706-145">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="01706-146">在`Authentication:Twitter:ConsumerKey`Azure`Authentication:Twitter:ConsumerSecret`門戶中將 和 設置為應用程式設置。</span><span class="sxs-lookup"><span data-stu-id="01706-146">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="01706-147">配置系統設置為從環境變數讀取密鑰。</span><span class="sxs-lookup"><span data-stu-id="01706-147">The configuration system is set up to read keys from environment variables.</span></span>
