---
title: 資料保護的金鑰管理和 ASP.NET Core 中的存留期
author: rick-anderson
description: 深入了解資料保護的金鑰管理和 ASP.NET Core 中的存留期。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 2f022a4c7519485fe629ce47c27d214c8c27d5bc
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159207"
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a>資料保護的金鑰管理和 ASP.NET Core 中的存留期

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="key-management"></a>金鑰管理

應用程式會嘗試偵測其操作環境和處理上自己的金鑰設定。

1. 如果應用程式裝載於[Azure 應用程式](https://azure.microsoft.com/services/app-service/)，金鑰會保存到 *%HOME%\ASP.NET\DataProtection-Keys*資料夾。 此資料夾使用網路儲存體進行保存，並會在裝載應用程式的所有電腦上同步。
   * 金鑰待用時不受保護。
   * *DataProtection 金鑰*資料夾提供金鑰環單一部署位置中的應用程式的所有執行個體。
   * 各部署位置，例如預備和生產位置，不會共用金鑰環。 當您交換部署位置，例如交換預備環境或生產環境，或使用 A 之間 / B 測試、 使用資料保護的任何應用程式將無法解密儲存的資料使用前一個位置內的金鑰環。 這會導致正在登入的使用者複製應用程式使用標準的 ASP.NET Core cookie 驗證，因為它使用資料保護來保護其 cookie。 如果您想要的位置無關的金鑰環，請使用外部金鑰環提供者，例如 Azure Blob 儲存體、 Azure Key Vault，SQL 存放區中，或 Redis 快取。

1. 如果使用者設定檔可用時，會保存索引鍵 *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys*資料夾。 如果作業系統是 Windows，在待用期間使用 DPAPI 加密金鑰。

   應用程式集區的 [setProfileEnvironment 屬性](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)也必須啟用。 `setProfileEnvironment` 的預設值為 `true`。 在某些情況下 (例如 Windows OS)，`setProfileEnvironment` 會設為 `false`。 如果金鑰並未如預期地儲存在使用者設定檔目錄中：

   1. 瀏覽至 *%windir%/system32/inetsrv/config* 資料夾。
   1. 開啟 *applicationHost.config* 檔案。
   1. 找到 `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` 項目。
   1. 確認 `setProfileEnvironment` 屬性不存在 (其預設值為 `true`)，或明確地將屬性值設為 `true`。

1. 如果應用程式裝載在 IIS 中，金鑰會保存到特殊的登錄機碼，只是背景工作處理序帳戶列入 Acl 中的 HKLM 登錄中。 在待用期間使用 DPAPI 加密金鑰。

1. 如果上述條件均不相符時，金鑰不會保存在目前的程序之外。 處理程序關閉時，所有已產生的金鑰將會遺失。

開發人員一律可以完全控制，並儲存金鑰，以及可以覆寫。 上述的前三個選項應該提供不錯的預設值對於大多數的應用程式，類似 ASP.NET  **\<machineKey >** 自動產生常式過去。 最終，後援選項是需要開發人員指定的唯一案例[組態](xref:security/data-protection/configuration/overview)預付如果他們想要金鑰的持續性，但此後援才會發生在少數情況下。

當裝載的 Docker 容器中，金鑰應保留在 Docker 磁碟區 （共用磁碟區或主應用程式掛接的磁碟區會保存超過容器的存留期） 的資料夾或外部提供者，例如[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)或是[Redis](https://redis.io/)。 如果應用程式無法存取的共用的網路磁碟區，還有在 web 伺服陣列案例中有用外部提供者 (請參閱[PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem)如需詳細資訊)。

> [!WARNING]
> 如果開發人員會覆寫上面所述的規則，並指向特定的金鑰存放庫資料保護系統，會停用自動加密待用的金鑰。 待用保護可以透過重新啟用[組態](xref:security/data-protection/configuration/overview)。

## <a name="key-lifetime"></a>金鑰存留期

根據預設，索引鍵具有 90 天的存留期。 索引鍵過期時，應用程式自動產生新的金鑰，並將新的金鑰設定為作用中金鑰。 只要已停用的索引鍵會保留在系統上，您的應用程式可以解密與其受保護的任何資料。 請參閱[金鑰管理](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling)如需詳細資訊。

## <a name="default-algorithms"></a>預設的演算法

使用預設承載的保護演算法會是 AES-256-CBC 機密性和 HMACSHA256 的真實性。 變更每隔 90 天，512 位元主要金鑰用來衍生兩個的子機碼，以使用這些演算法，每個裝載為基礎。 請參閱[子機碼衍生](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation)如需詳細資訊。

## <a name="additional-resources"></a>其他資源

* <xref:security/data-protection/extensibility/key-management>
* <xref:host-and-deploy/web-farm>
