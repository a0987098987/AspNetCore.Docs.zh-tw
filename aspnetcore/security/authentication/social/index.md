---
title: ASP.NET Core 中的 Facebook、Google 及外部提供者驗證
author: rick-anderson
description: 此教學課程示範如何搭配使用 OAuth 2.0 與外部驗證提供者，建立 ASP.NET Core 2.x 應用程式。
ms.author: riande
ms.date: 11/01/2016
uid: security/authentication/social/index
ms.openlocfilehash: 48a01ab241f9a6ad6ad3fb2ee9e210f459075c33
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336116"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="4ae9c-103">ASP.NET Core 中的 Facebook、Google 及外部提供者驗證</span><span class="sxs-lookup"><span data-stu-id="4ae9c-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="4ae9c-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4ae9c-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4ae9c-105">此教學課程會示範如何建立 ASP.NET Core 2.x 應用程式，讓使用者使用 OAuth 2.0 與來自外部驗證提供者的認證進行登入。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-105">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="4ae9c-106">下列各節涵蓋 [Facebook](xref:security/authentication/facebook-logins)、[Twitter](xref:security/authentication/twitter-logins)、[Google](xref:security/authentication/google-logins) 和 [Microsoft](xref:security/authentication/microsoft-logins) 的提供者。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="4ae9c-107">您可透過 [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) 和 [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers) 這類協力廠商套件，取得其他提供者。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Facebook、Twitter、Google+ 和 Windows 的社交媒體圖示](index/_static/social.png)

