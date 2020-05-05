---
title: ASP.NET Core 中的一般資料保護規定（GDPR）支援
author: rick-anderson
description: 瞭解如何存取 ASP.NET Core web 應用程式中的 GDPR 擴充點。
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/gdpr
ms.openlocfilehash: 68f8ebaafd1aaa725ef1ff41f2ffa9f605e49f7f
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776320"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>ASP.NET Core 中的 EU 一般資料保護規定（GDPR）支援

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 提供 Api 和範本，以協助符合[歐盟一般資料保護規定（GDPR）](https://www.eugdpr.org/)需求：

::: moniker range=">= aspnetcore-3.0"

* 專案範本包括擴充點和 stub 標記，您可以將其取代為您的隱私權和 cookie 使用原則。
* [ *Pages/隱私權*] 頁面或 [ *Views/Home/隱私權*] 視圖會提供頁面，以詳細說明網站的隱私權原則。

若要啟用預設的 cookie 同意功能，例如在 ASP.NET Core 3.0 範本產生的應用程式的 ASP.NET Core 2.2 範本中找到：

* 將`using Microsoft.AspNetCore.Http`加入 using 指示詞的清單。
* 將[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)新增`Startup.ConfigureServices`至[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) ，並`Startup.Configure`UseCookiePolicy 至：

  [!code-csharp[Main](gdpr/sample/RP3.0/Startup.cs?name=snippet1&highlight=12-19,38)]

* 將 cookie 同意部分新增至 *_Layout 的 cshtml*檔案：

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_Layout.cshtml?name=snippet&highlight=4)]

* 將* \_CookieConsentPartial*檔案新增至專案：

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_CookieConsentPartial.cshtml)]

