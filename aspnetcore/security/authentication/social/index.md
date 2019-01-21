---
title: ASP.NET Core 中的 Facebook、Google 及外部提供者驗證
author: rick-anderson
description: 本教學課程示範如何搭配使用 OAuth 2.0 與外部驗證提供者，建立 ASP.NET Core 2.x 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 1/19/2019
uid: security/authentication/social/index
ms.openlocfilehash: 48dd8b772234ff18158423a36ed1716102bc2f31
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396138"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="d0d58-103">ASP.NET Core 中的 Facebook、Google 及外部提供者驗證</span><span class="sxs-lookup"><span data-stu-id="d0d58-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="d0d58-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d0d58-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d0d58-105">本教學課程會示範如何建置 ASP.NET Core 2.2 應用程式，讓使用者可使用 OAuth 2.0 以外部驗證提供者提供的認證登入。</span><span class="sxs-lookup"><span data-stu-id="d0d58-105">This tutorial demonstrates how to build an ASP.NET Core 2.2 app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="d0d58-106">下列各節涵蓋 [Facebook](xref:security/authentication/facebook-logins)、[Twitter](xref:security/authentication/twitter-logins)、[Google](xref:security/authentication/google-logins) 和 [Microsoft](xref:security/authentication/microsoft-logins) 的提供者。</span><span class="sxs-lookup"><span data-stu-id="d0d58-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="d0d58-107">您可透過 [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) 和 [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers) 這類協力廠商套件，取得其他提供者。</span><span class="sxs-lookup"><span data-stu-id="d0d58-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Facebook、Twitter、Google+ 和 Windows 的社交媒體圖示](index/_static/social.png)

<span data-ttu-id="d0d58-109">若使用者可以利用其現有的認證登入，一方面對使用者來說十分方便，另一方面則可將管理登入程序的許多複雜工作轉移給協力廠商。</span><span class="sxs-lookup"><span data-stu-id="d0d58-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="d0d58-110">如需社交登入如何帶動流量和客戶轉換的範例，請參閱 [Facebook](https://www.facebook.com/unsupportedbrowser) 和 [Twitter](https://dev.twitter.com/resources/case-studies) 的案例研究。</span><span class="sxs-lookup"><span data-stu-id="d0d58-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="d0d58-111">建立新的 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="d0d58-111">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="d0d58-112">在 Visual Studio 2017 中，從起始頁面建立新專案，或透過 [檔案] > [新增] >  [專案]。</span><span class="sxs-lookup"><span data-stu-id="d0d58-112">In Visual Studio 2017, create a new project from the Start Page, or via **File** > **New** > **Project**.</span></span>

