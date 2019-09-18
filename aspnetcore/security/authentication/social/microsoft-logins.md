---
title: 使用 ASP.NET Core 的 Microsoft 帳戶外部登入設定
author: rick-anderson
description: 這個範例示範如何將 Microsoft 帳戶的使用者驗證整合到現有的 ASP.NET Core 應用程式中。
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 91ace293fd16cd180b3d5c183c637af6db1d08c3
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082333"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="444ec-103">使用 ASP.NET Core 的 Microsoft 帳戶外部登入設定</span><span class="sxs-lookup"><span data-stu-id="444ec-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="444ec-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="444ec-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="444ec-105">這個範例會示範如何使用在[上一頁](xref:security/authentication/social/index)建立的 ASP.NET Core 2.2 專案，讓使用者能夠用自己的 Microsoft 帳戶進行登入。</span><span class="sxs-lookup"><span data-stu-id="444ec-105">This sample shows you how to enable users to sign in with their Microsoft account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="444ec-106">在 Microsoft 開發人員入口網站中建立應用程式</span><span class="sxs-lookup"><span data-stu-id="444ec-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="444ec-107">流覽至[Azure 入口網站應用程式註冊](https://go.microsoft.com/fwlink/?linkid=2083908) 頁面，並建立或登入 Microsoft 帳戶：</span><span class="sxs-lookup"><span data-stu-id="444ec-107">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span></span>

<span data-ttu-id="444ec-108">如果您沒有 Microsoft 帳戶，請選取 [**建立一個**]。</span><span class="sxs-lookup"><span data-stu-id="444ec-108">If you don't have a Microsoft account, select **Create one**.</span></span> <span data-ttu-id="444ec-109">登入之後，系統會將您重新導向至 [**應用程式註冊**] 頁面：</span><span class="sxs-lookup"><span data-stu-id="444ec-109">After signing in you are redirected to the **App registrations** page:</span></span>

* <span data-ttu-id="444ec-110">選取 [**新增註冊**]</span><span class="sxs-lookup"><span data-stu-id="444ec-110">Select **New registration**</span></span>
* <span data-ttu-id="444ec-111">輸入 [**名稱**]。</span><span class="sxs-lookup"><span data-stu-id="444ec-111">Enter a **Name**.</span></span>
* <span data-ttu-id="444ec-112">選取**支援的帳戶類型**選項。</span><span class="sxs-lookup"><span data-stu-id="444ec-112">Select an option for **Supported account types**.</span></span>  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* <span data-ttu-id="444ec-113">在 [重新**導向 URI**] 底下，輸入`/signin-microsoft`附加的開發 URL。</span><span class="sxs-lookup"><span data-stu-id="444ec-113">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span></span> <span data-ttu-id="444ec-114">例如： `https://localhost:44389/signin-microsoft` 。</span><span class="sxs-lookup"><span data-stu-id="444ec-114">For example, `https://localhost:44389/signin-microsoft`.</span></span> <span data-ttu-id="444ec-115">稍後在此範例中設定的 Microsoft 驗證配置會自動處理路由`/signin-microsoft`上的要求，以執行 OAuth 流程。</span><span class="sxs-lookup"><span data-stu-id="444ec-115">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span></span>
* <span data-ttu-id="444ec-116">選取 [**註冊**]</span><span class="sxs-lookup"><span data-stu-id="444ec-116">Select **Register**</span></span>

### <a name="create-client-secret"></a><span data-ttu-id="444ec-117">建立用戶端密碼</span><span class="sxs-lookup"><span data-stu-id="444ec-117">Create client secret</span></span>

