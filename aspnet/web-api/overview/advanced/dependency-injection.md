---
uid: web-api/overview/advanced/dependency-injection
title: ASP.NET Web API 2 中的相依性插入 |Microsoft Docs
author: MikeWasson
description: 本教學課程會示範如何插入您的 ASP.NET Web API 控制器中的相依性。 在教學課程 Web API 2 Unity 應用程式區塊中使用的軟體版本...
ms.author: riande
ms.date: 01/20/2014
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 318a2f1c587feb360212a390bb5de7bdc127513d
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341822"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a>ASP.NET Web API 2 中的相依性插入
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> 本教學課程會示範如何插入您的 ASP.NET Web API 控制器中的相依性。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - Web API 2
> - [Unity Application Block](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 （也可使用這個第 5 版）


## <a name="what-is-dependency-injection"></a>相依性插入是什麼？

「相依性」是另一個物件所需的任何物件。 比方說，是很常見來定義[存放庫](http://martinfowler.com/eaaCatalog/repository.html)可處理的資料存取。 讓我們舉例說明。 首先，我們會定義領域模型：

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

以下是將項目儲存在資料庫中，使用 Entity Framework 的簡單的儲存機制類別。

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

現在讓我們定義支援 GET 要求的 Web API 控制器`Product`實體。 （我所出文章和其他方法，為求簡化離開）。以下是第一次嘗試：

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

請注意，控制器類別取決於`ProductRepository`，我們會讓建立控制器和`ProductRepository`執行個體。 不過，它是個好主意，硬式相依性，如此一來，有幾個原因。

- 如果您想要取代`ProductRepository`與不同的實作中，您還需要修改控制器類別。
- 如果`ProductRepository`具有相依性，您必須設定這些控制器內。 針對大型專案中使用多個控制站，將組態程式碼變得散布在您的專案。
- 因為控制器是硬式編碼為查詢資料庫，它很難使用單元測試。 如需單元測試，您應該使用模擬或虛設常式儲存機制，也就是不可能以成為目前的設計。

我們可以解決這些問題，由*插入*到控制器的存放庫。 首先，重構`ProductRepository`介面的類別：

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

然後提供`IProductRepository`做為建構函式參數：

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

這個範例會使用[建構函式插入](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)。 您也可以使用*setter 插入*，您將設定透過 setter 方法或屬性的相依性。

但是，現在還有一個問題，因為您的應用程式不會直接建立控制器。 當它路由傳送要求，且 Web API 並不知道的任何項目時，web API 建立控制器`IProductRepository`。 這是 Web API 的相依性解析程式之處。

## <a name="the-web-api-dependency-resolver"></a>Web API 的相依性解析程式

Web API 定義**IDependencyResolver**介面解析相依性。 以下是介面的定義：

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

**IDependencyScope**介面有兩種方法：

- **GetService**建立類型的執行個體。
- **GetServices**建立指定類型之物件的集合。

**IDependencyResolver**方法繼承**IDependencyScope** ，並將**BeginScope**方法。 本教學課程稍後，我將討論範圍。

當 Web API 建立控制器執行個體時，它會先呼叫**IDependencyResolver.GetService**，傳遞控制站類型。 您可以使用此擴充性攔截程序來建立控制器，解析任何相依性。 如果**GetService**傳回 null，Web API 控制器類別上的無參數建構函式的外觀。

## <a name="dependency-resolution-with-the-unity-container"></a>使用 Unity 容器相依性解析

雖然您可以撰寫完整**IDependencyResolver**介面從頭實作真的被設計來做為 Web API 與現有的 IoC 容器之間的橋樑。

IoC 容器是負責管理相依性的軟體元件。 您向容器註冊類型，然後再使用 建立物件的 容器。 容器會自動找出相依性關聯性。 許多 IoC 容器也可讓您控制物件存留期和範圍等項目。

> [!NOTE]
> 「 IoC 」 代表 「 的控制項反轉 」，這是一種架構會呼叫應用程式程式碼的一般模式。 IoC 容器建構您的物件，其中 「 反轉 」 一般控制流程。


本教學課程中，我們將使用[Unity](https://msdn.microsoft.com/library/ff647202.aspx)從 Microsoft Patterns&amp;作法。 (其他熱門的程式庫包含[Castle Windsor](http://www.castleproject.org/)， [Spring.Net](http://www.springframework.net/)， [Autofac](https://code.google.com/p/autofac/)， [Ninject](http://www.ninject.org/)，和[StructureMap](http://structuremap.github.io/documentation/).)您可以使用 NuGet 套件管理員安裝 Unity。 從**工具**功能表，在 Visual Studio 中，選取**NuGet 套件管理員**，然後選取**Package Manager Console**。 在 [套件管理員主控台] 視窗中，輸入下列命令：

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

以下是實作**IDependencyResolver**包裝 Unity 容器。

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> 如果**GetService**的型別時，無法解析方法，它應該會傳回**null**。 如果**GetServices**方法無法解析型別，它應該會傳回空集合物件。 不會擲回未知類型的例外狀況。


## <a name="configuring-the-dependency-resolver"></a>設定相依性解析程式

設定相依性解析程式**DependencyResolver**屬性的全域**HttpConfiguration**物件。

下列程式碼的暫存器`IProductRepository`使用 Unity 的介面，然後建立`UnityResolver`。

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>相依性範圍和控制站的存留期

控制站會建立每個要求。 若要管理物件存留期， **IDependencyResolver**使用的概念*範圍*。

相依性解析程式附加至**HttpConfiguration**物件具有全域範圍。 當 Web API 建立的控制器時，它會呼叫**BeginScope**。 這個方法會傳回**IDependencyScope**表示子範圍。

然後呼叫 web API **GetService**子領域建立控制站上。 要求完成時，會呼叫 Web API**處置**子範圍。 使用**處置**處置控制器的相依性的方法。

如何實作**BeginScope** IoC 容器而定。 For Unity，範圍會對應至子容器中：

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

大部分的 IoC 容器具有類似的對等項目。