* <span data-ttu-id="d0d58-113">選取在 [Visual C#] > [.NET Core] 類別中提供的 [ASP.NET Core Web 應用程式] 範本：</span><span class="sxs-lookup"><span data-stu-id="d0d58-113">Select the **ASP.NET Core Web Application** template available in the **Visual C#** > **.NET Core** category:</span></span>
* <span data-ttu-id="d0d58-114">選取 [變更驗證]，然後將驗證設定為 [個別使用者帳戶]。</span><span class="sxs-lookup"><span data-stu-id="d0d58-114">Select **Change Authentication** and set authentication to **Individual User Accounts**.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="d0d58-115">套用移轉</span><span class="sxs-lookup"><span data-stu-id="d0d58-115">Apply migrations</span></span>

* <span data-ttu-id="d0d58-116">執行應用程式並選取 [登錄] 連結。</span><span class="sxs-lookup"><span data-stu-id="d0d58-116">Run the app and select the **Register** link.</span></span>
* <span data-ttu-id="d0d58-117">輸入新帳戶的電子郵件和密碼，然後選取 [註冊]。</span><span class="sxs-lookup"><span data-stu-id="d0d58-117">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="d0d58-118">遵循指示以套用移轉。</span><span class="sxs-lookup"><span data-stu-id="d0d58-118">Follow the instructions to apply migrations.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="d0d58-119">使用 SecretManager 來儲存登入提供者指派的權杖</span><span class="sxs-lookup"><span data-stu-id="d0d58-119">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="d0d58-120">社交登入提供者會在註冊程序期間指派**應用程式識別碼**和**應用程式密碼**權杖。</span><span class="sxs-lookup"><span data-stu-id="d0d58-120">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="d0d58-121">確切權杖名稱會依提供者而有所不同。</span><span class="sxs-lookup"><span data-stu-id="d0d58-121">The exact token names vary by provider.</span></span> <span data-ttu-id="d0d58-122">這些權杖代表您的應用程式用來存取其 API 的認證。</span><span class="sxs-lookup"><span data-stu-id="d0d58-122">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="d0d58-123">這些權杖會組成「祕密」，在[祕密管理員](xref:security/app-secrets#secret-manager)的協助下連結到您的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="d0d58-123">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="d0d58-124">相較於在設定檔 (例如 *appsettings.json*) 中儲存權杖，祕密管理員是較安全的替代方案。</span><span class="sxs-lookup"><span data-stu-id="d0d58-124">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d0d58-125">祕密管理員僅供用於開發用途。</span><span class="sxs-lookup"><span data-stu-id="d0d58-125">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="d0d58-126">您可以透過 [Azure Key Vault 設定提供者](xref:security/key-vault-configuration)儲存及保護 Azure 測試與生產祕密。</span><span class="sxs-lookup"><span data-stu-id="d0d58-126">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="d0d58-127">請遵循 [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) (在 ASP.NET Core 開發過程中安全地儲存應用程式祕密) 主題中的步驟，儲存下方各個登入提供者指派的權杖。</span><span class="sxs-lookup"><span data-stu-id="d0d58-127">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="d0d58-128">設定應用程式所需的登入提供者</span><span class="sxs-lookup"><span data-stu-id="d0d58-128">Setup login providers required by your application</span></span>

<span data-ttu-id="d0d58-129">若要將應用程式設定為使用相應的提供者，請使用下列主題：</span><span class="sxs-lookup"><span data-stu-id="d0d58-129">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="d0d58-130">[Facebook](xref:security/authentication/facebook-logins) 指示</span><span class="sxs-lookup"><span data-stu-id="d0d58-130">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="d0d58-131">[Twitter](xref:security/authentication/twitter-logins) 指示</span><span class="sxs-lookup"><span data-stu-id="d0d58-131">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="d0d58-132">[Google](xref:security/authentication/google-logins) 指示</span><span class="sxs-lookup"><span data-stu-id="d0d58-132">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="d0d58-133">[Microsoft](xref:security/authentication/microsoft-logins) 指示</span><span class="sxs-lookup"><span data-stu-id="d0d58-133">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="d0d58-134">[其他提供者](xref:security/authentication/otherlogins)指示</span><span class="sxs-lookup"><span data-stu-id="d0d58-134">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="d0d58-135">選擇性地設定密碼</span><span class="sxs-lookup"><span data-stu-id="d0d58-135">Optionally set password</span></span>

<span data-ttu-id="d0d58-136">當您使用外部登入提供者註冊時，您並未向應用程式註冊密碼。</span><span class="sxs-lookup"><span data-stu-id="d0d58-136">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="d0d58-137">這樣可以減輕您建立網站密碼與記住密碼的壓力，但這也會讓您依賴外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="d0d58-137">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="d0d58-138">如果無法使用外部登入提供者，您就無法登入網站。</span><span class="sxs-lookup"><span data-stu-id="d0d58-138">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="d0d58-139">若要建立密碼，並使用您在外部提供者登入程序期間所設的電子郵件進行登入：</span><span class="sxs-lookup"><span data-stu-id="d0d58-139">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="d0d58-140">選取右上角的 [Hello &lt;電子郵件別名&gt;] 連結以巡覽至 [管理] 檢視。</span><span class="sxs-lookup"><span data-stu-id="d0d58-140">Select the **Hello &lt;email alias&gt;** link at the top right corner to navigate to the **Manage** view.</span></span>

![Web 應用程式的 [管理] 檢視](index/_static/pass1a.png)

* <span data-ttu-id="d0d58-142">選取 [建立]</span><span class="sxs-lookup"><span data-stu-id="d0d58-142">Select **Create**</span></span>

![[設定密碼] 頁面](index/_static/pass2a.png)

* <span data-ttu-id="d0d58-144">設定有效的密碼，以便使用此密碼與電子郵件進行登入。</span><span class="sxs-lookup"><span data-stu-id="d0d58-144">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d0d58-145">後續步驟</span><span class="sxs-lookup"><span data-stu-id="d0d58-145">Next steps</span></span>

* <span data-ttu-id="d0d58-146">本文介紹了外部驗證，並說明將外部登入新增至 ASP.NET Core 應用程式所需的必要條件。</span><span class="sxs-lookup"><span data-stu-id="d0d58-146">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="d0d58-147">請參考提供者的特定頁面，以設定應用程式所需的提供者登入項目。</span><span class="sxs-lookup"><span data-stu-id="d0d58-147">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="d0d58-148">您想要保存有關使用者與其存取和重新整理權杖的額外資料。</span><span class="sxs-lookup"><span data-stu-id="d0d58-148">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="d0d58-149">如需詳細資訊，請參閱<xref:security/authentication/social/additional-claims>。</span><span class="sxs-lookup"><span data-stu-id="d0d58-149">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
