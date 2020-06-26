---
title: ASP.NET Core 中的 Facebook、Google 及外部提供者驗證
author: rick-anderson
description: 本教學課程示範如何使用 OAuth 2.0 搭配外部驗證提供者來建立 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/social/index
ms.openlocfilehash: a2c068df85290a59d45432cb168b2c882b59405b
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85405363"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="fec13-103">ASP.NET Core 中的 Facebook、Google 及外部提供者驗證</span><span class="sxs-lookup"><span data-stu-id="fec13-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="fec13-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fec13-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fec13-105">本教學課程示範如何建立 ASP.NET Core 3.0 應用程式，讓使用者能夠使用 OAuth 2.0 搭配外部驗證提供者的認證來登入。</span><span class="sxs-lookup"><span data-stu-id="fec13-105">This tutorial demonstrates how to build an ASP.NET Core 3.0 app that enables users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="fec13-106">下列各節涵蓋[Facebook](xref:security/authentication/facebook-logins)、 [Twitter](xref:security/authentication/twitter-logins)、 [Google](xref:security/authentication/google-logins)和[Microsoft](xref:security/authentication/microsoft-logins)提供者，並使用本文中建立的入門專案。</span><span class="sxs-lookup"><span data-stu-id="fec13-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections and use the starter project created in this article.</span></span> <span data-ttu-id="fec13-107">您可透過 [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) 和 [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers) 這類協力廠商套件，取得其他提供者。</span><span class="sxs-lookup"><span data-stu-id="fec13-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

<span data-ttu-id="fec13-108">讓使用者透過現有認證登入：</span><span class="sxs-lookup"><span data-stu-id="fec13-108">Enabling users to sign in with their existing credentials:</span></span>

* <span data-ttu-id="fec13-109">對使用者而言十分方便。</span><span class="sxs-lookup"><span data-stu-id="fec13-109">Is convenient for the users.</span></span>
* <span data-ttu-id="fec13-110">可將複雜的登入程序管理作業轉移至協力廠商。</span><span class="sxs-lookup"><span data-stu-id="fec13-110">Shifts many of the complexities of managing the sign-in process onto a third party.</span></span>

