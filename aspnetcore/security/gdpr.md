---
title: 支援 ASP.NET Core 中的一般資料保護規定 (GDPR)
author: rick-anderson
description: 了解如何存取 ASP.NET Core web 應用程式中的 GDPR 擴充點。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/29/2018
uid: security/gdpr
ms.openlocfilehash: 10384d2abad7692d45f2be19f3ba7f8f8e8c3e17
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832026"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>ASP.NET Core 中的歐盟一般資料保護規定 (GDPR) 支援

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 提供 Api 和範本，以協助符合某些[歐盟一般資料保護規定 (GDPR)](https://www.eugdpr.org/)需求：

* 專案範本包括擴充點以及附加虛設常式的標記，您可以使用您的隱私權與 cookie 的使用原則來取代。
* Cookie 同意功能可讓您要求 （和追蹤） 同意從您的使用者，用來儲存個人資訊。 如果使用者尚未同意資料收集，而且應用程式有[CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded)設定為`true`，非必要 cookie 不傳送至瀏覽器。
* Cookie 可以標示為重要。 基本的 cookie 會傳送至瀏覽器中，即使當使用者尚未同意，並已停用追蹤。
* [TempData 和工作階段 cookie](#tempdata)追蹤已停用時不會運作。
* [身分識別管理](#pd)頁面提供連結讓您下載及刪除使用者資料。

[範例應用程式](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample)可讓您測試大部分的 GDPR 擴充點，而且 Api 新增至 ASP.NET Core 2.1 範本。 請參閱[讀我檔案](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample)檔案以供測試的指示。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>ASP.NET Core 範本中的 GDPR 支援產生的程式碼

Razor Pages 和 MVC 專案範本建立的專案包含下列的 GDPR 支援：

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)並[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy)中所設定`Startup`。
* *_CookieConsentPartial.cshtml* [部分檢視](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)。
* *Pages/Privacy.cshtml*頁面或*Views/Home/Privacy.cshtml*檢視會提供頁面的詳細說明您網站的隱私權原則。 *_CookieConsentPartial.cshtml*檔案會產生 [隱私權] 頁面的連結。
* 使用個別使用者帳戶建立的應用程式，[管理] 頁面會提供連結來下載並刪除[使用者個人資料](#pd)。

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions 和 UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)會初始化`Startup.ConfigureServices`:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy)呼叫`Startup.Configure`:

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a>_CookieConsentPartial.cshtml 部分檢視

*_CookieConsentPartial.cshtml*部分檢視：

[!code-html[](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

這個部分中：

* 取得使用者追蹤的狀態。 如果應用程式設定為需要同意，使用者必須同意之前可以追蹤 cookie。 如果需要同意的話，cookie 同意不固定在所建立的導覽列頂端 *_Layout.cshtml*檔案。
* 提供 HTML`<p>`項目，以摘述您的隱私權與 cookie 使用的原則。
* 提供隱私權頁面或檢視，其中詳細說明您網站的隱私權原則的連結。

## <a name="essential-cookies"></a>基本的 cookie

如果尚未被授與同意，就會標示為重要的 cookie 傳送至瀏覽器。 下列程式碼可讓您基本的 cookie:

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a>Tempdata 提供者和工作階段狀態的 cookie 不重要

[Tempdata 提供者](xref:fundamentals/app-state#tempdata)cookie 不必要的元素。 如果已停用追蹤，Tempdata 提供者將無法運作。 若要停用追蹤時，請啟用 Tempdata 提供者，將 TempData cookie 標示為以`Startup.ConfigureServices`:

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

[工作階段狀態](xref:fundamentals/app-state)cookie 不重要。 停用追蹤時，工作階段狀態未作用。

<a name="pd"></a>

## <a name="personal-data"></a>個人資料

使用個別使用者帳戶建立的 ASP.NET Core 應用程式包含下載和刪除個人資料的程式碼。

選取的使用者名稱，然後選取**個人資料**:

![管理個人資料頁面](gdpr/_static/pd.png)

附註：

* 若要產生`Account/Manage`程式碼，請參閱 < [Scaffold 識別](xref:security/authentication/scaffold-identity)。
* 刪除和下載只會影響預設身分識別資料。 建立自訂使用者資料的應用程式必須延伸到 delete/下載自訂的使用者資料。 如需詳細資訊，請參閱 <<c0> [ 加入、 下載及刪除身分識別的自訂使用者資料](xref:security/authentication/add-user-data)。
* 儲存使用者的身分識別資料庫資料表中儲存的語彙基元`AspNetUserTokens`串聯的 delete 行為，因為透過刪除使用者時，會刪除[外部索引鍵](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152)。

## <a name="encryption-at-rest"></a>待用加密

某些資料庫和儲存機制允許進行待用加密。 待用加密：

* 自動加密儲存的資料。
* 加密而不需要設定、 程式設計中或其他軟體，以存取資料的工作。
* 是最簡單且最安全的選項。
* 讓資料庫可管理金鑰和加密。

例如: 

* Microsoft SQL 和 Azure SQL 提供[透明資料加密](/sql/relational-databases/security/encryption/transparent-data-encryption)(TDE)。
* [SQL Azure 會將預設加密資料庫](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [預設加密 azure Blob、 檔案、 資料表和佇列儲存體](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/)。

不會提供內建的加密靜止的資料庫，您可以使用磁碟加密來提供相同的保護。 例如: 

* [適用於 Windows Server 的 BitLocker](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* Linux:
  * [eCryptfs](https://launchpad.net/ecryptfs)
  * [EncFS](https://github.com/vgough/encfs)。

## <a name="additional-resources"></a>其他資源

* [Microsoft.com/GDPR](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)
