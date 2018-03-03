---
title: "金鑰管理"
author: rick-anderson
description: "本文件概述的 ASP.NET Core 資料保護金鑰管理應用程式開發介面的實作詳細資料。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: a1fd7c55ec94d5def569bb407c064f4fd2fe9695
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="key-management"></a>金鑰管理

<a name="data-protection-implementation-key-management"></a>

資料保護系統會自動管理用來保護且取消保護內容的主要金鑰的存留期。 每個索引鍵可以存在於其中一個四個階段：

* 建立索引鍵存在於鑰匙圈，但尚未啟動。 機碼不應該用於新的保護作業有足夠的時間經過之前索引鍵有傳播到所有機器正在耗用此鑰匙圈的機會。

* 作用中的索引鍵中存在鑰匙圈，適用於所有新的保護作業。

* 過期-索引鍵已執行其自然的存留期，並不會再用於新的保護作業。

* 撤銷-金鑰受到危害，且必須不適用於新的保護作業。

建立、 使用中和已過期的索引鍵可能都可以用來取消保護內送裝載。 依預設已撤銷的索引鍵不能取消保護裝載，但應用程式開發人員可以[覆寫這個行為](../consumer-apis/dangerous-unprotect.md#data-protection-consumer-apis-dangerous-unprotect)如有必要。

>[!WARNING]
> 開發人員可能會想要從索引鍵環刪除機碼 （例如，藉由從檔案系統中刪除對應的檔案）。 此時，金鑰所保護的所有資料都都會永久觸，並都沒有緊急覆寫沒有使用已撤銷的索引鍵。 刪除索引鍵是真正破壞性的行為，因此資料保護系統會公開任何第一級的 API 執行此作業。

## <a name="default-key-selection"></a>預設選取項目索引鍵

當資料保護系統從備份儲存機制讀取鑰匙圈時，它會嘗試找出鑰匙圈的 「 預設 」 金鑰。 預設索引鍵用於新的保護作業。

一般的啟發學習法是資料保護系統選擇做為預設索引鍵的最新的啟動日期索引鍵。 （沒有小型是因素，以允許伺服器對伺服器的時鐘誤差。）如果金鑰已過期或撤銷，以及如果應用程式沒有停用自動金鑰產生，則會產生新的金鑰，以立即啟用每個[金鑰到期以及輪流](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration)下方的原則。

資料保護系統的原因會立即產生新的金鑰，而不是正在回復到不同的索引鍵是新的金鑰產生應視為新的金鑰之前已啟用的所有索引鍵的隱含到期日。 新的金鑰可能已設定使用不同的演算法或靜態加密機制，與舊的索引鍵，而系統應該而不用的目前組態回到一般概念。

沒有例外狀況。 如果應用程式開發人員有[停用自動產生金鑰](xref:security/data-protection/configuration/overview#disableautomatickeygeneration)，然後在資料保護系統必須選擇做為預設索引鍵的項目。 在此後援的案例中，系統會選擇非撤銷鍵的最新的啟動日期，加上指定的時間才會傳播到叢集中的其他機器索引鍵為喜好設定。 回溯系統可能會因此選擇過期的預設索引鍵。 回溯系統將會永遠不會選擇已撤銷的索引鍵，做為預設索引鍵，如果鑰匙圈是空的或每個索引鍵已被撤銷。 然後系統會產生錯誤時初始化。

<a name="data-protection-implementation-key-management-expiration"></a>

## <a name="key-expiration-and-rolling"></a>金鑰到期時間和循環

建立金鑰時，它會自動具 {now + 2 天} 啟用日和到期日的 {now + 90 天}。 2 天之前的延遲啟動提供的索引鍵的時間才能傳播到整個系統。 也就是說，它可讓其他應用程式指向備份存放區來觀察在其下一步 自動重新整理期間，索引鍵，因此最大化，金鑰環變成沒有作用中傳播到它時可能需要的所有應用程式使用它的機會。

如果預設金鑰將在 2 天內過期，而鑰匙圈還沒有將會啟用在到期時的預設索引鍵的索引鍵，資料保護系統將會自動保存鑰匙圈新的金鑰。 這個新的金鑰已啟用的日期 {預設金鑰到期日} 和 {now + 90 天} 的到期日。 這可讓系統自動復原不中斷服務的定期的索引鍵。

可能的情況下會立即啟動建立金鑰。 當應用程式尚未執行的時間，而鑰匙圈中的所有索引鍵已過期時，就是一個範例。 發生這種情況，索引鍵有 {現在} 沒有一般 2 天的啟用延遲啟動日期。

預設金鑰存留時間為 90 天，但這是可設定，如下列範例所示。

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
```

雖然明確呼叫，以系統管理員也可以變更預設全系統`SetDefaultKeyLifetime`將會覆寫任何全系統原則。 預設金鑰存留期不可短於 7 天。

## <a name="automatic-key-ring-refresh"></a>自動鑰匙圈重新整理

當資料保護系統初始化時，它會鑰匙圈讀取從基礎儲存機制，並放入快取在記憶體中。 此快取可讓您保護且取消保護的作業繼續進行而不叫用的備份存放區。 大約每隔 24 小時或目前的預設金鑰過期時，何者較早，系統將自動檢查有變更的備份存放區。

>[!WARNING]
> （如果有的話） 開發人員應該很少需要直接使用 Api 的金鑰管理。 資料保護系統將會執行自動金鑰管理，如上面所述。

資料保護系統公開介面`IKeyManager`，可用來檢查及變更索引鍵環形。 DI 系統提供的執行個體`IDataProtectionProvider`也可以提供的執行個體`IKeyManager`供您使用。 或者，您可以拉`IKeyManager`直接從`IServiceProvider`如下列範例所示。

任何可修改鑰匙圈 （明確地建立新的金鑰，或執行撤銷） 作業會使記憶體中快取失效。 下次呼叫`Protect`或`Unprotect`會導致重新讀取鑰匙圈，並重新建立快取的資料保護系統。

下列範例將示範如何使用`IKeyManager`介面來檢查及操作鑰匙圈，包括撤銷，則現有的索引鍵，並手動產生新的金鑰。

[!code-csharp[](key-management/samples/key-management.cs)]

## <a name="key-storage"></a>金鑰儲存

資料保護系統有啟發學習法，它會嘗試自動推算適當金鑰的儲存位置和 rest 機制加密。 這也是由應用程式開發人員可設定。 下列文件將討論這些機制的內建實作：

* [內建金鑰儲存提供者](key-storage-providers.md#data-protection-implementation-key-storage-providers)

* [在其他提供者的內建金鑰加密](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers)
