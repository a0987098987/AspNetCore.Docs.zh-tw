---
title: 微軟帳戶外部登錄設置與ASP.NET核心
author: rick-anderson
description: 此示例演示了 Microsoft 帳戶使用者身份驗證整合到現有ASP.NET核心應用。
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 12c86456dad86731b86487a3a4dd725f36677002
ms.sourcegitcommit: f29a12486313e38e0163a643d8a97c8cecc7e871
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/14/2020
ms.locfileid: "81384050"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="1de7a-103">微軟帳戶外部登錄設置與ASP.NET核心</span><span class="sxs-lookup"><span data-stu-id="1de7a-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="1de7a-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1de7a-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1de7a-105">此示例演示如何允許使用者使用[上一頁上](xref:security/authentication/social/index)創建的 ASP.NET Core 3.0 專案使用他們的工作、學校或個人 Microsoft 帳戶登錄。</span><span class="sxs-lookup"><span data-stu-id="1de7a-105">This sample shows you how to enable users to sign in with their work, school, or personal Microsoft account using the ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="1de7a-106">在 Microsoft 開發人員門戶中建立應用</span><span class="sxs-lookup"><span data-stu-id="1de7a-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="1de7a-107">將[Microsoft.AspNetCore.身份驗證.Microsoft 帳戶](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount/)NuGet 包添加到專案中。</span><span class="sxs-lookup"><span data-stu-id="1de7a-107">Add the [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount/) NuGet package to the project.</span></span>
* <span data-ttu-id="1de7a-108">導覽[到 Azure 門戶 - 應用程式註冊](https://go.microsoft.com/fwlink/?linkid=2083908)頁面,然後建立或登入 Microsoft 帳戶:</span><span class="sxs-lookup"><span data-stu-id="1de7a-108">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span></span>

<span data-ttu-id="1de7a-109">如果您沒有 Microsoft 帳戶,請選擇「**創建一個**」。。</span><span class="sxs-lookup"><span data-stu-id="1de7a-109">If you don't have a Microsoft account, select **Create one**.</span></span> <span data-ttu-id="1de7a-110">登錄後,您將重定向到**應用註冊**頁面:</span><span class="sxs-lookup"><span data-stu-id="1de7a-110">After signing in, you are redirected to the **App registrations** page:</span></span>

* <span data-ttu-id="1de7a-111">選擇**新註冊**</span><span class="sxs-lookup"><span data-stu-id="1de7a-111">Select **New registration**</span></span>
* <span data-ttu-id="1de7a-112">輸入**名稱**。</span><span class="sxs-lookup"><span data-stu-id="1de7a-112">Enter a **Name**.</span></span>
* <span data-ttu-id="1de7a-113">為**受支援的帳戶類型**選擇一個選項。</span><span class="sxs-lookup"><span data-stu-id="1de7a-113">Select an option for **Supported account types**.</span></span>  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts. It took 24 hours after setting this up for the keys to work -->
* <span data-ttu-id="1de7a-114">在**重定向 URI**下`/signin-microsoft`,輸入追加的開發 URL。</span><span class="sxs-lookup"><span data-stu-id="1de7a-114">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span></span> <span data-ttu-id="1de7a-115">例如： `https://localhost:5001/signin-microsoft` 。</span><span class="sxs-lookup"><span data-stu-id="1de7a-115">For example, `https://localhost:5001/signin-microsoft`.</span></span> <span data-ttu-id="1de7a-116">本示例中稍後配置的 Microsoft 身份驗證方案將自動處理路`/signin-microsoft`由中 實現 OAuth 流的請求。</span><span class="sxs-lookup"><span data-stu-id="1de7a-116">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span></span>
* <span data-ttu-id="1de7a-117">選擇 **"註冊"**</span><span class="sxs-lookup"><span data-stu-id="1de7a-117">Select **Register**</span></span>

### <a name="create-client-secret"></a><span data-ttu-id="1de7a-118">建立用戶端機密</span><span class="sxs-lookup"><span data-stu-id="1de7a-118">Create client secret</span></span>

* <span data-ttu-id="1de7a-119">在左側窗格中,選擇 **「證書&機密**」。</span><span class="sxs-lookup"><span data-stu-id="1de7a-119">In the left pane, select **Certificates & secrets**.</span></span>
* <span data-ttu-id="1de7a-120">在 **"用戶端機密**"下,選擇 **"新用戶端機密**"</span><span class="sxs-lookup"><span data-stu-id="1de7a-120">Under **Client secrets**, select **New client secret**</span></span>

  * <span data-ttu-id="1de7a-121">添加用戶端機密的說明。</span><span class="sxs-lookup"><span data-stu-id="1de7a-121">Add a description for the client secret.</span></span>
  * <span data-ttu-id="1de7a-122">選擇「**新增**」 按鈕。</span><span class="sxs-lookup"><span data-stu-id="1de7a-122">Select the **Add** button.</span></span>

* <span data-ttu-id="1de7a-123">在**用戶端機密**下,複製用戶端機密的值。</span><span class="sxs-lookup"><span data-stu-id="1de7a-123">Under **Client secrets**, copy the value of the client secret.</span></span>

<span data-ttu-id="1de7a-124">URI`/signin-microsoft`段設定為 Microsoft 身份驗證提供程式的預設回調。</span><span class="sxs-lookup"><span data-stu-id="1de7a-124">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="1de7a-125">您可以透過 Microsoft[帳戶選項](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions)類別的繼承[遠端身份驗證選項.callbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性設定 Microsoft 身份驗證中間件時更改預設回檔 URI。</span><span class="sxs-lookup"><span data-stu-id="1de7a-125">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

## <a name="store-the-microsoft-client-id-and-secret"></a><span data-ttu-id="1de7a-126">儲存 Microsoft 客戶端 ID 和機器</span><span class="sxs-lookup"><span data-stu-id="1de7a-126">Store the Microsoft client ID and secret</span></span>

<span data-ttu-id="1de7a-127">使用[密鑰管理員](xref:security/app-secrets)儲存敏感設定,如Microsoft用戶端ID和機密值。</span><span class="sxs-lookup"><span data-stu-id="1de7a-127">Store sensitive settings such as the Microsoft client ID and secret values with [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="1de7a-128">對於此示例,請使用以下步驟:</span><span class="sxs-lookup"><span data-stu-id="1de7a-128">For this sample, use the following steps:</span></span>

1. <span data-ttu-id="1de7a-129">根據[啟用密鑰存儲](xref:security/app-secrets#enable-secret-storage)的指令初始化了機密存儲的專案。</span><span class="sxs-lookup"><span data-stu-id="1de7a-129">Initialize the project for secret storage per the instructions at [Enable secret storage](xref:security/app-secrets#enable-secret-storage).</span></span>
1. <span data-ttu-id="1de7a-130">使用金鑰`Authentication:Microsoft:ClientId`與將敏感設定儲存在本地機密儲存中`Authentication:Microsoft:ClientSecret`:</span><span class="sxs-lookup"><span data-stu-id="1de7a-130">Store the sensitive settings in the local secret store with the secret keys `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`:</span></span>

    ```dotnetcli
    dotnet user-secrets set "Authentication:Microsoft:ClientId" "<client-id>"
    dotnet user-secrets set "Authentication:Microsoft:ClientSecret" "<client-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="1de7a-131">設定 Microsoft 帳戶身份驗證</span><span class="sxs-lookup"><span data-stu-id="1de7a-131">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="1de7a-132">將 Microsoft 帳號服務新增`Startup.ConfigureServices`到 :</span><span class="sxs-lookup"><span data-stu-id="1de7a-132">Add the Microsoft Account service to the `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupMS3x.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

<span data-ttu-id="1de7a-133">有關 Microsoft 帳戶身份驗證支援的配置選項的詳細資訊,請參閱[Microsoft 帳戶選項](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions)API 參考。</span><span class="sxs-lookup"><span data-stu-id="1de7a-133">For more information about configuration options supported by Microsoft Account authentication, see the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference.</span></span> <span data-ttu-id="1de7a-134">這可用於請求有關使用者的不同資訊。</span><span class="sxs-lookup"><span data-stu-id="1de7a-134">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="1de7a-135">使用 Microsoft 帳戶登入</span><span class="sxs-lookup"><span data-stu-id="1de7a-135">Sign in with Microsoft Account</span></span>

<span data-ttu-id="1de7a-136">運行應用並按下「**登錄**」。</span><span class="sxs-lookup"><span data-stu-id="1de7a-136">Run the app and click **Log in**.</span></span> <span data-ttu-id="1de7a-137">將顯示一個與 Microsoft 登錄的選項。</span><span class="sxs-lookup"><span data-stu-id="1de7a-137">An option to sign in with Microsoft appears.</span></span> <span data-ttu-id="1de7a-138">按下 Microsoft 時,您將重定向到 Microsoft 進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="1de7a-138">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="1de7a-139">使用 Microsoft 帳號登入後,系統將提示您讓應用程式存取您的資訊:</span><span class="sxs-lookup"><span data-stu-id="1de7a-139">After signing in with your Microsoft Account, you will be prompted to let the app access your info:</span></span>

<span data-ttu-id="1de7a-140">點按 **"是**",您將被重定向回網站,您可以在該網站設置電子郵件。</span><span class="sxs-lookup"><span data-stu-id="1de7a-140">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="1de7a-141">您現在使用 Microsoft 認證登入:</span><span class="sxs-lookup"><span data-stu-id="1de7a-141">You are now logged in using your Microsoft credentials:</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="1de7a-142">疑難排解</span><span class="sxs-lookup"><span data-stu-id="1de7a-142">Troubleshooting</span></span>

* <span data-ttu-id="1de7a-143">如果 Microsoft 帳戶提供程式將您重定向到登入錯誤頁,請記下錯誤標題和說明查詢字串參數,直接遵循`#`Uri 中的 (hashtag)。</span><span class="sxs-lookup"><span data-stu-id="1de7a-143">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="1de7a-144">儘管錯誤消息似乎表明 Microsoft 身份驗證有問題,但最常見的原因是應用程式 Uri 與為**Web**平臺指定的重定向**URI**不匹配。</span><span class="sxs-lookup"><span data-stu-id="1de7a-144">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="1de7a-145">如果未透過除錯`services.AddIdentity`設定識別`ConfigureServices`,則試著認證會導致*參數異常:必須提供「SignInScheme」選項*。</span><span class="sxs-lookup"><span data-stu-id="1de7a-145">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="1de7a-146">此示例中使用的專案範本可確保完成此操作。</span><span class="sxs-lookup"><span data-stu-id="1de7a-146">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="1de7a-147">如果尚未透過應用程式初始移至建立網站資料庫,則在處理請求錯誤時,將取得*資料庫操作失敗*。</span><span class="sxs-lookup"><span data-stu-id="1de7a-147">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="1de7a-148">點按 **「應用遷移**」以建立資料庫並刷新以繼續結束錯誤。</span><span class="sxs-lookup"><span data-stu-id="1de7a-148">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1de7a-149">後續步驟</span><span class="sxs-lookup"><span data-stu-id="1de7a-149">Next steps</span></span>

* <span data-ttu-id="1de7a-150">本文展示了如何使用 Microsoft 進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="1de7a-150">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="1de7a-151">您可以採用類似的方法對[上一頁](xref:security/authentication/social/index)中列出的其他提供程式進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="1de7a-151">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="1de7a-152">將網站發佈到 Azure Web 應用後,在 Microsoft 開發人員門戶中創建新的用戶端機密。</span><span class="sxs-lookup"><span data-stu-id="1de7a-152">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="1de7a-153">在`Authentication:Microsoft:ClientId`Azure`Authentication:Microsoft:ClientSecret`門戶中將 和 設置為應用程式設置。</span><span class="sxs-lookup"><span data-stu-id="1de7a-153">Set the `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="1de7a-154">配置系統設置為從環境變數讀取密鑰。</span><span class="sxs-lookup"><span data-stu-id="1de7a-154">The configuration system is set up to read keys from environment variables.</span></span>
