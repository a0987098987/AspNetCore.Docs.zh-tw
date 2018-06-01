---
title: 在 ASP.NET Core 的一般資料保護規定 (GDPR) 支援
author: rick-anderson
description: 示範如何存取 GDPR 擴充點，在 ASP.NET Core web 應用程式。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/gdpr
ms.openlocfilehash: 92a7000f4f8e4c2097065cb530fe106ef0e98545
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2018
ms.locfileid: "34688623"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>在 ASP.NET Core EU 一般資料保護規定 (GDPR) 支援

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 提供應用程式開發介面和範本可協助符合某些[EU 一般資料保護規定 (GDPR)](https://www.eugdpr.org/)需求：

* 專案範本包含了擴充點，以及您可以取代您的隱私權和 cookie 的使用原則的 stub 的標記。
* Cookie 同意功能可讓您要求 （和追蹤） 同意從您的使用者，用來儲存個人資訊。 如果使用者不同意資料收集，且應用程式設定與[CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded)至`true`，非必要 cookie 將不會傳送至瀏覽器。
* Cookie 可以標示為重要。 基本的 cookie 會傳送至瀏覽器中，即使當使用者不同意並追蹤已停用。
* [TempData 和工作階段 cookie](#tempdata)停用追蹤時，會無法運作。
* [身分識別管理](#pd)頁面提供的連結下載及刪除使用者資料。

[範例應用程式](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample)可讓您測試的大部分 GDPR 擴充點和應用程式開發介面加入至 ASP.NET Core 2.1 範本。 請參閱[讀我檔案](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample)檔，以測試指示。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>在範本中的 ASP.NET Core GDPR 支援產生的程式碼

Razor 頁面和 MVC 專案範本建立專案，包含下列 GDPR 支援：

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0)和[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_)中設定`Startup`。
* *_CookieConsentPartial.cshtml* [部分檢視](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)。
* *Pages/Privacy.cshtml*或*Home/Privacy.cshtml*檢視會提供您網站的隱私權原則的詳細資料頁面。 *_CookieConsentPartial.cshtml*檔案會產生隱私權頁面的連結。
* 建立個別的使用者帳戶的應用程式，如 [管理] 頁面提供下載和刪除連結[個人使用者資料](#pd)。

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions 和 UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0)中初始化`Startup`類別`ConfigureServices`方法：

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_)中呼叫`Startup`類別`Configure`方法：

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a>_CookieConsentPartial.cshtml 部分檢視

*_CookieConsentPartial.cshtml*部分檢視：

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

此部分：

* 取得使用者追蹤的狀態。 如果應用程式已設定為需要同意，使用者必須同意之前可以追蹤 cookie。 如果需要同意的話，cookie 同意 chrome 固定的上方導覽列中建立*Pages/Shared/_Layout.cshtml*檔案。
* 提供 HTML`<p>`摘要說明您的隱私權和 cookie 的項目使用的原則。
* 提供的連結*Pages/Privacy.cshtml*其中詳細說明網站的隱私權原則。

## <a name="essential-cookies"></a>基本的 cookie

如果尚未被授與同意，標示為重要的 cookie 會傳送至瀏覽器。 下列程式碼會進行基本的 cookie:

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a>Tempdata 提供者和工作階段狀態的 cookie 不需對內文

[Tempdata 提供者](xref:fundamentals/app-state#tempdata)cookie 並不重要。 如果追蹤已停用，Tempdata 提供者不是功能。 若要停用追蹤時，請啟用 Tempdata 提供者，將 TempData cookie 標示為重要中`ConfigureServices`:

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

[工作階段狀態](xref:fundamentals/app-state)cookie 不重要。 停用追蹤時，工作階段狀態沒有作用。

<a name="pd"></a>

## <a name="personal-data"></a>個人資料

使用個別的使用者帳戶建立的 ASP.NET Core 應用程式包括下載和刪除個人資料的程式碼。

選取的使用者名稱，然後選取**個人資料**:

![管理個人資料頁面](gdpr/_static/pd.png)

附註：

* 若要產生`Account/Manage`程式碼，請參閱[Scaffold 識別](xref:security/authentication/scaffold-identity)。
* 刪除與下載只會影響預設識別資料。 刪除下載的自訂使用者資料，您必須擴充應用程式建立自訂使用者資料。 GitHub 問題[如何加入/刪除自訂的使用者身分識別資料](https://github.com/aspnet/Docs/issues/6226)會建議的文件追蹤上建立自訂/刪除/下載自訂使用者資料。 如果您想要請參閱該主題設定優先權，將保留反應顆星的問題。
* 儲存使用者識別資料庫資料表中儲存的語彙基元`AspNetUserTokens`串聯刪除行為因為透過刪除使用者時，會刪除[外部索引鍵](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152)。

## <a name="encryption-at-rest"></a>靜態加密

某些資料庫和儲存機制允許靜態加密。 靜態加密：

* 會自動加密儲存的資料。
* 設定、 程式設計中或其他軟體存取之資料的工作不會加密。
* 是最簡單且最安全的選項。
* 可讓管理金鑰和加密的資料庫。

例如: 

* Microsoft SQL 和 Azure SQL 提供[透明資料加密](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017)(TDE)。
* [SQL Azure 會根據預設來加密資料庫](https://azure.microsoft.com/en-us/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [Azure Blob、 檔案、 資料表和佇列儲存體預設會經過加密](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/)。

對於不提供內建加密在靜止的資料庫，您可以使用磁碟加密，以提供相同的保護。 例如: 

* [適用於 windows server 的 Bitlocker](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* Linux:
  * [eCryptfs](https://launchpad.net/ecryptfs)
  * [EncFS](https://github.com/vgough/encfs)。

## <a name="additional-resources"></a>其他資源

* [Microsoft.com/GDPR](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)