<span data-ttu-id="fec13-111">如需社交登入如何帶動流量和客戶轉換的範例，請參閱 [Facebook](https://www.facebook.com/unsupportedbrowser) 和 [Twitter](https://dev.twitter.com/resources/case-studies) 的案例研究。</span><span class="sxs-lookup"><span data-stu-id="fec13-111">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="fec13-112">建立新的 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="fec13-112">Create a New ASP.NET Core Project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="fec13-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fec13-113">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fec13-114">建立新專案。</span><span class="sxs-lookup"><span data-stu-id="fec13-114">Create a new project.</span></span>
* <span data-ttu-id="fec13-115">選取 [ASP.NET Core Web 應用程式]\*\*\*\* 和 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="fec13-115">Select **ASP.NET Core Web Application** and **Next**.</span></span>
* <span data-ttu-id="fec13-116">提供**專案名稱**並確認或變更**位置**。</span><span class="sxs-lookup"><span data-stu-id="fec13-116">Provide a **Project name** and confirm or change the **Location**.</span></span> <span data-ttu-id="fec13-117">選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="fec13-117">Select **Create**.</span></span>
* <span data-ttu-id="fec13-118">在下拉式（**ASP.NET Core {X. Y}**）中選取最新版本的 ASP.NET Core，然後選取 [ **Web 應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="fec13-118">Select the latest version of ASP.NET Core in the drop-down (**ASP.NET Core {X.Y}**), and then select **Web Application**.</span></span>
* <span data-ttu-id="fec13-119">選取 [驗證]\*\*\*\* 下的 [變更]\*\*\*\*，並將驗證設定為 [個別使用者帳戶]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="fec13-119">Under **Authentication**, select **Change** and set the authentication to **Individual User Accounts**.</span></span> <span data-ttu-id="fec13-120">選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="fec13-120">Select **OK**.</span></span>
* <span data-ttu-id="fec13-121">在 [建立新的 ASP.NET Core Web 應用程式]\*\*\*\* 視窗中選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="fec13-121">In the **Create a new ASP.NET Core Web Application** window, select **Create**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="fec13-122">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="fec13-122">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="fec13-123">開啟終端機。</span><span class="sxs-lookup"><span data-stu-id="fec13-123">Open the terminal.</span></span>  <span data-ttu-id="fec13-124">對於 Visual Studio Code，您可以開啟[整合式終端](https://code.visualstudio.com/docs/editor/integrated-terminal)機。</span><span class="sxs-lookup"><span data-stu-id="fec13-124">For Visual Studio Code you can open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="fec13-125">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="fec13-125">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="fec13-126">針對 Windows，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="fec13-126">For Windows, run the following command:</span></span>

  ```dotnetcli
  dotnet new webapp -o WebApp1 -au Individual -uld
  ```

  <span data-ttu-id="fec13-127">針對 macOS 和 Linux，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="fec13-127">For macOS and Linux, run the following command:</span></span>

  ```dotnetcli
  dotnet new webapp -o WebApp1 -au Individual
  ```

  * <span data-ttu-id="fec13-128">`dotnet new`命令會 Razor 在*WebApp1*資料夾中建立新的頁面專案。</span><span class="sxs-lookup"><span data-stu-id="fec13-128">The `dotnet new` command creates a new Razor Pages project in the *WebApp1* folder.</span></span>
  * <span data-ttu-id="fec13-129">`-au Individual` 會建立程式碼以進行個別驗證。</span><span class="sxs-lookup"><span data-stu-id="fec13-129">`-au Individual` creates the code for Individual authentication.</span></span>
  * <span data-ttu-id="fec13-130">`-uld`使用 LocalDB，這是 Windows SQL Server Express 的輕量版本。</span><span class="sxs-lookup"><span data-stu-id="fec13-130">`-uld` uses LocalDB, a lightweight version of SQL Server Express for Windows.</span></span> <span data-ttu-id="fec13-131">省略 `-uld` 以使用 SQLite。</span><span class="sxs-lookup"><span data-stu-id="fec13-131">Omit `-uld` to use SQLite.</span></span>
  * <span data-ttu-id="fec13-132">`code` 命令會在新的 Visual Studio Code 執行個體中開啟 [WebApp1]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="fec13-132">The `code` command opens the *WebApp1* folder in a new instance of Visual Studio Code.</span></span>

---

## <a name="apply-migrations"></a><span data-ttu-id="fec13-133">套用移轉</span><span class="sxs-lookup"><span data-stu-id="fec13-133">Apply migrations</span></span>

* <span data-ttu-id="fec13-134">執行應用程式並選取 [登錄]\*\*\*\* 連結。</span><span class="sxs-lookup"><span data-stu-id="fec13-134">Run the app and select the **Register** link.</span></span>
* <span data-ttu-id="fec13-135">輸入新帳戶的電子郵件和密碼，然後選取 [註冊]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="fec13-135">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="fec13-136">遵循指示以套用移轉。</span><span class="sxs-lookup"><span data-stu-id="fec13-136">Follow the instructions to apply migrations.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="fec13-137">使用 SecretManager 來儲存登入提供者指派的權杖</span><span class="sxs-lookup"><span data-stu-id="fec13-137">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="fec13-138">社交登入提供者會在註冊程序期間指派**應用程式識別碼**和**應用程式密碼**權杖。</span><span class="sxs-lookup"><span data-stu-id="fec13-138">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="fec13-139">確切權杖名稱會依提供者而有所不同。</span><span class="sxs-lookup"><span data-stu-id="fec13-139">The exact token names vary by provider.</span></span> <span data-ttu-id="fec13-140">這些權杖代表您的應用程式用來存取其 API 的認證。</span><span class="sxs-lookup"><span data-stu-id="fec13-140">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="fec13-141">這些權杖會組成「祕密」，在[祕密管理員](xref:security/app-secrets#secret-manager)的協助下連結到您的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="fec13-141">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="fec13-142">相較於在設定檔 (例如 *appsettings.json*) 中儲存權杖，祕密管理員是較安全的替代方案。</span><span class="sxs-lookup"><span data-stu-id="fec13-142">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fec13-143">祕密管理員僅供用於開發用途。</span><span class="sxs-lookup"><span data-stu-id="fec13-143">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="fec13-144">您可以透過 [Azure Key Vault 設定提供者](xref:security/key-vault-configuration)儲存及保護 Azure 測試與生產祕密。</span><span class="sxs-lookup"><span data-stu-id="fec13-144">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="fec13-145">請遵循 [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) (在 ASP.NET Core 開發過程中安全地儲存應用程式祕密) 主題中的步驟，儲存下方各個登入提供者指派的權杖。</span><span class="sxs-lookup"><span data-stu-id="fec13-145">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="fec13-146">設定應用程式所需的登入提供者</span><span class="sxs-lookup"><span data-stu-id="fec13-146">Setup login providers required by your application</span></span>

<span data-ttu-id="fec13-147">若要將應用程式設定為使用相應的提供者，請使用下列主題：</span><span class="sxs-lookup"><span data-stu-id="fec13-147">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="fec13-148">[Facebook](xref:security/authentication/facebook-logins) 指示</span><span class="sxs-lookup"><span data-stu-id="fec13-148">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="fec13-149">[Twitter](xref:security/authentication/twitter-logins) 指示</span><span class="sxs-lookup"><span data-stu-id="fec13-149">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="fec13-150">[Google](xref:security/authentication/google-logins) 指示</span><span class="sxs-lookup"><span data-stu-id="fec13-150">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="fec13-151">[Microsoft](xref:security/authentication/microsoft-logins) 指示</span><span class="sxs-lookup"><span data-stu-id="fec13-151">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="fec13-152">[其他提供者](xref:security/authentication/otherlogins)指示</span><span class="sxs-lookup"><span data-stu-id="fec13-152">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="fec13-153">選擇性地設定密碼</span><span class="sxs-lookup"><span data-stu-id="fec13-153">Optionally set password</span></span>

<span data-ttu-id="fec13-154">當您使用外部登入提供者註冊時，您並未向應用程式註冊密碼。</span><span class="sxs-lookup"><span data-stu-id="fec13-154">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="fec13-155">這樣可以減輕您建立網站密碼與記住密碼的壓力，但這也會讓您依賴外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="fec13-155">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="fec13-156">如果無法使用外部登入提供者，您就無法登入網站。</span><span class="sxs-lookup"><span data-stu-id="fec13-156">If the external login provider is unavailable, you won't be able to sign in to the web site.</span></span>

<span data-ttu-id="fec13-157">若要建立密碼，並使用您在外部提供者登入程序期間所設的電子郵件進行登入：</span><span class="sxs-lookup"><span data-stu-id="fec13-157">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="fec13-158">選取右上角的 [Hello &lt;電子郵件別名&gt;]\*\*\*\* 連結以瀏覽至 [管理]\*\*\*\* 檢視。</span><span class="sxs-lookup"><span data-stu-id="fec13-158">Select the **Hello &lt;email alias&gt;** link at the top-right corner to navigate to the **Manage** view.</span></span>

![Web 應用程式的 [管理] 檢視](index/_static/pass1a.png)

* <span data-ttu-id="fec13-160">選取 [建立] </span><span class="sxs-lookup"><span data-stu-id="fec13-160">Select **Create**</span></span>

![[設定密碼] 頁面](index/_static/pass2a.png)

* <span data-ttu-id="fec13-162">設定有效的密碼，以便使用此密碼與電子郵件進行登入。</span><span class="sxs-lookup"><span data-stu-id="fec13-162">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fec13-163">後續步驟</span><span class="sxs-lookup"><span data-stu-id="fec13-163">Next steps</span></span>

* <span data-ttu-id="fec13-164">如需如何自訂登入按鈕的相關資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/10563)。</span><span class="sxs-lookup"><span data-stu-id="fec13-164">See [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/10563) for information on how to customize the login buttons.</span></span>
* <span data-ttu-id="fec13-165">本文介紹了外部驗證，並說明將外部登入新增至 ASP.NET Core 應用程式所需的必要條件。</span><span class="sxs-lookup"><span data-stu-id="fec13-165">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>
* <span data-ttu-id="fec13-166">請參考提供者的特定頁面，以設定應用程式所需的提供者登入項目。</span><span class="sxs-lookup"><span data-stu-id="fec13-166">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>
* <span data-ttu-id="fec13-167">您想要保存有關使用者與其存取和重新整理權杖的額外資料。</span><span class="sxs-lookup"><span data-stu-id="fec13-167">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="fec13-168">如需詳細資訊，請參閱<xref:security/authentication/social/additional-claims>。</span><span class="sxs-lookup"><span data-stu-id="fec13-168">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
