---
title: ASP.NET Core 中的分散式快取標籤協助程式
author: pkellner
description: 示範如何使用快取標籤協助程式
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: d33c22802030eb9bc77baa64b83c9bbd7e902195
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275171"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>ASP.NET Core 中的分散式快取標籤協助程式

由 [Peter Kellner](http://peterkellner.net) 提供 

分散式快取標籤協助程式可將 ASP.NET Core 應用程式內容快取至分散式快取來源，因此大幅提升應用程式的效能。

分散式快取標籤協助程式繼承自與快取標籤協助程式相同的基底類別。 所有與快取標籤協助程式建立關聯的屬性也會適用於分散式標籤協助程式。

分散式快取標籤協助程式遵循**明確的相依性原則**，稱為**建構函式插入**。 具體來說，`IDistributedCache` 介面容器會傳入分散式快取標籤協助程式的建構函式。 如果 `ConfigureServices` 中未建立 `IDistributedCache` 的特定具體實作 (通常位於 startup.cs)，則分散式快取標籤協助程式將會使用與基本快取標籤協助程式相同的記憶體內部提供者來儲存快取資料。

## <a name="distributed-cache-tag-helper-attributes"></a>分散式快取標籤協助程式屬性

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority

如需定義，請參閱＜快取標籤協助程式＞。 分散式快取標籤協助程式繼承自與快取標籤協助程式相同的類別，因此上述所有屬性都是快取標籤協助程式中的通用屬性。

- - -

### <a name="name-required"></a>名稱 (必要)

| 屬性類型    | 範例值     |
|----------------   |----------------   |
| 字串    | "my-distributed-cache-unique-key-101"     |

此必要的 `name` 屬性可作為針對每個分散式快取標籤協助程式執行個體所儲存的快取索引鍵。 不同於基本快取標記協助程式是根據 Razor 頁面名稱和 Razor 頁面中的標記協助程式位置，來對每個快取標記協助程式執行個體指派一個索引鍵，分散式快取標記協助程式只會以 `name` 屬性為其索引鍵根據

使用範例：

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>分散式快取標記協助程式 IDistributedCache 實作

ASP.NET Core 內建兩項 `IDistributedCache` 實作。 其中一項是以 SQL Server 為基礎，另一項是以 Redis 為基礎。 如需這些實作的詳細資料，請參閱<xref:performance/caching/distributed>。 這兩項實作都需要在 ASP.NET Core 的 *Startup.cs* 中設定 `IDistributedCache` 執行個體。

沒有特別與使用 `IDistributedCache` 的任何特定實作建立關聯的標籤屬性。

## <a name="additional-resources"></a>其他資源

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
