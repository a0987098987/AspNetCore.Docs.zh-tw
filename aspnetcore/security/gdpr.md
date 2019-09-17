---
title: ASP.NET Core 中的一般資料保護規定（GDPR）支援
author: rick-anderson
description: 瞭解如何存取 ASP.NET Core web 應用程式中的 GDPR 擴充點。
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
uid: security/gdpr
ms.openlocfilehash: 1086c22c2f3c27373d8cb779f4b1d8eb6792ec2e
ms.sourcegitcommit: 2fa0ffe82a47c7317efc9ea908365881cbcb8ed7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2019
ms.locfileid: "69572881"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="e4988-103">ASP.NET Core 中的 EU 一般資料保護規定（GDPR）支援</span><span class="sxs-lookup"><span data-stu-id="e4988-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="e4988-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e4988-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e4988-105">ASP.NET Core 提供 Api 和範本，以協助符合[歐盟一般資料保護規定（GDPR）](https://www.eugdpr.org/)需求：</span><span class="sxs-lookup"><span data-stu-id="e4988-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="e4988-106">專案範本包括擴充點和 stub 標記，您可以將其取代為您的隱私權和 cookie 使用原則。</span><span class="sxs-lookup"><span data-stu-id="e4988-106">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="e4988-107">[ *Pages/隱私權*] 頁面或 [ *Views/Home/隱私權*] 視圖會提供頁面，以詳細說明網站的隱私權原則。</span><span class="sxs-lookup"><span data-stu-id="e4988-107">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span>

<span data-ttu-id="e4988-108">若要啟用預設的 cookie 同意功能，例如在 ASP.NET Core 3.0 範本產生的應用程式的 ASP.NET Core 2.2 範本中找到：</span><span class="sxs-lookup"><span data-stu-id="e4988-108">To enable the default cookie consent feature like that found in the ASP.NET Core 2.2 templates in an ASP.NET Core 3.0 template generated app:</span></span>

* <span data-ttu-id="e4988-109">將`using Microsoft.AspNetCore.Http`加入 using 指示詞的清單。</span><span class="sxs-lookup"><span data-stu-id="e4988-109">Add `using Microsoft.AspNetCore.Http` to the list of using directives.</span></span>
* <span data-ttu-id="e4988-110">將[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)新增`Startup.ConfigureServices`至，並`Startup.Configure`[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) 至：</span><span class="sxs-lookup"><span data-stu-id="e4988-110">Add [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) to `Startup.ConfigureServices` and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) to `Startup.Configure`:</span></span>

  [!code-csharp[Main](gdpr/sample/RP3.0/Startup.cs?name=snippet1&highlight=12-19,38)]

* <span data-ttu-id="e4988-111">將 cookie 同意部分新增至 *_Layout*檔案：</span><span class="sxs-lookup"><span data-stu-id="e4988-111">Add the cookie consent partial to the *_Layout.cshtml* file:</span></span>

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_Layout.cshtml?name=snippet&highlight=4)]

* <span data-ttu-id="e4988-112">將 CookieConsentPartial 檔案新增至專案： *\_*</span><span class="sxs-lookup"><span data-stu-id="e4988-112">Add the *\_CookieConsentPartial.cshtml* file to the project:</span></span>

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_CookieConsentPartial.cshtml)]

