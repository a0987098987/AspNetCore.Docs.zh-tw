---
title: 在 ASP.NET Core 中使用 ObjectPool 的物件重複使用
author: rick-anderson
description: 使用 ObjectPool 增加 ASP.NET Core 應用程式效能的秘訣。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 04/11/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: performance/ObjectPool
ms.openlocfilehash: 9df7f370eb550172493478bcd8d94a9541926fec
ms.sourcegitcommit: 895e952aec11c91d703fbdd3640a979307b8cc67
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/01/2020
ms.locfileid: "85793561"
---
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a><span data-ttu-id="52551-103">在 ASP.NET Core 中使用 ObjectPool 的物件重複使用</span><span class="sxs-lookup"><span data-stu-id="52551-103">Object reuse with ObjectPool in ASP.NET Core</span></span>

<span data-ttu-id="52551-104">作者： [Steve Gordon](https://twitter.com/stevejgordon)、 [Ryan Nowak](https://github.com/rynowak)和[Günther Foidl](https://github.com/gfoidl)</span><span class="sxs-lookup"><span data-stu-id="52551-104">By [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak), and [Günther Foidl](https://github.com/gfoidl)</span></span>

<span data-ttu-id="52551-105"><xref:Microsoft.Extensions.ObjectPool>是 ASP.NET Core 基礎結構的一部分，可支援將一組物件保留在記憶體中以供重複使用，而不是允許物件進行垃圾收集。</span><span class="sxs-lookup"><span data-stu-id="52551-105"><xref:Microsoft.Extensions.ObjectPool> is part of the ASP.NET Core infrastructure that supports keeping a group of objects in memory for reuse rather than allowing the objects to be garbage collected.</span></span>

<span data-ttu-id="52551-106">如果受管理的物件是，您可能會想要使用物件集區：</span><span class="sxs-lookup"><span data-stu-id="52551-106">You might want to use the object pool if the objects that are being managed are:</span></span>

- <span data-ttu-id="52551-107">配置/初始化耗費資源。</span><span class="sxs-lookup"><span data-stu-id="52551-107">Expensive to allocate/initialize.</span></span>
- <span data-ttu-id="52551-108">代表一些有限的資源。</span><span class="sxs-lookup"><span data-stu-id="52551-108">Represent some limited resource.</span></span>
- <span data-ttu-id="52551-109">可預測且經常使用。</span><span class="sxs-lookup"><span data-stu-id="52551-109">Used predictably and frequently.</span></span>

<span data-ttu-id="52551-110">例如，ASP.NET Core 架構會在某些地方使用物件集區，以重複使用 <xref:System.Text.StringBuilder> 實例。</span><span class="sxs-lookup"><span data-stu-id="52551-110">For example, the ASP.NET Core framework uses the object pool in some places to reuse <xref:System.Text.StringBuilder> instances.</span></span> <span data-ttu-id="52551-111">`StringBuilder`配置及管理自己的緩衝區以保存字元資料。</span><span class="sxs-lookup"><span data-stu-id="52551-111">`StringBuilder` allocates and manages its own buffers to hold character data.</span></span> <span data-ttu-id="52551-112">ASP.NET Core 定期使用 `StringBuilder` 來執行功能，並重複使用它們來提供效能優勢。</span><span class="sxs-lookup"><span data-stu-id="52551-112">ASP.NET Core regularly uses `StringBuilder` to implement features, and reusing them provides a performance benefit.</span></span>

<span data-ttu-id="52551-113">物件共用不一定會改善效能：</span><span class="sxs-lookup"><span data-stu-id="52551-113">Object pooling doesn't always improve performance:</span></span>

- <span data-ttu-id="52551-114">除非物件的初始化成本很高，否則從集區取得物件通常會比較慢。</span><span class="sxs-lookup"><span data-stu-id="52551-114">Unless the initialization cost of an object is high, it's usually slower to get the object from the pool.</span></span>
- <span data-ttu-id="52551-115">在解除配置集區之前，不會取消配置由集區管理的物件。</span><span class="sxs-lookup"><span data-stu-id="52551-115">Objects managed by the pool aren't de-allocated until the pool is de-allocated.</span></span>

<span data-ttu-id="52551-116">只有在使用應用程式或程式庫的實際案例收集效能資料之後，才使用物件共用。</span><span class="sxs-lookup"><span data-stu-id="52551-116">Use object pooling only after collecting performance data using realistic scenarios for your app or library.</span></span>

::: moniker range="< aspnetcore-3.0"
<span data-ttu-id="52551-117">**警告： `ObjectPool` 不會執行 `IDisposable` 。我們不建議您將它與需要處置的類型搭配使用。**</span><span class="sxs-lookup"><span data-stu-id="52551-117">**WARNING: The `ObjectPool` doesn't implement `IDisposable`. We don't recommend using it with types that need disposal.**</span></span> <span data-ttu-id="52551-118">`ObjectPool`在 ASP.NET Core 3.0 和更新版本中支援 `IDisposable` 。</span><span class="sxs-lookup"><span data-stu-id="52551-118">`ObjectPool` in ASP.NET Core 3.0 and later supports `IDisposable`.</span></span>
::: moniker-end

<span data-ttu-id="52551-119">**注意： ObjectPool 不會限制它所配置的物件數目，而會限制它將保留的物件數目。**</span><span class="sxs-lookup"><span data-stu-id="52551-119">**NOTE: The ObjectPool doesn't place a limit on the number of objects that it will allocate, it places a limit on the number of objects it will retain.**</span></span>

## <a name="concepts"></a><span data-ttu-id="52551-120">概念</span><span class="sxs-lookup"><span data-stu-id="52551-120">Concepts</span></span>

<span data-ttu-id="52551-121"><xref:Microsoft.Extensions.ObjectPool.ObjectPool`1>-基本物件集區抽象概念。</span><span class="sxs-lookup"><span data-stu-id="52551-121"><xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> - the basic object pool abstraction.</span></span> <span data-ttu-id="52551-122">用來取得和傳回物件。</span><span class="sxs-lookup"><span data-stu-id="52551-122">Used to get and return objects.</span></span>

<span data-ttu-id="52551-123"><xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601>-執行此程式，以自訂物件的建立方式，以及其在傳回集區時的*重設*方式。</span><span class="sxs-lookup"><span data-stu-id="52551-123"><xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> - implement this to customize how an object is created and how it is *reset* when returned to the pool.</span></span> <span data-ttu-id="52551-124">這可以傳遞至您直接建立的物件集區 .。。或</span><span class="sxs-lookup"><span data-stu-id="52551-124">This can be passed into an object pool that you construct directly.... OR</span></span>

<span data-ttu-id="52551-125"><xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*>作為建立物件集區的 factory。</span><span class="sxs-lookup"><span data-stu-id="52551-125"><xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> acts as a factory for creating object pools.</span></span>
<!-- REview, there is no ObjectPoolProvider<T> -->

<span data-ttu-id="52551-126">ObjectPool 可在應用程式中以多種方式使用：</span><span class="sxs-lookup"><span data-stu-id="52551-126">The ObjectPool can be used in an app in multiple ways:</span></span>

* <span data-ttu-id="52551-127">具現化集區。</span><span class="sxs-lookup"><span data-stu-id="52551-127">Instantiating a pool.</span></span>
* <span data-ttu-id="52551-128">在相依性[插入](xref:fundamentals/dependency-injection)（DI）中註冊集區做為實例。</span><span class="sxs-lookup"><span data-stu-id="52551-128">Registering a pool in [Dependency injection](xref:fundamentals/dependency-injection) (DI) as an instance.</span></span>
* <span data-ttu-id="52551-129">`ObjectPoolProvider<>`在 DI 中註冊，並使用它做為 factory。</span><span class="sxs-lookup"><span data-stu-id="52551-129">Registering the `ObjectPoolProvider<>` in DI and using it as a factory.</span></span>

## <a name="how-to-use-objectpool"></a><span data-ttu-id="52551-130">如何使用 ObjectPool</span><span class="sxs-lookup"><span data-stu-id="52551-130">How to use ObjectPool</span></span>

<span data-ttu-id="52551-131">呼叫 <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Get*> 以取得物件，並傳回 <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> 物件。</span><span class="sxs-lookup"><span data-stu-id="52551-131">Call <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Get*> to get an object and <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> to return the object.</span></span>  <span data-ttu-id="52551-132">您不需要傳回每個物件。</span><span class="sxs-lookup"><span data-stu-id="52551-132">There's no requirement that you return every object.</span></span> <span data-ttu-id="52551-133">如果您未傳回物件，則會進行垃圾收集。</span><span class="sxs-lookup"><span data-stu-id="52551-133">If you don't return an object, it will be garbage collected.</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="52551-134">當 <xref:Microsoft.Extensions.ObjectPool.DefaultObjectPoolProvider> 使用並 `T` 實行時 `IDisposable` ：</span><span class="sxs-lookup"><span data-stu-id="52551-134">When <xref:Microsoft.Extensions.ObjectPool.DefaultObjectPoolProvider> is used and `T` implements `IDisposable`:</span></span>

* <span data-ttu-id="52551-135">***未***傳回集區的專案將會被處置。</span><span class="sxs-lookup"><span data-stu-id="52551-135">Items that are ***not*** returned to the pool will be disposed.</span></span>
* <span data-ttu-id="52551-136">當 DI 處置集區時，會處置集區中的所有專案。</span><span class="sxs-lookup"><span data-stu-id="52551-136">When the pool gets disposed by DI, all items in the pool are disposed.</span></span>

<span data-ttu-id="52551-137">注意：處置集區之後：</span><span class="sxs-lookup"><span data-stu-id="52551-137">NOTE: After the pool is disposed:</span></span>

* <span data-ttu-id="52551-138">呼叫會擲回 `Get` `ObjectDisposedException` 。</span><span class="sxs-lookup"><span data-stu-id="52551-138">Calling `Get` throws a `ObjectDisposedException`.</span></span>
* <span data-ttu-id="52551-139">`return`處置指定的專案。</span><span class="sxs-lookup"><span data-stu-id="52551-139">`return` disposes the given item.</span></span>

::: moniker-end

## <a name="objectpool-sample"></a><span data-ttu-id="52551-140">ObjectPool 範例</span><span class="sxs-lookup"><span data-stu-id="52551-140">ObjectPool sample</span></span>

<span data-ttu-id="52551-141">下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="52551-141">The following code:</span></span>

* <span data-ttu-id="52551-142">將加入至相依性 `ObjectPoolProvider` [插入](xref:fundamentals/dependency-injection)（DI）容器。</span><span class="sxs-lookup"><span data-stu-id="52551-142">Adds `ObjectPoolProvider` to the [Dependency injection](xref:fundamentals/dependency-injection) (DI) container.</span></span>
* <span data-ttu-id="52551-143">將和設定 `ObjectPool<StringBuilder>` 為 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="52551-143">Adds and configures `ObjectPool<StringBuilder>` to the DI container.</span></span>
* <span data-ttu-id="52551-144">加入 `BirthdayMiddleware` 。</span><span class="sxs-lookup"><span data-stu-id="52551-144">Adds the `BirthdayMiddleware`.</span></span>

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

<span data-ttu-id="52551-145">下列程式碼會實行`BirthdayMiddleware`</span><span class="sxs-lookup"><span data-stu-id="52551-145">The following code implements `BirthdayMiddleware`</span></span>

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]

[!INCLUDE[request localized comments](~/includes/code-comments-loc.md)]
