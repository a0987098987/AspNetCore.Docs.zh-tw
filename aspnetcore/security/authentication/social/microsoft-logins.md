---
title: 使用 ASP.NET Core 的 Microsoft 帳戶外部登入設定
author: rick-anderson
description: 這個範例會示範整合到現有的 ASP.NET Core 應用程式的 Microsoft 帳戶使用者驗證。
ms.author: riande
ms.custom: mvc
ms.date: 5/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 1c78cc957b6ff77c91c8ca4aef59a1cacd85a8ca
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517094"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="07b57-103">使用 ASP.NET Core 的 Microsoft 帳戶外部登入設定</span><span class="sxs-lookup"><span data-stu-id="07b57-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="07b57-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="07b57-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="07b57-105">這個範例會示範如何讓使用者使用他們使用 ASP.NET Core 2.2 專案上建立的 Microsoft 帳戶登入[上一頁](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="07b57-105">This sample shows you how to enable users to sign in with their Microsoft account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="07b57-106">在 Microsoft 開發人員入口網站中建立應用程式</span><span class="sxs-lookup"><span data-stu-id="07b57-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="07b57-107">瀏覽至[Azure 入口網站-應用程式註冊](https://go.microsoft.com/fwlink/?linkid=2083908)頁面上，建立或登入 Microsoft 帳戶：</span><span class="sxs-lookup"><span data-stu-id="07b57-107">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span></span>

<span data-ttu-id="07b57-108">如果您沒有 Microsoft 帳戶，請選取**建立一個**。</span><span class="sxs-lookup"><span data-stu-id="07b57-108">If you don't have a Microsoft account, select **Create one**.</span></span> <span data-ttu-id="07b57-109">登入之後將您重新導向**應用程式註冊**頁面：</span><span class="sxs-lookup"><span data-stu-id="07b57-109">After signing in you are redirected to the **App registrations** page:</span></span>

* <span data-ttu-id="07b57-110">選取**新增註冊**</span><span class="sxs-lookup"><span data-stu-id="07b57-110">Select **New registration**</span></span>
* <span data-ttu-id="07b57-111">請輸入**名稱**。</span><span class="sxs-lookup"><span data-stu-id="07b57-111">Enter a **Name**.</span></span>
* <span data-ttu-id="07b57-112">選取的選項**支援的帳戶類型**。</span><span class="sxs-lookup"><span data-stu-id="07b57-112">Select an option for **Supported account types**.</span></span>  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* <span data-ttu-id="07b57-113">底下**重新導向 URI**，輸入您的開發 URL 與`/signin-microsoft`附加。</span><span class="sxs-lookup"><span data-stu-id="07b57-113">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span></span> <span data-ttu-id="07b57-114">例如， `https://localhost:44389/signin-microsoft` 。</span><span class="sxs-lookup"><span data-stu-id="07b57-114">For example, `https://localhost:44389/signin-microsoft`.</span></span> <span data-ttu-id="07b57-115">稍後在此範例中所設定的 Microsoft 驗證配置會自動處理在要求`/signin-microsoft`實作 OAuth 流程的路由。</span><span class="sxs-lookup"><span data-stu-id="07b57-115">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span></span>
* <span data-ttu-id="07b57-116">選取**註冊**</span><span class="sxs-lookup"><span data-stu-id="07b57-116">Select **Register**</span></span>

### <a name="create-client-secret"></a><span data-ttu-id="07b57-117">建立用戶端祕密</span><span class="sxs-lookup"><span data-stu-id="07b57-117">Create client secret</span></span>

* <span data-ttu-id="07b57-118">在左窗格中，選取**憑證與祕密**。</span><span class="sxs-lookup"><span data-stu-id="07b57-118">In the left pane, select **Certificates & secrets**.</span></span>
* <span data-ttu-id="07b57-119">底下**用戶端祕密**，選取**新的用戶端祕密**</span><span class="sxs-lookup"><span data-stu-id="07b57-119">Under **Client secrets**, select **New client secret**</span></span>

  * <span data-ttu-id="07b57-120">加入用戶端祕密的描述。</span><span class="sxs-lookup"><span data-stu-id="07b57-120">Add a description for the client secret.</span></span>
  * <span data-ttu-id="07b57-121">選取 [**新增**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="07b57-121">Select the **Add** button.</span></span>

* <span data-ttu-id="07b57-122">底下**用戶端祕密**，複製用戶端祕密的值。</span><span class="sxs-lookup"><span data-stu-id="07b57-122">Under **Client secrets**, copy the value of the client secret.</span></span>

> [!NOTE]
> <span data-ttu-id="07b57-123">URI 區段`/signin-microsoft`設為預設回呼的 Microsoft 驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="07b57-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="07b57-124">您可以變更預設的回呼 URI 時設定 Microsoft 驗證中的介軟體，透過繼承[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性[MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions)類別。</span><span class="sxs-lookup"><span data-stu-id="07b57-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

## <a name="store-the-microsoft-client-id-and-client-secret"></a><span data-ttu-id="07b57-125">儲存 Microsoft 用戶端識別碼和用戶端祕密</span><span class="sxs-lookup"><span data-stu-id="07b57-125">Store the Microsoft client ID and client secret</span></span>

<span data-ttu-id="07b57-126">執行下列命令，以安全地儲存`ClientId`並`ClientSecret`使用[Secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="07b57-126">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```console
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

<span data-ttu-id="07b57-127">連結機密設定，例如 Microsoft`ClientId`並`ClientSecret`至您的應用程式的組態使用[Secret Manager](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="07b57-127">Link sensitive settings like Microsoft `ClientId` and `ClientSecret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="07b57-128">基於此範例的目的，名稱語彙基元`Authentication:Microsoft:ClientId`和`Authentication:Microsoft:ClientSecret`。</span><span class="sxs-lookup"><span data-stu-id="07b57-128">For the purposes of this sample, name the tokens `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="07b57-129">設定 Microsoft 帳戶驗證</span><span class="sxs-lookup"><span data-stu-id="07b57-129">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="07b57-130">新增 Microsoft 帳戶服務中的`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="07b57-130">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="07b57-131">請參閱[MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API 參考，如需有關 Microsoft 帳戶驗證所支援的組態選項。</span><span class="sxs-lookup"><span data-stu-id="07b57-131">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="07b57-132">這可用來要求不同使用者的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="07b57-132">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="07b57-133">使用 Microsoft 帳戶登入</span><span class="sxs-lookup"><span data-stu-id="07b57-133">Sign in with Microsoft Account</span></span>

<span data-ttu-id="07b57-134">執行，按一下 **登入**。</span><span class="sxs-lookup"><span data-stu-id="07b57-134">Run the and click **Log in**.</span></span> <span data-ttu-id="07b57-135">若要使用 Microsoft 登入的選項會出現。</span><span class="sxs-lookup"><span data-stu-id="07b57-135">An option to sign in with Microsoft appears.</span></span> <span data-ttu-id="07b57-136">當您按一下 Microsoft 時，您會重新導向到 Microsoft 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="07b57-136">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="07b57-137">之後您 Microsoft 帳戶登入 （如果您尚未登入） 將會提示您讓應用程式存取您的資訊：</span><span class="sxs-lookup"><span data-stu-id="07b57-137">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

<span data-ttu-id="07b57-138">點選**是**您就會重新導向回 web 站台，您可以在其中設定您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="07b57-138">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="07b57-139">您現在用來登入您的 Microsoft 認證：</span><span class="sxs-lookup"><span data-stu-id="07b57-139">You are now logged in using your Microsoft credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="07b57-140">疑難排解</span><span class="sxs-lookup"><span data-stu-id="07b57-140">Troubleshooting</span></span>

* <span data-ttu-id="07b57-141">如果 Microsoft 帳戶提供者會將您導向登入錯誤頁面，請注意錯誤標題和描述查詢字串參數直接跟`#`（主題標籤） 中的 Uri。</span><span class="sxs-lookup"><span data-stu-id="07b57-141">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="07b57-142">雖然指出使用 Microsoft 驗證問題似乎出現錯誤訊息，最常見的原因是您的應用程式 Uri 不符合任一**重新導向 Uri**指定**Web**平台.</span><span class="sxs-lookup"><span data-stu-id="07b57-142">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="07b57-143">如果身分識別未設定藉由呼叫`services.AddIdentity`中`ConfigureServices`，來驗證嘗試會導致*ArgumentException:必須提供 'SignInScheme' 選項*。</span><span class="sxs-lookup"><span data-stu-id="07b57-143">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="07b57-144">此範例中使用的專案範本可確保，這麼做。</span><span class="sxs-lookup"><span data-stu-id="07b57-144">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="07b57-145">如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時資料庫作業失敗*時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="07b57-145">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="07b57-146">點選**套用移轉**建立資料庫，並重新整理 以忽略錯誤繼續執行。</span><span class="sxs-lookup"><span data-stu-id="07b57-146">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="07b57-147">後續步驟</span><span class="sxs-lookup"><span data-stu-id="07b57-147">Next steps</span></span>

* <span data-ttu-id="07b57-148">本文章說明您可以向 Microsoft。</span><span class="sxs-lookup"><span data-stu-id="07b57-148">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="07b57-149">您可以依照類似的方法，來驗證與列在其他提供者[上一頁](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="07b57-149">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="07b57-150">一旦您發行至 Azure web 應用程式的網站，建立新的用戶端祕密中 Microsoft 開發人員入口網站。</span><span class="sxs-lookup"><span data-stu-id="07b57-150">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="07b57-151">設定`Authentication:Microsoft:ClientId`和`Authentication:Microsoft:ClientSecret`當做 Azure 入口網站中的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="07b57-151">Set the `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="07b57-152">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="07b57-152">The configuration system is set up to read keys from environment variables.</span></span>