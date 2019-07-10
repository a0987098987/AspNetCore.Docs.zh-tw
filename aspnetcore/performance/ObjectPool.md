---
title: 使用 ASP.NET Core 中的 ObjectPool 物件重複使用
author: rick-anderson
description: 增加使用 ObjectPool 的 ASP.NET Core 應用程式中的效能的秘訣。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 4/11/2019
uid: performance/ObjectPool
ms.openlocfilehash: 92106d5add7dbda36e451614429baa0db420f0e8
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724832"
---
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a><span data-ttu-id="87730-103">使用 ASP.NET Core 中的 ObjectPool 物件重複使用</span><span class="sxs-lookup"><span data-stu-id="87730-103">Object reuse with ObjectPool in ASP.NET Core</span></span>

<span data-ttu-id="87730-104">藉由[Steve Gordon](https://twitter.com/stevejgordon)， [Ryan Nowak](https://github.com/rynowak)，和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="87730-104">By [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="87730-105"><xref:Microsoft.Extensions.ObjectPool> 是支援一整組物件保留在記憶體中的重複使用，而不是讓要進行記憶體回收物件收集的 ASP.NET Core 基礎結構的一部分。</span><span class="sxs-lookup"><span data-stu-id="87730-105"><xref:Microsoft.Extensions.ObjectPool> is part of the ASP.NET Core infrastructure that supports keeping a group of objects in memory for reuse rather than allowing the objects to be garbage collected.</span></span>

<span data-ttu-id="87730-106">您可能想要使用的物件集區，如果受管理物件：</span><span class="sxs-lookup"><span data-stu-id="87730-106">You might want to use the object pool if the objects that are being managed are:</span></span>

- <span data-ttu-id="87730-107">配置/初始化成本。</span><span class="sxs-lookup"><span data-stu-id="87730-107">Expensive to allocate/initialize.</span></span>
- <span data-ttu-id="87730-108">代表某些有限的資源。</span><span class="sxs-lookup"><span data-stu-id="87730-108">Represent some limited resource.</span></span>
- <span data-ttu-id="87730-109">使用可預測的方式和常見問題。</span><span class="sxs-lookup"><span data-stu-id="87730-109">Used predictably and frequently.</span></span>

<span data-ttu-id="87730-110">例如，ASP.NET Core 架構會使用物件集區中的某些位置重複使用<xref:System.Text.StringBuilder>執行個體。</span><span class="sxs-lookup"><span data-stu-id="87730-110">For example, the ASP.NET Core framework uses the object pool in some places to reuse <xref:System.Text.StringBuilder> instances.</span></span> <span data-ttu-id="87730-111">`StringBuilder` 配置並管理它自己的緩衝區來容納的字元資料。</span><span class="sxs-lookup"><span data-stu-id="87730-111">`StringBuilder` allocates and manages its own buffers to hold character data.</span></span> <span data-ttu-id="87730-112">ASP.NET Core 定期使用`StringBuilder`實作功能，並重複使用它們提供效能優勢。</span><span class="sxs-lookup"><span data-stu-id="87730-112">ASP.NET Core regularly uses `StringBuilder` to implement features, and reusing them provides a performance benefit.</span></span>

<span data-ttu-id="87730-113">物件集區不一定會改善效能：</span><span class="sxs-lookup"><span data-stu-id="87730-113">Object pooling doesn't always improve performance:</span></span>

- <span data-ttu-id="87730-114">物件的初始化成本很高，除非它是從集區取得的物件通常速度較慢。</span><span class="sxs-lookup"><span data-stu-id="87730-114">Unless the initialization cost of an object is high, it's usually slower to get the object from the pool.</span></span>
- <span data-ttu-id="87730-115">解除配置的集區之前，受管理的集區物件並不解除配置。</span><span class="sxs-lookup"><span data-stu-id="87730-115">Objects managed by the pool aren't de-allocated until the pool is de-allocated.</span></span>

<span data-ttu-id="87730-116">使用物件集區才收集應用程式或程式庫使用真實案例的效能資料。</span><span class="sxs-lookup"><span data-stu-id="87730-116">Use object pooling only after collecting performance data using realistic scenarios for your app or library.</span></span>

<span data-ttu-id="87730-117">**警告：`ObjectPool`不會實作`IDisposable`。我們不建議使用需要處置的類型。**</span><span class="sxs-lookup"><span data-stu-id="87730-117">**WARNING: The `ObjectPool` doesn't implement `IDisposable`. We don't recommend using it with types that need disposal.**</span></span>

<span data-ttu-id="87730-118">**注意：ObjectPool 不會限制它將會配置的物件數目，它會使用它將會保留的物件數目的限制。**</span><span class="sxs-lookup"><span data-stu-id="87730-118">**NOTE: The ObjectPool doesn't place a limit on the number of objects that it will allocate, it places a limit on the number of objects it will retain.**</span></span>

## <a name="concepts"></a><span data-ttu-id="87730-119">概念</span><span class="sxs-lookup"><span data-stu-id="87730-119">Concepts</span></span>

<span data-ttu-id="87730-120"><xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> -基本物件集區抽象概念。</span><span class="sxs-lookup"><span data-stu-id="87730-120"><xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> - the basic object pool abstraction.</span></span> <span data-ttu-id="87730-121">用來取得並傳回物件。</span><span class="sxs-lookup"><span data-stu-id="87730-121">Used to get and return objects.</span></span>

<span data-ttu-id="87730-122"><xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> -實作此項以自訂物件的建立方式和其用法*重設*時傳回到集區。</span><span class="sxs-lookup"><span data-stu-id="87730-122"><xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> - implement this to customize how an object is created and how it is *reset* when returned to the pool.</span></span> <span data-ttu-id="87730-123">這可以傳入您直接建構的物件集區...OR</span><span class="sxs-lookup"><span data-stu-id="87730-123">This can be passed into an object pool that you construct directly.... OR</span></span>

<span data-ttu-id="87730-124"><xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> 當做的 factory 來建立物件集區。</span><span class="sxs-lookup"><span data-stu-id="87730-124"><xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> acts as a factory for creating object pools.</span></span>
<!-- REview, there is no ObjectPoolProvider<T> -->

<span data-ttu-id="87730-125">在應用程式中以多種方式可用 ObjectPool:</span><span class="sxs-lookup"><span data-stu-id="87730-125">The ObjectPool can be used in an app in multiple ways:</span></span>

* <span data-ttu-id="87730-126">具現化的集區。</span><span class="sxs-lookup"><span data-stu-id="87730-126">Instantiating a pool.</span></span>
* <span data-ttu-id="87730-127">註冊中的集區[相依性插入](xref:fundamentals/dependency-injection)(DI) 做為執行個體。</span><span class="sxs-lookup"><span data-stu-id="87730-127">Registering a pool in [Dependency injection](xref:fundamentals/dependency-injection) (DI) as an instance.</span></span>
* <span data-ttu-id="87730-128">註冊`ObjectPoolProvider<>`DI 並使用它做為 factory。</span><span class="sxs-lookup"><span data-stu-id="87730-128">Registering the `ObjectPoolProvider<>` in DI and using it as a factory.</span></span>

## <a name="how-to-use-objectpool"></a><span data-ttu-id="87730-129">如何使用 ObjectPool</span><span class="sxs-lookup"><span data-stu-id="87730-129">How to use ObjectPool</span></span>

<span data-ttu-id="87730-130">呼叫<xref:Microsoft.Extensions.ObjectPool.ObjectPool`1>取得的物件和<xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*>傳回的物件。</span><span class="sxs-lookup"><span data-stu-id="87730-130">Call <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> to get an object and <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> to return the object.</span></span>  <span data-ttu-id="87730-131">沒有，您就會傳回每個物件的需求。</span><span class="sxs-lookup"><span data-stu-id="87730-131">There's no requirement that you return every object.</span></span> <span data-ttu-id="87730-132">如果您不會傳回物件，它會進行記憶體回收。</span><span class="sxs-lookup"><span data-stu-id="87730-132">If you don't return an object, it will be garbage collected.</span></span>

## <a name="objectpool-sample"></a><span data-ttu-id="87730-133">ObjectPool 範例</span><span class="sxs-lookup"><span data-stu-id="87730-133">ObjectPool sample</span></span>

<span data-ttu-id="87730-134">下列程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="87730-134">The following code:</span></span>

* <span data-ttu-id="87730-135">新增`ObjectPoolProvider`要[相依性插入](xref:fundamentals/dependency-injection)(DI) 容器。</span><span class="sxs-lookup"><span data-stu-id="87730-135">Adds `ObjectPoolProvider` to the [Dependency injection](xref:fundamentals/dependency-injection) (DI) container.</span></span>
* <span data-ttu-id="87730-136">新增並設定`ObjectPool<StringBuilder>`至 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="87730-136">Adds and configures `ObjectPool<StringBuilder>` to the DI container.</span></span>
* <span data-ttu-id="87730-137">新增`BirthdayMiddleware`。</span><span class="sxs-lookup"><span data-stu-id="87730-137">Adds the `BirthdayMiddleware`.</span></span>

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

<span data-ttu-id="87730-138">下列程式碼實作 `BirthdayMiddleware`</span><span class="sxs-lookup"><span data-stu-id="87730-138">The following code implements `BirthdayMiddleware`</span></span>

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
