---
title: 索引鍵中 ASP.NET Core 的不變性和金鑰設定
author: rick-anderson
description: 了解 ASP.NET Core 資料保護金鑰不變性應用程式開發介面的實作詳細資料。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: e918b00562aca9821de87c38f10242177517d8a5
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a>索引鍵中 ASP.NET Core 的不變性和金鑰設定

一旦物件會保存到備份存放區，其表示是永久固定的。 備份存放區中，可以加入新的資料，但是可以永遠不會變更現有的資料。 此行為的主要用途是防止資料損毀。

此行為的一種結果是一旦金鑰會寫入備份存放區，即不可變。 其建立、 啟用和到期日期可以永遠不會變更，但它可以撤銷使用`IKeyManager`。 此外，其基礎的演算法資訊、 主要金鑰處理內容和加密其他屬性也是不變。

如果開發人員變更任何設定，會影響金鑰的持續性，這些變更將不會生效之前產生金鑰時，透過明確呼叫在下一次`IKeyManager.CreateNewKey`或透過資料保護系統本身[自動金鑰產生](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)行為。 會影響金鑰的持續性的設定如下所示：

* [預設金鑰存留期](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [在 rest 機制金鑰加密](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)

* [包含金鑰的演算法資訊](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

如果您需要早於下一步 自動金鑰循環時間開始進行這些設定，請考慮進行明確呼叫`IKeyManager.CreateNewKey`強制建立新的金鑰。 請記得要提供明確的啟動日期 （{現在 + 2 天} 是最佳經驗法則，讓變更傳播的時間） 與呼叫中的到期日。

>[!TIP]
> 所有碰觸的儲存機制的應用程式應該指定相同的設定與`IDataProtectionBuilder`擴充方法。 否則，保存金鑰的屬性將會相依於特定叫用的索引鍵產生常式的應用程式。
