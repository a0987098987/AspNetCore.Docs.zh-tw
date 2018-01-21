---
title: "資料保護金鑰管理和 ASP.NET Core 存留期"
author: rick-anderson
description: "了解資料保護的金鑰管理和 ASP.NET Core 存留期。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 0993c68e37944a3aad863b98f92fe0140cfb746d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a>資料保護金鑰管理和 ASP.NET Core 存留期

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="key-management"></a>金鑰管理

應用程式會嘗試偵測其操作環境和處理自己的金鑰組態而有所不同。

1. 如果應用程式裝載在[Azure 應用程式](https://azure.microsoft.com/services/app-service/)，金鑰會保存到*%HOME%\ASP.NET\DataProtection-Keys*資料夾。 此資料夾受到網路儲存裝置，而且裝載應用程式的所有電腦上同步處理。
   * 索引鍵未受保護靜止。
   * *DataProtection 金鑰*資料夾提供單一部署位置中的應用程式的所有執行個體索引鍵環形。
   * 不同的部署位置，例如預備和生產環境，不會共用金鑰環形。 當您交換兩個部署位置，例如交換到生產環境的臨時區域，或使用 A / B 測試，使用資料保護的任何應用程式將無法使用金鑰環形內的前一個插槽的預存的資料解密。 這會導致使用者記錄從應用程式會使用標準 ASP.NET Core cookie 驗證中，因為它使用資料保護來保護其 cookie。 如果您想要獨立位置的索引鍵環形，使用外部索引鍵環提供者，例如 Azure Blob 儲存體，Azure 金鑰保存庫中，SQL 存放區或 Redis 快取。

1. 如果使用者設定檔可用時，會保存金鑰*%LOCALAPPDATA%\ASP.NET\DataProtection-Keys*資料夾。 如果作業系統為 Windows，該金鑰會加密在靜止使用 DPAPI。

1. 如果應用程式裝載在 IIS 中，會保存 ACLed 只是為了背景工作處理序帳戶的特殊的登錄機碼 HKLM 登錄機碼。 在待用期間使用 DPAPI 加密金鑰。

1. 如果上述條件均不相符時，索引鍵不被保存在目前的程序之外。 處理程序關閉時，產生所有索引鍵將會遺失。

開發人員一定是完全控制，而且可以覆寫的方式，以及索引鍵儲存位置。 上面的前三個選項應該很好的預設值提供給大部分的應用程式，類似 ASP.NET  **\<machineKey >**自動產生常式過去。 最後，後援選項是需要開發人員指定的唯一案例[組態](xref:security/data-protection/configuration/overview)前方如果他們想要的索引鍵的持續性，但此後援只會在極少數的情況下發生。

當主控在 Docker 容器中，金鑰應保留在 Docker 磁碟區 （共用磁碟區或主機掛接的磁碟區容器的存留期保存） 的資料夾或外部提供者，例如[Azure 金鑰保存庫](https://azure.microsoft.com/services/key-vault/)或[Redis](https://redis.io/)。 外部提供者也是在 web 伺服陣列案例中很有用的應用程式無法存取網路共用磁碟區 (請參閱[PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem)如需詳細資訊)。

> [!WARNING]
> 如果開發人員覆寫以上所述的規則，並指向特定的金鑰儲存機制的資料保護系統，會停用自動加密在靜止的索引鍵。 在 rest 保護可以透過重新啟用[組態](xref:security/data-protection/configuration/overview)。

## <a name="key-lifetime"></a>金鑰存留期

索引鍵具有預設的 90 天的存留期。 金鑰過期時，應用程式會自動產生新的金鑰，然後將新的金鑰設定為作用中金鑰。 只要已停用的索引鍵會保留在系統上，您的應用程式可以解密與它們保護的任何資料。 請參閱[金鑰管理](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling)如需詳細資訊。

## <a name="default-algorithms"></a>預設的演算法

使用的預設裝載保護演算法為 256-AES-CBC 機密性和 HMACSHA256 的真實性。 512 位元主要金鑰，每 90 天變更，用來衍生這些每個裝載為基礎的演算法使用兩個子機碼。 請參閱[子機碼衍生](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation)如需詳細資訊。

## <a name="see-also"></a>另請參閱

* [金鑰管理擴充性](xref:security/data-protection/extensibility/key-management)
