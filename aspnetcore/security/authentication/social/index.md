---
title: "使用 Facebook、Google 和其他外部提供者啟用驗證"
author: rick-anderson
description: 
keywords: "ASP.NET Core, 驗證, 社交, 驗證提供者, Google, Facebook, Twitter, Microsoft 帳戶"
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.assetid: eda7ee17-f38c-462e-8d1d-63f459901cf3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/social/index
ms.openlocfilehash: b561dcee5435dfc34cfa0b9b15babf75ca8f3508
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="enabling-authentication-using-facebook-google-and-other-external-providers"></a><span data-ttu-id="1c5b3-103">使用 Facebook、Google 和其他外部提供者啟用驗證</span><span class="sxs-lookup"><span data-stu-id="1c5b3-103">Enabling authentication using Facebook, Google and other external providers</span></span>

<a name=security-authentication-social-logins></a>

<span data-ttu-id="1c5b3-104">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1c5b3-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1c5b3-105">本教學課程會示範如何建立 ASP.NET Core 2.x 應用程式，讓使用者使用 OAuth 2.0 與來自外部驗證提供者的認證進行登入。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-105">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="1c5b3-106">下列各節涵蓋 [Facebook](facebook-logins.md)、[Twitter](twitter-logins.md)、[Google](google-logins.md) 和 [Microsoft](microsoft-logins.md) 的提供者。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-106">[Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), and [Microsoft](microsoft-logins.md) providers are covered in the following sections.</span></span> <span data-ttu-id="1c5b3-107">您可透過 [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) 和 [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers) 這類協力廠商套件，取得其他提供者。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Facebook、Twitter、Google+ 和 Windows 的社交媒體圖示](index/_static/social.png)

