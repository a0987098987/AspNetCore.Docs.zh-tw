---
title: ASP.NET Core 中的金鑰管理
author: rick-anderson
description: 瞭解 ASP.NET Core 資料保護金鑰管理 Api 的執行細節。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: c571222d734fa69183563aefa5cc6ce5a10e7612
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664707"
---
# <a name="key-management-in-aspnet-core"></a>ASP.NET Core 中的金鑰管理

<a name="data-protection-implementation-key-management"></a>

資料保護系統會自動管理主要金鑰存留期，用來保護和解除保護承載。 每個金鑰都可以存在四個階段的其中一個：

* 已建立-金鑰已存在於金鑰環中，但尚未啟用。 金鑰不應用於新的保護作業，直到有足夠的時間可將金鑰傳播到取用此金鑰信號的所有電腦為止。

* Active-金鑰會出現在金鑰環中，而且應該用於所有新的保護作業。

* 已過期-金鑰已執行其自然存留期，不應再用於新的保護作業。

* 已撤銷-金鑰遭到入侵，不得用於新的保護作業。

[已建立]、[作用中] 和 [已過期] 金鑰可用來解除保護傳入的裝載。 根據預設，已撤銷的金鑰可能不會用來取消保護裝載，但應用程式開發人員可以視需要覆[寫此行為](xref:security/data-protection/consumer-apis/dangerous-unprotect#data-protection-consumer-apis-dangerous-unprotect)。

>[!WARNING]
> 開發人員可能會想要從金鑰環刪除金鑰（例如，從檔案系統中刪除對應的檔案）。 此時，金鑰所保護的所有資料都會永久 undecipherable，而且不會有像撤銷金鑰一樣的緊急覆寫。 刪除金鑰是真正的破壞性行為，因此資料保護系統不會公開可執行這項作業的第一類 API。

## <a name="default-key-selection"></a>預設金鑰選取專案

當資料保護系統從支援存放庫讀取金鑰信號時，它會嘗試從金鑰環形找出「預設」金鑰。 預設金鑰會用於新的保護作業。

一般啟發學習法是資料保護系統會選擇具有最新啟用日期的金鑰做為預設金鑰。 （有一個小型的巧克力因素可允許伺服器對伺服器的時鐘誤差）。如果金鑰已過期或已撤銷，而且應用程式未停用自動產生金鑰，則會產生新的金鑰，並根據以下的[金鑰到期和](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration)輪流原則立即啟用。

資料保護系統會立即產生新的金鑰而不是回到另一個金鑰的原因是，應該將新的金鑰產生視為在新金鑰之前啟動之所有金鑰的隱含到期。 一般的概念是，新的金鑰可能已設定為使用不同的演算法或待用加密機制，而不是舊的金鑰，而且系統應該偏好目前的設定，而不是回復。

發生例外狀況。 如果應用程式開發人員已[停用自動產生金鑰](xref:security/data-protection/configuration/overview#disableautomatickeygeneration)，則資料保護系統必須選擇做為預設金鑰。 在此回溯案例中，系統會選擇具有最新啟動日期的非撤銷金鑰，並將其喜好設定指定給已有時間傳播到叢集中其他電腦的金鑰。 回退系統最後可能會選擇已過期的預設金鑰做為結果。 Fallback 系統永遠不會選擇撤銷的金鑰做為預設金鑰，而且如果金鑰環是空的或每個金鑰都已被撤銷，則系統會在初始化時產生錯誤。

<a name="data-protection-implementation-key-management-expiration"></a>

## <a name="key-expiration-and-rolling"></a>金鑰到期和輪流

建立金鑰時，會自動將啟用日期指定為 {now + 2 天}，到期日為 {now + 90 天}。 啟用前的2天延遲會讓金鑰時間能夠透過系統傳播。 也就是說，它可讓指向備份存放區的其他應用程式在下一次自動重新整理期間觀察金鑰，藉此充分發揮金鑰信號變成作用中的機會，並將其傳播到可能需要使用它的所有應用程式。

如果預設金鑰會在2天內過期，而且如果金鑰環形尚未有在預設金鑰到期時將會作用的金鑰，則資料保護系統會自動將新的金鑰保存到金鑰環。 此新金鑰的啟用日期為 {預設金鑰的到期日}，到期日為 {now + 90 天}。 這可讓系統定期自動定期輪替金鑰，而不會中斷服務。

在某些情況下，將會建立具有立即啟用的金鑰。 其中一個範例是應用程式未執行一段時間，且金鑰通道中的所有金鑰已過期。 發生這種情況時，系統會將啟用日期指定為 {now}，但不含正常的2天啟用延遲。

預設的金鑰存留期為90天，但可設定為，如下列範例所示。

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
```

系統管理員也可以變更全系統的預設值，不過明確呼叫 `SetDefaultKeyLifetime` 將會覆寫任何全系統原則。 預設金鑰存留期不得少於7天。

## <a name="automatic-key-ring-refresh"></a>自動金鑰迴圈重新整理

當資料保護系統初始化時，它會從基礎存放庫讀取金鑰信號，並將它快取到記憶體中。 此快取可讓您進行保護和取消保護作業，而不需要叫用備份存放區。 系統每隔24小時會自動檢查備份存放區是否有變更，或目前的預設金鑰過期時（以先發生者為准）。

>[!WARNING]
> 開發人員應該很少（如果有的話）需要直接使用金鑰管理 Api。 資料保護系統會如上述所述執行自動金鑰管理。

資料保護系統會公開介面 `IKeyManager`，可用來檢查和變更金鑰環。 提供 `IDataProtectionProvider` 實例的 DI 系統也可以為您的耗用量提供 `IKeyManager` 的實例。 或者，您可以直接從 `IServiceProvider` 提取 `IKeyManager`，如下列範例所示。

修改金鑰通道（明確建立新的金鑰或執行撤銷）的任何作業，都會使記憶體中的快取失效。 下一次呼叫 `Protect` 或 `Unprotect` 會導致資料保護系統重新讀取金鑰信號，然後重新建立快取。

下列範例示範如何使用 `IKeyManager` 介面來檢查和操作金鑰環，包括撤銷現有的金鑰，以及手動產生新的金鑰。

[!code-csharp[](key-management/samples/key-management.cs)]

[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

## <a name="key-storage"></a>金鑰儲存

資料保護系統有一個啟發學習法，它會嘗試自動推算適當的金鑰儲存位置和待用加密機制。 金鑰持續性機制也可由應用程式開發人員設定。 下列檔將討論這些機制的內建的內部部署：

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>
