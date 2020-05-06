---
title: ASP.NET Core 中的資料保護金鑰管理和存留期
author: rick-anderson
description: 深入瞭解 ASP.NET Core 中的資料保護金鑰管理和存留期。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 1db5177230fd4076af080e208f094ce4d6537c62
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82777445"
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a>ASP.NET Core 中的資料保護金鑰管理和存留期

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="key-management"></a>金鑰管理

應用程式會嘗試偵測其操作環境，並自行處理金鑰設定。

1. 如果應用程式裝載于[Azure 應用程式](https://azure.microsoft.com/services/app-service/)中，金鑰會保存至 *%HOME%\ASP.NET\DataProtection-Keys*資料夾。 此資料夾使用網路儲存體進行保存，並會在裝載應用程式的所有電腦上同步。
   * 金鑰待用時不受保護。
   * [ *DataProtection Keys* ] 資料夾會在單一部署位置中，將金鑰環提供給應用程式的所有實例。
   * 各部署位置，例如預備和生產位置，不會共用金鑰環。 當您在部署位置之間交換時（例如交換暫存至生產環境或使用 A/B 測試），任何使用資料保護的應用程式將無法使用先前插槽內的金鑰環來解密儲存的資料。 這會導致使用者登出使用標準 ASP.NET Core cookie 驗證的應用程式，因為它使用資料保護來保護其 cookie。 如果您想要與位置無關的金鑰環，請使用外部金鑰環形提供者，例如 Azure Blob 儲存體、Azure Key Vault、SQL 存放區或 Redis 快取。

1. 如果有可用的使用者設定檔，金鑰會保存在 *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys*資料夾中。 如果作業系統是 Windows，則會使用 DPAPI 在待用時加密金鑰。

   應用程式集區的 [setProfileEnvironment 屬性](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)也必須啟用。 `setProfileEnvironment` 的預設值為 `true`。 在某些情況下 (例如 Windows OS)，`setProfileEnvironment` 會設為 `false`。 如果金鑰並未如預期地儲存在使用者設定檔目錄中：

   1. 瀏覽至 *%windir%/system32/inetsrv/config* 資料夾。
   1. 開啟 *applicationHost.config* 檔案。
   1. 找出 `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` 元素。
   1. 確認 `setProfileEnvironment` 屬性不存在 (其預設值為 `true`)，或明確地將屬性值設為 `true`。

1. 如果應用程式裝載于 IIS 中，則金鑰會保存到 HKLM 登錄中，只會碰到到背景工作進程帳戶的特殊登錄機碼中。 在待用期間使用 DPAPI 加密金鑰。

1. 如果這些條件都不符合，則索引鍵不會保存在目前的進程之外。 當進程關閉時，所有產生的金鑰都會遺失。

開發人員永遠都是完全控制，而且可以覆寫儲存金鑰的方式和位置。 上述前三個選項應該為大部分應用程式提供良好的預設值，類似于 ASP.NET ** \<machineKey>** 自動產生常式在過去的運作方式。 最終的 [回溯] 選項是唯一需要開發[人員指定預先](xref:security/data-protection/configuration/overview)設定的情況（如果他們想要保留金鑰），但只有在罕見的情況下才會發生這種情況。

在 Docker 容器中裝載時，金鑰應保存在屬於 Docker 磁片區的資料夾中（共用磁片區或保存在容器存留期以外的主機裝載磁片區），或在外部提供者（例如[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)或[Redis](https://redis.io/)）中。 如果應用程式無法存取共用網路磁片區，則外部提供者也適用于 web 伺服陣列案例（如需詳細資訊，請參閱[PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) ）。

> [!WARNING]
> 如果開發人員覆寫上面所述的規則，並將資料保護系統指向特定的金鑰存放庫，則會停用待用金鑰的自動加密功能。 待用保護可以透過設定重新[啟用。](xref:security/data-protection/configuration/overview)

## <a name="key-lifetime"></a>金鑰存留期

金鑰預設為90天的存留期。 金鑰過期時，應用程式會自動產生新的金鑰，並將新的金鑰設定為作用中金鑰。 只要已淘汰的金鑰保留在系統上，您的應用程式就可以將任何受其保護的資料解密。 如需詳細資訊，請參閱[金鑰管理](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling)。

## <a name="default-algorithms"></a>預設演算法

使用的預設承載保護演算法是 AES-256-CBC，適用于機密性和 HMACSHA256 的真實性。 512位主要金鑰（每90天變更）用來針對每個承載，衍生用於這些演算法的兩個子機碼。 如需詳細資訊，請參閱子機碼[衍生](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation)。

## <a name="additional-resources"></a>其他資源

* <xref:security/data-protection/extensibility/key-management>
* <xref:host-and-deploy/web-farm>