* <span data-ttu-id="444ec-118">在左窗格中，選取 [**憑證 & 密碼**]。</span><span class="sxs-lookup"><span data-stu-id="444ec-118">In the left pane, select **Certificates & secrets**.</span></span>
* <span data-ttu-id="444ec-119">在 [**用戶端密碼**] 底下，選取 [**新增用戶端密碼**]</span><span class="sxs-lookup"><span data-stu-id="444ec-119">Under **Client secrets**, select **New client secret**</span></span>

  * <span data-ttu-id="444ec-120">新增用戶端密碼的描述。</span><span class="sxs-lookup"><span data-stu-id="444ec-120">Add a description for the client secret.</span></span>
  * <span data-ttu-id="444ec-121">選取 [**新增**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="444ec-121">Select the **Add** button.</span></span>

* <span data-ttu-id="444ec-122">在 [**用戶端密碼**] 下，複製用戶端密碼的值。</span><span class="sxs-lookup"><span data-stu-id="444ec-122">Under **Client secrets**, copy the value of the client secret.</span></span>

> [!NOTE]
> <span data-ttu-id="444ec-123">URI 區段`/signin-microsoft`會設定為 Microsoft 驗證提供者的預設回呼。</span><span class="sxs-lookup"><span data-stu-id="444ec-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="444ec-124">您可以在設定 Microsoft 驗證中介軟體時，透過[MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions)類別的繼承[RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性來變更預設的回呼 URI。</span><span class="sxs-lookup"><span data-stu-id="444ec-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

## <a name="store-the-microsoft-client-id-and-client-secret"></a><span data-ttu-id="444ec-125">儲存 Microsoft 用戶端識別碼和用戶端秘密</span><span class="sxs-lookup"><span data-stu-id="444ec-125">Store the Microsoft client ID and client secret</span></span>

<span data-ttu-id="444ec-126">執行下列命令以安全地儲存`ClientId`和`ClientSecret`使用「[秘密管理員](xref:security/app-secrets)」：</span><span class="sxs-lookup"><span data-stu-id="444ec-126">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

<span data-ttu-id="444ec-127">使用[秘密管理員](xref:security/app-secrets)，將`ClientId` Microsoft `ClientSecret`和等機密設定連結至您的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="444ec-127">Link sensitive settings like Microsoft `ClientId` and `ClientSecret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="444ec-128">基於此範例的目的，請將權杖`Authentication:Microsoft:ClientId`命名為和。 `Authentication:Microsoft:ClientSecret`</span><span class="sxs-lookup"><span data-stu-id="444ec-128">For the purposes of this sample, name the tokens `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="444ec-129">設定 Microsoft 帳戶驗證</span><span class="sxs-lookup"><span data-stu-id="444ec-129">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="444ec-130">將 Microsoft 帳戶服務新增至`Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="444ec-130">Add the Microsoft Account service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="444ec-131">如需 Microsoft 帳戶驗證所支援之設定選項的詳細資訊，請參閱[MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API 參考。</span><span class="sxs-lookup"><span data-stu-id="444ec-131">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="444ec-132">這可用來要求不同使用者的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="444ec-132">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="444ec-133">使用 Microsoft 帳戶登入帳戶</span><span class="sxs-lookup"><span data-stu-id="444ec-133">Sign in with Microsoft Account</span></span>

<span data-ttu-id="444ec-134">執行，然後按一下 [**登入**]。</span><span class="sxs-lookup"><span data-stu-id="444ec-134">Run the and click **Log in**.</span></span> <span data-ttu-id="444ec-135">[使用 Microsoft 登入] 選項隨即出現。</span><span class="sxs-lookup"><span data-stu-id="444ec-135">An option to sign in with Microsoft appears.</span></span> <span data-ttu-id="444ec-136">當您按一下 [Microsoft] 時，系統會將您重新導向至 Microsoft 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="444ec-136">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="444ec-137">使用您的 Microsoft 帳戶（如果尚未登入）登入之後，系統會提示您讓應用程式存取您的資訊：</span><span class="sxs-lookup"><span data-stu-id="444ec-137">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

<span data-ttu-id="444ec-138">請按一下 [**是]** ，系統會將您重新導向至可設定電子郵件的網站。</span><span class="sxs-lookup"><span data-stu-id="444ec-138">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="444ec-139">您現在已使用 Microsoft 認證登入：</span><span class="sxs-lookup"><span data-stu-id="444ec-139">You are now logged in using your Microsoft credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="444ec-140">疑難排解</span><span class="sxs-lookup"><span data-stu-id="444ec-140">Troubleshooting</span></span>

* <span data-ttu-id="444ec-141">如果 Microsoft 帳戶提供者將您重新導向至 [登入錯誤] 頁面，請注意 Uri 中`#` （主題標籤）後面的 [錯誤標題] 和 [描述] 查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="444ec-141">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="444ec-142">雖然錯誤訊息似乎表示 Microsoft 驗證有問題，但最常見的原因是您的應用程式 Uri 不符合為**Web**平臺指定的任何重新**導向 uri** 。</span><span class="sxs-lookup"><span data-stu-id="444ec-142">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="444ec-143">如果未透過在中`services.AddIdentity` `ConfigureServices`呼叫*來設定身分識別，則嘗試進行驗證會導致 ArgumentException：必須提供*' SignInScheme ' 選項。</span><span class="sxs-lookup"><span data-stu-id="444ec-143">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="444ec-144">此範例中使用的專案範本可確保完成此作業。</span><span class="sxs-lookup"><span data-stu-id="444ec-144">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="444ec-145">如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時資料庫作業失敗*時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="444ec-145">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="444ec-146">點選**套用移轉**建立資料庫，並重新整理 以忽略錯誤繼續執行。</span><span class="sxs-lookup"><span data-stu-id="444ec-146">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="444ec-147">後續步驟</span><span class="sxs-lookup"><span data-stu-id="444ec-147">Next steps</span></span>

* <span data-ttu-id="444ec-148">本文說明了您可以如何向 Microsoft 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="444ec-148">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="444ec-149">您可以依照類似的方法，來驗證與列在其他提供者[上一頁](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="444ec-149">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="444ec-150">一旦您將網站發佈至 Azure web 應用程式，請在 Microsoft 開發人員入口網站中建立新的用戶端密碼。</span><span class="sxs-lookup"><span data-stu-id="444ec-150">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="444ec-151">設定`Authentication:Microsoft:ClientId`和`Authentication:Microsoft:ClientSecret`當做 Azure 入口網站中的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="444ec-151">Set the `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="444ec-152">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="444ec-152">The configuration system is set up to read keys from environment variables.</span></span>