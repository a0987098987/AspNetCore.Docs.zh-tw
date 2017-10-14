---
title: "金鑰管理和存留期"
author: rick-anderson
description: "描述金鑰管理和存留期。"
keywords: "ASP.NET Core 金鑰管理，DPAPI，DataProtection"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ef7dad2a-7029-4ae5-8f06-1fbebedccaa4
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 5ac2d80e7d1cebcbc792e1247608e67991ec36f4
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/13/2017
---
# <a name="key-management-and-lifetime"></a>金鑰管理和存留期

<a name="data-protection-default-settings"></a>

## <a name="key-management"></a>金鑰管理

系統會嘗試偵測其操作環境，並提供良好的零設定行為預設值。 使用啟發學習法如下所示。

1. 如果系統正在裝載在 Azure 網站中，索引鍵會保存到"%home%\asp.net\dataprotection-keys"資料夾中。 此資料夾受到網路儲存裝置，而且裝載應用程式的所有電腦上同步處理。 索引鍵未受保護靜止。 這個資料夾可提供單一部署位置中的應用程式的所有執行個體索引鍵環形。 不同的部署位置，例如預備和生產環境，不會共用金鑰環形。 當您交換兩個部署位置，例如交換到生產環境的臨時區域，或使用 A / B 測試，使用資料保護任何系統將無法使用金鑰環形內的前一個插槽的預存的資料解密。 這會導致使用者記錄從 ASP.NET 應用程式使用標準 ASP.NET cookie 中介軟體，因為它使用資料保護來保護其 cookie。 如果您想要獨立位置的索引鍵環形，使用外部索引鍵環提供者，例如 Azure Blob 儲存體，Azure 金鑰保存庫中，SQL 存放區或 Redis 快取。

2. 如果使用者設定檔可用時，金鑰會保存到"%localappdata%\asp.net\dataprotection-keys"資料夾。 此外，如果作業系統為 Windows，它們會在待用期間使用 DPAPI 會加密。

3. 如果應用程式裝載在 IIS 中，會保存 ACLed 只是為了背景工作處理序帳戶的特殊的登錄機碼 HKLM 登錄機碼。 在待用期間使用 DPAPI 加密金鑰。

4. 如果上述條件符合時，索引鍵不會保存在目前的程序之外。 處理程序關閉時，產生所有索引鍵將會遺失。

開發人員一定是完全控制，而且可以覆寫的方式，以及索引鍵儲存位置。 上面的前三個選項都應該很好的預設值對於大部分應用程式類似 ASP.NET<machineKey>自動產生常式過去。 最後，控制項會回復選項是真正需要開發人員指定的唯一案例[組態](overview.md)前方如果他們想要的索引鍵的持續性，但只在極少數的情況下發生此後援。

>[!WARNING]
> 如果開發人員會覆寫這個啟發學習法，並指向特定的金鑰儲存機制的資料保護系統，將會停用自動加密在靜止的索引鍵。 待用保護可以透過重新啟用[組態](overview.md)。

## <a name="key-lifetime"></a>金鑰存留期

預設的索引鍵具有 90 天的存留期。 金鑰過期時，系統會自動產生新的金鑰，並將新的金鑰設定為作用中金鑰。 只要已停用的索引鍵會保留在系統上仍然可以解密受保護的任何資料。 請參閱[金鑰管理](../implementation/key-management.md#data-protection-implementation-key-management-expiration)如需詳細資訊。

## <a name="default-algorithms"></a>預設的演算法

使用的預設裝載保護演算法為 256-AES-CBC 機密性和 HMACSHA256 的真實性。 512 位元主要金鑰，回復每隔 90 天，用來衍生這些每個裝載為基礎的演算法使用兩個子機碼。 請參閱[子機碼衍生](../implementation/subkeyderivation.md#data-protection-implementation-subkey-derivation-aad)如需詳細資訊。
