---
title: 在 ASP.NET Core 中使用 ObjectPool 的物件重複使用
author: rick-anderson
description: 使用 ObjectPool 增加 ASP.NET Core 應用程式效能的秘訣。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 04/11/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: performance/ObjectPool
ms.openlocfilehash: 004ca5724517bf3fbf6512c0b9653793f4e0f702
ms.sourcegitcommit: dd2a1542a4a377123490034153368c135fdbd09e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85241002"
---
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a>在 ASP.NET Core 中使用 ObjectPool 的物件重複使用

作者： [Steve Gordon](https://twitter.com/stevejgordon)、 [Ryan Nowak](https://github.com/rynowak)和[Rick Anderson](https://twitter.com/RickAndMSFT)

<xref:Microsoft.Extensions.ObjectPool>是 ASP.NET Core 基礎結構的一部分，可支援將一組物件保留在記憶體中以供重複使用，而不是允許物件進行垃圾收集。

如果受管理的物件是，您可能會想要使用物件集區：

- 配置/初始化耗費資源。
- 代表一些有限的資源。
- 可預測且經常使用。

例如，ASP.NET Core 架構會在某些地方使用物件集區，以重複使用 <xref:System.Text.StringBuilder> 實例。 `StringBuilder`配置及管理自己的緩衝區以保存字元資料。 ASP.NET Core 定期使用 `StringBuilder` 來執行功能，並重複使用它們來提供效能優勢。

物件共用不一定會改善效能：

- 除非物件的初始化成本很高，否則從集區取得物件通常會比較慢。
- 在解除配置集區之前，不會取消配置由集區管理的物件。

只有在使用應用程式或程式庫的實際案例收集效能資料之後，才使用物件共用。

**警告： `ObjectPool` 不會執行 `IDisposable` 。我們不建議您將它與需要處置的類型搭配使用。**

**注意： ObjectPool 不會限制它所配置的物件數目，而會限制它將保留的物件數目。**

## <a name="concepts"></a>概念

<xref:Microsoft.Extensions.ObjectPool.ObjectPool`1>-基本物件集區抽象概念。 用來取得和傳回物件。

<xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601>-執行此程式，以自訂物件的建立方式，以及其在傳回集區時的*重設*方式。 這可以傳遞至您直接建立的物件集區 .。。或

<xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*>作為建立物件集區的 factory。
<!-- REview, there is no ObjectPoolProvider<T> -->

ObjectPool 可在應用程式中以多種方式使用：

* 具現化集區。
* 在相依性[插入](xref:fundamentals/dependency-injection)（DI）中註冊集區做為實例。
* `ObjectPoolProvider<>`在 DI 中註冊，並使用它做為 factory。

## <a name="how-to-use-objectpool"></a>如何使用 ObjectPool

呼叫 <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> 以取得物件，並傳回 <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> 物件。  您不需要傳回每個物件。 如果您未傳回物件，則會進行垃圾收集。

## <a name="objectpool-sample"></a>ObjectPool 範例

下列程式碼：

* 將加入至相依性 `ObjectPoolProvider` [插入](xref:fundamentals/dependency-injection)（DI）容器。
* 將和設定 `ObjectPool<StringBuilder>` 為 DI 容器。
* 加入 `BirthdayMiddleware` 。

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

下列程式碼會實行`BirthdayMiddleware`

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]

[!INCLUDE[request localized comments](~/includes/code-comments-loc.md)]