* <span data-ttu-id="e4988-113">選取本文的 ASP.NET Core 2.2 版本，以閱讀 cookie 同意功能的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="e4988-113">Select the ASP.NET Core 2.2 version of this article to read about the cookie consent feature.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* <span data-ttu-id="e4988-114">專案範本包括擴充點和 stub 標記，您可以將其取代為您的隱私權和 cookie 使用原則。</span><span class="sxs-lookup"><span data-stu-id="e4988-114">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="e4988-115">Cookie 同意功能可讓您向使用者要求（並追蹤）同意，以儲存個人資訊。</span><span class="sxs-lookup"><span data-stu-id="e4988-115">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="e4988-116">如果使用者尚未同意資料收集，而應用程式已將[CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded)設定為`true`，則不會將非必要的 cookie 傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="e4988-116">If a user hasn't consented to data collection and the app has [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) set to `true`, non-essential cookies aren't sent to the browser.</span></span>
* <span data-ttu-id="e4988-117">Cookie 可以標示為必要。</span><span class="sxs-lookup"><span data-stu-id="e4988-117">Cookies can be marked as essential.</span></span> <span data-ttu-id="e4988-118">即使使用者尚未同意，也會將必要的 cookie 傳送至瀏覽器，而且會停用追蹤。</span><span class="sxs-lookup"><span data-stu-id="e4988-118">Essential cookies are sent to the browser even when the user hasn't consented and tracking is disabled.</span></span>
* <span data-ttu-id="e4988-119">停用追蹤時[，TempData 和會話 cookie](#tempdata)無法正常運作。</span><span class="sxs-lookup"><span data-stu-id="e4988-119">[TempData and Session cookies](#tempdata) aren't functional when tracking is disabled.</span></span>
* <span data-ttu-id="e4988-120">[身分[識別管理](#pd)] 頁面提供下載和刪除使用者資料的連結。</span><span class="sxs-lookup"><span data-stu-id="e4988-120">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="e4988-121">[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample)可讓您測試新增至 ASP.NET Core 2.1 範本的大部分 GDPR 擴充點和 api。</span><span class="sxs-lookup"><span data-stu-id="e4988-121">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) allows you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="e4988-122">如需測試指示，請參閱[自述](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample)檔。</span><span class="sxs-lookup"><span data-stu-id="e4988-122">See the [ReadMe](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="e4988-123">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e4988-123">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="e4988-124">在範本產生的程式碼中 ASP.NET Core GDPR 支援</span><span class="sxs-lookup"><span data-stu-id="e4988-124">ASP.NET Core GDPR support in template-generated code</span></span>

<span data-ttu-id="e4988-125">使用專案範本建立的 Razor Pages 和 MVC 專案包含下列 GDPR 支援：</span><span class="sxs-lookup"><span data-stu-id="e4988-125">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="e4988-126">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)和[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy)是在`Startup`類別中設定。</span><span class="sxs-lookup"><span data-stu-id="e4988-126">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) are set in the `Startup` class.</span></span>
* <span data-ttu-id="e4988-127">*\_CookieConsentPartial*的[部分視圖](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="e4988-127">The *\_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="e4988-128">此檔案包含 [**接受**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e4988-128">An **Accept** button is included in this file.</span></span> <span data-ttu-id="e4988-129">當使用者按一下 [**接受**] 按鈕時，就會提供同意存放區 cookie。</span><span class="sxs-lookup"><span data-stu-id="e4988-129">When the user clicks the **Accept** button, consent to store cookies is provided.</span></span>
* <span data-ttu-id="e4988-130">[ *Pages/隱私權*] 頁面或 [ *Views/Home/隱私權*] 視圖會提供頁面，以詳細說明網站的隱私權原則。</span><span class="sxs-lookup"><span data-stu-id="e4988-130">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="e4988-131">CookieConsentPartial 會產生 [隱私權] 頁面的連結。 *\_*</span><span class="sxs-lookup"><span data-stu-id="e4988-131">The *\_CookieConsentPartial.cshtml* file generates a link to the Privacy page.</span></span>
* <span data-ttu-id="e4988-132">針對使用個別使用者帳戶所建立的應用程式，[管理] 頁面會提供下載和刪除[個人使用者資料](#pd)的連結。</span><span class="sxs-lookup"><span data-stu-id="e4988-132">For apps created with individual user accounts, the Manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="e4988-133">CookiePolicyOptions 和 UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="e4988-133">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="e4988-134">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)在中`Startup.ConfigureServices`初始化：</span><span class="sxs-lookup"><span data-stu-id="e4988-134">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) are initialized in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="e4988-135">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy)是在中`Startup.Configure`呼叫：</span><span class="sxs-lookup"><span data-stu-id="e4988-135">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) is called in `Startup.Configure`:</span></span>

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="_cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="e4988-136">\_CookieConsentPartial. cshtml 部分視圖</span><span class="sxs-lookup"><span data-stu-id="e4988-136">\_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="e4988-137">*\_CookieConsentPartial*的部分視圖：</span><span class="sxs-lookup"><span data-stu-id="e4988-137">The *\_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[](gdpr/sample/RP2.2/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="e4988-138">這部分：</span><span class="sxs-lookup"><span data-stu-id="e4988-138">This partial:</span></span>

* <span data-ttu-id="e4988-139">取得使用者的追蹤狀態。</span><span class="sxs-lookup"><span data-stu-id="e4988-139">Obtains the state of tracking for the user.</span></span> <span data-ttu-id="e4988-140">如果應用程式已設定為需要同意，使用者必須先同意，才能追蹤 cookie。</span><span class="sxs-lookup"><span data-stu-id="e4988-140">If the app is configured to require consent, the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="e4988-141">如果需要同意，cookie 同意面板會固定在配置 *\_. cshtml*檔案所建立的導覽列上方。</span><span class="sxs-lookup"><span data-stu-id="e4988-141">If consent is required, the cookie consent panel is fixed at top of the navigation bar created by the *\_Layout.cshtml* file.</span></span>
* <span data-ttu-id="e4988-142">提供 HTML `<p>`元素來總結您的隱私權與 cookie 使用原則。</span><span class="sxs-lookup"><span data-stu-id="e4988-142">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="e4988-143">提供 [隱私權] 頁面或 [流覽] 的連結，您可以在其中詳細說明網站的隱私權原則。</span><span class="sxs-lookup"><span data-stu-id="e4988-143">Provides a link to Privacy page or view where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="e4988-144">基本 cookie</span><span class="sxs-lookup"><span data-stu-id="e4988-144">Essential cookies</span></span>

<span data-ttu-id="e4988-145">如果尚未提供同意存放區 cookie，則只會將標示為必要的 cookie 傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="e4988-145">If consent to store cookies hasn't been provided, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="e4988-146">下列程式碼會讓 cookie 變得很重要：</span><span class="sxs-lookup"><span data-stu-id="e4988-146">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP2.2/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

### <a name="tempdata-provider-and-session-state-cookies-arent-essential"></a><span data-ttu-id="e4988-147">TempData 提供者和會話狀態 cookie 不是必要的</span><span class="sxs-lookup"><span data-stu-id="e4988-147">TempData provider and session state cookies aren't essential</span></span>

<span data-ttu-id="e4988-148">[TempData 提供者](xref:fundamentals/app-state#tempdata)cookie 並不重要。</span><span class="sxs-lookup"><span data-stu-id="e4988-148">The [TempData provider](xref:fundamentals/app-state#tempdata) cookie isn't essential.</span></span> <span data-ttu-id="e4988-149">如果停用追蹤，TempData 提供者就無法運作。</span><span class="sxs-lookup"><span data-stu-id="e4988-149">If tracking is disabled, the TempData provider isn't functional.</span></span> <span data-ttu-id="e4988-150">若要在停用追蹤時啟用 TempData 提供者，請在中將 TempData `Startup.ConfigureServices`cookie 標記為必要項：</span><span class="sxs-lookup"><span data-stu-id="e4988-150">To enable the TempData provider when tracking is disabled, mark the TempData cookie as essential in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP2.2/Startup.cs?name=snippet1)]

<span data-ttu-id="e4988-151">[會話狀態](xref:fundamentals/app-state)cookie 並不重要。</span><span class="sxs-lookup"><span data-stu-id="e4988-151">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="e4988-152">停用追蹤時，會話狀態無法運作。</span><span class="sxs-lookup"><span data-stu-id="e4988-152">Session state isn't functional when tracking is disabled.</span></span> <span data-ttu-id="e4988-153">下列程式碼會讓會話 cookie 變得很重要：</span><span class="sxs-lookup"><span data-stu-id="e4988-153">The following code makes session cookies essential:</span></span>

[!code-csharp[](gdpr/sample/RP2.2/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="e4988-154">個人資料</span><span class="sxs-lookup"><span data-stu-id="e4988-154">Personal data</span></span>

<span data-ttu-id="e4988-155">使用個別使用者帳戶所建立 ASP.NET Core 應用程式包含下載和刪除個人資料的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e4988-155">ASP.NET Core apps created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="e4988-156">選取使用者名稱，然後選取 [**個人資料**]：</span><span class="sxs-lookup"><span data-stu-id="e4988-156">Select the user name and then select **Personal data**:</span></span>

![管理個人資料頁面](gdpr/_static/pd.png)

<span data-ttu-id="e4988-158">附註：</span><span class="sxs-lookup"><span data-stu-id="e4988-158">Notes:</span></span>

* <span data-ttu-id="e4988-159">若要產生`Account/Manage`程式碼，請參閱[Scaffold Identity](xref:security/authentication/scaffold-identity)。</span><span class="sxs-lookup"><span data-stu-id="e4988-159">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="e4988-160">[**刪除**] 和 [**下載**] 連結只會對預設身分識別資料採取行動。</span><span class="sxs-lookup"><span data-stu-id="e4988-160">The **Delete** and **Download** links only act on the default identity data.</span></span> <span data-ttu-id="e4988-161">建立自訂使用者資料的應用程式必須擴充，才能刪除/下載自訂使用者資料。</span><span class="sxs-lookup"><span data-stu-id="e4988-161">Apps that create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="e4988-162">如需詳細資訊，請參閱[將自訂使用者資料新增、下載及刪除至身分識別](xref:security/authentication/add-user-data)。</span><span class="sxs-lookup"><span data-stu-id="e4988-162">For more information, see [Add, download, and delete custom user data to Identity](xref:security/authentication/add-user-data).</span></span>
* <span data-ttu-id="e4988-163">當使用者因為[外鍵](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152)而透過串聯刪除行為刪除時， `AspNetUserTokens`會刪除儲存在身分識別資料庫資料表中之使用者的已儲存權杖。</span><span class="sxs-lookup"><span data-stu-id="e4988-163">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span></span>
* <span data-ttu-id="e4988-164">在接受 cookie 原則之前，無法使用[外部提供者驗證](xref:security/authentication/social/index)（例如 Facebook 和 Google）。</span><span class="sxs-lookup"><span data-stu-id="e4988-164">[External provider authentication](xref:security/authentication/social/index), such as Facebook and Google, isn't available before the cookie policy is accepted.</span></span>

::: moniker-end

## <a name="encryption-at-rest"></a><span data-ttu-id="e4988-165">待用加密</span><span class="sxs-lookup"><span data-stu-id="e4988-165">Encryption at rest</span></span>

<span data-ttu-id="e4988-166">某些資料庫和儲存機制允許待用加密。</span><span class="sxs-lookup"><span data-stu-id="e4988-166">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="e4988-167">待用加密：</span><span class="sxs-lookup"><span data-stu-id="e4988-167">Encryption at rest:</span></span>

* <span data-ttu-id="e4988-168">自動加密儲存的資料。</span><span class="sxs-lookup"><span data-stu-id="e4988-168">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="e4988-169">針對存取資料的軟體，不需要設定、程式設計或其他工作即可進行加密。</span><span class="sxs-lookup"><span data-stu-id="e4988-169">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="e4988-170">是最簡單且最安全的選項。</span><span class="sxs-lookup"><span data-stu-id="e4988-170">Is the easiest and safest option.</span></span>
* <span data-ttu-id="e4988-171">允許資料庫管理金鑰和加密。</span><span class="sxs-lookup"><span data-stu-id="e4988-171">Allows the database to manage keys and encryption.</span></span>

<span data-ttu-id="e4988-172">例如：</span><span class="sxs-lookup"><span data-stu-id="e4988-172">For example:</span></span>

* <span data-ttu-id="e4988-173">Microsoft SQL 和 Azure SQL 提供[透明資料加密](/sql/relational-databases/security/encryption/transparent-data-encryption)（TDE）。</span><span class="sxs-lookup"><span data-stu-id="e4988-173">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span></span>
* [<span data-ttu-id="e4988-174">SQL Azure 預設會加密資料庫</span><span class="sxs-lookup"><span data-stu-id="e4988-174">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="e4988-175">[預設會加密 Azure blob、檔案、資料表和佇列儲存體](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/)。</span><span class="sxs-lookup"><span data-stu-id="e4988-175">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="e4988-176">對於未提供內建靜態加密的資料庫，您可以使用磁片加密來提供相同的保護。</span><span class="sxs-lookup"><span data-stu-id="e4988-176">For databases that don't provide built-in encryption at rest, you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="e4988-177">例如：</span><span class="sxs-lookup"><span data-stu-id="e4988-177">For example:</span></span>

* [<span data-ttu-id="e4988-178">適用于 Windows Server 的 BitLocker</span><span class="sxs-lookup"><span data-stu-id="e4988-178">BitLocker for Windows Server</span></span>](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="e4988-179">Linux：</span><span class="sxs-lookup"><span data-stu-id="e4988-179">Linux:</span></span>
  * [<span data-ttu-id="e4988-180">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="e4988-180">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="e4988-181">[EncFS](https://github.com/vgough/encfs)。</span><span class="sxs-lookup"><span data-stu-id="e4988-181">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e4988-182">其他資源</span><span class="sxs-lookup"><span data-stu-id="e4988-182">Additional resources</span></span>

* [<span data-ttu-id="e4988-183">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="e4988-183">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/trustcenter/Privacy/GDPR)
* <span data-ttu-id="e4988-184">[GDPR-在 ASP.NET Core 中新增 [撤銷同意] 按鈕](https://www.joeaudette.com/blog/2018/08/28/gdpr---adding-a-revoke-consent-button-in-aspnet-core)</span><span class="sxs-lookup"><span data-stu-id="e4988-184">[GDPR - Adding a Revoke Consent Button in ASP.NET Core](https://www.joeaudette.com/blog/2018/08/28/gdpr---adding-a-revoke-consent-button-in-aspnet-core)</span></span>