<span data-ttu-id="4ae9c-109">若使用者可以利用其現有的認證登入，一方面對使用者來說十分方便，另一方面則可將管理登入程序的許多複雜工作轉移給協力廠商。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="4ae9c-110">如需社交登入如何帶動流量和客戶轉換的範例，請參閱 [Facebook](https://www.facebook.com/unsupportedbrowser) 和 [Twitter](https://dev.twitter.com/resources/case-studies) 的案例研究。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="4ae9c-111">注意：此處提供的套件摘錄了大量 OAuth 驗證流程的複雜內容，但在您進行疑難排解時，可能有必要了解詳細資料。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-111">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="4ae9c-112">可用的資源很多；例如，您可以參閱 [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) (OAuth 2 簡介) 或 [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/) (了解 OAuth 2)。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-112">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="4ae9c-113">您也可以查看[適用於提供者套件的 ASP.NET Core 原始程式碼](https://github.com/aspnet/Security/tree/master/src)，來解決部分問題。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-113">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/master/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="4ae9c-114">建立新的 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="4ae9c-114">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="4ae9c-115">在 Visual Studio 2017 中，您可以從 [開始] 頁面來建立新的專案，或透過 [檔案] > [新增] > [專案] 來進行。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-115">In Visual Studio 2017, create a new project from the Start Page, or via **File > New > Project**.</span></span>

* <span data-ttu-id="4ae9c-116">在 [Visual C#] > [.NET Core] 類別中，選取可用的 [ASP.NET Core Web 應用程式] 範本：</span><span class="sxs-lookup"><span data-stu-id="4ae9c-116">Select the **ASP.NET Core Web Application** template available in **Visual C# > .NET Core** category:</span></span>

![[新增專案] 對話](index/_static/new-project.png)

* <span data-ttu-id="4ae9c-118">點選 [Web 應用程式]，並驗證已將 [驗證] 設定為 [個別使用者帳戶]：</span><span class="sxs-lookup"><span data-stu-id="4ae9c-118">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![[新增 Web 應用程式] 對話方塊](index/_static/select-project.png)

<span data-ttu-id="4ae9c-120">注意：此教學課程適用於 ASP.NET Core 2.0 SDK 版本；您可以在精靈的頂端選取此版本。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-120">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="4ae9c-121">套用移轉</span><span class="sxs-lookup"><span data-stu-id="4ae9c-121">Apply migrations</span></span>

* <span data-ttu-id="4ae9c-122">執行應用程式並選取 [登入] 連結。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-122">Run the app and select the **Log in** link.</span></span>
* <span data-ttu-id="4ae9c-123">選取 [以新使用者身分註冊] 連結。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-123">Select the **Register as a new user** link.</span></span>
* <span data-ttu-id="4ae9c-124">輸入新帳戶的電子郵件和密碼，然後選取 [註冊]。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-124">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="4ae9c-125">遵循指示以套用移轉。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-125">Follow the instructions to apply migrations.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="4ae9c-126">需要 SSL</span><span class="sxs-lookup"><span data-stu-id="4ae9c-126">Require SSL</span></span>

<span data-ttu-id="4ae9c-127">OAuth 2.0 要求使用 SSL 以透過 HTTPS 通訊協定進行驗證。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-127">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="4ae9c-128">注意：如果您是使用適用於 ASP.NET Core 2.x 的 [Web 應用程式] 或 [Web API] 專案範本來建立專案，且專案精靈的 [變更驗證] 對話方塊已如上所示選取 [個別使用者帳戶] 選項的話，則系統會自動將該專案設為啟用 SSL，並使用 https URL 來啟動。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-128">Note: Projects created using **Web Application** or **Web API** project templates for ASP.NET Core 2.x are automatically configured to enable SSL and launch with https URL if the **Individual User Accounts** option was selected on **Change Authentication dialog** in the project wizard as shown above.</span></span>

* <span data-ttu-id="4ae9c-129">遵循[在 ASP.NET Core 應用程式中強制執行 SSL](xref:security/enforcing-ssl) 主題中的步驟，要求網站使用 SSL。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-129">Require SSL on your site by following the steps in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) topic.</span></span>

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="4ae9c-130">使用 SecretManager 來儲存登入提供者指派的權杖</span><span class="sxs-lookup"><span data-stu-id="4ae9c-130">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="4ae9c-131">社交登入提供者會在註冊程序期間指派**應用程式識別碼**和**應用程式密碼**權杖。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-131">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="4ae9c-132">確切權杖名稱會依提供者而有所不同。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-132">The exact token names vary by provider.</span></span> <span data-ttu-id="4ae9c-133">這些權杖代表您的應用程式用來存取其 API 的認證。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-133">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="4ae9c-134">這些權杖會組成「祕密」，在[祕密管理員](xref:security/app-secrets#secret-manager)的協助下連結到您的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-134">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="4ae9c-135">相較於在設定檔 (例如 *appsettings.json*) 中儲存權杖，祕密管理員是較安全的替代方案。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-135">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4ae9c-136">祕密管理員僅供用於開發用途。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-136">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="4ae9c-137">您可以透過 [Azure Key Vault 設定提供者](xref:security/key-vault-configuration)儲存及保護 Azure 測試與生產祕密。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-137">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="4ae9c-138">請遵循 [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) (在 ASP.NET Core 開發過程中安全地儲存應用程式祕密) 主題中的步驟，儲存下方各個登入提供者指派的權杖。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-138">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="4ae9c-139">設定應用程式所需的登入提供者</span><span class="sxs-lookup"><span data-stu-id="4ae9c-139">Setup login providers required by your application</span></span>

<span data-ttu-id="4ae9c-140">若要將應用程式設定為使用相應的提供者，請使用下列主題：</span><span class="sxs-lookup"><span data-stu-id="4ae9c-140">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="4ae9c-141">[Facebook](xref:security/authentication/facebook-logins) 指示</span><span class="sxs-lookup"><span data-stu-id="4ae9c-141">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="4ae9c-142">[Twitter](xref:security/authentication/twitter-logins) 指示</span><span class="sxs-lookup"><span data-stu-id="4ae9c-142">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="4ae9c-143">[Google](xref:security/authentication/google-logins) 指示</span><span class="sxs-lookup"><span data-stu-id="4ae9c-143">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="4ae9c-144">[Microsoft](xref:security/authentication/microsoft-logins) 指示</span><span class="sxs-lookup"><span data-stu-id="4ae9c-144">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="4ae9c-145">[其他提供者](xref:security/authentication/otherlogins)指示</span><span class="sxs-lookup"><span data-stu-id="4ae9c-145">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](~/includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="4ae9c-146">選擇性地設定密碼</span><span class="sxs-lookup"><span data-stu-id="4ae9c-146">Optionally set password</span></span>

<span data-ttu-id="4ae9c-147">當您使用外部登入提供者註冊時，您並未向應用程式註冊密碼。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-147">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="4ae9c-148">這樣可以減輕您建立網站密碼與記住密碼的壓力，但這也會讓您依賴外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-148">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="4ae9c-149">如果無法使用外部登入提供者，您就無法登入網站。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-149">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="4ae9c-150">若要建立密碼，並使用您在外部提供者登入程序期間所設的電子郵件進行登入：</span><span class="sxs-lookup"><span data-stu-id="4ae9c-150">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="4ae9c-151">點選右上角的 [Hello &lt;電子郵件別名&gt;] 連結以巡覽至 [管理] 檢視。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-151">Tap the **Hello &lt;email alias&gt;** link at the top right corner to navigate to the **Manage** view.</span></span>

![Web 應用程式的 [管理] 檢視](index/_static/pass1a.png)

* <span data-ttu-id="4ae9c-153">點選 [建立]</span><span class="sxs-lookup"><span data-stu-id="4ae9c-153">Tap **Create**</span></span>

![[設定密碼] 頁面](index/_static/pass2a.png)

* <span data-ttu-id="4ae9c-155">設定有效的密碼，以便使用此密碼與電子郵件進行登入。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-155">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4ae9c-156">後續步驟</span><span class="sxs-lookup"><span data-stu-id="4ae9c-156">Next steps</span></span>

* <span data-ttu-id="4ae9c-157">本文介紹了外部驗證，並說明將外部登入新增至 ASP.NET Core 應用程式所需的必要條件。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-157">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="4ae9c-158">請參考提供者的特定頁面，以設定應用程式所需的提供者登入項目。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-158">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="4ae9c-159">您想要保存有關使用者與其存取和重新整理權杖的額外資料。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-159">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="4ae9c-160">如需詳細資訊，請參閱<xref:security/authentication/social/additional-claims>。</span><span class="sxs-lookup"><span data-stu-id="4ae9c-160">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