* 選取本文的 ASP.NET Core 2.2 版本，以閱讀 cookie 同意功能的相關資訊。

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* 專案範本包括擴充點和 stub 標記，您可以將其取代為您的隱私權和 cookie 使用原則。
* Cookie 同意功能可讓您向使用者要求（並追蹤）同意，以儲存個人資訊。 如果使用者尚未同意資料收集，而應用程式已將[CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded)設定為`true`，則不會將非必要的 cookie 傳送至瀏覽器。
* Cookie 可以標示為必要。 即使使用者尚未同意，也會將必要的 cookie 傳送至瀏覽器，而且會停用追蹤。
* 停用追蹤時[，TempData 和會話 cookie](#tempdata)無法正常運作。
* [身分[識別管理](#pd)] 頁面提供下載和刪除使用者資料的連結。

[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample)可讓您測試新增至 ASP.NET Core 2.1 範本的大部分 GDPR 擴充點和 api。 如需測試指示，請參閱[自述](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample)檔。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>在範本產生的程式碼中 ASP.NET Core GDPR 支援

使用專案範本建立的 Razor Pages 和 MVC 專案包含下列 GDPR 支援：

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)和[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy)是在`Startup`類別中設定。
* * \_CookieConsentPartial*的[部分視圖](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)。 此檔案包含 [**接受**] 按鈕。 當使用者按一下 [**接受**] 按鈕時，就會提供同意存放區 cookie。
* [ *Pages/隱私權*] 頁面或 [ *Views/Home/隱私權*] 視圖會提供頁面，以詳細說明網站的隱私權原則。 CookieConsentPartial 會產生 [隱私權] 頁面的連結。 * \_ *
* 針對使用個別使用者帳戶所建立的應用程式，[管理] 頁面會提供下載和刪除[個人使用者資料](#pd)的連結。

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions 和 UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)在中`Startup.ConfigureServices`初始化：

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy)是在中`Startup.Configure`呼叫：

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="_cookieconsentpartialcshtml-partial-view"></a>\_CookieConsentPartial. cshtml 部分視圖

* \_CookieConsentPartial*的部分視圖：

[!code-html[](gdpr/sample/RP2.2/Pages/Shared/_CookieConsentPartial.cshtml)]

這部分：

* 取得使用者的追蹤狀態。 如果應用程式已設定為需要同意，使用者必須先同意，才能追蹤 cookie。 如果需要同意，cookie 同意面板會固定在配置* \_. cshtml*檔案所建立的導覽列上方。
* 提供 HTML `<p>`元素來總結您的隱私權與 cookie 使用原則。
* 提供 [隱私權] 頁面或 [流覽] 的連結，您可以在其中詳細說明網站的隱私權原則。

## <a name="essential-cookies"></a>基本 cookie

如果尚未提供同意存放區 cookie，則只會將標示為必要的 cookie 傳送至瀏覽器。 下列程式碼會讓 cookie 變得很重要：

[!code-csharp[Main](gdpr/sample/RP2.2/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

### <a name="tempdata-provider-and-session-state-cookies-arent-essential"></a>TempData 提供者和會話狀態 cookie 不是必要的

[TempData 提供者](xref:fundamentals/app-state#tempdata)cookie 並不重要。 如果停用追蹤，TempData 提供者就無法運作。 若要在停用追蹤時啟用 TempData 提供者，請在中將 TempData `Startup.ConfigureServices`cookie 標記為必要項：

[!code-csharp[Main](gdpr/sample/RP2.2/Startup.cs?name=snippet1)]

[會話狀態](xref:fundamentals/app-state)cookie 並不重要。 停用追蹤時，會話狀態無法運作。 下列程式碼會讓會話 cookie 變得很重要：

[!code-csharp[](gdpr/sample/RP2.2/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a>個人資料

使用個別使用者帳戶所建立 ASP.NET Core 應用程式包含下載和刪除個人資料的程式碼。

選取使用者名稱，然後選取 [**個人資料**]：

![管理個人資料頁面](gdpr/_static/pd.png)

注意：

* 若要產生`Account/Manage`程式碼，請參閱[Scaffold Identity](xref:security/authentication/scaffold-identity)。
* [**刪除**] 和 [**下載**] 連結只會對預設身分識別資料採取行動。 建立自訂使用者資料的應用程式必須擴充，才能刪除/下載自訂使用者資料。 如需詳細資訊，請參閱[將自訂使用者資料新增、下載及刪除至身分識別](xref:security/authentication/add-user-data)。
* 當使用者因為[外鍵](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152)而透過串聯刪除行為刪除時， `AspNetUserTokens`會刪除儲存在身分識別資料庫資料表中之使用者的已儲存權杖。
* 在接受 cookie 原則之前，無法使用[外部提供者驗證](xref:security/authentication/social/index)（例如 Facebook 和 Google）。

::: moniker-end

## <a name="encryption-at-rest"></a>待用加密

某些資料庫和儲存機制允許待用加密。 待用加密：

* 自動加密儲存的資料。
* 針對存取資料的軟體，不需要設定、程式設計或其他工作即可進行加密。
* 是最簡單且最安全的選項。
* 允許資料庫管理金鑰和加密。

例如：

* Microsoft SQL 和 Azure SQL 提供[透明資料加密](/sql/relational-databases/security/encryption/transparent-data-encryption)（TDE）。
* [SQL Azure 預設會加密資料庫](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [預設會加密 Azure blob、檔案、資料表和佇列儲存體](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/)。

對於未提供內建靜態加密的資料庫，您可以使用磁片加密來提供相同的保護。 例如：

* [適用于 Windows Server 的 BitLocker](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* Linux：
  * [eCryptfs](https://launchpad.net/ecryptfs)
  * [EncFS](https://github.com/vgough/encfs)。

## <a name="additional-resources"></a>其他資源

* [Microsoft.com/GDPR](https://www.microsoft.com/trustcenter/Privacy/GDPR)
* [GDPR-在 ASP.NET Core 中新增 [撤銷同意] 按鈕](https://www.joeaudette.com/blog/2018/08/28/gdpr---adding-a-revoke-consent-button-in-aspnet-core)
