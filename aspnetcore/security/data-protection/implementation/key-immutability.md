---
title: "金鑰的不變性和變更設定"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fc911ae3-feca-4ffe-8b43-362bc632fe04
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 3afce8f84ebe3b709ea169c7db27f99829f157ab
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="key-immutability-and-changing-settings"></a>索引鍵不變性和變更設定

一旦物件會保存到備份存放區，其表示是永久固定的。 備份存放區中，可以加入新的資料，但是可以永遠不會變更現有的資料。 此行為的主要用途是防止資料損毀。

此行為的一種結果是一旦金鑰會寫入備份存放區，即不可變。 絕不可以變更其建立、 啟用和到期的日期，但它可以使用 IKeyManager 撤銷。 此外，其基礎的演算法資訊、 主要金鑰處理內容和加密其他屬性也是不變。

如果開發人員變更任何設定，會影響金鑰的持續性，這些變更將不會進入效果下次產生金鑰，不論是透過明確呼叫 IKeyManager.CreateNewKey，或是透過資料保護系統本身[自動索引鍵產生](key-management.md#data-protection-implementation-key-management)行為。 會影響金鑰的持續性的設定如下所示：

* [預設金鑰存留期](key-management.md#data-protection-implementation-key-management)

* [在 rest 機制金鑰加密](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [包含金鑰的演算法資訊](../configuration/overview.md#data-protection-changing-algorithms)

如果您需要早於下一步 自動金鑰循環時間開始進行這些設定，請考慮製作 IKeyManager.CreateNewKey 強制新的金鑰建立的明確呼叫。 請記得要提供明確的啟動日期 （{現在 + 2 天} 是最佳經驗法則，讓變更傳播的時間） 與呼叫中的到期日。

>[!TIP]
> 所有碰觸的儲存機制的應用程式應指定含有 IDataProtectionBuilder 擴充方法的相同設定，否則保存金鑰的屬性將取決於特定叫用的索引鍵產生常式的應用程式.
