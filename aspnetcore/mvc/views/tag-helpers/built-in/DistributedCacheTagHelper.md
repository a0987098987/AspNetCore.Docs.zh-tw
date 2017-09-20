---
title: "分散式快取標記協助程式 |Microsoft 文件"
author: pkellner
description: "示範如何使用快取標記協助程式"
keywords: "ASP.NET Core,標記協助程式"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a022
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper
ms.openlocfilehash: 2b260624fb2d85ab1a2625511397bcb4a85b6e77
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2017
---
# <a name="distributed-cache-tag-helper"></a>分散式快取標記協助程式

由 [Peter Kellner](http://peterkellner.net) 提供 


分散式快取標記協助程式提供了可大幅改善其分散式快取來源的內容快取的 ASP.NET Core 應用程式的效能。

分散式快取標記協助程式會從快取標記協助程式位於同一個基底類別繼承。  快取標記協助程式相關聯的所有屬性也都適用於分散式標記協助程式。


分散式快取標記協助程式遵循**明確的相依性原則**稱為**建構函式插入**。  具體來說，`IDistributedCache`介面容器傳入分散式快取標記協助專家的建構函式。  如果沒有特定的具象實作`IDistributedCache`中已建立`ConfigureServices`，通常位於 startup.cs，然後分散式快取標記協助程式會將快取的資料儲存為基本的快取標記協助程式使用相同的記憶體提供者。

## <a name="distributed-cache-tag-helper-attributes"></a>分散式快取標記協助程式屬性

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>啟用到期日到期之後到期-滑動改變-依標頭改變由-查詢改變由-路由改變-由-cookie 改變由位使用者不同依據優先順序

如需定義，請參閱快取標記協助程式。 分散式快取標記協助程式會繼承相同快取標記協助程式類別，讓所有這些屬性都是從快取標記協助程式一般。

- - -

### <a name="name-required"></a>名稱 （必填）

| 屬性類型    | 範例值     |
|----------------   |----------------   |
| 字串    | 「 my-distributed-cache-unique-key-101"     |

所需`name`屬性做為分散式快取標記協助程式的每個執行個體儲存該快取的索引鍵。  不同於基本根據 Razor 頁面名稱和位置標記協助程式 razor 頁面中的每個快取標記協助程式執行個體指定索引鍵的快取標記 Helper，分散式快取標記協助程式只根據它的索引鍵屬性`name`

使用範例：

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>分散式快取標記協助程式 IDistributedCache 實作

有兩種實作方式`IDistributedCache`內建於 ASP.NET Core。  其中根據**Sql Server** ，並且根據其他**Redis**。 可以在以下具名 「 使用分散式快取 」 參照的資源中找到這些實作的詳細資料。 這兩種實作牽涉到設定的執行個體`IDistributedCache`中 ASP.NET Core **startup.cs**。

沒有特別與任何特定實作方式的相關聯的標記屬性`IDistributedCache`。



- - -



## <a name="additional-resources"></a>其他資源

* <xref:mvc/views/tag-helpers/builtin-th/CacheTagHelper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
