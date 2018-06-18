---
title: 在 ASP.NET Core 的一般資料保護規定 (GDPR) 支援
author: rick-anderson
description: 了解如何存取 GDPR 擴充點，在 ASP.NET Core web 應用程式。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/gdpr
ms.openlocfilehash: eb9173bfe554b8b00ef8deb255e8347a534e7ba3
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/18/2018
ms.locfileid: "35725792"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="e30d1-103">在 ASP.NET Core EU 一般資料保護規定 (GDPR) 支援</span><span class="sxs-lookup"><span data-stu-id="e30d1-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="e30d1-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e30d1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e30d1-105">ASP.NET Core 提供應用程式開發介面和範本可協助符合某些[EU 一般資料保護規定 (GDPR)](https://www.eugdpr.org/)需求：</span><span class="sxs-lookup"><span data-stu-id="e30d1-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

* <span data-ttu-id="e30d1-106">專案範本包含了擴充點，以及您可以取代您的隱私權和 cookie 的使用原則的 stub 的標記。</span><span class="sxs-lookup"><span data-stu-id="e30d1-106">The project templates include extension points and stubbed markup you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="e30d1-107">Cookie 同意功能可讓您要求 （和追蹤） 同意從您的使用者，用來儲存個人資訊。</span><span class="sxs-lookup"><span data-stu-id="e30d1-107">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="e30d1-108">如果使用者不同意資料收集，且應用程式設定與[CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded)至`true`，非必要 cookie 將不會傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="e30d1-108">If a user has not consented to data collection and the app is set with [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) to `true`, non-essential cookies will not be sent to the browser.</span></span>
* <span data-ttu-id="e30d1-109">Cookie 可以標示為重要。</span><span class="sxs-lookup"><span data-stu-id="e30d1-109">Cookies can be marked as essential.</span></span> <span data-ttu-id="e30d1-110">基本的 cookie 會傳送至瀏覽器中，即使當使用者不同意並追蹤已停用。</span><span class="sxs-lookup"><span data-stu-id="e30d1-110">Essential cookies are sent to the browser even when the user has not consented and tracking is disabled.</span></span>
* <span data-ttu-id="e30d1-111">[TempData 和工作階段 cookie](#tempdata)停用追蹤時，會無法運作。</span><span class="sxs-lookup"><span data-stu-id="e30d1-111">[TempData and Session cookies](#tempdata) are not functional when tracking is disabled.</span></span>
* <span data-ttu-id="e30d1-112">[身分識別管理](#pd)頁面提供的連結下載及刪除使用者資料。</span><span class="sxs-lookup"><span data-stu-id="e30d1-112">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="e30d1-113">[範例應用程式](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample)可讓您測試的大部分 GDPR 擴充點和應用程式開發介面加入至 ASP.NET Core 2.1 範本。</span><span class="sxs-lookup"><span data-stu-id="e30d1-113">The [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="e30d1-114">請參閱[讀我檔案](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample)檔，以測試指示。</span><span class="sxs-lookup"><span data-stu-id="e30d1-114">See the [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="e30d1-115">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e30d1-115">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="e30d1-116">在範本中的 ASP.NET Core GDPR 支援產生的程式碼</span><span class="sxs-lookup"><span data-stu-id="e30d1-116">ASP.NET Core GDPR support in template generated code</span></span>

<span data-ttu-id="e30d1-117">Razor 頁面和 MVC 專案範本建立專案，包含下列 GDPR 支援：</span><span class="sxs-lookup"><span data-stu-id="e30d1-117">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="e30d1-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)和[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy)中設定`Startup`。</span><span class="sxs-lookup"><span data-stu-id="e30d1-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) are set in `Startup`.</span></span>
* <span data-ttu-id="e30d1-119">*_CookieConsentPartial.cshtml* [部分檢視](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="e30d1-119">The *_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span>
* <span data-ttu-id="e30d1-120">*Pages/Privacy.cshtml*或*Home/Privacy.cshtml*檢視會提供您網站的隱私權原則的詳細資料頁面。</span><span class="sxs-lookup"><span data-stu-id="e30d1-120">The *Pages/Privacy.cshtml* or *Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="e30d1-121">*_CookieConsentPartial.cshtml*檔案會產生隱私權頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="e30d1-121">The *_CookieConsentPartial.cshtml* file generates a link to the privacy page.</span></span>
* <span data-ttu-id="e30d1-122">建立個別的使用者帳戶的應用程式，如 [管理] 頁面提供下載和刪除連結[個人使用者資料](#pd)。</span><span class="sxs-lookup"><span data-stu-id="e30d1-122">For applications created with individual user accounts, the manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="e30d1-123">CookiePolicyOptions 和 UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="e30d1-123">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="e30d1-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)中初始化`Startup`類別`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="e30d1-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) are initialized in the `Startup` class `ConfigureServices` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="e30d1-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy)中呼叫`Startup`類別`Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="e30d1-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) is called in the `Startup` class `Configure` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="e30d1-126">_CookieConsentPartial.cshtml 部分檢視</span><span class="sxs-lookup"><span data-stu-id="e30d1-126">_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="e30d1-127">*_CookieConsentPartial.cshtml*部分檢視：</span><span class="sxs-lookup"><span data-stu-id="e30d1-127">The *_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="e30d1-128">此部分：</span><span class="sxs-lookup"><span data-stu-id="e30d1-128">This partial:</span></span>

* <span data-ttu-id="e30d1-129">取得使用者追蹤的狀態。</span><span class="sxs-lookup"><span data-stu-id="e30d1-129">Gets the state of tracking for the user.</span></span> <span data-ttu-id="e30d1-130">如果應用程式已設定為需要同意，使用者必須同意之前可以追蹤 cookie。</span><span class="sxs-lookup"><span data-stu-id="e30d1-130">If the application is configured to require consent the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="e30d1-131">如果需要同意的話，cookie 同意 chrome 固定的上方導覽列中建立*Pages/Shared/_Layout.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="e30d1-131">If consent is required, the cookie consent chrome is fixed on top of the navigation bar created in the *Pages/Shared/_Layout.cshtml* file.</span></span>
* <span data-ttu-id="e30d1-132">提供 HTML`<p>`摘要說明您的隱私權和 cookie 的項目使用的原則。</span><span class="sxs-lookup"><span data-stu-id="e30d1-132">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="e30d1-133">提供的連結*Pages/Privacy.cshtml*其中詳細說明網站的隱私權原則。</span><span class="sxs-lookup"><span data-stu-id="e30d1-133">Provides a link to *Pages/Privacy.cshtml* where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="e30d1-134">基本的 cookie</span><span class="sxs-lookup"><span data-stu-id="e30d1-134">Essential cookies</span></span>

<span data-ttu-id="e30d1-135">如果尚未被授與同意，標示為重要的 cookie 會傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="e30d1-135">If consent has not been given, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="e30d1-136">下列程式碼會進行基本的 cookie:</span><span class="sxs-lookup"><span data-stu-id="e30d1-136">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a><span data-ttu-id="e30d1-137">Tempdata 提供者和工作階段狀態的 cookie 不需對內文</span><span class="sxs-lookup"><span data-stu-id="e30d1-137">Tempdata provider and session state cookies are not essential</span></span>

<span data-ttu-id="e30d1-138">[Tempdata 提供者](xref:fundamentals/app-state#tempdata)cookie 並不重要。</span><span class="sxs-lookup"><span data-stu-id="e30d1-138">The [Tempdata provider](xref:fundamentals/app-state#tempdata) cookie is not essential.</span></span> <span data-ttu-id="e30d1-139">如果追蹤已停用，Tempdata 提供者不是功能。</span><span class="sxs-lookup"><span data-stu-id="e30d1-139">If tracking is disabled, the Tempdata provider is not functional.</span></span> <span data-ttu-id="e30d1-140">若要停用追蹤時，請啟用 Tempdata 提供者，將 TempData cookie 標示為重要中`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e30d1-140">To enable the Tempdata provider when tracking is disabled, mark the TempData cookie as essential in `ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

<span data-ttu-id="e30d1-141">[工作階段狀態](xref:fundamentals/app-state)cookie 不重要。</span><span class="sxs-lookup"><span data-stu-id="e30d1-141">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="e30d1-142">停用追蹤時，工作階段狀態沒有作用。</span><span class="sxs-lookup"><span data-stu-id="e30d1-142">Session state is not functional when tracking is disabled.</span></span>

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="e30d1-143">個人資料</span><span class="sxs-lookup"><span data-stu-id="e30d1-143">Personal data</span></span>

<span data-ttu-id="e30d1-144">使用個別的使用者帳戶建立的 ASP.NET Core 應用程式包括下載和刪除個人資料的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e30d1-144">ASP.NET Core applications created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="e30d1-145">選取的使用者名稱，然後選取**個人資料**:</span><span class="sxs-lookup"><span data-stu-id="e30d1-145">Select the user name and then select **Personal data**:</span></span>

![管理個人資料頁面](gdpr/_static/pd.png)

<span data-ttu-id="e30d1-147">附註：</span><span class="sxs-lookup"><span data-stu-id="e30d1-147">Notes:</span></span>

* <span data-ttu-id="e30d1-148">若要產生`Account/Manage`程式碼，請參閱[Scaffold 識別](xref:security/authentication/scaffold-identity)。</span><span class="sxs-lookup"><span data-stu-id="e30d1-148">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="e30d1-149">刪除與下載只會影響預設識別資料。</span><span class="sxs-lookup"><span data-stu-id="e30d1-149">Delete and download only impact the default identity data.</span></span> <span data-ttu-id="e30d1-150">刪除下載的自訂使用者資料，您必須擴充應用程式建立自訂使用者資料。</span><span class="sxs-lookup"><span data-stu-id="e30d1-150">Apps the create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="e30d1-151">GitHub 問題[如何加入/刪除自訂的使用者身分識別資料](https://github.com/aspnet/Docs/issues/6226)會建議的文件追蹤上建立自訂/刪除/下載自訂使用者資料。</span><span class="sxs-lookup"><span data-stu-id="e30d1-151">GitHub issue [How to add/delete custom user data to Identity](https://github.com/aspnet/Docs/issues/6226) tracks a proposed article on creating custom/deleting/downloading custom user data.</span></span> <span data-ttu-id="e30d1-152">如果您想要請參閱該主題設定優先權，將保留反應顆星的問題。</span><span class="sxs-lookup"><span data-stu-id="e30d1-152">If you'd like to see that topic prioritized, leave a thumbs up reaction in the issue.</span></span>
* <span data-ttu-id="e30d1-153">儲存使用者識別資料庫資料表中儲存的語彙基元`AspNetUserTokens`串聯刪除行為因為透過刪除使用者時，會刪除[外部索引鍵](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152)。</span><span class="sxs-lookup"><span data-stu-id="e30d1-153">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span></span>

## <a name="encryption-at-rest"></a><span data-ttu-id="e30d1-154">靜態加密</span><span class="sxs-lookup"><span data-stu-id="e30d1-154">Encryption at rest</span></span>

<span data-ttu-id="e30d1-155">某些資料庫和儲存機制允許靜態加密。</span><span class="sxs-lookup"><span data-stu-id="e30d1-155">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="e30d1-156">靜態加密：</span><span class="sxs-lookup"><span data-stu-id="e30d1-156">Encryption at rest:</span></span>

* <span data-ttu-id="e30d1-157">會自動加密儲存的資料。</span><span class="sxs-lookup"><span data-stu-id="e30d1-157">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="e30d1-158">設定、 程式設計中或其他軟體存取之資料的工作不會加密。</span><span class="sxs-lookup"><span data-stu-id="e30d1-158">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="e30d1-159">是最簡單且最安全的選項。</span><span class="sxs-lookup"><span data-stu-id="e30d1-159">Is the easiest and safest option.</span></span>
* <span data-ttu-id="e30d1-160">可讓管理金鑰和加密的資料庫。</span><span class="sxs-lookup"><span data-stu-id="e30d1-160">Lets the database manage keys and encryption.</span></span>

<span data-ttu-id="e30d1-161">例如: </span><span class="sxs-lookup"><span data-stu-id="e30d1-161">For example:</span></span>

* <span data-ttu-id="e30d1-162">Microsoft SQL 和 Azure SQL 提供[透明資料加密](/sql/relational-databases/security/encryption/transparent-data-encryption)(TDE)。</span><span class="sxs-lookup"><span data-stu-id="e30d1-162">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span></span>
* [<span data-ttu-id="e30d1-163">SQL Azure 會根據預設來加密資料庫</span><span class="sxs-lookup"><span data-stu-id="e30d1-163">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="e30d1-164">[Azure Blob、 檔案、 資料表和佇列儲存體預設會經過加密](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/)。</span><span class="sxs-lookup"><span data-stu-id="e30d1-164">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="e30d1-165">對於不提供內建加密在靜止的資料庫，您可以使用磁碟加密，以提供相同的保護。</span><span class="sxs-lookup"><span data-stu-id="e30d1-165">For databases that don't provide built-in encryption at rest you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="e30d1-166">例如: </span><span class="sxs-lookup"><span data-stu-id="e30d1-166">For example:</span></span>

* [<span data-ttu-id="e30d1-167">適用於 Windows Server 的 BitLocker</span><span class="sxs-lookup"><span data-stu-id="e30d1-167">BitLocker for Windows Server</span></span>](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="e30d1-168">Linux:</span><span class="sxs-lookup"><span data-stu-id="e30d1-168">Linux:</span></span>
  * [<span data-ttu-id="e30d1-169">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="e30d1-169">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="e30d1-170">[EncFS](https://github.com/vgough/encfs)。</span><span class="sxs-lookup"><span data-stu-id="e30d1-170">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e30d1-171">其他資源</span><span class="sxs-lookup"><span data-stu-id="e30d1-171">Additional Resources</span></span>

* [<span data-ttu-id="e30d1-172">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="e30d1-172">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)
