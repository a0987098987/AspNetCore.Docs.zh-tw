---
title: ASP.NET Core 中的分散式快取標籤協助程式
author: pkellner
description: 了解如何使用分散式快取標籤協助程式。
ms.author: riande
ms.custom: mvc
ms.date: 01/24/2020
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: e5100d7244600358186b653073990985f48434a7
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2020
ms.locfileid: "76809051"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>ASP.NET Core 中的分散式快取標籤協助程式

作者：[Peter Kellner](https://peterkellner.net) 與 [Luke Latham](https://github.com/guardrex)

分散式快取標籤協助程式可將 ASP.NET Core 應用程式內容快取至分散式快取來源，因此大幅提升應用程式的效能。

如需標籤協助程式的概觀，請參閱 <xref:mvc/views/tag-helpers/intro>。

分散式快取標籤協助程式繼承自與快取標籤協助程式相同的基底類別。 分散式標籤協助程式可以使用所有[快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)屬性。

分散式快取標籤協助程式會使用[建構函式插入](xref:fundamentals/dependency-injection#constructor-injection-behavior)。 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 介面會被傳入分散式快取標籤協助程式的建構函式。 如果 `Startup.ConfigureServices` 中未建立 `IDistributedCache` 的具體實作 (*Startup.cs*)，[分散式快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)將會使用與快取標籤協助程式相同的記憶體內部提供者來儲存快取資料。

## <a name="distributed-cache-tag-helper-attributes"></a>分散式快取標籤協助程式屬性

### <a name="attributes-shared-with-the-cache-tag-helper"></a>與快取標籤協助程式共用的屬性

* `enabled`
* `expires-on`
* `expires-after`
* `expires-sliding`
* `vary-by-header`
* `vary-by-query`
* `vary-by-route`
* `vary-by-cookie`
* `vary-by-user`
* `vary-by priority`

分散式快取標籤協助程式繼承自與快取標籤協助程式相同的類別。 如需這些屬性的描述，請參閱[快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)。

### <a name="name"></a>{2&gt;名稱&lt;2}

| 屬性類型 | 範例                               |
| -------------- | ------------------------------------- |
| 字串         | `my-distributed-cache-unique-key-101` |

`name` 是必要的。 `name` 屬性會作為每個預存快取執行個體的索引鍵。 不同於快取標記協助程式是根據 Razor 頁面名稱和 Razor 頁面中的位置，來對每個執行個體指派一個快取索引鍵，分散式快取標記協助程式只會以 `name` 屬性為其索引鍵根據。

範例：

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>分散式快取標記協助程式 IDistributedCache 實作

ASP.NET Core 內建兩項 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 實作。 其中一個是以 SQL Server 為基礎，另一項是以 Redis 為基礎。 協力廠商的執行也可供使用，例如[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)。 如需這些實作的詳細資料，請參閱<xref:performance/caching/distributed>。 這兩個實作都涉及在 `Startup` 的 `IDistributedCache` 中設定執行個體。

沒有特別與使用 `IDistributedCache` 的任何特定實作建立關聯的標籤屬性。

## <a name="additional-resources"></a>其他資源

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
