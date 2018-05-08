---
title: ASP.NET Core 中的分散式快取標籤協助程式
author: pkellner
description: 示範如何使用快取標籤協助程式
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 929156633048b8ee68a66290f44b12026a08c8c9
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="d05c8-103">ASP.NET Core 中的分散式快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="d05c8-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="d05c8-104">由 [Peter Kellner](http://peterkellner.net) 提供</span><span class="sxs-lookup"><span data-stu-id="d05c8-104">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="d05c8-105">分散式快取標籤協助程式可將 ASP.NET Core 應用程式內容快取至分散式快取來源，因此大幅提升應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="d05c8-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="d05c8-106">分散式快取標籤協助程式繼承自與快取標籤協助程式相同的基底類別。</span><span class="sxs-lookup"><span data-stu-id="d05c8-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span>  <span data-ttu-id="d05c8-107">所有與快取標籤協助程式建立關聯的屬性也會適用於分散式標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="d05c8-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>


<span data-ttu-id="d05c8-108">分散式快取標籤協助程式遵循**明確的相依性原則**，稱為**建構函式插入**。</span><span class="sxs-lookup"><span data-stu-id="d05c8-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span>  <span data-ttu-id="d05c8-109">具體來說，`IDistributedCache` 介面容器會傳入分散式快取標籤協助程式的建構函式。</span><span class="sxs-lookup"><span data-stu-id="d05c8-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span>  <span data-ttu-id="d05c8-110">如果 `ConfigureServices` 中未建立 `IDistributedCache` 的特定具體實作 (通常位於 startup.cs)，則分散式快取標籤協助程式將會使用與基本快取標籤協助程式相同的記憶體內部提供者來儲存快取資料。</span><span class="sxs-lookup"><span data-stu-id="d05c8-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="d05c8-111">分散式快取標籤協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="d05c8-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="d05c8-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span><span class="sxs-lookup"><span data-stu-id="d05c8-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="d05c8-113">如需定義，請參閱＜快取標籤協助程式＞。</span><span class="sxs-lookup"><span data-stu-id="d05c8-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="d05c8-114">分散式快取標籤協助程式繼承自與快取標籤協助程式相同的類別，因此上述所有屬性都是快取標籤協助程式中的通用屬性。</span><span class="sxs-lookup"><span data-stu-id="d05c8-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="d05c8-115">名稱 (必要)</span><span class="sxs-lookup"><span data-stu-id="d05c8-115">name (required)</span></span>

| <span data-ttu-id="d05c8-116">屬性類型</span><span class="sxs-lookup"><span data-stu-id="d05c8-116">Attribute Type</span></span>    | <span data-ttu-id="d05c8-117">範例值</span><span class="sxs-lookup"><span data-stu-id="d05c8-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="d05c8-118">字串</span><span class="sxs-lookup"><span data-stu-id="d05c8-118">string</span></span>    | <span data-ttu-id="d05c8-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="d05c8-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="d05c8-120">此必要的 `name` 屬性可作為針對每個分散式快取標籤協助程式執行個體所儲存的快取索引鍵。</span><span class="sxs-lookup"><span data-stu-id="d05c8-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span>  <span data-ttu-id="d05c8-121">不同於基本快取標籤協助程式是根據 Razor 頁面名稱和 Razor 頁面中的標籤協助程式位置，來對每個快取標籤協助程式執行個體指派一個索引鍵，分散式快取標記協助程式只會以 `name` 屬性為其索引鍵根據</span><span class="sxs-lookup"><span data-stu-id="d05c8-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the tag helper in the razor page, the Distributed Cache Tag Helper only bases it's key on the attribute `name`</span></span>

<span data-ttu-id="d05c8-122">使用範例：</span><span class="sxs-lookup"><span data-stu-id="d05c8-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="d05c8-123">分散式快取標籤協助程式 IDistributedCache 實作</span><span class="sxs-lookup"><span data-stu-id="d05c8-123">Distributed Cache Tag Helper IDistributedCache Implementations</span></span>

<span data-ttu-id="d05c8-124">ASP.NET Core 內建兩項 `IDistributedCache` 實作。</span><span class="sxs-lookup"><span data-stu-id="d05c8-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span>  <span data-ttu-id="d05c8-125">其中一項是以 **SQL Server** 為基礎，另一項是以 **Redis** 為基礎。</span><span class="sxs-lookup"><span data-stu-id="d05c8-125">One is based on **Sql Server** and the other is based on **Redis**.</span></span> <span data-ttu-id="d05c8-126">如需這些實作的詳細資料，請參閱下面的＜使用分散式快取＞參考資源。</span><span class="sxs-lookup"><span data-stu-id="d05c8-126">Details of these implementations can be found at the resource referenced below named "Working with a distributed cache".</span></span> <span data-ttu-id="d05c8-127">這兩項實作都需要設定 ASP.NET Core 之 **startup.cs** 中的 `IDistributedCache` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="d05c8-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's **startup.cs**.</span></span>

<span data-ttu-id="d05c8-128">沒有特別與使用 `IDistributedCache` 的任何特定實作建立關聯的標籤屬性。</span><span class="sxs-lookup"><span data-stu-id="d05c8-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>



- - -



## <a name="additional-resources"></a><span data-ttu-id="d05c8-129">其他資源</span><span class="sxs-lookup"><span data-stu-id="d05c8-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