<span data-ttu-id="1c5b3-109">若使用者可以利用其現有的認證登入，一方面對使用者來說十分方便，另一方面則可將管理登入程序的許多複雜工作轉移給協力廠商。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="1c5b3-110">如需社交登入如何帶動流量和客戶轉換的範例，請參閱 [Facebook](https://www.facebook.com/unsupportedbrowser) 和 [Twitter](https://dev.twitter.com/resources/case-studies) 的案例研究。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="1c5b3-111">注意：此處提供的套件摘錄了大量 OAuth 驗證流程的複雜內容，但在您進行疑難排解時，可能有必要了解詳細資料。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-111">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="1c5b3-112">可用的資源很多；例如，您可以參閱 [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) (OAuth 2 簡介) 或 [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/) (了解 OAuth 2)。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-112">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="1c5b3-113">您也可以查看[適用於提供者套件的 ASP.NET Core 原始程式碼](https://github.com/aspnet/Security/tree/dev/src)，來解決部分問題。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-113">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/dev/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="1c5b3-114">建立新的 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="1c5b3-114">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="1c5b3-115">在 Visual Studio 2017 中，您可以從 [開始] 頁面來建立新的專案，或透過 [檔案] > [新增] > [專案] 來進行。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-115">In Visual Studio 2017, create a new project from the Start Page, or via **File > New > Project**.</span></span>

* <span data-ttu-id="1c5b3-116">在 [Visual C#] > [.NET Core] 類別中，選取可用的 [ASP.NET Core Web 應用程式] 範本：</span><span class="sxs-lookup"><span data-stu-id="1c5b3-116">Select the **ASP.NET Core Web Application** template available in **Visual C# > .NET Core** category:</span></span>

![[新增專案] 對話](index/_static/new-project.png)

* <span data-ttu-id="1c5b3-118">點選 [Web 應用程式]，並驗證已將 [驗證] 設定為 [個別使用者帳戶]：</span><span class="sxs-lookup"><span data-stu-id="1c5b3-118">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![[新增 Web 應用程式] 對話方塊](index/_static/select-project.png)

<span data-ttu-id="1c5b3-120">注意：本教學課程適用於 ASP.NET Core 2.0 SDK 版本；您可以在精靈的頂端選取此版本。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-120">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="1c5b3-121">需要 SSL</span><span class="sxs-lookup"><span data-stu-id="1c5b3-121">Require SSL</span></span>

<span data-ttu-id="1c5b3-122">OAuth 2.0 要求使用 SSL 以透過 HTTPS 通訊協定進行驗證。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-122">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="1c5b3-123">注意：如果您是使用適用於 ASP.NET Core 2.x 的 [Web 應用程式] 或 [Web API] 專案範本來建立專案，且專案精靈的 [變更驗證] 對話方塊已如上所示選取 [個別使用者帳戶] 選項的話，則系統會自動將該專案設為啟用 SSL，並使用 https URL 來啟動。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-123">Note: Projects created using **Web Application** or **Web API** project templates for ASP.NET Core 2.x are automatically configured to enable SSL and launch with https URL if the **Individual User Accounts** option was selected on **Change Authentication dialog** in the project wizard as shown above.</span></span>

* <span data-ttu-id="1c5b3-124">了解如何遵循[設定 HTTPS 進行 ASP.NET Core 開發](xref:security/https)主題中的步驟以手動啟用 SSL。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-124">Learn how to enable SSL manually by following the steps in [Setting up HTTPS for development in ASP.NET Core](xref:security/https) topic.</span></span>

* <span data-ttu-id="1c5b3-125">然後，遵循[強制執行 ASP.NET Core 應用程式中的 SSL](xref:security/enforcing-ssl) 主題的下列步驟，要求網站使用 SSL。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-125">Then, require SSL on your site by following the steps in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) topic.</span></span>

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="1c5b3-126">使用 SecretManager 來儲存登入提供者指派的權杖</span><span class="sxs-lookup"><span data-stu-id="1c5b3-126">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="1c5b3-127">社交登入提供者會在註冊程序期間指派「應用程式識別碼」和「應用程式密碼」權杖 (確切的命名因提供者而異)。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-127">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process (exact naming varies by provider).</span></span>

<span data-ttu-id="1c5b3-128">這些值都是應用程式用來存取其 API 的有效「使用者名稱」和「密碼」，並構成可連結至應用程式組態的「密碼」(透過**密碼管理員**來完成，而不是直接儲存在組態檔中或將其硬式編碼)。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-128">These values are effectively the *user name* and *password* your application uses to access their API, and constitute the "secrets" that can be linked to your application configuration with the help of **Secret Manager** instead of storing them in configuration files directly or hard-coding them.</span></span>

<span data-ttu-id="1c5b3-129">請遵循[在 ASP.NET Core 開發期間安全儲存應用程式密碼](xref:security/app-secrets)主題中的步驟，儲存下列每個登入提供者所指派的權杖。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-129">Follow the steps in [Safe storage of app secrets during development in ASP.NET Core](xref:security/app-secrets) topic so that you can store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="1c5b3-130">設定應用程式所需的登入提供者</span><span class="sxs-lookup"><span data-stu-id="1c5b3-130">Setup login providers required by your application</span></span>

<span data-ttu-id="1c5b3-131">若要將應用程式設定為使用相應的提供者，請使用下列主題：</span><span class="sxs-lookup"><span data-stu-id="1c5b3-131">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="1c5b3-132">[Facebook](facebook-logins.md) 指示</span><span class="sxs-lookup"><span data-stu-id="1c5b3-132">[Facebook](facebook-logins.md) instructions</span></span>
* <span data-ttu-id="1c5b3-133">[Twitter](twitter-logins.md) 指示</span><span class="sxs-lookup"><span data-stu-id="1c5b3-133">[Twitter](twitter-logins.md) instructions</span></span>
* <span data-ttu-id="1c5b3-134">[Google](google-logins.md) 指示</span><span class="sxs-lookup"><span data-stu-id="1c5b3-134">[Google](google-logins.md) instructions</span></span>
* <span data-ttu-id="1c5b3-135">[Microsoft](microsoft-logins.md) 指示</span><span class="sxs-lookup"><span data-stu-id="1c5b3-135">[Microsoft](microsoft-logins.md) instructions</span></span>
* <span data-ttu-id="1c5b3-136">[其他提供者](other-logins.md)指示</span><span class="sxs-lookup"><span data-stu-id="1c5b3-136">[Other provider](other-logins.md) instructions</span></span>

## <a name="optionally-set-password"></a><span data-ttu-id="1c5b3-137">選擇性地設定密碼</span><span class="sxs-lookup"><span data-stu-id="1c5b3-137">Optionally set password</span></span>

<span data-ttu-id="1c5b3-138">當您使用外部登入提供者註冊時，您並未向應用程式註冊密碼。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-138">When you register with an external login provider, you do not have a password registered with the app.</span></span> <span data-ttu-id="1c5b3-139">這樣可以減輕您建立網站密碼與記住密碼的壓力，但這也會讓您依賴外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-139">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="1c5b3-140">如果無法使用外部登入提供者，您就無法登入網站。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-140">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="1c5b3-141">若要建立密碼，並使用您在外部提供者登入程序期間所設的電子郵件進行登入：</span><span class="sxs-lookup"><span data-stu-id="1c5b3-141">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="1c5b3-142">點選右上角的 **Hello <email alias>** 連結，巡覽至 [管理] 檢視。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-142">Tap the **Hello <email alias>** link at the top right corner to navigate to the **Manage** view.</span></span>

![Web 應用程式的 [管理] 檢視](index/_static/pass1a.png)

* <span data-ttu-id="1c5b3-144">點選 [建立]</span><span class="sxs-lookup"><span data-stu-id="1c5b3-144">Tap **Create**</span></span>

![[設定密碼] 頁面](index/_static/pass2a.png)

* <span data-ttu-id="1c5b3-146">設定有效的密碼，以便使用此密碼與電子郵件進行登入。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-146">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1c5b3-147">後續步驟</span><span class="sxs-lookup"><span data-stu-id="1c5b3-147">Next steps</span></span>

* <span data-ttu-id="1c5b3-148">本文介紹了外部驗證，並說明將外部登入新增至 ASP.NET Core 應用程式所需的必要條件。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-148">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="1c5b3-149">請參考提供者的特定頁面，以設定應用程式所需的提供者登入項目。</span><span class="sxs-lookup"><span data-stu-id="1c5b3-149">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>
