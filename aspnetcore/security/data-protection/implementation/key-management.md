---
title: ASP.NET Core 中的金鑰管理
author: rick-anderson
description: 了解 ASP.NET Core 資料保護金鑰管理的 Api 實作詳細資料。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: 431bdf2d3076c83279b78f327ddb647f69e6e584
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219247"
---
# <a name="key-management-in-aspnet-core"></a>ASP.NET Core 中的金鑰管理

<a name="data-protection-implementation-key-management"></a>

資料保護系統會自動管理主要金鑰用來保護且取消保護承載的存留的期。 每個索引鍵可以存在於其中的四個階段：

* 建立索引鍵存在於金鑰環，但是尚未啟動。 索引鍵不應該用於新的保護作業有足夠的時間經過之前，機碼有機會傳播到所有的機器會使用此金鑰環。

* 使用中-索引鍵存在於金鑰環，並應用於所有新的保護作業。

* 到期-機碼已執行其自然的存留期，並無法再用於新的保護作業。

* 撤銷-金鑰遭到入侵，且必須不適用於新的保護作業。

建立、 使用中，且已過期的金鑰所有可用來取消保護傳入的裝載。 依預設已撤銷的金鑰不可取消保護內容，但應用程式開發人員可以[覆寫這個行為](xref:security/data-protection/consumer-apis/dangerous-unprotect#data-protection-consumer-apis-dangerous-unprotect)如有必要。

>[!WARNING]
> 開發人員可能會想要從金鑰環刪除金鑰，（例如，藉由從檔案系統中刪除對應的檔案）。 此時，金鑰所保護的所有資料都都會永久觸，並都沒有緊急的覆寫像是使用已撤銷的索引鍵。 刪除索引鍵是真正破壞性的行為，因此資料保護系統會公開任何第一級的 API 執行此作業。

## <a name="default-key-selection"></a>預設索引鍵選取範圍

當資料保護系統會讀取備份存放庫中的金鑰環時，它會嘗試找出金鑰環的 「 預設 」 金鑰。 預設索引鍵用於新的保護作業。

一般的啟發學習法是資料保護系統會選擇具有最新的啟動日期做為預設索引鍵的索引鍵。 （沒有以允許伺服器對伺服器的時鐘誤差小餘裕。）如果金鑰已過期或撤銷，且如果應用程式具有未停用自動金鑰產生，則會立即啟用每個產生新的金鑰[金鑰到期日和輪流](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration)下方的原則。

資料保護系統的原因會立即產生新的金鑰，而不是正在回復到不同的索引鍵是新的金鑰產生應該視為隱含在新的金鑰之前所啟動的所有索引鍵的到期時間。 新的金鑰可能已設定使用不同的演算法或比舊的金鑰，待用加密機制，而系統應該會偏好使用目前的設定透過回到的基本概念。

沒有例外狀況。 如果應用程式開發人員[停用自動產生金鑰](xref:security/data-protection/configuration/overview#disableautomatickeygeneration)，則資料保護系統必須選擇做為預設索引鍵的項目。 在此後援的案例中，系統會選擇非撤銷鍵的最新的啟動日期，加上提供給有時間才能傳播至叢集中的其他電腦的索引鍵的喜好設定。 後援系統可能會得到結果選擇過期的預設索引鍵。 後援系統將永遠不會選擇做為預設索引鍵已撤銷的金鑰，而且如果 keyring 是空的或已撤銷的每個金鑰然後系統會產生在初始化時錯誤。

<a name="data-protection-implementation-key-management-expiration"></a>

## <a name="key-expiration-and-rolling"></a>金鑰到期日和正在復原

建立金鑰時，它會自動提供 {now + 2 天} 啟用日和到期日的 {now + 90 天}。 2 天前的延遲啟動過程提供關鍵的時間才能傳播至整個系統。 也就是說，它可讓其他應用程式指向備份存放區索引鍵觀察在其下一步 自動重新整理的間隔，因而能充分利用，當金鑰信號會變成作用中，它已傳播到所有的應用程式可能需要使用它的機會。

如果預設金鑰將在 2 天內過期，且金鑰環還沒有可使用在到期時的預設索引鍵的索引鍵，資料保護系統會自動保存金鑰環新的金鑰。 這個新的金鑰具有 {預設的金鑰到期日} 啟用日和到期日的 {now + 90 天}。 這可讓系統自動不中斷服務的定期輪替金鑰。

有可能在某些情況下立即啟用建立金鑰的位置。 其中一個範例就是當應用程式尚未執行的時間，而金鑰環中的所有金鑰已都過期。 當發生這種情況時，索引鍵有 {現在} 啟用日期，而不會正常 2 天的啟用延遲。

預設金鑰存留期會是 90 天，但這是可設定，如下列範例所示。

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
```

雖然明確呼叫，以系統管理員也可以變更預設全系統的`SetDefaultKeyLifetime`會覆寫任何全系統原則。 預設金鑰存留期不能少於 7 天。

## <a name="automatic-key-ring-refresh"></a>自動金鑰環重新整理

資料保護系統初始化時，它會讀取基礎儲存機制中的金鑰環，並快取在記憶體中。 此快取可讓您保護且取消保護作業繼續進行而不叫用的備份存放區。 大約每隔 24 小時或目前的預設金鑰到期，視何者先時，系統會自動檢查變更的備份存放區。

>[!WARNING]
> （如果有的話） 開發人員應該很少需要直接使用管理 Api。 資料保護系統會執行自動金鑰管理，如上面所述。

資料保護系統公開介面`IKeyManager`，可用來檢查及變更金鑰環。 DI 系統提供的執行個體`IDataProtectionProvider`也可以提供的執行個體`IKeyManager`供您取用。 或者，您可以提取`IKeyManager`直接從`IServiceProvider`如下列範例所示。

可修改 keyring （明確地建立新的金鑰，或執行撤銷） 的任何作業會導致無效的記憶體中快取。 下次呼叫`Protect`或`Unprotect`會導致重新讀取金鑰環，並重新建立快取的資料保護系統。

下列範例示範如何使用`IKeyManager`介面，以檢查及操作 keyring，包括撤銷現有的機碼和手動產生新的金鑰。

[!code-csharp[](key-management/samples/key-management.cs)]

## <a name="key-storage"></a>金鑰儲存

資料保護系統有啟發學習法，因此它會嘗試自動推斷適當金鑰的儲存位置和待用加密機制。 也可由應用程式開發人員設定金鑰持續性機制。 下列文件將討論這些機制的內建實作：

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>
