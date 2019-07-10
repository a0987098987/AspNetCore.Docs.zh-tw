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
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a>使用 ASP.NET Core 中的 ObjectPool 物件重複使用

藉由[Steve Gordon](https://twitter.com/stevejgordon)， [Ryan Nowak](https://github.com/rynowak)，和[Rick Anderson](https://twitter.com/RickAndMSFT)

<xref:Microsoft.Extensions.ObjectPool> 是支援一整組物件保留在記憶體中的重複使用，而不是讓要進行記憶體回收物件收集的 ASP.NET Core 基礎結構的一部分。

您可能想要使用的物件集區，如果受管理物件：

- 配置/初始化成本。
- 代表某些有限的資源。
- 使用可預測的方式和常見問題。

例如，ASP.NET Core 架構會使用物件集區中的某些位置重複使用<xref:System.Text.StringBuilder>執行個體。 `StringBuilder` 配置並管理它自己的緩衝區來容納的字元資料。 ASP.NET Core 定期使用`StringBuilder`實作功能，並重複使用它們提供效能優勢。

物件集區不一定會改善效能：

- 物件的初始化成本很高，除非它是從集區取得的物件通常速度較慢。
- 解除配置的集區之前，受管理的集區物件並不解除配置。

使用物件集區才收集應用程式或程式庫使用真實案例的效能資料。

**警告：`ObjectPool`不會實作`IDisposable`。我們不建議使用需要處置的類型。**

**注意：ObjectPool 不會限制它將會配置的物件數目，它會使用它將會保留的物件數目的限制。**

## <a name="concepts"></a>概念

<xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> -基本物件集區抽象概念。 用來取得並傳回物件。

<xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> -實作此項以自訂物件的建立方式和其用法*重設*時傳回到集區。 這可以傳入您直接建構的物件集區...OR

<xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> 當做的 factory 來建立物件集區。
<!-- REview, there is no ObjectPoolProvider<T> -->

在應用程式中以多種方式可用 ObjectPool:

* 具現化的集區。
* 註冊中的集區[相依性插入](xref:fundamentals/dependency-injection)(DI) 做為執行個體。
* 註冊`ObjectPoolProvider<>`DI 並使用它做為 factory。

## <a name="how-to-use-objectpool"></a>如何使用 ObjectPool

呼叫<xref:Microsoft.Extensions.ObjectPool.ObjectPool`1>取得的物件和<xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*>傳回的物件。  沒有，您就會傳回每個物件的需求。 如果您不會傳回物件，它會進行記憶體回收。

## <a name="objectpool-sample"></a>ObjectPool 範例

下列程式碼範例：

* 新增`ObjectPoolProvider`要[相依性插入](xref:fundamentals/dependency-injection)(DI) 容器。
* 新增並設定`ObjectPool<StringBuilder>`至 DI 容器。
* 新增`BirthdayMiddleware`。

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

下列程式碼實作 `BirthdayMiddleware`

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
