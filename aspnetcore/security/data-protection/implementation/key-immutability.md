---
title: 金鑰的不變性和 ASP.NET Core 中的機碼設定
author: rick-anderson
description: 了解 ASP.NET Core 資料保護金鑰的不變性 Api 的實作詳細資料。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 7796cb102c0f6f03809704016fd36b28c7a82438
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892285"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a>金鑰的不變性和 ASP.NET Core 中的機碼設定

一旦將物件保存至備份存放區，其表示法是永久固定的。 新的資料可以加入至備份存放區，但現有的資料永遠不會變動。 此行為的主要目的是為了避免資料損毀。

此行為的一個結果是，一旦金鑰會寫入備份存放區，它是不變。 其建立、 啟動及到期的日期可以永遠不會變更，但它可以使用撤銷`IKeyManager`。 此外，其基礎的演算法資訊、 主要金鑰材料和加密在 rest 屬性也是不可變。

如果開發人員變更任何設定，會影響金鑰持續性，這些變更將不會生效之前會產生金鑰，透過明確呼叫在下一次`IKeyManager.CreateNewKey`或透過資料保護系統本身[自動金鑰產生](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)行為。 會影響金鑰持續性的設定如下所示：

* [預設金鑰存留期](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [在 rest 機制金鑰的加密](xref:security/data-protection/implementation/key-encryption-at-rest)

* [機碼內所包含的演算法資訊](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

如果您需要早於下一步 的自動金鑰輪換的時間開始進行這些設定，請考慮進行明確呼叫`IKeyManager.CreateNewKey`強制建立新的金鑰。 請記住，若要提供明確的啟動日期 （{現在 + 2 天} 是很好的經驗法則，讓變更傳播的時間） 和到期日，在呼叫中的。

>[!TIP]
> 觸碰存放庫的所有應用程式應指定相同的設定與`IDataProtectionBuilder`擴充方法。 否則，保存金鑰的屬性會取決於特定的應用程式叫用的金鑰產生常式。
