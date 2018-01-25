---
title: "金鑰的不變性和變更設定"
author: rick-anderson
description: "本文概述 ASP.NET Core 資料保護金鑰的不變性應用程式開發介面的實作詳細資料。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 425b8ba9769c2b5ac635693b045e52c110f25205
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="key-immutability-and-changing-settings"></a>索引鍵不變性和變更設定

一旦物件會保存到備份存放區，其表示是永久固定的。 備份存放區中，可以加入新的資料，但是可以永遠不會變更現有的資料。 此行為的主要用途是防止資料損毀。

此行為的一種結果是一旦金鑰會寫入備份存放區，即不可變。 其建立、 啟用和到期日期可以永遠不會變更，但它可以撤銷使用`IKeyManager`。 此外，其基礎的演算法資訊、 主要金鑰處理內容和加密其他屬性也是不變。

如果開發人員變更任何設定，會影響金鑰的持續性，這些變更將不會生效之前產生金鑰時，透過明確呼叫在下一次`IKeyManager.CreateNewKey`或透過資料保護系統本身[自動金鑰產生](key-management.md#data-protection-implementation-key-management)行為。 會影響金鑰的持續性的設定如下所示：

* [預設金鑰存留期](key-management.md#data-protection-implementation-key-management)

* [在 rest 機制金鑰加密](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [包含金鑰的演算法資訊](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

如果您需要早於下一步 自動金鑰循環時間開始進行這些設定，請考慮進行明確呼叫`IKeyManager.CreateNewKey`強制建立新的金鑰。 請記得要提供明確的啟動日期 （{現在 + 2 天} 是最佳經驗法則，讓變更傳播的時間） 與呼叫中的到期日。

>[!TIP]
> 所有碰觸的儲存機制的應用程式應該指定相同的設定與`IDataProtectionBuilder`擴充方法。 否則，保存金鑰的屬性將會相依於特定叫用的索引鍵產生常式的應用程式。
