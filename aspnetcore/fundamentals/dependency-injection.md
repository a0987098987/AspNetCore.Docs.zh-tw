---
title: "在 ASP.NET Core 的相依性插入"
author: ardalis
description: "了解 ASP.NET Core 實作相依性插入的方式以及如何使用它。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/dependency-injection
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7a5a0991694b2c7caa79dbc09f6471d614f67dac
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-dependency-injection-in-aspnet-core"></a><span data-ttu-id="5aaa5-103">在 ASP.NET Core 的相依性插入的簡介</span><span class="sxs-lookup"><span data-stu-id="5aaa5-103">Introduction to Dependency Injection in ASP.NET Core</span></span>

<a name="fundamentals-dependency-injection"></a>

<span data-ttu-id="5aaa5-104">由[Steve Smith](https://ardalis.com/)和[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="5aaa5-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="5aaa5-105">ASP.NET Core 被設計，註冊支援，並利用相依性插入。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-105">ASP.NET Core is designed from the ground up to support and leverage dependency injection.</span></span> <span data-ttu-id="5aaa5-106">ASP.NET Core 應用程式可以利用內建架構服務，讓它們插入到啟動類別中的方法，可以將應用程式服務設定以及資料隱碼。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-106">ASP.NET Core applications can leverage built-in framework services by having them injected into methods in the Startup class, and application services can be configured for injection as well.</span></span> <span data-ttu-id="5aaa5-107">ASP.NET Core 所提供的預設服務容器會提供最少的功能設定，並且不想要取代其他容器。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-107">The default services container provided by ASP.NET Core provides a minimal feature set and isn't intended to replace other containers.</span></span>

<span data-ttu-id="5aaa5-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5aaa5-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="5aaa5-109">相依性插入是什麼？</span><span class="sxs-lookup"><span data-stu-id="5aaa5-109">What is Dependency Injection?</span></span>

<span data-ttu-id="5aaa5-110">相依性插入 (DI) 是針對達到物件和共同作業者或相依性之間的鬆散偶合的技術。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-110">Dependency injection (DI) is a technique for achieving loose coupling between objects and their collaborators, or dependencies.</span></span> <span data-ttu-id="5aaa5-111">而非直接具現化共同作業者，或使用靜態參考，才能執行其動作的類別需要的物件會提供以某些方式中的類別。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-111">Rather than directly instantiating collaborators, or using static references, the objects a class needs in order to perform its actions are provided to the class in some fashion.</span></span> <span data-ttu-id="5aaa5-112">大多數情況下，類別會宣告它們的相依性，透過其建構函式，讓他們遵循[明確的相依性原則](http://deviq.com/explicit-dependencies-principle/)。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-112">Most often, classes will declare their dependencies via their constructor, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span> <span data-ttu-id="5aaa5-113">這種方法稱為 「 建構函式資料隱碼 >。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-113">This approach is known as "constructor injection".</span></span>

<span data-ttu-id="5aaa5-114">類別專為 DI 設計，當它們正在多鬆散耦合因為它們沒有直接的硬式編碼相依性及其共同作業者上。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-114">When classes are designed with DI in mind, they're more loosely coupled because they don't have direct, hard-coded dependencies on their collaborators.</span></span> <span data-ttu-id="5aaa5-115">這會遵循[相依性反向原則](http://deviq.com/dependency-inversion-principle/)，其指出*「 高的層級模組不應該依賴低層級的模組; 兩者都需要具備的抽象概念 」。*</span><span class="sxs-lookup"><span data-stu-id="5aaa5-115">This follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), which states that *"high level modules shouldn't depend on low level modules; both should depend on abstractions."*</span></span> <span data-ttu-id="5aaa5-116">而不是參考特定實作，類別會要求抽象層 (通常`interfaces`) 建構類別時，這提供給它們。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-116">Instead of referencing specific implementations, classes request abstractions (typically `interfaces`) which are provided to them when the class is constructed.</span></span> <span data-ttu-id="5aaa5-117">擷取相依性介面，並提供實作這些介面做為參數也是範例[策略設計模式](http://deviq.com/strategy-design-pattern/)。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-117">Extracting dependencies into interfaces and providing implementations of these interfaces as parameters is also an example of the [Strategy design pattern](http://deviq.com/strategy-design-pattern/).</span></span>

<span data-ttu-id="5aaa5-118">系統設計為使用 DI，具有許多要求透過其建構函式 （或屬性），及其相依性的類別時有專門用來建立這些類別及其相關聯的相依性的類別幫助。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-118">When a system is designed to use DI, with many classes requesting their dependencies via their constructor (or properties), it's helpful to have a class dedicated to creating these classes with their associated dependencies.</span></span> <span data-ttu-id="5aaa5-119">這些類別統稱為*容器*，更具體來說，[逆轉控制 (IoC)](http://deviq.com/inversion-of-control/)容器或相依性插入 (DI) 容器。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-119">These classes are referred to as *containers*, or more specifically, [Inversion of Control (IoC)](http://deviq.com/inversion-of-control/) containers or Dependency Injection (DI) containers.</span></span> <span data-ttu-id="5aaa5-120">容器是基本上會負責提供向它要求類型的執行個體的 factory。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-120">A container is essentially a factory that's responsible for providing instances of types that are requested from it.</span></span> <span data-ttu-id="5aaa5-121">如果指定的型別已經宣告，它有相依性，容器已設定為提供相依性類型，它會建立在建立要求的執行個體的相依性。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-121">If a given type has declared that it has dependencies, and the container has been configured to provide the dependency types, it will create the dependencies as part of creating the requested instance.</span></span> <span data-ttu-id="5aaa5-122">如此一來，可以提供複雜的相依性圖形，而不需要任何硬式編碼的物件建構的類別。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-122">In this way, complex dependency graphs can be provided to classes without the need for any hard-coded object construction.</span></span> <span data-ttu-id="5aaa5-123">除了建立物件相依性，容器通常會管理應用程式中的物件存留期。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-123">In addition to creating objects with their dependencies, containers typically manage object lifetimes within the application.</span></span>

<span data-ttu-id="5aaa5-124">ASP.NET Core 包含簡單的內建容器 (由`IServiceProvider`介面)，根據預設，支援建構函式插入並 ASP.NET 的特定服務提供透過 DI。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-124">ASP.NET Core includes a simple built-in container (represented by the `IServiceProvider` interface) that supports constructor injection by default, and ASP.NET makes certain services available through DI.</span></span> <span data-ttu-id="5aaa5-125">ASP。網路的容器做為其所管理的類型是指*服務*。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-125">ASP.NET's container refers to the types it manages as *services*.</span></span> <span data-ttu-id="5aaa5-126">此篇文章的其餘*服務*會參考受 ASP.NET Core IoC 容器的類型。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-126">Throughout the rest of this article, *services* will refer to types that are managed by ASP.NET Core's IoC container.</span></span> <span data-ttu-id="5aaa5-127">設定中的內建的容器服務`ConfigureServices`應用程式中的方法`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-127">You configure the built-in container's services in the `ConfigureServices` method in your application's `Startup` class.</span></span>

> [!NOTE]
> <span data-ttu-id="5aaa5-128">Martin Fowler 已寫入大量的發行項上[逆轉控制容器和相依性插入模式](https://www.martinfowler.com/articles/injection.html)。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-128">Martin Fowler has written an extensive article on [Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html).</span></span> <span data-ttu-id="5aaa5-129">Microsoft Patterns and Practices 也有很棒的描述[相依性插入](https://msdn.microsoft.com/library/hh323705.aspx)。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-129">Microsoft Patterns and Practices also has a great description of [Dependency Injection](https://msdn.microsoft.com/library/hh323705.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="5aaa5-130">本文涵蓋相依性插入套用到所有的 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-130">This article covers Dependency Injection as it applies to all ASP.NET applications.</span></span> <span data-ttu-id="5aaa5-131">MVC 控制器內的相依性插入在講述[相依性插入和控制器](../mvc/controllers/dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-131">Dependency Injection within MVC controllers is covered in [Dependency Injection and Controllers](../mvc/controllers/dependency-injection.md).</span></span>

### <a name="constructor-injection-behavior"></a><span data-ttu-id="5aaa5-132">建構函式資料隱碼行為</span><span class="sxs-lookup"><span data-stu-id="5aaa5-132">Constructor Injection Behavior</span></span>

<span data-ttu-id="5aaa5-133">插入建構函式需要有問題的建構函式是*公用*。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-133">Constructor injection requires that the constructor in question be *public*.</span></span> <span data-ttu-id="5aaa5-134">否則，您的應用程式將會擲回`InvalidOperationException`:</span><span class="sxs-lookup"><span data-stu-id="5aaa5-134">Otherwise, your app will throw an `InvalidOperationException`:</span></span>

> <span data-ttu-id="5aaa5-135">找不到類型 'YourType' 適合建構函式。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-135">A suitable constructor for type 'YourType' couldn't be located.</span></span> <span data-ttu-id="5aaa5-136">請確定類型是具象，且服務中註冊的所有參數的公用建構函式。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-136">Ensure the type is concrete and services are registered for all parameters of a public constructor.</span></span>


<span data-ttu-id="5aaa5-137">插入建構函式需要該只能有一個適用於建構函式存在。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-137">Constructor injection requires that only one applicable constructor exist.</span></span> <span data-ttu-id="5aaa5-138">支援的建構函式多載，但只有一個多載可以存在於其引數可以所有滿足的相依性插入。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-138">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="5aaa5-139">如果有多個，您的應用程式將會擲回`InvalidOperationException`:</span><span class="sxs-lookup"><span data-stu-id="5aaa5-139">If more than one exists, your app will throw an `InvalidOperationException`:</span></span>

> <span data-ttu-id="5aaa5-140">類型 'YourType' 中已找到多個建構函式接受所有指定的引數類型。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-140">Multiple constructors accepting all given argument types have been found in type 'YourType'.</span></span> <span data-ttu-id="5aaa5-141">只能有一個適用的建構函式。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-141">There should only be one applicable constructor.</span></span>

<span data-ttu-id="5aaa5-142">建構函式可以接受所未提供的相依性插入的引數，但這些項目必須支援的預設值。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-142">Constructors can accept arguments that are not provided by dependency injection, but these must support default values.</span></span> <span data-ttu-id="5aaa5-143">例如: </span><span class="sxs-lookup"><span data-stu-id="5aaa5-143">For example:</span></span>

```csharp
// throws InvalidOperationException: Unable to resolve service for type 'System.String'...
public CharactersController(ICharacterRepository characterRepository, string title)
{
    _characterRepository = characterRepository;
    _title = title;
}

// runs without error
public CharactersController(ICharacterRepository characterRepository, string title = "Characters")
{
    _characterRepository = characterRepository;
    _title = title;
}
```

## <a name="using-framework-provided-services"></a><span data-ttu-id="5aaa5-144">使用 Framework 提供的服務</span><span class="sxs-lookup"><span data-stu-id="5aaa5-144">Using Framework-Provided Services</span></span>

<span data-ttu-id="5aaa5-145">`ConfigureServices`方法中的`Startup`類別會負責定義將使用的應用程式，包括平台功能，例如 Entity Framework Core 與 ASP.NET Core MVC 的服務。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-145">The `ConfigureServices` method in the `Startup` class is responsible for defining the services the application will use, including platform features like Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="5aaa5-146">一開始，`IServiceCollection`提供給`ConfigureServices`具有下列服務定義 (取決於[主應用程式的設定方式](xref:fundamentals/hosting)):</span><span class="sxs-lookup"><span data-stu-id="5aaa5-146">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/hosting)):</span></span>

| <span data-ttu-id="5aaa5-147">服務類型</span><span class="sxs-lookup"><span data-stu-id="5aaa5-147">Service Type</span></span> | <span data-ttu-id="5aaa5-148">存留期</span><span class="sxs-lookup"><span data-stu-id="5aaa5-148">Lifetime</span></span> |
| ----- | ------- |
| [<span data-ttu-id="5aaa5-149">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="5aaa5-149">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) | <span data-ttu-id="5aaa5-150">單一</span><span class="sxs-lookup"><span data-stu-id="5aaa5-150">Singleton</span></span> |
| [<span data-ttu-id="5aaa5-151">Microsoft.Extensions.Logging.ILoggerFactory</span><span class="sxs-lookup"><span data-stu-id="5aaa5-151">Microsoft.Extensions.Logging.ILoggerFactory</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.iloggerfactory) | <span data-ttu-id="5aaa5-152">單一</span><span class="sxs-lookup"><span data-stu-id="5aaa5-152">Singleton</span></span> |
| [<span data-ttu-id="5aaa5-153">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="5aaa5-153">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) | <span data-ttu-id="5aaa5-154">單一</span><span class="sxs-lookup"><span data-stu-id="5aaa5-154">Singleton</span></span> |
| [<span data-ttu-id="5aaa5-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span><span class="sxs-lookup"><span data-stu-id="5aaa5-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | <span data-ttu-id="5aaa5-156">暫時性</span><span class="sxs-lookup"><span data-stu-id="5aaa5-156">Transient</span></span> |
| [<span data-ttu-id="5aaa5-157">Microsoft.AspNetCore.Http.IHttpContextFactory</span><span class="sxs-lookup"><span data-stu-id="5aaa5-157">Microsoft.AspNetCore.Http.IHttpContextFactory</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.ihttpcontextfactory) | <span data-ttu-id="5aaa5-158">暫時性</span><span class="sxs-lookup"><span data-stu-id="5aaa5-158">Transient</span></span> |
| [<span data-ttu-id="5aaa5-159">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="5aaa5-159">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) | <span data-ttu-id="5aaa5-160">單一</span><span class="sxs-lookup"><span data-stu-id="5aaa5-160">Singleton</span></span> |
| [<span data-ttu-id="5aaa5-161">System.Diagnostics.DiagnosticSource</span><span class="sxs-lookup"><span data-stu-id="5aaa5-161">System.Diagnostics.DiagnosticSource</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | <span data-ttu-id="5aaa5-162">單一</span><span class="sxs-lookup"><span data-stu-id="5aaa5-162">Singleton</span></span> |
| [<span data-ttu-id="5aaa5-163">System.Diagnostics.DiagnosticListener</span><span class="sxs-lookup"><span data-stu-id="5aaa5-163">System.Diagnostics.DiagnosticListener</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | <span data-ttu-id="5aaa5-164">單一</span><span class="sxs-lookup"><span data-stu-id="5aaa5-164">Singleton</span></span> |
| [<span data-ttu-id="5aaa5-165">Microsoft.AspNetCore.Hosting.IStartupFilter</span><span class="sxs-lookup"><span data-stu-id="5aaa5-165">Microsoft.AspNetCore.Hosting.IStartupFilter</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartupfilter) | <span data-ttu-id="5aaa5-166">暫時性</span><span class="sxs-lookup"><span data-stu-id="5aaa5-166">Transient</span></span> |
| [<span data-ttu-id="5aaa5-167">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span><span class="sxs-lookup"><span data-stu-id="5aaa5-167">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.objectpool.objectpoolprovider) | <span data-ttu-id="5aaa5-168">單一</span><span class="sxs-lookup"><span data-stu-id="5aaa5-168">Singleton</span></span> |
| [<span data-ttu-id="5aaa5-169">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="5aaa5-169">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.iconfigureoptions-1) | <span data-ttu-id="5aaa5-170">暫時性</span><span class="sxs-lookup"><span data-stu-id="5aaa5-170">Transient</span></span> |
| [<span data-ttu-id="5aaa5-171">Microsoft.AspNetCore.Hosting.Server.IServer</span><span class="sxs-lookup"><span data-stu-id="5aaa5-171">Microsoft.AspNetCore.Hosting.Server.IServer</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) | <span data-ttu-id="5aaa5-172">單一</span><span class="sxs-lookup"><span data-stu-id="5aaa5-172">Singleton</span></span> |
| [<span data-ttu-id="5aaa5-173">Microsoft.AspNetCore.Hosting.IStartup</span><span class="sxs-lookup"><span data-stu-id="5aaa5-173">Microsoft.AspNetCore.Hosting.IStartup</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartup) | <span data-ttu-id="5aaa5-174">單一</span><span class="sxs-lookup"><span data-stu-id="5aaa5-174">Singleton</span></span> |
| [<span data-ttu-id="5aaa5-175">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="5aaa5-175">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | <span data-ttu-id="5aaa5-176">單一</span><span class="sxs-lookup"><span data-stu-id="5aaa5-176">Singleton</span></span> |

<span data-ttu-id="5aaa5-177">以下是範例，將其他服務加入至容器使用的擴充方法類似的數字的`AddDbContext`， `AddIdentity`，和`AddMvc`。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-177">Below is an example of how to add additional services to the container using a number of extension methods like `AddDbContext`, `AddIdentity`, and `AddMvc`.</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

<span data-ttu-id="5aaa5-178">功能與中介軟體提供的 ASP.NET MVC 中，例如依照慣例，以使用單一新增的*ServiceName*來註冊所有該功能所需之服務的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-178">The features and middleware provided by ASP.NET, such as MVC, follow a convention of using a single Add*ServiceName* extension method to register all of the services required by that feature.</span></span>

>[!TIP]
> <span data-ttu-id="5aaa5-179">您可以要求內的特定架構提供服務`Startup`方法，透過其參數清單中-請參閱[應用程式啟動](startup.md)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-179">You can request certain framework-provided services within `Startup` methods through their parameter lists - see [Application Startup](startup.md) for more details.</span></span>

## <a name="registering-your-own-services"></a><span data-ttu-id="5aaa5-180">註冊您自己的服務</span><span class="sxs-lookup"><span data-stu-id="5aaa5-180">Registering Your Own Services</span></span>

<span data-ttu-id="5aaa5-181">您可以註冊自己的應用程式服務，如下所示。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-181">You can register your own application services as follows.</span></span> <span data-ttu-id="5aaa5-182">第一個泛型型別代表將會要求從容器的類型 （通常的介面）。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-182">The first generic type represents the type (typically an interface) that will be requested from the container.</span></span> <span data-ttu-id="5aaa5-183">第二個泛型型別表示的具象型別會具現化的容器，用來滿足這類要求。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-183">The second generic type represents the concrete type that will be instantiated by the container and used to fulfill such requests.</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> <span data-ttu-id="5aaa5-184">每個`services.Add<ServiceName>`擴充方法新增 （並可能設定） 服務。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-184">Each `services.Add<ServiceName>` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="5aaa5-185">例如，`services.AddMvc()`新增 MVC 要求的服務。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-185">For example, `services.AddMvc()` adds the services MVC requires.</span></span> <span data-ttu-id="5aaa5-186">建議您遵循這個慣例，將中的擴充方法放`Microsoft.Extensions.DependencyInjection`命名空間，來封裝服務註冊的群組。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-186">It's recommended that you follow this convention, placing extension methods in the `Microsoft.Extensions.DependencyInjection` namespace, to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="5aaa5-187">`AddTransient`方法用來將抽象型別對應至具象執行個體化分開，每個物件所需的服務。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-187">The `AddTransient` method is used to map abstract types to concrete services that are instantiated separately for every object that requires it.</span></span> <span data-ttu-id="5aaa5-188">這稱為 「 服務*存留期*，和其他的存留期選項說明如下。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-188">This is known as the service's *lifetime*, and additional lifetime options are described below.</span></span> <span data-ttu-id="5aaa5-189">請務必選擇適當的存留時間為每個您註冊服務。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-189">It's important to choose an appropriate lifetime for each of the services you register.</span></span> <span data-ttu-id="5aaa5-190">應該用於每個要求的類別提供服務的新執行個體嗎？</span><span class="sxs-lookup"><span data-stu-id="5aaa5-190">Should a new instance of the service be provided to each class that requests it?</span></span> <span data-ttu-id="5aaa5-191">應該有一個執行個體用於給定的 web 要求嗎？</span><span class="sxs-lookup"><span data-stu-id="5aaa5-191">Should one instance be used throughout a given web request?</span></span> <span data-ttu-id="5aaa5-192">或應該單一執行個體使用的應用程式的存留期間嗎？</span><span class="sxs-lookup"><span data-stu-id="5aaa5-192">Or should a single instance be used for the lifetime of the application?</span></span>

<span data-ttu-id="5aaa5-193">在本文範例中，沒有顯示字元的名稱，稱為簡單控制器`CharactersController`。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-193">In the sample for this article, there's a simple controller that displays character names, called `CharactersController`.</span></span> <span data-ttu-id="5aaa5-194">其`Index`方法會顯示目前的應用程式中所儲存的字元清單，並初始化集合少數幾個字元，如果皆不存在。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-194">Its `Index` method displays the current list of characters that have been stored in the application, and initializes the collection with a handful of characters if none exist.</span></span> <span data-ttu-id="5aaa5-195">請注意，雖然此應用程式使用 Entity Framework Core 和`ApplicationDbContext`其持續性的類別，並明顯控制器中。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-195">Note that although this application uses Entity Framework Core and the `ApplicationDbContext` class for its persistence, none of that's apparent in the controller.</span></span> <span data-ttu-id="5aaa5-196">特定資料存取機制相反地，抽象介面背後`ICharacterRepository`，哪些如下所示[儲存機制模式](http://deviq.com/repository-pattern/)。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-196">Instead, the specific data access mechanism has been abstracted behind an interface, `ICharacterRepository`, which follows the [repository pattern](http://deviq.com/repository-pattern/).</span></span> <span data-ttu-id="5aaa5-197">執行個體`ICharacterRepository`已透過建構函式要求，並指派給私用欄位，然後用來依需要存取的字元。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-197">An instance of `ICharacterRepository` is requested via the constructor and assigned to a private field, which is then used to access characters as necessary.</span></span>

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

<span data-ttu-id="5aaa5-198">`ICharacterRepository`定義兩個方法，控制站需要搭配`Character`執行個體。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-198">The `ICharacterRepository` defines the two methods the controller needs to work with `Character` instances.</span></span>

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

<span data-ttu-id="5aaa5-199">具象型別，依序實作這個介面`CharacterRepository`，用來在執行階段。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-199">This interface is in turn implemented by a concrete type, `CharacterRepository`, that's used at runtime.</span></span>

> [!NOTE]
> <span data-ttu-id="5aaa5-200">搭配使用的方式 DI`CharacterRepository`類別是您可以依照您的應用程式服務，不只是在 「 儲存機制 」 或 「 資料存取類別的所有一般模型。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-200">The way DI is used with the `CharacterRepository` class is a general model you can follow for all of your application services, not just in "repositories" or data access classes.</span></span>

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

<span data-ttu-id="5aaa5-201">請注意，`CharacterRepository`要求`ApplicationDbContext`其建構函式中。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-201">Note that `CharacterRepository` requests an `ApplicationDbContext` in its constructor.</span></span> <span data-ttu-id="5aaa5-202">它不是異常相依性插入使用像這樣，與每個要求的相依性，接著要求它自己的相依性鏈結的方式。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-202">It's not unusual for dependency injection to be used in a chained fashion like this, with each requested dependency in turn requesting its own dependencies.</span></span> <span data-ttu-id="5aaa5-203">容器是負責解決所有圖形中的相依性，並傳回完整解析的服務。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-203">The container is responsible for resolving all of the dependencies in the graph and returning the fully resolved service.</span></span>

> [!NOTE]
> <span data-ttu-id="5aaa5-204">建立要求的物件，和所有物件，它需要，以及所有物件，這些需要，有時稱為*物件圖形*。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-204">Creating the requested object, and all of the objects it requires, and all of the objects those require, is sometimes referred to as an *object graph*.</span></span> <span data-ttu-id="5aaa5-205">同樣地，必須先解析的相依性集合組通常稱為*相依性樹狀結構*或*相依性圖形*。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-205">Likewise, the collective set of dependencies that must be resolved is typically referred to as a *dependency tree* or *dependency graph*.</span></span>

<span data-ttu-id="5aaa5-206">在此情況下，同時`ICharacterRepository`，進而`ApplicationDbContext`必須向 「 服務 」 容器中`ConfigureServices`中`Startup`。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-206">In this case, both `ICharacterRepository` and in turn `ApplicationDbContext` must be registered with the services container in `ConfigureServices` in `Startup`.</span></span> <span data-ttu-id="5aaa5-207">`ApplicationDbContext`設定利用擴充方法呼叫`AddDbContext<T>`。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-207">`ApplicationDbContext` is configured with the call to the extension method `AddDbContext<T>`.</span></span> <span data-ttu-id="5aaa5-208">下列程式碼會顯示註冊`CharacterRepository`型別。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-208">The following code shows the registration of the `CharacterRepository` type.</span></span>

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

<span data-ttu-id="5aaa5-209">Entity Framework 內容應該加入至服務容器使用`Scoped`存留期。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-209">Entity Framework contexts should be added to the services container using the `Scoped` lifetime.</span></span> <span data-ttu-id="5aaa5-210">這是處理自動如果您使用的 helper 方法如上所示。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-210">This is taken care of automatically if you use the helper methods as shown above.</span></span> <span data-ttu-id="5aaa5-211">儲存機制，可讓您使用 Entity Framework 應該使用相同的存留期。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-211">Repositories that will make use of Entity Framework should use the same lifetime.</span></span>

>[!WARNING]
> <span data-ttu-id="5aaa5-212">正在解析主要危險来謹慎`Scoped`服務從單一子句。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-212">The main danger to be wary of is resolving a `Scoped` service from a singleton.</span></span> <span data-ttu-id="5aaa5-213">很可能在此情況下處理後續要求時，服務會造成不正確的狀態。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-213">It's likely in such a case that the service will have incorrect state when processing subsequent requests.</span></span>

<span data-ttu-id="5aaa5-214">具有相依性服務應該註冊這些伺服器在容器中。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-214">Services that have dependencies should register them in the container.</span></span> <span data-ttu-id="5aaa5-215">如果服務的建構函式可如需要基本型別`string`，這可以藉由插入[組態](xref:fundamentals/configuration/index)和[選項模式](xref:fundamentals/configuration/options)。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-215">If a service's constructor requires a primitive, such as a `string`, this can be injected by using [configuration](xref:fundamentals/configuration/index) and the [options pattern](xref:fundamentals/configuration/options).</span></span>

## <a name="service-lifetimes-and-registration-options"></a><span data-ttu-id="5aaa5-216">服務的存留期和註冊選項</span><span class="sxs-lookup"><span data-stu-id="5aaa5-216">Service Lifetimes and Registration Options</span></span>

<span data-ttu-id="5aaa5-217">ASP.NET 服務可以使用下列的存留時間設定：</span><span class="sxs-lookup"><span data-stu-id="5aaa5-217">ASP.NET services can be configured with the following lifetimes:</span></span>

<span data-ttu-id="5aaa5-218">**Transient**</span><span class="sxs-lookup"><span data-stu-id="5aaa5-218">**Transient**</span></span>

<span data-ttu-id="5aaa5-219">每次在收到要求時，會建立暫時性的存留時間服務。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-219">Transient lifetime services are created each time they're requested.</span></span> <span data-ttu-id="5aaa5-220">此存留時間最適合用於輕量型、 無狀態服務。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-220">This lifetime works best for lightweight, stateless services.</span></span>

<span data-ttu-id="5aaa5-221">**範圍**</span><span class="sxs-lookup"><span data-stu-id="5aaa5-221">**Scoped**</span></span>

<span data-ttu-id="5aaa5-222">已設定領域的存留時間服務會建立一次，每個要求。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-222">Scoped lifetime services are created once per request.</span></span>

<span data-ttu-id="5aaa5-223">**Singleton**</span><span class="sxs-lookup"><span data-stu-id="5aaa5-223">**Singleton**</span></span>

<span data-ttu-id="5aaa5-224">單一存留時間服務會建立第一次在收到要求 (或當`ConfigureServices`如果您指定執行個體執行) 然後每個後續的要求將會使用相同的執行個體。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-224">Singleton lifetime services are created the first time they're requested (or when `ConfigureServices` is run if you specify an instance there) and then every subsequent request will use the same instance.</span></span> <span data-ttu-id="5aaa5-225">如果您的應用程式需要單一行為，允許 「 服務 」 容器來管理服務的存留期建議而不是實作單一設計模式，並管理您自己的類別中物件的存留期。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-225">If your application requires singleton behavior, allowing the services container to manage the service's lifetime is recommended instead of implementing the singleton design pattern and managing your object's lifetime in the class yourself.</span></span>

<span data-ttu-id="5aaa5-226">可以與幾種容器登錄服務。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-226">Services can be registered with the container in several ways.</span></span> <span data-ttu-id="5aaa5-227">我們已經看過如何藉由指定要使用的具象類型，含指定之類型註冊服務實作。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-227">We have already seen how to register a service implementation with a given type by specifying the concrete type to use.</span></span> <span data-ttu-id="5aaa5-228">此外，此處理站可以指定，然後將用來建立隨選執行個體。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-228">In addition, a factory can be specified, which will then be used to create the instance on demand.</span></span> <span data-ttu-id="5aaa5-229">第三種方法是直接指定類型的執行個體使用，在其中案例容器將永遠不會嘗試建立執行個體 （也不會將處置的執行個體）。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-229">The third approach is to directly specify the instance of the type to use, in which case the container will never attempt to create an instance (nor will it dispose of the instance).</span></span>

<span data-ttu-id="5aaa5-230">若要示範這些存留期和註冊的選項之間的差異，請考慮一個簡單的介面，表示為一或多個工作*作業*具有唯一的識別碼， `OperationId`。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-230">To demonstrate the difference between these lifetime and registration options, consider a simple interface that represents one or more tasks as an *operation* with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="5aaa5-231">根據我們設定此服務的存留期，容器會提供相同或不同的執行個體的服務要求的類別。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-231">Depending on how we configure the lifetime for this service, the container will provide either the same or different instances of the service to the requesting class.</span></span> <span data-ttu-id="5aaa5-232">若要清除所要求的存留期，我們將建立 lifetime 選項每一種類型：</span><span class="sxs-lookup"><span data-stu-id="5aaa5-232">To make it clear which lifetime is being requested, we will create one type per lifetime option:</span></span>

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

<span data-ttu-id="5aaa5-233">我們會使用單一類別，這些介面實作`Operation`，接受`Guid`在其建構函式或使用新`Guid`若未提供。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-233">We implement these interfaces using a single class, `Operation`, that accepts a `Guid` in its constructor, or uses a new `Guid` if none is provided.</span></span>

<span data-ttu-id="5aaa5-234">接下來，在`ConfigureServices`，每個類型加入至的容器，根據其具名的存留期：</span><span class="sxs-lookup"><span data-stu-id="5aaa5-234">Next, in `ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

<span data-ttu-id="5aaa5-235">請注意，`IOperationSingletonInstance`服務所使用的特定執行個體的已知識別碼`Guid.Empty`會清除此型別時 （其 Guid 會是全部為零） 的使用中。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-235">Note that the `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty` so it will be clear when this type is in use (its Guid will be all zeroes).</span></span> <span data-ttu-id="5aaa5-236">我們也已註冊`OperationService`相依於每個其他`Operation`類型，因此它會要求內清除這項服務是否即將控制器或新方案，每個作業類型相同的執行個體。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-236">We have also registered an `OperationService` that depends on each of the other `Operation` types, so that it will be clear within a request whether this service is getting the same instance as the controller, or a new one, for each operation type.</span></span> <span data-ttu-id="5aaa5-237">此服務的作用是公開其相依性為屬性，以便在檢視中顯示。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-237">All this service does is expose its dependencies as properties, so they can be displayed in the view.</span></span>

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

<span data-ttu-id="5aaa5-238">若要示範物件存留期間內以及個別應用程式的個別要求之間，此範例包含`OperationsController`，每種類型的要求`IOperation`型別，以及`OperationService`。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-238">To demonstrate the object lifetimes within and between separate individual requests to the application, the sample includes an `OperationsController` that requests each kind of `IOperation` type as well as an `OperationService`.</span></span> <span data-ttu-id="5aaa5-239">`Index`動作還會顯示所有的控制站和服務的`OperationId`值。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-239">The `Index` action then displays all of the controller's and service's `OperationId` values.</span></span>

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

<span data-ttu-id="5aaa5-240">現在兩個不同的要求是對此控制器的動作：</span><span class="sxs-lookup"><span data-stu-id="5aaa5-240">Now two separate requests are made to this controller action:</span></span>

![[作業] 檢視顯示暫時性、 Scoped、 單一值，和執行個體控制站和作業的第一個要求的服務作業的作業識別碼值 (GUID) 的 Microsoft Edge 中執行的相依性插入範例 web 應用程式。](dependency-injection/_static/lifetimes_request1.png)

![作業檢視顯示第二個要求的作業識別碼值。](dependency-injection/_static/lifetimes_request2.png)

<span data-ttu-id="5aaa5-243">觀察哪些`OperationId`內要求，並在要求之間的值會不同。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-243">Observe which of the `OperationId` values vary within a request, and between requests.</span></span>

* <span data-ttu-id="5aaa5-244">*暫時性*物件總是不同; 的新執行個體提供給每個控制站與每個服務。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-244">*Transient* objects are always different; a new instance is provided to every controller and every service.</span></span>

* <span data-ttu-id="5aaa5-245">*範圍*物件是相同的要求，但不同跨不同的要求內</span><span class="sxs-lookup"><span data-stu-id="5aaa5-245">*Scoped* objects are the same within a request, but different across different requests</span></span>

* <span data-ttu-id="5aaa5-246">*單一*都適用於每個物件與每個要求相同的物件 (不論是否會提供執行個體中`ConfigureServices`)</span><span class="sxs-lookup"><span data-stu-id="5aaa5-246">*Singleton* objects are the same for every object and every request (regardless of whether an instance is provided in `ConfigureServices`)</span></span>

## <a name="request-services"></a><span data-ttu-id="5aaa5-247">要求的服務</span><span class="sxs-lookup"><span data-stu-id="5aaa5-247">Request Services</span></span>

<span data-ttu-id="5aaa5-248">在 ASP.NET 中可用的服務要求從`HttpContext`公開會透過`RequestServices`集合。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-248">The services available within an ASP.NET request from `HttpContext` are exposed through the `RequestServices` collection.</span></span>

![指出要求服務取得或設定提供要求的服務容器的存取權的 IServiceProvider HttpContext 要求服務 Intellisense 的內容對話方塊。](dependency-injection/_static/request-services.png)

<span data-ttu-id="5aaa5-250">要求服務代表的服務，設定要求做為您的應用程式的一部分。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-250">Request Services represent the services you configure and request as part of your application.</span></span> <span data-ttu-id="5aaa5-251">當您的物件指定相依性時，這些由中找到的類型來滿足`RequestServices`，而非`ApplicationServices`。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-251">When your objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="5aaa5-252">一般而言，您不應該使用這些屬性直接偏好改為要求的型別必須透過您的類別建構函式，您的類別，並讓架構插入這些相依性。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-252">Generally, you shouldn't use these properties directly, preferring instead to request the types your classes you require via your class's constructor, and letting the framework inject these dependencies.</span></span> <span data-ttu-id="5aaa5-253">這會產生較易於測試的類別 (請參閱[測試](../testing/index.md)) 且更鬆散偶合。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-253">This yields classes that are easier to test (see [Testing](../testing/index.md)) and are more loosely coupled.</span></span>

> [!NOTE]
> <span data-ttu-id="5aaa5-254">偏好做為建構函式參數，存取要求的相依性`RequestServices`集合。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-254">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="designing-your-services-for-dependency-injection"></a><span data-ttu-id="5aaa5-255">設計您的服務相依性插入</span><span class="sxs-lookup"><span data-stu-id="5aaa5-255">Designing Your Services For Dependency Injection</span></span>

<span data-ttu-id="5aaa5-256">您應該設計服務，以取得其共同作業者使用相依性插入。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-256">You should design your services to use dependency injection to get their collaborators.</span></span> <span data-ttu-id="5aaa5-257">這表示避免使用可設定狀態的靜態方法呼叫 (這會導致程式碼的氣味稱為[靜態黏貼](http://deviq.com/static-cling/)) 和您的服務內的相依類別的直接具現化。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-257">This means avoiding the use of stateful static method calls (which result in a code smell known as [static cling](http://deviq.com/static-cling/)) and the direct instantiation of dependent classes within your services.</span></span> <span data-ttu-id="5aaa5-258">它有助於記住片語，[新是黏附](https://ardalis.com/new-is-glue)，當您選擇是否要具現化型別，或要求透過相依性插入。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-258">It may help to remember the phrase, [New is Glue](https://ardalis.com/new-is-glue), when choosing whether to instantiate a type or to request it via dependency injection.</span></span> <span data-ttu-id="5aaa5-259">依照[實心原則的物件導向設計](http://deviq.com/solid/)，類別自然會傾向小型、 構造良好且輕鬆地測試。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-259">By following the [SOLID Principles of Object Oriented Design](http://deviq.com/solid/), your classes will naturally tend to be small, well-factored, and easily tested.</span></span>

<span data-ttu-id="5aaa5-260">如果找到您的類別通常有方式媵檔晻太多的相依性？</span><span class="sxs-lookup"><span data-stu-id="5aaa5-260">What if you find that your classes tend to have way too many dependencies being injected?</span></span> <span data-ttu-id="5aaa5-261">這通常是一個符號，嘗試太多，您的類別，並可能違反 SRP-[單一責任原則](http://deviq.com/single-responsibility-principle/)。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-261">This is generally a sign that your class is trying to do too much, and is probably violating SRP - the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/).</span></span> <span data-ttu-id="5aaa5-262">如果您能夠重構類別將某些其負責的部分移到新的類別，請參閱。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-262">See if you can refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="5aaa5-263">請注意，您`Controller`類別應該專注於 UI 考量，讓商務規則和資料存取實作詳細資料保存在適用於這些類別[將重點分開](http://deviq.com/separation-of-concerns/)。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-263">Keep in mind that your `Controller` classes should be focused on UI concerns, so business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](http://deviq.com/separation-of-concerns/).</span></span>

<span data-ttu-id="5aaa5-264">資料存取與具體來說，您可以將插入`DbContext`到您的控制站 (假設您已加入中服務容器中的 EF `ConfigureServices`)。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-264">With regards to data access specifically, you can inject the `DbContext` into your controllers (assuming you've added EF to the services container in `ConfigureServices`).</span></span> <span data-ttu-id="5aaa5-265">有些開發人員偏好使用至資料庫的儲存機制介面而不是插入`DbContext`直接。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-265">Some developers prefer to use a repository interface to the database rather than injecting the `DbContext` directly.</span></span> <span data-ttu-id="5aaa5-266">使用介面來封裝資料在一個地方存取邏輯可以減少您必須變更您的資料庫時變更的數位數。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-266">Using an interface to encapsulate the data access logic in one place can minimize how many places you will have to change when your database changes.</span></span>

### <a name="disposing-of-services"></a><span data-ttu-id="5aaa5-267">處置服務</span><span class="sxs-lookup"><span data-stu-id="5aaa5-267">Disposing of services</span></span>

<span data-ttu-id="5aaa5-268">容器會呼叫`Dispose`如`IDisposable`它所建立的類型。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-268">The container will call `Dispose` for `IDisposable` types it creates.</span></span> <span data-ttu-id="5aaa5-269">不過，如果您將執行個體加入容器自行，它將不會處置。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-269">However, if you add an instance to the container yourself, it will not be disposed.</span></span>

<span data-ttu-id="5aaa5-270">範例：</span><span class="sxs-lookup"><span data-stu-id="5aaa5-270">Example:</span></span>

```csharp
// Services implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}


public void ConfigureServices(IServiceCollection services)
{
    // container will create the instance(s) of these types and will dispose them
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // container didn't create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> <span data-ttu-id="5aaa5-271">在 1.0 版中，容器會呼叫 dispose 上*所有*`IDisposable`物件，包括它未建立。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-271">In version 1.0, the container called dispose on *all* `IDisposable` objects, including those it didn't create.</span></span>

## <a name="replacing-the-default-services-container"></a><span data-ttu-id="5aaa5-272">取代預設服務容器</span><span class="sxs-lookup"><span data-stu-id="5aaa5-272">Replacing the default services container</span></span>

<span data-ttu-id="5aaa5-273">內建的服務容器是要做的架構和建置在其上的大部分取用者應用程式的基本需求。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-273">The built-in services container is meant to serve the basic needs of the framework and most consumer applications built on it.</span></span> <span data-ttu-id="5aaa5-274">不過，開發人員可以使用其偏好的容器取代內建的容器。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-274">However, developers can replace the built-in container with their preferred container.</span></span> <span data-ttu-id="5aaa5-275">`ConfigureServices`方法通常會傳回`void`，但是，如果其簽章變更為傳回`IServiceProvider`，可設定與傳回不同的容器。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-275">The `ConfigureServices` method typically returns `void`, but if its signature is changed to return `IServiceProvider`, a different container can be configured and returned.</span></span> <span data-ttu-id="5aaa5-276">沒有適用於.NET 的許多 IOC 容器。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-276">There are many IOC containers available for .NET.</span></span> <span data-ttu-id="5aaa5-277">在此範例中， [Autofac](https://autofac.org/)使用套件。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-277">In this example, the [Autofac](https://autofac.org/) package is used.</span></span>

<span data-ttu-id="5aaa5-278">首先，安裝適當的容器封裝：</span><span class="sxs-lookup"><span data-stu-id="5aaa5-278">First, install the appropriate container package(s):</span></span>

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

<span data-ttu-id="5aaa5-279">接下來，設定中的容器`ConfigureServices`並傳回`IServiceProvider`:</span><span class="sxs-lookup"><span data-stu-id="5aaa5-279">Next, configure the container in `ConfigureServices` and return an `IServiceProvider`:</span></span>

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    // Add other framework services

    // Add Autofac
    var containerBuilder = new ContainerBuilder();
    containerBuilder.RegisterModule<DefaultModule>();
    containerBuilder.Populate(services);
    var container = containerBuilder.Build();
    return new AutofacServiceProvider(container);
}
```

> [!NOTE]
> <span data-ttu-id="5aaa5-280">當使用協力廠商 DI 容器，您必須變更`ConfigureServices`，使它傳回`IServiceProvider`而不是`void`。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-280">When using a third-party DI container, you must change `ConfigureServices` so that it returns `IServiceProvider` instead of `void`.</span></span>

<span data-ttu-id="5aaa5-281">最後，像在平常一樣設定 Autofac `DefaultModule`:</span><span class="sxs-lookup"><span data-stu-id="5aaa5-281">Finally, configure Autofac as normal in `DefaultModule`:</span></span>

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

<span data-ttu-id="5aaa5-282">在執行階段，Autofac 將用於解析型別，以及將相依性。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-282">At runtime, Autofac will be used to resolve types and inject dependencies.</span></span> <span data-ttu-id="5aaa5-283">[深入了解使用 Autofac 和 ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html)。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-283">[Learn more about using Autofac and ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="5aaa5-284">執行緒安全</span><span class="sxs-lookup"><span data-stu-id="5aaa5-284">Thread safety</span></span>

<span data-ttu-id="5aaa5-285">單一服務需要具備執行緒安全。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-285">Singleton services need to be thread safe.</span></span> <span data-ttu-id="5aaa5-286">如果單一服務有暫時性服務相依性，暫時性服務可能也需要安全視它由單一執行緒。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-286">If a singleton service has a dependency on a transient service, the transient service may also need to be thread safe depending how it’s used by the singleton.</span></span>

## <a name="recommendations"></a><span data-ttu-id="5aaa5-287">建議</span><span class="sxs-lookup"><span data-stu-id="5aaa5-287">Recommendations</span></span>

<span data-ttu-id="5aaa5-288">當使用相依性插入，請記住下列建議：</span><span class="sxs-lookup"><span data-stu-id="5aaa5-288">When working with dependency injection, keep the following recommendations in mind:</span></span>

* <span data-ttu-id="5aaa5-289">DI 適用於具有複雜的相依性物件。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-289">DI is for objects that have complex dependencies.</span></span> <span data-ttu-id="5aaa5-290">控制站、 服務、 配接器，以及儲存機制是所有的範例 DI 可能加入的物件。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-290">Controllers, services, adapters, and repositories are all examples of objects that might be added to DI.</span></span>

* <span data-ttu-id="5aaa5-291">避免直接在 DI 中儲存資料和設定。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-291">Avoid storing data and configuration directly in DI.</span></span> <span data-ttu-id="5aaa5-292">例如，使用者的購物車通常不應該新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-292">For example, a user's shopping cart shouldn't typically be added to the services container.</span></span> <span data-ttu-id="5aaa5-293">組態應該使用[選項模式](xref:fundamentals/configuration/options)。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-293">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="5aaa5-294">同樣地，避免只存在於以允許存取其他物件的 「 資料持有者 」 物件。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-294">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="5aaa5-295">最好是要求透過 DI，盡可能所需的實際項目。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-295">It's better to request the actual item needed via DI, if possible.</span></span>

* <span data-ttu-id="5aaa5-296">避免靜態存取服務。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-296">Avoid static access to services.</span></span>

* <span data-ttu-id="5aaa5-297">避免在應用程式程式碼中的服務位置。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-297">Avoid service location in your application code.</span></span>

* <span data-ttu-id="5aaa5-298">避免靜態存取`HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-298">Avoid static access to `HttpContext`.</span></span>

> [!NOTE]
> <span data-ttu-id="5aaa5-299">建議的所有集合，例如您可能會遇到的情況下忽略其中一個必要。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-299">Like all sets of recommendations, you may encounter situations where ignoring one is required.</span></span> <span data-ttu-id="5aaa5-300">我們發現例外狀況很少見-主要是非常特殊的情況下，架構本身內。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-300">We have found exceptions to be rare -- mostly very special cases within the framework itself.</span></span>

<span data-ttu-id="5aaa5-301">請記住，相依性插入是*替代*靜態/全域物件存取模式。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-301">Remember, dependency injection is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="5aaa5-302">您將無法實現 DI 的優點，如果您混具有靜態物件的存取權。</span><span class="sxs-lookup"><span data-stu-id="5aaa5-302">You won't be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5aaa5-303">其他資源</span><span class="sxs-lookup"><span data-stu-id="5aaa5-303">Additional Resources</span></span>

* [<span data-ttu-id="5aaa5-304">應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="5aaa5-304">Application Startup</span></span>](startup.md)

* [<span data-ttu-id="5aaa5-305">測試</span><span class="sxs-lookup"><span data-stu-id="5aaa5-305">Testing</span></span>](../testing/index.md)

* [<span data-ttu-id="5aaa5-306">相依性插入 (MSDN) 與 ASP.NET Core 中撰寫全新的程式碼</span><span class="sxs-lookup"><span data-stu-id="5aaa5-306">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)

* [<span data-ttu-id="5aaa5-307">容器管理應用程式的設計，Prelude： 位置？ 容器屬於</span><span class="sxs-lookup"><span data-stu-id="5aaa5-307">Container-Managed Application Design, Prelude: Where does the Container Belong?</span></span>](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)

* [<span data-ttu-id="5aaa5-308">明確的相依性原則</span><span class="sxs-lookup"><span data-stu-id="5aaa5-308">Explicit Dependencies Principle</span></span>](http://deviq.com/explicit-dependencies-principle/)

* <span data-ttu-id="5aaa5-309">[逆轉控制容器和相依性插入模式](https://www.martinfowler.com/articles/injection.html)(Fowler)</span><span class="sxs-lookup"><span data-stu-id="5aaa5-309">[Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html) (Fowler)</span></span>
