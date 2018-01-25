---
uid: web-api/overview/advanced/dependency-injection
title: "ASP.NET Web API 2 中的相依性插入 |Microsoft 文件"
author: MikeWasson
description: "本教學課程會示範如何插入您的 ASP.NET Web API 控制器的相依性。 在教學課程 Web API 2 Unity 應用程式區塊中使用的軟體版本..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 7f64cc83e36c80b0ffd53edfc629557c0847b200
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="c976c-104">ASP.NET Web API 2 中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="c976c-104">Dependency Injection in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="c976c-105">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c976c-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c976c-106">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="c976c-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="c976c-107">本教學課程會示範如何插入您的 ASP.NET Web API 控制器的相依性。</span><span class="sxs-lookup"><span data-stu-id="c976c-107">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c976c-108">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="c976c-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c976c-109">Web API 2</span><span class="sxs-lookup"><span data-stu-id="c976c-109">Web API 2</span></span>
> - [<span data-ttu-id="c976c-110">Unity 應用程式區塊</span><span class="sxs-lookup"><span data-stu-id="c976c-110">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="c976c-111">Entity Framework 6 （也可使用這個第 5 版）</span><span class="sxs-lookup"><span data-stu-id="c976c-111">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="c976c-112">相依性插入是什麼？</span><span class="sxs-lookup"><span data-stu-id="c976c-112">What is Dependency Injection?</span></span>

<span data-ttu-id="c976c-113">A*相依性*是另一個物件需要的任何物件。</span><span class="sxs-lookup"><span data-stu-id="c976c-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="c976c-114">比方說，是通用定義[儲存機制](http://martinfowler.com/eaaCatalog/repository.html)可處理的資料存取。</span><span class="sxs-lookup"><span data-stu-id="c976c-114">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="c976c-115">讓我們以範例說明。</span><span class="sxs-lookup"><span data-stu-id="c976c-115">Let's illustrate with an example.</span></span> <span data-ttu-id="c976c-116">首先，我們會定義網域模型：</span><span class="sxs-lookup"><span data-stu-id="c976c-116">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="c976c-117">以下是將項目儲存在資料庫中，使用 Entity Framework 的簡單的儲存機制類別。</span><span class="sxs-lookup"><span data-stu-id="c976c-117">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="c976c-118">現在讓我們先定義支援 GET 要求的 Web API 控制器`Product`實體。</span><span class="sxs-lookup"><span data-stu-id="c976c-118">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="c976c-119">（I 留下任何 POST 和簡化的其他方法。）以下是第一次嘗試：</span><span class="sxs-lookup"><span data-stu-id="c976c-119">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="c976c-120">請注意，控制器類別取決於`ProductRepository`，我們會讓建立控制器和`ProductRepository`執行個體。</span><span class="sxs-lookup"><span data-stu-id="c976c-120">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="c976c-121">不過，它是不好的主意，硬式相依性，如此一來，有幾個原因。</span><span class="sxs-lookup"><span data-stu-id="c976c-121">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="c976c-122">如果您想要取代`ProductRepository`利用不同的實作，您還需要修改控制器類別。</span><span class="sxs-lookup"><span data-stu-id="c976c-122">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="c976c-123">如果`ProductRepository`具有相依性，您必須設定這些控制器內。</span><span class="sxs-lookup"><span data-stu-id="c976c-123">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="c976c-124">針對具有多個控制站的大型專案，組態程式碼變得分散您的專案。</span><span class="sxs-lookup"><span data-stu-id="c976c-124">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="c976c-125">很難單元測試，因為控制器是硬式編碼查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="c976c-125">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="c976c-126">單元測試，您應該使用模擬或虛設常式儲存機制，這是不可能與目前設計。</span><span class="sxs-lookup"><span data-stu-id="c976c-126">For a unit test, you should use a mock or stub repository, which is not possible with the currect design.</span></span>

<span data-ttu-id="c976c-127">我們可以解決這些問題的*插入*到控制器的儲存機制。</span><span class="sxs-lookup"><span data-stu-id="c976c-127">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="c976c-128">首先，重構`ProductRepository`至介面的類別：</span><span class="sxs-lookup"><span data-stu-id="c976c-128">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="c976c-129">然後提供`IProductRepository`做為建構函式參數：</span><span class="sxs-lookup"><span data-stu-id="c976c-129">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="c976c-130">這個範例會使用[建構函式插入](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)。</span><span class="sxs-lookup"><span data-stu-id="c976c-130">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="c976c-131">您也可以使用*setter 資料隱碼*，您將設定透過 setter 方法或屬性的相依性。</span><span class="sxs-lookup"><span data-stu-id="c976c-131">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="c976c-132">但現在有問題，因為您的應用程式不會直接建立控制器。</span><span class="sxs-lookup"><span data-stu-id="c976c-132">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="c976c-133">Web API 控制器時建立它路由傳送要求，及 Web API 並不知道任何關於`IProductRepository`。</span><span class="sxs-lookup"><span data-stu-id="c976c-133">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="c976c-134">這是 Web 應用程式開發介面的相依性解析程式的運作方式。</span><span class="sxs-lookup"><span data-stu-id="c976c-134">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="c976c-135">Web 應用程式開發介面的相依性解析程式</span><span class="sxs-lookup"><span data-stu-id="c976c-135">The Web API Dependency Resolver</span></span>

<span data-ttu-id="c976c-136">Web 應用程式開發介面會定義**IDependencyResolver**介面解析相依性。</span><span class="sxs-lookup"><span data-stu-id="c976c-136">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="c976c-137">以下是介面的定義：</span><span class="sxs-lookup"><span data-stu-id="c976c-137">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="c976c-138">**IDependencyScope**介面有兩種方法：</span><span class="sxs-lookup"><span data-stu-id="c976c-138">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="c976c-139">**GetService**建立類型的一個執行個體。</span><span class="sxs-lookup"><span data-stu-id="c976c-139">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="c976c-140">**GetServices**建立指定類型之物件的集合。</span><span class="sxs-lookup"><span data-stu-id="c976c-140">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="c976c-141">**IDependencyResolver**方法繼承**IDependencyScope** ，並將**BeginScope**方法。</span><span class="sxs-lookup"><span data-stu-id="c976c-141">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="c976c-142">我將討論稍後在本教學課程的範圍。</span><span class="sxs-lookup"><span data-stu-id="c976c-142">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="c976c-143">當 Web 應用程式開發介面建立控制器執行個體時，它會先呼叫**IDependencyResolver.GetService**，並傳入控制器類型。</span><span class="sxs-lookup"><span data-stu-id="c976c-143">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="c976c-144">您可以使用此擴充性攔截程序來建立控制器，解決任何相依性。</span><span class="sxs-lookup"><span data-stu-id="c976c-144">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="c976c-145">如果**GetService** ，則傳回 null，Web API 控制器類別上的無參數建構函式的查詢。</span><span class="sxs-lookup"><span data-stu-id="c976c-145">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="c976c-146">相依性解析與 Unity 容器</span><span class="sxs-lookup"><span data-stu-id="c976c-146">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="c976c-147">雖然您可以撰寫完整**IDependencyResolver**從頭介面的實作真的設計來做為 Web API 和現有 IoC 容器之間的橋樑。</span><span class="sxs-lookup"><span data-stu-id="c976c-147">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="c976c-148">IoC 容器是負責管理相依性的軟體元件。</span><span class="sxs-lookup"><span data-stu-id="c976c-148">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="c976c-149">您使用容器，註冊型別，並將容器來建立物件。</span><span class="sxs-lookup"><span data-stu-id="c976c-149">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="c976c-150">容器會自動找出的相依性關聯性。</span><span class="sxs-lookup"><span data-stu-id="c976c-150">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="c976c-151">許多 IoC 容器也可讓您控制等物件存留期範圍。</span><span class="sxs-lookup"><span data-stu-id="c976c-151">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="c976c-152">"IoC 「 代表 」 的控制項反轉 」，這是一般模式架構會呼叫應用程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="c976c-152">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="c976c-153">IoC 容器建構您的物件，其中"反轉 」 一般控制流程。</span><span class="sxs-lookup"><span data-stu-id="c976c-153">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="c976c-154">此教學課程中，我們將使用[Unity](https://msdn.microsoft.com/library/ff647202.aspx)從 Microsoft Patterns&amp;作法。</span><span class="sxs-lookup"><span data-stu-id="c976c-154">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="c976c-155">(其他常用的程式庫包含[城堡 Windsor](http://www.castleproject.org/)， [Spring.Net](http://www.springframework.net/)， [Autofac](https://code.google.com/p/autofac/)， [Ninject](http://www.ninject.org/)，和[StructureMap](http://docs.structuremap.net/).)安裝 Unity，您可以使用 NuGet 套件管理員。</span><span class="sxs-lookup"><span data-stu-id="c976c-155">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://docs.structuremap.net/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="c976c-156">從**工具**在 Visual Studio 中，選取功能表**程式庫套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="c976c-156">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="c976c-157">在 [封裝管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="c976c-157">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="c976c-158">以下是實作**IDependencyResolver**包裝 Unity 容器。</span><span class="sxs-lookup"><span data-stu-id="c976c-158">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="c976c-159">如果**GetService**方法無法解析類型，它應該傳回**null**。</span><span class="sxs-lookup"><span data-stu-id="c976c-159">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="c976c-160">如果**GetServices**方法無法解析類型，它應該會傳回空集合物件。</span><span class="sxs-lookup"><span data-stu-id="c976c-160">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="c976c-161">不會擲回未知類型的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c976c-161">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="c976c-162">設定相依性解析程式</span><span class="sxs-lookup"><span data-stu-id="c976c-162">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="c976c-163">設定相依性解析程式上**dependencyresolver 來註冊**的全域屬性**HttpConfiguration**物件。</span><span class="sxs-lookup"><span data-stu-id="c976c-163">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="c976c-164">下列程式碼會註冊`IProductRepository`和 Unity 的介面，然後建立`UnityResolver`。</span><span class="sxs-lookup"><span data-stu-id="c976c-164">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="c976c-165">相依性範圍和控制器存留期</span><span class="sxs-lookup"><span data-stu-id="c976c-165">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="c976c-166">控制站會建立每個要求。</span><span class="sxs-lookup"><span data-stu-id="c976c-166">Controllers are created per request.</span></span> <span data-ttu-id="c976c-167">若要管理物件的存留期， **IDependencyResolver**使用的概念*範圍*。</span><span class="sxs-lookup"><span data-stu-id="c976c-167">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="c976c-168">相依性解析程式附加至**HttpConfiguration**物件具有全域範圍。</span><span class="sxs-lookup"><span data-stu-id="c976c-168">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="c976c-169">當 Web 應用程式開發介面建立控制器時，它會呼叫**BeginScope**。</span><span class="sxs-lookup"><span data-stu-id="c976c-169">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="c976c-170">這個方法會傳回**IDependencyScope**表示子範圍。</span><span class="sxs-lookup"><span data-stu-id="c976c-170">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="c976c-171">然後呼叫 web API **GetService**子領域建立控制站上。</span><span class="sxs-lookup"><span data-stu-id="c976c-171">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="c976c-172">完成要求時，會呼叫 Web API**處置**上子領域。</span><span class="sxs-lookup"><span data-stu-id="c976c-172">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="c976c-173">使用**處置**方法處置控制器的相依性。</span><span class="sxs-lookup"><span data-stu-id="c976c-173">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="c976c-174">如何實作**BeginScope**取決於 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="c976c-174">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="c976c-175">Unity，範圍會對應至子容器：</span><span class="sxs-lookup"><span data-stu-id="c976c-175">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="c976c-176">大部分的 IoC 容器具有類似的對等項目。</span><span class="sxs-lookup"><span data-stu-id="c976c-176">Most IoC containers have similar equivalents.</span></span>
