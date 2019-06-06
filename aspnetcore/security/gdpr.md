---
title: 支援 ASP.NET Core 中的一般資料保護規定 (GDPR)
author: rick-anderson
description: 了解如何存取 ASP.NET Core web 應用程式中的 GDPR 擴充點。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/05/2019
uid: security/gdpr
ms.openlocfilehash: 967f3246836c93a1af56f7109edb056220606b58
ms.sourcegitcommit: c716ea9155a6b404c1f3d3d34e2388454cd276d7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2019
ms.locfileid: "66716344"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="92c65-103">ASP.NET Core 中的歐盟一般資料保護規定 (GDPR) 支援</span><span class="sxs-lookup"><span data-stu-id="92c65-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="92c65-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="92c65-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="92c65-105">ASP.NET Core 提供 Api 和範本，以協助符合某些[歐盟一般資料保護規定 (GDPR)](https://www.eugdpr.org/)需求：</span><span class="sxs-lookup"><span data-stu-id="92c65-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

* <span data-ttu-id="92c65-106">專案範本包括擴充點以及附加虛設常式的標記，您可以使用您的隱私權與 cookie 的使用原則來取代。</span><span class="sxs-lookup"><span data-stu-id="92c65-106">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="92c65-107">Cookie 同意功能可讓您要求 （和追蹤） 同意從您的使用者，用來儲存個人資訊。</span><span class="sxs-lookup"><span data-stu-id="92c65-107">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="92c65-108">如果使用者尚未同意資料收集，而且應用程式有[CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded)設定為`true`，非必要 cookie 不傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="92c65-108">If a user hasn't consented to data collection and the app has [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) set to `true`, non-essential cookies aren't sent to the browser.</span></span>
* <span data-ttu-id="92c65-109">Cookie 可以標示為重要。</span><span class="sxs-lookup"><span data-stu-id="92c65-109">Cookies can be marked as essential.</span></span> <span data-ttu-id="92c65-110">基本的 cookie 會傳送至瀏覽器中，即使當使用者尚未同意，並已停用追蹤。</span><span class="sxs-lookup"><span data-stu-id="92c65-110">Essential cookies are sent to the browser even when the user hasn't consented and tracking is disabled.</span></span>
* <span data-ttu-id="92c65-111">[TempData 和工作階段 cookie](#tempdata)追蹤已停用時不會運作。</span><span class="sxs-lookup"><span data-stu-id="92c65-111">[TempData and Session cookies](#tempdata) aren't functional when tracking is disabled.</span></span>
* <span data-ttu-id="92c65-112">[身分識別管理](#pd)頁面提供連結讓您下載及刪除使用者資料。</span><span class="sxs-lookup"><span data-stu-id="92c65-112">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="92c65-113">[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample)可讓您測試大部分的 GDPR 擴充點，而且 Api 新增至 ASP.NET Core 2.1 範本。</span><span class="sxs-lookup"><span data-stu-id="92c65-113">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) allows you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="92c65-114">請參閱[讀我檔案](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample)檔案以供測試的指示。</span><span class="sxs-lookup"><span data-stu-id="92c65-114">See the [ReadMe](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="92c65-115">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="92c65-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="92c65-116">在範本產生的程式碼中的 ASP.NET Core GDPR 支援</span><span class="sxs-lookup"><span data-stu-id="92c65-116">ASP.NET Core GDPR support in template-generated code</span></span>

<span data-ttu-id="92c65-117">Razor Pages 和 MVC 專案範本建立的專案包含下列的 GDPR 支援：</span><span class="sxs-lookup"><span data-stu-id="92c65-117">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="92c65-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)並[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy)中所設定`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="92c65-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) are set in the `Startup` class.</span></span>
* <span data-ttu-id="92c65-119">*\_CookieConsentPartial.cshtml* [部分檢視](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="92c65-119">The *\_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="92c65-120">**接受**按鈕包含在這個檔案。</span><span class="sxs-lookup"><span data-stu-id="92c65-120">An **Accept** button is included in this file.</span></span> <span data-ttu-id="92c65-121">當使用者按一下**接受**按鈕，同意將 cookie 提供。</span><span class="sxs-lookup"><span data-stu-id="92c65-121">When the user clicks the **Accept** button, consent to store cookies is provided.</span></span>
* <span data-ttu-id="92c65-122">*Pages/Privacy.cshtml*頁面或*Views/Home/Privacy.cshtml*檢視會提供頁面的詳細說明您網站的隱私權原則。</span><span class="sxs-lookup"><span data-stu-id="92c65-122">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="92c65-123">*\_CookieConsentPartial.cshtml*檔案會產生 [隱私權] 頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="92c65-123">The *\_CookieConsentPartial.cshtml* file generates a link to the Privacy page.</span></span>
* <span data-ttu-id="92c65-124">使用個別使用者帳戶建立的應用程式，[管理] 頁面會提供連結來下載並刪除[使用者個人資料](#pd)。</span><span class="sxs-lookup"><span data-stu-id="92c65-124">For apps created with individual user accounts, the Manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="92c65-125">CookiePolicyOptions 和 UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="92c65-125">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="92c65-126">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)會初始化`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="92c65-126">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) are initialized in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="92c65-127">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy)呼叫`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="92c65-127">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) is called in `Startup.Configure`:</span></span>

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="92c65-128">\_CookieConsentPartial.cshtml partial view</span><span class="sxs-lookup"><span data-stu-id="92c65-128">\_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="92c65-129">*\_CookieConsentPartial.cshtml*部分檢視：</span><span class="sxs-lookup"><span data-stu-id="92c65-129">The *\_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="92c65-130">這個部分中：</span><span class="sxs-lookup"><span data-stu-id="92c65-130">This partial:</span></span>

* <span data-ttu-id="92c65-131">取得使用者追蹤的狀態。</span><span class="sxs-lookup"><span data-stu-id="92c65-131">Obtains the state of tracking for the user.</span></span> <span data-ttu-id="92c65-132">如果應用程式設定為需要同意，使用者必須同意之前可以追蹤 cookie。</span><span class="sxs-lookup"><span data-stu-id="92c65-132">If the app is configured to require consent, the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="92c65-133">如果需要同意的話，cookie 同意不固定在所建立的導覽列頂端 *\_Layout.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="92c65-133">If consent is required, the cookie consent panel is fixed at top of the navigation bar created by the *\_Layout.cshtml* file.</span></span>
* <span data-ttu-id="92c65-134">提供 HTML`<p>`項目，以摘述您的隱私權與 cookie 使用的原則。</span><span class="sxs-lookup"><span data-stu-id="92c65-134">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="92c65-135">提供隱私權頁面或檢視，其中詳細說明您網站的隱私權原則的連結。</span><span class="sxs-lookup"><span data-stu-id="92c65-135">Provides a link to Privacy page or view where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="92c65-136">基本的 cookie</span><span class="sxs-lookup"><span data-stu-id="92c65-136">Essential cookies</span></span>

<span data-ttu-id="92c65-137">如果同意將 cookie 未提供，標示為重要的 cookie 傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="92c65-137">If consent to store cookies hasn't been provided, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="92c65-138">下列程式碼可讓您基本的 cookie:</span><span class="sxs-lookup"><span data-stu-id="92c65-138">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

### <a name="tempdata-provider-and-session-state-cookies-arent-essential"></a><span data-ttu-id="92c65-139">TempData 提供者和工作階段狀態的 cookie 並不一定</span><span class="sxs-lookup"><span data-stu-id="92c65-139">TempData provider and session state cookies aren't essential</span></span>

<span data-ttu-id="92c65-140">[TempData 提供者](xref:fundamentals/app-state#tempdata)cookie 不必要的元素。</span><span class="sxs-lookup"><span data-stu-id="92c65-140">The [TempData provider](xref:fundamentals/app-state#tempdata) cookie isn't essential.</span></span> <span data-ttu-id="92c65-141">如果已停用追蹤，TempData 提供者將無法運作。</span><span class="sxs-lookup"><span data-stu-id="92c65-141">If tracking is disabled, the TempData provider isn't functional.</span></span> <span data-ttu-id="92c65-142">若要停用追蹤時，請啟用 TempData 提供者，將 TempData cookie 標示為以`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="92c65-142">To enable the TempData provider when tracking is disabled, mark the TempData cookie as essential in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

<span data-ttu-id="92c65-143">[工作階段狀態](xref:fundamentals/app-state)cookie 不重要。</span><span class="sxs-lookup"><span data-stu-id="92c65-143">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="92c65-144">停用追蹤時，工作階段狀態未作用。</span><span class="sxs-lookup"><span data-stu-id="92c65-144">Session state isn't functional when tracking is disabled.</span></span> <span data-ttu-id="92c65-145">下列程式碼可讓工作階段 cookie 不可或缺：</span><span class="sxs-lookup"><span data-stu-id="92c65-145">The following code makes session cookies essential:</span></span>

[!code-csharp[](gdpr/sample/RP/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="92c65-146">個人資料</span><span class="sxs-lookup"><span data-stu-id="92c65-146">Personal data</span></span>

<span data-ttu-id="92c65-147">使用個別使用者帳戶建立的 ASP.NET Core 應用程式包含下載和刪除個人資料的程式碼。</span><span class="sxs-lookup"><span data-stu-id="92c65-147">ASP.NET Core apps created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="92c65-148">選取的使用者名稱，然後選取**個人資料**:</span><span class="sxs-lookup"><span data-stu-id="92c65-148">Select the user name and then select **Personal data**:</span></span>

![管理個人資料頁面](gdpr/_static/pd.png)

<span data-ttu-id="92c65-150">附註：</span><span class="sxs-lookup"><span data-stu-id="92c65-150">Notes:</span></span>

* <span data-ttu-id="92c65-151">若要產生`Account/Manage`程式碼，請參閱 < [Scaffold 識別](xref:security/authentication/scaffold-identity)。</span><span class="sxs-lookup"><span data-stu-id="92c65-151">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="92c65-152">**刪除**並**下載**連結只會依據預設身分識別資料。</span><span class="sxs-lookup"><span data-stu-id="92c65-152">The **Delete** and **Download** links only act on the default identity data.</span></span> <span data-ttu-id="92c65-153">建立自訂使用者資料的應用程式必須延伸到 delete/下載自訂的使用者資料。</span><span class="sxs-lookup"><span data-stu-id="92c65-153">Apps that create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="92c65-154">如需詳細資訊，請參閱 <<c0> [ 加入、 下載及刪除身分識別的自訂使用者資料](xref:security/authentication/add-user-data)。</span><span class="sxs-lookup"><span data-stu-id="92c65-154">For more information, see [Add, download, and delete custom user data to Identity](xref:security/authentication/add-user-data).</span></span>
* <span data-ttu-id="92c65-155">儲存使用者的身分識別資料庫資料表中儲存的語彙基元`AspNetUserTokens`串聯的 delete 行為，因為透過刪除使用者時，會刪除[外部索引鍵](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152)。</span><span class="sxs-lookup"><span data-stu-id="92c65-155">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span></span>
* <span data-ttu-id="92c65-156">[外部提供者驗證](xref:security/authentication/social/index)，例如 Facebook 和 Google，無法使用之前接受 cookie 的原則。</span><span class="sxs-lookup"><span data-stu-id="92c65-156">[External provider authentication](xref:security/authentication/social/index), such as Facebook and Google, isn't available before the cookie policy is accepted.</span></span>

## <a name="encryption-at-rest"></a><span data-ttu-id="92c65-157">待用加密</span><span class="sxs-lookup"><span data-stu-id="92c65-157">Encryption at rest</span></span>

<span data-ttu-id="92c65-158">某些資料庫和儲存機制允許進行待用加密。</span><span class="sxs-lookup"><span data-stu-id="92c65-158">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="92c65-159">待用加密：</span><span class="sxs-lookup"><span data-stu-id="92c65-159">Encryption at rest:</span></span>

* <span data-ttu-id="92c65-160">自動加密儲存的資料。</span><span class="sxs-lookup"><span data-stu-id="92c65-160">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="92c65-161">加密而不需要設定、 程式設計中或其他軟體，以存取資料的工作。</span><span class="sxs-lookup"><span data-stu-id="92c65-161">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="92c65-162">是最簡單且最安全的選項。</span><span class="sxs-lookup"><span data-stu-id="92c65-162">Is the easiest and safest option.</span></span>
* <span data-ttu-id="92c65-163">讓資料庫可管理金鑰和加密。</span><span class="sxs-lookup"><span data-stu-id="92c65-163">Allows the database to manage keys and encryption.</span></span>

<span data-ttu-id="92c65-164">例如：</span><span class="sxs-lookup"><span data-stu-id="92c65-164">For example:</span></span>

* <span data-ttu-id="92c65-165">Microsoft SQL 和 Azure SQL 提供[透明資料加密](/sql/relational-databases/security/encryption/transparent-data-encryption)(TDE)。</span><span class="sxs-lookup"><span data-stu-id="92c65-165">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span></span>
* [<span data-ttu-id="92c65-166">SQL Azure 會將預設加密資料庫</span><span class="sxs-lookup"><span data-stu-id="92c65-166">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="92c65-167">[預設加密 azure Blob、 檔案、 資料表和佇列儲存體](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/)。</span><span class="sxs-lookup"><span data-stu-id="92c65-167">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="92c65-168">不會提供內建的加密靜止的資料庫，您可以使用磁碟加密來提供相同的保護。</span><span class="sxs-lookup"><span data-stu-id="92c65-168">For databases that don't provide built-in encryption at rest, you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="92c65-169">例如:</span><span class="sxs-lookup"><span data-stu-id="92c65-169">For example:</span></span>

* [<span data-ttu-id="92c65-170">BitLocker for Windows Server</span><span class="sxs-lookup"><span data-stu-id="92c65-170">BitLocker for Windows Server</span></span>](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="92c65-171">Linux：</span><span class="sxs-lookup"><span data-stu-id="92c65-171">Linux:</span></span>
  * [<span data-ttu-id="92c65-172">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="92c65-172">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="92c65-173">[EncFS](https://github.com/vgough/encfs)。</span><span class="sxs-lookup"><span data-stu-id="92c65-173">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="92c65-174">其他資源</span><span class="sxs-lookup"><span data-stu-id="92c65-174">Additional resources</span></span>

* [<span data-ttu-id="92c65-175">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="92c65-175">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/trustcenter/Privacy/GDPR)
