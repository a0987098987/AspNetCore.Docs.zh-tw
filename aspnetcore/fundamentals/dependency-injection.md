---
title: ".NET Core 中的相依性插入"
author: ardalis
description: "了解 ASP.NET Core 如何實作相依性插入以及如何使用它。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/dependency-injection
ms.openlocfilehash: 43c937ff9631be3edc1f95b3689650e4574abfbd
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/01/2018
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="7d5c0-103">.NET Core 中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="7d5c0-103">Dependency injection in ASP.NET Core</span></span>

<a name="fundamentals-dependency-injection"></a>

<span data-ttu-id="7d5c0-104">作者：[Steve Smith](https://ardalis.com/) 和 [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="7d5c0-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="7d5c0-105">ASP.NET Core 的設計完全是為了支援並利用相依性插入。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-105">ASP.NET Core is designed from the ground up to support and leverage dependency injection.</span></span> <span data-ttu-id="7d5c0-106">ASP.NET Core 應用程式可以藉由讓內建的架構服務插入至 Startup 類別中的方法來利用它們，也可以設定應用程式服務以便插入。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-106">ASP.NET Core applications can leverage built-in framework services by having them injected into methods in the Startup class, and application services can be configured for injection as well.</span></span> <span data-ttu-id="7d5c0-107">ASP.NET Core 所提供的預設服務容器會提供最少的功能集，並不旨在取代其他容器。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-107">The default services container provided by ASP.NET Core provides a minimal feature set and isn't intended to replace other containers.</span></span>

<span data-ttu-id="7d5c0-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7d5c0-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="7d5c0-109">相依性插入是什麼？</span><span class="sxs-lookup"><span data-stu-id="7d5c0-109">What is dependency injection?</span></span>

<span data-ttu-id="7d5c0-110">相依性插入 (DI) 是為了達到物件和共同作業者之間鬆散結合 (也就是相依性) 的技術。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-110">Dependency injection (DI) is a technique for achieving loose coupling between objects and their collaborators, or dependencies.</span></span> <span data-ttu-id="7d5c0-111">類別要執行其動作所需的物件，會以某些方式提供給類別，而不是直接具現化共同作業者，或使用靜態參考。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-111">Rather than directly instantiating collaborators, or using static references, the objects a class needs in order to perform its actions are provided to the class in some fashion.</span></span> <span data-ttu-id="7d5c0-112">大多數情況下，類別會透過其建構函式宣告它們的相依性，讓它們能遵循[明確相依性準則](http://deviq.com/explicit-dependencies-principle/)。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-112">Most often, classes will declare their dependencies via their constructor, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span> <span data-ttu-id="7d5c0-113">這種方法稱為「建構函式插入」。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-113">This approach is known as "constructor injection".</span></span>

<span data-ttu-id="7d5c0-114">當類別專為 DI 設計時，它們會較為鬆散結合，因為它們對於其共同作業者沒有硬式編碼的直接相依性。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-114">When classes are designed with DI in mind, they're more loosely coupled because they don't have direct, hard-coded dependencies on their collaborators.</span></span> <span data-ttu-id="7d5c0-115">這遵循了[相依性反向準則](http://deviq.com/dependency-inversion-principle/)，其指出「高層級模組不應該相依於低層級模組；兩者都應該相依於抽象概念」。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-115">This follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), which states that *"high level modules shouldn't depend on low level modules; both should depend on abstractions."*</span></span> <span data-ttu-id="7d5c0-116">類別會要求在建構類別時提供給它們的抽象概念 (通常是 `interfaces`)，而不是參考特定實作。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-116">Instead of referencing specific implementations, classes request abstractions (typically `interfaces`) which are provided to them when the class is constructed.</span></span> <span data-ttu-id="7d5c0-117">將相依性擷取成介面，並提供這些介面的實作作為參數，也是[策略設計模式](http://deviq.com/strategy-design-pattern/)的範例。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-117">Extracting dependencies into interfaces and providing implementations of these interfaces as parameters is also an example of the [Strategy design pattern](http://deviq.com/strategy-design-pattern/).</span></span>

<span data-ttu-id="7d5c0-118">當系統設計為使用 DI，並且有許多類別透過其建構函式 (或屬性) 要求相依性時，讓類別專門用來建立這些類別及其相關聯相依性會很有助益。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-118">When a system is designed to use DI, with many classes requesting their dependencies via their constructor (or properties), it's helpful to have a class dedicated to creating these classes with their associated dependencies.</span></span> <span data-ttu-id="7d5c0-119">這些類別統稱為「容器」，或是更具體來說，稱為[逆轉控制 (IoC)](http://deviq.com/inversion-of-control/) 容器或相依性插入 (DI) 容器。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-119">These classes are referred to as *containers*, or more specifically, [Inversion of Control (IoC)](http://deviq.com/inversion-of-control/) containers or Dependency Injection (DI) containers.</span></span> <span data-ttu-id="7d5c0-120">容器基本上是負責提供向它要求的類別之執行個體的 actory。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-120">A container is essentially a factory that's responsible for providing instances of types that are requested from it.</span></span> <span data-ttu-id="7d5c0-121">如果指定的類型已經宣告它有相依性，且容器已設定為提供相依性類型，則它會在建立所要求的執行個體時建立相依性。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-121">If a given type has declared that it has dependencies, and the container has been configured to provide the dependency types, it will create the dependencies as part of creating the requested instance.</span></span> <span data-ttu-id="7d5c0-122">如此一來，可以提供類別複雜的相依性圖形，而不需要任何硬式編碼的物件建構。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-122">In this way, complex dependency graphs can be provided to classes without the need for any hard-coded object construction.</span></span> <span data-ttu-id="7d5c0-123">除了建立具有相依性的物件之外，容器通常也會管理應用程式中的物件存留期。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-123">In addition to creating objects with their dependencies, containers typically manage object lifetimes within the application.</span></span>

<span data-ttu-id="7d5c0-124">ASP.NET Core 包含簡單的內建容器 (由 `IServiceProvider` 介面代表)，它預設會支援建構函式插入，而 ASP.NET 則透過 DI 提供特定服務。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-124">ASP.NET Core includes a simple built-in container (represented by the `IServiceProvider` interface) that supports constructor injection by default, and ASP.NET makes certain services available through DI.</span></span> <span data-ttu-id="7d5c0-125">ASP.NET 的容器將其管理的類型稱為「服務」。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-125">ASP.NET's container refers to the types it manages as *services*.</span></span> <span data-ttu-id="7d5c0-126">本文的其餘部分中，「服務」將會指 ASP.NET Core IoC 容器所管理的類型。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-126">Throughout the rest of this article, *services* will refer to types that are managed by ASP.NET Core's IoC container.</span></span> <span data-ttu-id="7d5c0-127">您在應用程式 `Startup` 類別的 `ConfigureServices` 方法中設定內建容器的服務。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-127">You configure the built-in container's services in the `ConfigureServices` method in your application's `Startup` class.</span></span>

> [!NOTE]
> <span data-ttu-id="7d5c0-128">Martin Fowler 撰寫了關於 [Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html) (逆轉控制容器和相依性插入模式) 的詳盡文章。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-128">Martin Fowler has written an extensive article on [Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html).</span></span> <span data-ttu-id="7d5c0-129">Microsoft Patterns and Practices 也對[相依性插入](https://msdn.microsoft.com/library/hh323705.aspx)有很棒的描述。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-129">Microsoft Patterns and Practices also has a great description of [Dependency Injection](https://msdn.microsoft.com/library/hh323705.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="7d5c0-130">本文介紹相依性插入適用於所有 ASP.NET 應用程式的方面。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-130">This article covers Dependency Injection as it applies to all ASP.NET applications.</span></span> <span data-ttu-id="7d5c0-131">MVC 控制器內的相依性插入，於[相依性插入和控制器](../mvc/controllers/dependency-injection.md)中介紹。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-131">Dependency Injection within MVC controllers is covered in [Dependency Injection and Controllers](../mvc/controllers/dependency-injection.md).</span></span>

### <a name="constructor-injection-behavior"></a><span data-ttu-id="7d5c0-132">建構函式插入行為</span><span class="sxs-lookup"><span data-stu-id="7d5c0-132">Constructor injection behavior</span></span>

<span data-ttu-id="7d5c0-133">建構函式插入需要討論中的建構函式為「公用」。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-133">Constructor injection requires that the constructor in question be *public*.</span></span> <span data-ttu-id="7d5c0-134">否則，您的應用程式將會擲回 `InvalidOperationException`：</span><span class="sxs-lookup"><span data-stu-id="7d5c0-134">Otherwise, your app will throw an `InvalidOperationException`:</span></span>

> <span data-ttu-id="7d5c0-135">找不到類型 'YourType' 的合適建構函式。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-135">A suitable constructor for type 'YourType' couldn't be located.</span></span> <span data-ttu-id="7d5c0-136">請確定類型是具象類型，且已針對公用建構函式的所有參數註冊服務。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-136">Ensure the type is concrete and services are registered for all parameters of a public constructor.</span></span>


<span data-ttu-id="7d5c0-137">建構函式插入需要僅存在一個適用的建構函式。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-137">Constructor injection requires that only one applicable constructor exist.</span></span> <span data-ttu-id="7d5c0-138">支援建構函式多載，但只能有一個多載存在，其引數可以藉由相依性插入而完成。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-138">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="7d5c0-139">如果有多個存在，您的應用程式將會擲回 `InvalidOperationException`：</span><span class="sxs-lookup"><span data-stu-id="7d5c0-139">If more than one exists, your app will throw an `InvalidOperationException`:</span></span>

> <span data-ttu-id="7d5c0-140">類型 'YourType' 中已找到接受所有指定引數類型的多個建構函式。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-140">Multiple constructors accepting all given argument types have been found in type 'YourType'.</span></span> <span data-ttu-id="7d5c0-141">只能有一個適用的建構函式。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-141">There should only be one applicable constructor.</span></span>

<span data-ttu-id="7d5c0-142">建構函式可以接受不是由相依性插入提供的引數，但這些引數必須支援預設值。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-142">Constructors can accept arguments that are not provided by dependency injection, but these must support default values.</span></span> <span data-ttu-id="7d5c0-143">例如: </span><span class="sxs-lookup"><span data-stu-id="7d5c0-143">For example:</span></span>

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

## <a name="using-framework-provided-services"></a><span data-ttu-id="7d5c0-144">使用架構提供的服務</span><span class="sxs-lookup"><span data-stu-id="7d5c0-144">Using framework-provided services</span></span>

<span data-ttu-id="7d5c0-145">`Startup` 類別中的 `ConfigureServices` 方法負責定義應用程式將使用的服務，包括像是 Entity Framework Core 與 ASP.NET Core MVC 等平台功能。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-145">The `ConfigureServices` method in the `Startup` class is responsible for defining the services the application will use, including platform features like Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="7d5c0-146">一開始，提供給 `ConfigureServices` 的 `IServiceCollection` 具有下列已定義的服務 (取決於[主機的設定方式](xref:fundamentals/hosting))：</span><span class="sxs-lookup"><span data-stu-id="7d5c0-146">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/hosting)):</span></span>

| <span data-ttu-id="7d5c0-147">服務類型</span><span class="sxs-lookup"><span data-stu-id="7d5c0-147">Service Type</span></span> | <span data-ttu-id="7d5c0-148">存留期</span><span class="sxs-lookup"><span data-stu-id="7d5c0-148">Lifetime</span></span> |
| ----- | ------- |
| [<span data-ttu-id="7d5c0-149">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="7d5c0-149">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) | <span data-ttu-id="7d5c0-150">單一</span><span class="sxs-lookup"><span data-stu-id="7d5c0-150">Singleton</span></span> |
| [<span data-ttu-id="7d5c0-151">Microsoft.Extensions.Logging.ILoggerFactory</span><span class="sxs-lookup"><span data-stu-id="7d5c0-151">Microsoft.Extensions.Logging.ILoggerFactory</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.iloggerfactory) | <span data-ttu-id="7d5c0-152">單一</span><span class="sxs-lookup"><span data-stu-id="7d5c0-152">Singleton</span></span> |
| [<span data-ttu-id="7d5c0-153">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="7d5c0-153">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) | <span data-ttu-id="7d5c0-154">單一</span><span class="sxs-lookup"><span data-stu-id="7d5c0-154">Singleton</span></span> |
| [<span data-ttu-id="7d5c0-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span><span class="sxs-lookup"><span data-stu-id="7d5c0-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | <span data-ttu-id="7d5c0-156">暫時性</span><span class="sxs-lookup"><span data-stu-id="7d5c0-156">Transient</span></span> |
| [<span data-ttu-id="7d5c0-157">Microsoft.AspNetCore.Http.IHttpContextFactory</span><span class="sxs-lookup"><span data-stu-id="7d5c0-157">Microsoft.AspNetCore.Http.IHttpContextFactory</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.ihttpcontextfactory) | <span data-ttu-id="7d5c0-158">暫時性</span><span class="sxs-lookup"><span data-stu-id="7d5c0-158">Transient</span></span> |
| [<span data-ttu-id="7d5c0-159">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="7d5c0-159">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) | <span data-ttu-id="7d5c0-160">單一</span><span class="sxs-lookup"><span data-stu-id="7d5c0-160">Singleton</span></span> |
| [<span data-ttu-id="7d5c0-161">System.Diagnostics.DiagnosticSource</span><span class="sxs-lookup"><span data-stu-id="7d5c0-161">System.Diagnostics.DiagnosticSource</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | <span data-ttu-id="7d5c0-162">單一</span><span class="sxs-lookup"><span data-stu-id="7d5c0-162">Singleton</span></span> |
| [<span data-ttu-id="7d5c0-163">System.Diagnostics.DiagnosticListener</span><span class="sxs-lookup"><span data-stu-id="7d5c0-163">System.Diagnostics.DiagnosticListener</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | <span data-ttu-id="7d5c0-164">單一</span><span class="sxs-lookup"><span data-stu-id="7d5c0-164">Singleton</span></span> |
| [<span data-ttu-id="7d5c0-165">Microsoft.AspNetCore.Hosting.IStartupFilter</span><span class="sxs-lookup"><span data-stu-id="7d5c0-165">Microsoft.AspNetCore.Hosting.IStartupFilter</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartupfilter) | <span data-ttu-id="7d5c0-166">暫時性</span><span class="sxs-lookup"><span data-stu-id="7d5c0-166">Transient</span></span> |
| [<span data-ttu-id="7d5c0-167">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span><span class="sxs-lookup"><span data-stu-id="7d5c0-167">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.objectpool.objectpoolprovider) | <span data-ttu-id="7d5c0-168">單一</span><span class="sxs-lookup"><span data-stu-id="7d5c0-168">Singleton</span></span> |
| [<span data-ttu-id="7d5c0-169">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="7d5c0-169">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.iconfigureoptions-1) | <span data-ttu-id="7d5c0-170">暫時性</span><span class="sxs-lookup"><span data-stu-id="7d5c0-170">Transient</span></span> |
| [<span data-ttu-id="7d5c0-171">Microsoft.AspNetCore.Hosting.Server.IServer</span><span class="sxs-lookup"><span data-stu-id="7d5c0-171">Microsoft.AspNetCore.Hosting.Server.IServer</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) | <span data-ttu-id="7d5c0-172">單一</span><span class="sxs-lookup"><span data-stu-id="7d5c0-172">Singleton</span></span> |
| [<span data-ttu-id="7d5c0-173">Microsoft.AspNetCore.Hosting.IStartup</span><span class="sxs-lookup"><span data-stu-id="7d5c0-173">Microsoft.AspNetCore.Hosting.IStartup</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartup) | <span data-ttu-id="7d5c0-174">單一</span><span class="sxs-lookup"><span data-stu-id="7d5c0-174">Singleton</span></span> |
| [<span data-ttu-id="7d5c0-175">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="7d5c0-175">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | <span data-ttu-id="7d5c0-176">單一</span><span class="sxs-lookup"><span data-stu-id="7d5c0-176">Singleton</span></span> |

<span data-ttu-id="7d5c0-177">以下範例說明如何使用許多擴充方法，例如 `AddDbContext`、`AddIdentity` 和 `AddMvc`，將其他服務新增至容器。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-177">Below is an example of how to add additional services to the container using a number of extension methods like `AddDbContext`, `AddIdentity`, and `AddMvc`.</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

<span data-ttu-id="7d5c0-178">ASP.NET 提供的功能與中介軟體，例如 MVC，會遵循使用單一 Add*ServiceName* 的擴充方法註冊該功能所需之所有服務的慣例。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-178">The features and middleware provided by ASP.NET, such as MVC, follow a convention of using a single Add*ServiceName* extension method to register all of the services required by that feature.</span></span>

>[!TIP]
> <span data-ttu-id="7d5c0-179">您可以透過 `Startup` 方法的參數清單在方法內要求特定的架構提供服務 - 如需詳細資料，請參閱[應用程式啟動](startup.md)。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-179">You can request certain framework-provided services within `Startup` methods through their parameter lists - see [Application Startup](startup.md) for more details.</span></span>

## <a name="registering-services"></a><span data-ttu-id="7d5c0-180">註冊服務</span><span class="sxs-lookup"><span data-stu-id="7d5c0-180">Registering services</span></span>

<span data-ttu-id="7d5c0-181">您可以註冊自己的應用程式服務，如下所示。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-181">You can register your own application services as follows.</span></span> <span data-ttu-id="7d5c0-182">第一個泛型型別代表將從容器要求的類型 (通常是介面)。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-182">The first generic type represents the type (typically an interface) that will be requested from the container.</span></span> <span data-ttu-id="7d5c0-183">第二個泛型型別代表容器將具現化且用來完成這類要求的具象類型。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-183">The second generic type represents the concrete type that will be instantiated by the container and used to fulfill such requests.</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> <span data-ttu-id="7d5c0-184">每個 `services.Add<ServiceName>` 擴充方法都會新增 (並且可能會設定) 服務。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-184">Each `services.Add<ServiceName>` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="7d5c0-185">例如，`services.AddMvc()` 會新增 MVC 要求的服務。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-185">For example, `services.AddMvc()` adds the services MVC requires.</span></span> <span data-ttu-id="7d5c0-186">建議您遵循這個慣例，並將擴充方法放在 `Microsoft.Extensions.DependencyInjection` 命名空間中，來封裝服務註冊的群組。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-186">It's recommended that you follow this convention, placing extension methods in the `Microsoft.Extensions.DependencyInjection` namespace, to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="7d5c0-187">`AddTransient` 方法用來針對每個需要它的物件，將抽象類型對應至個別具現化的具象服務。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-187">The `AddTransient` method is used to map abstract types to concrete services that are instantiated separately for every object that requires it.</span></span> <span data-ttu-id="7d5c0-188">這稱為服務的「存留期」，其他的存留期選項描述如下。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-188">This is known as the service's *lifetime*, and additional lifetime options are described below.</span></span> <span data-ttu-id="7d5c0-189">請務必為每個您註冊的服務選擇適當的存留期。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-189">It's important to choose an appropriate lifetime for each of the services you register.</span></span> <span data-ttu-id="7d5c0-190">服務的新執行個體應該提供給每個要求它的類別嗎？</span><span class="sxs-lookup"><span data-stu-id="7d5c0-190">Should a new instance of the service be provided to each class that requests it?</span></span> <span data-ttu-id="7d5c0-191">一個執行個體應該用於整個給定 Web 要求嗎？</span><span class="sxs-lookup"><span data-stu-id="7d5c0-191">Should one instance be used throughout a given web request?</span></span> <span data-ttu-id="7d5c0-192">或是單一執行個體應該用於應用程式的存留期嗎？</span><span class="sxs-lookup"><span data-stu-id="7d5c0-192">Or should a single instance be used for the lifetime of the application?</span></span>

<span data-ttu-id="7d5c0-193">在本文的範例中，有顯示字元名稱的簡單控制器，稱為 `CharactersController`。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-193">In the sample for this article, there's a simple controller that displays character names, called `CharactersController`.</span></span> <span data-ttu-id="7d5c0-194">其 `Index` 方法會顯示目前已儲存在應用程式中的字元清單，並初始化少數字元的集合，如果尚無集合存在的話。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-194">Its `Index` method displays the current list of characters that have been stored in the application, and initializes the collection with a handful of characters if none exist.</span></span> <span data-ttu-id="7d5c0-195">請注意，雖然此應用程式為了持續性使用 Entity Framework Core 和 `ApplicationDbContext` 類別，但那在控制器中都不明顯。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-195">Note that although this application uses Entity Framework Core and the `ApplicationDbContext` class for its persistence, none of that's apparent in the controller.</span></span> <span data-ttu-id="7d5c0-196">相反地，在介面背後已抽象化特定資料存取機制 `ICharacterRepository`，它遵循[儲存機制模式](http://deviq.com/repository-pattern/)。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-196">Instead, the specific data access mechanism has been abstracted behind an interface, `ICharacterRepository`, which follows the [repository pattern](http://deviq.com/repository-pattern/).</span></span> <span data-ttu-id="7d5c0-197">已透過建構函式要求 `ICharacterRepository` 的執行個體，並指派給私用欄位，然後視需要使用該欄位來存取字元。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-197">An instance of `ICharacterRepository` is requested via the constructor and assigned to a private field, which is then used to access characters as necessary.</span></span>

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

<span data-ttu-id="7d5c0-198">`ICharacterRepository` 定義了控制器搭配 `Character` 執行個體所需要的兩個方法。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-198">The `ICharacterRepository` defines the two methods the controller needs to work with `Character` instances.</span></span>

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

<span data-ttu-id="7d5c0-199">接著這個介面由具象類型 `CharacterRepository` 實作，用於執行階段。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-199">This interface is in turn implemented by a concrete type, `CharacterRepository`, that's used at runtime.</span></span>

> [!NOTE]
> <span data-ttu-id="7d5c0-200">DI 搭配 `CharacterRepository` 類別使用的方式是一個一般模型，您可以針對應用程式服務遵循該模型，而不只是在「儲存機制」或資料存取類別中。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-200">The way DI is used with the `CharacterRepository` class is a general model you can follow for all of your application services, not just in "repositories" or data access classes.</span></span>

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

<span data-ttu-id="7d5c0-201">請注意，`CharacterRepository` 在其建構函式中會要求 `ApplicationDbContext`。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-201">Note that `CharacterRepository` requests an `ApplicationDbContext` in its constructor.</span></span> <span data-ttu-id="7d5c0-202">相依性插入用於像這樣的鏈結方式並不少見，每個要求的相依性接著都會要求它自己的相依性。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-202">It's not unusual for dependency injection to be used in a chained fashion like this, with each requested dependency in turn requesting its own dependencies.</span></span> <span data-ttu-id="7d5c0-203">容器負責解決圖形中的所有相依性，並傳回完全解析的服務。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-203">The container is responsible for resolving all of the dependencies in the graph and returning the fully resolved service.</span></span>

> [!NOTE]
> <span data-ttu-id="7d5c0-204">建立所要求的物件、它需要的所有物件，以及那些物件需要的所有物件，有時稱為「物件圖形」。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-204">Creating the requested object, and all of the objects it requires, and all of the objects those require, is sometimes referred to as an *object graph*.</span></span> <span data-ttu-id="7d5c0-205">同樣地，必須先解析的相依性集合組通常稱為「相依性樹狀結構」或「相依性圖形」。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-205">Likewise, the collective set of dependencies that must be resolved is typically referred to as a *dependency tree* or *dependency graph*.</span></span>

<span data-ttu-id="7d5c0-206">在此情況下，`ICharacterRepository` 以及 `ApplicationDbContext` 都必須向 `Startup` 中 `ConfigureServices` 的服務容器註冊。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-206">In this case, both `ICharacterRepository` and in turn `ApplicationDbContext` must be registered with the services container in `ConfigureServices` in `Startup`.</span></span> <span data-ttu-id="7d5c0-207">`ApplicationDbContext` 的設定是藉由呼叫擴充方法 `AddDbContext<T>` 進行。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-207">`ApplicationDbContext` is configured with the call to the extension method `AddDbContext<T>`.</span></span> <span data-ttu-id="7d5c0-208">下列程式碼顯示 `CharacterRepository` 類型的註冊。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-208">The following code shows the registration of the `CharacterRepository` type.</span></span>

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

<span data-ttu-id="7d5c0-209">Entity Framework 內容應該使用 `Scoped` 存留期新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-209">Entity Framework contexts should be added to the services container using the `Scoped` lifetime.</span></span> <span data-ttu-id="7d5c0-210">如果您使用上述的協助程式方法，便會自動處理這點。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-210">This is taken care of automatically if you use the helper methods as shown above.</span></span> <span data-ttu-id="7d5c0-211">將利用 Entity Framework 的儲存機制應該使用相同的存留期。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-211">Repositories that will make use of Entity Framework should use the same lifetime.</span></span>

>[!WARNING]
> <span data-ttu-id="7d5c0-212">要小心的主要危險是從單一項目解析 `Scoped` 服務。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-212">The main danger to be wary of is resolving a `Scoped` service from a singleton.</span></span> <span data-ttu-id="7d5c0-213">很可能在此情況下，處理後續要求時，服務會有不正確的狀態。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-213">It's likely in such a case that the service will have incorrect state when processing subsequent requests.</span></span>

<span data-ttu-id="7d5c0-214">具有相依性的服務應該在容器中註冊它們。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-214">Services that have dependencies should register them in the container.</span></span> <span data-ttu-id="7d5c0-215">如果服務的建構函式需要基本類型，例如 `string`，這可以藉由使用[設定](xref:fundamentals/configuration/index)和[選項模式](xref:fundamentals/configuration/options)來插入。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-215">If a service's constructor requires a primitive, such as a `string`, this can be injected by using [configuration](xref:fundamentals/configuration/index) and the [options pattern](xref:fundamentals/configuration/options).</span></span>

## <a name="service-lifetimes-and-registration-options"></a><span data-ttu-id="7d5c0-216">服務存留期和註冊選項</span><span class="sxs-lookup"><span data-stu-id="7d5c0-216">Service lifetimes and registration options</span></span>

<span data-ttu-id="7d5c0-217">ASP.NET 服務可以使用下列存留期進行設定：</span><span class="sxs-lookup"><span data-stu-id="7d5c0-217">ASP.NET services can be configured with the following lifetimes:</span></span>

<span data-ttu-id="7d5c0-218">**暫時性**</span><span class="sxs-lookup"><span data-stu-id="7d5c0-218">**Transient**</span></span>

<span data-ttu-id="7d5c0-219">每次要求暫時性存留期服務時都會建立它們。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-219">Transient lifetime services are created each time they're requested.</span></span> <span data-ttu-id="7d5c0-220">此存留期最適合用於輕量型的無狀態服務。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-220">This lifetime works best for lightweight, stateless services.</span></span>

<span data-ttu-id="7d5c0-221">**具範圍**</span><span class="sxs-lookup"><span data-stu-id="7d5c0-221">**Scoped**</span></span>

<span data-ttu-id="7d5c0-222">具範圍存留期服務會針對每個要求建立一次。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-222">Scoped lifetime services are created once per request.</span></span>

<span data-ttu-id="7d5c0-223">**單一**</span><span class="sxs-lookup"><span data-stu-id="7d5c0-223">**Singleton**</span></span>

<span data-ttu-id="7d5c0-224">單一存留期服務會在第一次要求它們時建立 (或是當執行 `ConfigureServices` 時，如果您指定了執行個體)，然後每個後續的要求將會使用相同的執行個體。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-224">Singleton lifetime services are created the first time they're requested (or when `ConfigureServices` is run if you specify an instance there) and then every subsequent request will use the same instance.</span></span> <span data-ttu-id="7d5c0-225">如果您的應用程式需要單一行為，則建議允許服務容器管理服務的存留期，而不是實作單一設計模式，並自行管理類別中的物件存留期。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-225">If your application requires singleton behavior, allowing the services container to manage the service's lifetime is recommended instead of implementing the singleton design pattern and managing your object's lifetime in the class yourself.</span></span>

<span data-ttu-id="7d5c0-226">有數種方法可以向容器註冊服務。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-226">Services can be registered with the container in several ways.</span></span> <span data-ttu-id="7d5c0-227">我們已經看過如何藉由指定要使用的具象類型，註冊具有指定類型的服務實作。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-227">We have already seen how to register a service implementation with a given type by specifying the concrete type to use.</span></span> <span data-ttu-id="7d5c0-228">此外，可以指定 Factory，然後將它用來視需要建立執行個體。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-228">In addition, a factory can be specified, which will then be used to create the instance on demand.</span></span> <span data-ttu-id="7d5c0-229">第三種方法是直接指定要使用之類型的執行個體，在這種情況下，容器將永遠不會嘗試建立執行個體 (也不會處置執行個體)。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-229">The third approach is to directly specify the instance of the type to use, in which case the container will never attempt to create an instance (nor will it dispose of the instance).</span></span>

<span data-ttu-id="7d5c0-230">為了示範這些存留期和註冊選項之間的差異，請考慮一個簡單的介面，以具有唯一識別碼 `OperationId` 的「作業」代表一或多個工作。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-230">To demonstrate the difference between these lifetime and registration options, consider a simple interface that represents one or more tasks as an *operation* with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="7d5c0-231">根據我們設定此服務存留期的方式，容器會提供相同或不同的服務執行個體給要求的類別。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-231">Depending on how we configure the lifetime for this service, the container will provide either the same or different instances of the service to the requesting class.</span></span> <span data-ttu-id="7d5c0-232">為了清楚表示所要求的存留期，我們將為每個存留期選項建立一個類型：</span><span class="sxs-lookup"><span data-stu-id="7d5c0-232">To make it clear which lifetime is being requested, we will create one type per lifetime option:</span></span>

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

<span data-ttu-id="7d5c0-233">我們會使用單一類別 `Operation` 來實作這些介面，它在其建構函式接受 `Guid`，如果未提供則使用新的 `Guid`。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-233">We implement these interfaces using a single class, `Operation`, that accepts a `Guid` in its constructor, or uses a new `Guid` if none is provided.</span></span>

<span data-ttu-id="7d5c0-234">接下來，在 `ConfigureServices` 中，每個類型會根據其具名存留期新增至容器：</span><span class="sxs-lookup"><span data-stu-id="7d5c0-234">Next, in `ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

<span data-ttu-id="7d5c0-235">請注意，`IOperationSingletonInstance` 服務使用特定執行個體，它具有已知識別碼 `Guid.Empty`，因此可以清楚知道此類型在使用中的時候 (其 Guid 會是全部為零)。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-235">Note that the `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty` so it will be clear when this type is in use (its Guid will be all zeroes).</span></span> <span data-ttu-id="7d5c0-236">我們也已註冊 `OperationService`，它相依於每一個其他 `Operation` 類型，因此可以清楚知道在要求內這個服務針對每個作業類型是獲得與控制器相同的執行個體，還是新的執行個體。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-236">We have also registered an `OperationService` that depends on each of the other `Operation` types, so that it will be clear within a request whether this service is getting the same instance as the controller, or a new one, for each operation type.</span></span> <span data-ttu-id="7d5c0-237">此服務所做的就是將其相依性公開為屬性，以便在檢視中顯示。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-237">All this service does is expose its dependencies as properties, so they can be displayed in the view.</span></span>

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

<span data-ttu-id="7d5c0-238">為了示範個別應用程式要求內與之間的物件存留期，範例包含了 `OperationsController`，它會要求每種 `IOperation` 類型，以及 `OperationService`。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-238">To demonstrate the object lifetimes within and between separate individual requests to the application, the sample includes an `OperationsController` that requests each kind of `IOperation` type as well as an `OperationService`.</span></span> <span data-ttu-id="7d5c0-239">然後 `Index` 動作會顯示控制器和服務的所有 `OperationId` 值。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-239">The `Index` action then displays all of the controller's and service's `OperationId` values.</span></span>

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

<span data-ttu-id="7d5c0-240">現在，對此控制器動作進行了兩個不同的要求：</span><span class="sxs-lookup"><span data-stu-id="7d5c0-240">Now two separate requests are made to this controller action:</span></span>

![Microsoft Edge 中執行的相依性插入範例 Web 應用程式 [作業] 檢視，並在第一個要求顯示暫時性、具範圍、單一和執行個體控制器和作業服務作業的作業識別碼值 (GUID)。](dependency-injection/_static/lifetimes_request1.png)

![顯示第二個要求的作業識別碼值的 [作業] 檢視。](dependency-injection/_static/lifetimes_request2.png)

<span data-ttu-id="7d5c0-243">觀察哪些 `OperationId` 值在要求內以及要求之間不同。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-243">Observe which of the `OperationId` values vary within a request, and between requests.</span></span>

* <span data-ttu-id="7d5c0-244">「暫時性」物件一定會不同；會提供新的執行個體給每個控制器與每個服務。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-244">*Transient* objects are always different; a new instance is provided to every controller and every service.</span></span>

* <span data-ttu-id="7d5c0-245">「具範圍」物件在要求內相同，但跨不同的要求時會不同</span><span class="sxs-lookup"><span data-stu-id="7d5c0-245">*Scoped* objects are the same within a request, but different across different requests</span></span>

* <span data-ttu-id="7d5c0-246">「單一」物件對於每個物件與每個要求都相同 (不論執行個體是否提供於 `ConfigureServices`)</span><span class="sxs-lookup"><span data-stu-id="7d5c0-246">*Singleton* objects are the same for every object and every request (regardless of whether an instance is provided in `ConfigureServices`)</span></span>

## <a name="request-services"></a><span data-ttu-id="7d5c0-247">要求服務</span><span class="sxs-lookup"><span data-stu-id="7d5c0-247">Request Services</span></span>

<span data-ttu-id="7d5c0-248">來自 `HttpContext`，在 ASP.NET 要求內提供的服務是透過 `RequestServices` 集合公開。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-248">The services available within an ASP.NET request from `HttpContext` are exposed through the `RequestServices` collection.</span></span>

![HttpContext 要求服務 Intellisense 的內容對話方塊，指出要求服務會取得或設定提供要求之服務容器存取權的 IServiceProvider。](dependency-injection/_static/request-services.png)

<span data-ttu-id="7d5c0-250">要求服務代表您在應用程式中設定和要求的服務。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-250">Request Services represent the services you configure and request as part of your application.</span></span> <span data-ttu-id="7d5c0-251">當您的物件指定相依性時，這些會由 `RequestServices` 中找到的類型來滿足，而非 `ApplicationServices`。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-251">When your objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="7d5c0-252">一般而言，您不應該直接使用這些屬性，而是最好改為透過您的類別建構函式要求需要的類別類型，並讓架構插入這些相依性。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-252">Generally, you shouldn't use these properties directly, preferring instead to request the types your classes you require via your class's constructor, and letting the framework inject these dependencies.</span></span> <span data-ttu-id="7d5c0-253">這會產生較易於測試的類別 (請參閱[測試](../testing/index.md)) 且更鬆散結合。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-253">This yields classes that are easier to test (see [Testing](../testing/index.md)) and are more loosely coupled.</span></span>

> [!NOTE]
> <span data-ttu-id="7d5c0-254">偏好要求相依性作為建構函式參數，而不要存取 `RequestServices` 集合。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-254">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="designing-services-for-dependency-injection"></a><span data-ttu-id="7d5c0-255">針對相依性插入設計服務</span><span class="sxs-lookup"><span data-stu-id="7d5c0-255">Designing services for dependency injection</span></span>

<span data-ttu-id="7d5c0-256">您應該設計服務，以使用相依性插入取得其共同作業者。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-256">You should design your services to use dependency injection to get their collaborators.</span></span> <span data-ttu-id="7d5c0-257">這表示避免使用可設定狀態的靜態方法呼叫 (這會導致稱為[靜態黏貼](http://deviq.com/static-cling/)的程式碼特徵) 以及直接具現化您服務內的相依類別。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-257">This means avoiding the use of stateful static method calls (which result in a code smell known as [static cling](http://deviq.com/static-cling/)) and the direct instantiation of dependent classes within your services.</span></span> <span data-ttu-id="7d5c0-258">當您選擇是要具現化類型，還是透過相依性插入要求它時，記住 [New is Glue](https://ardalis.com/new-is-glue) 這句話可能有用。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-258">It may help to remember the phrase, [New is Glue](https://ardalis.com/new-is-glue), when choosing whether to instantiate a type or to request it via dependency injection.</span></span> <span data-ttu-id="7d5c0-259">藉由遵循[物件導向設計的 SOLID 準則](http://deviq.com/solid/)，您的類別自然而然會傾向小型、構造良好且輕鬆地測試。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-259">By following the [SOLID Principles of Object Oriented Design](http://deviq.com/solid/), your classes will naturally tend to be small, well-factored, and easily tested.</span></span>

<span data-ttu-id="7d5c0-260">如果您發現類別通常插入太多相依性怎麼辦？</span><span class="sxs-lookup"><span data-stu-id="7d5c0-260">What if you find that your classes tend to have way too many dependencies being injected?</span></span> <span data-ttu-id="7d5c0-261">這通常是表示您的類別嘗試完成太多事項，並且可能違反 SRP - [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/) (單一責任準則)。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-261">This is generally a sign that your class is trying to do too much, and is probably violating SRP - the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/).</span></span> <span data-ttu-id="7d5c0-262">將類別負責的某些部分移到新的類別，看看是否能夠重構類別。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-262">See if you can refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="7d5c0-263">請記住，您的 `Controller` 類別應該專注於 UI 考量，因此商務規則和資料存取實作詳細資料應該保存在適合這些 [Separation of Concerns](http://deviq.com/separation-of-concerns/) (不同考量) 的類別中。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-263">Keep in mind that your `Controller` classes should be focused on UI concerns, so business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](http://deviq.com/separation-of-concerns/).</span></span>

<span data-ttu-id="7d5c0-264">具體來說，關於資料存取，您可以將 `DbContext` 插入至您的控制器 (假設您已在 `ConfigureServices` 中將 EF 新增到服務容器)。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-264">With regards to data access specifically, you can inject the `DbContext` into your controllers (assuming you've added EF to the services container in `ConfigureServices`).</span></span> <span data-ttu-id="7d5c0-265">有些開發人員偏好使用資料庫的儲存機制介面，而不是直接插入 `DbContext`。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-265">Some developers prefer to use a repository interface to the database rather than injecting the `DbContext` directly.</span></span> <span data-ttu-id="7d5c0-266">使用介面將資料存取邏輯封裝在一個地方，可以在資料庫變更時將您必須變更的位置數目降至最低。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-266">Using an interface to encapsulate the data access logic in one place can minimize how many places you will have to change when your database changes.</span></span>

### <a name="disposing-of-services"></a><span data-ttu-id="7d5c0-267">處置服務</span><span class="sxs-lookup"><span data-stu-id="7d5c0-267">Disposing of services</span></span>

<span data-ttu-id="7d5c0-268">容器會為它建立的 `IDisposable` 類型呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-268">The container will call `Dispose` for `IDisposable` types it creates.</span></span> <span data-ttu-id="7d5c0-269">不過，如果您自行將執行個體新增到容器，將不會處置它。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-269">However, if you add an instance to the container yourself, it will not be disposed.</span></span>

<span data-ttu-id="7d5c0-270">範例：</span><span class="sxs-lookup"><span data-stu-id="7d5c0-270">Example:</span></span>

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
> <span data-ttu-id="7d5c0-271">在 1.0 版中，容器會對「所有」`IDisposable` 物件呼叫 dispose，包括它未建立的物件。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-271">In version 1.0, the container called dispose on *all* `IDisposable` objects, including those it didn't create.</span></span>

## <a name="replacing-the-default-services-container"></a><span data-ttu-id="7d5c0-272">取代預設服務容器</span><span class="sxs-lookup"><span data-stu-id="7d5c0-272">Replacing the default services container</span></span>

<span data-ttu-id="7d5c0-273">內建的服務容器是要服務架構和建置在其上的大部分取用者應用程式的基本需求。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-273">The built-in services container is meant to serve the basic needs of the framework and most consumer applications built on it.</span></span> <span data-ttu-id="7d5c0-274">不過，開發人員可以使用其偏好的容器取代內建的容器。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-274">However, developers can replace the built-in container with their preferred container.</span></span> <span data-ttu-id="7d5c0-275">`ConfigureServices` 方法通常會傳回 `void`，但如果其簽章變更為傳回 `IServiceProvider`，則可以設定與傳回不同的容器。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-275">The `ConfigureServices` method typically returns `void`, but if its signature is changed to return `IServiceProvider`, a different container can be configured and returned.</span></span> <span data-ttu-id="7d5c0-276">.NET 有許多 IOC 容器可用。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-276">There are many IOC containers available for .NET.</span></span> <span data-ttu-id="7d5c0-277">在此範例中，使用了 [Autofac](https://autofac.org/) 套件。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-277">In this example, the [Autofac](https://autofac.org/) package is used.</span></span>

<span data-ttu-id="7d5c0-278">首先，安裝適當的容器套件：</span><span class="sxs-lookup"><span data-stu-id="7d5c0-278">First, install the appropriate container package(s):</span></span>

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

<span data-ttu-id="7d5c0-279">接下來，在 `ConfigureServices` 中設定容器並傳回 `IServiceProvider`：</span><span class="sxs-lookup"><span data-stu-id="7d5c0-279">Next, configure the container in `ConfigureServices` and return an `IServiceProvider`:</span></span>

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
> <span data-ttu-id="7d5c0-280">當使用協力廠商 DI 容器時，您必須變更 `ConfigureServices`，使它傳回 `IServiceProvider` 而不是 `void`。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-280">When using a third-party DI container, you must change `ConfigureServices` so that it returns `IServiceProvider` instead of `void`.</span></span>

<span data-ttu-id="7d5c0-281">最後，像平常一樣在 `DefaultModule` 設定 Autofac：</span><span class="sxs-lookup"><span data-stu-id="7d5c0-281">Finally, configure Autofac as normal in `DefaultModule`:</span></span>

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

<span data-ttu-id="7d5c0-282">在執行階段，Autofac 將用於解析類型和插入相依性。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-282">At runtime, Autofac will be used to resolve types and inject dependencies.</span></span> <span data-ttu-id="7d5c0-283">[深入了解使用 Autofac 和 ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html)。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-283">[Learn more about using Autofac and ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="7d5c0-284">執行緒安全</span><span class="sxs-lookup"><span data-stu-id="7d5c0-284">Thread safety</span></span>

<span data-ttu-id="7d5c0-285">單一服務需要具備執行緒安全。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-285">Singleton services need to be thread safe.</span></span> <span data-ttu-id="7d5c0-286">如果單一服務相依於暫時性服務，暫時性服務可能也需要具備執行緒安全，視它如何由單一項目使用。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-286">If a singleton service has a dependency on a transient service, the transient service may also need to be thread safe depending how it's used by the singleton.</span></span>

## <a name="recommendations"></a><span data-ttu-id="7d5c0-287">建議</span><span class="sxs-lookup"><span data-stu-id="7d5c0-287">Recommendations</span></span>

<span data-ttu-id="7d5c0-288">使用相依性插入時，請記住下列建議：</span><span class="sxs-lookup"><span data-stu-id="7d5c0-288">When working with dependency injection, keep the following recommendations in mind:</span></span>

* <span data-ttu-id="7d5c0-289">DI 適用於具有複雜相依性的物件。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-289">DI is for objects that have complex dependencies.</span></span> <span data-ttu-id="7d5c0-290">控制器、服務、配接器以及儲存機制全都是可能新增至 DI 的物件範例。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-290">Controllers, services, adapters, and repositories are all examples of objects that might be added to DI.</span></span>

* <span data-ttu-id="7d5c0-291">避免直接在 DI 中儲存資料和組態。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-291">Avoid storing data and configuration directly in DI.</span></span> <span data-ttu-id="7d5c0-292">例如，使用者的購物車通常不應該新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-292">For example, a user's shopping cart shouldn't typically be added to the services container.</span></span> <span data-ttu-id="7d5c0-293">組態應該使用[選項模式](xref:fundamentals/configuration/options)。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-293">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="7d5c0-294">同樣地，請避免只存在以允許存取某個其他物件的「資料持有者」物件。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-294">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="7d5c0-295">最好是盡可能透過 DI 要求所需的實際項目。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-295">It's better to request the actual item needed via DI, if possible.</span></span>

* <span data-ttu-id="7d5c0-296">避免靜態存取服務。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-296">Avoid static access to services.</span></span>

* <span data-ttu-id="7d5c0-297">避免應用程式程式碼中的服務位置。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-297">Avoid service location in your application code.</span></span>

* <span data-ttu-id="7d5c0-298">避免靜態存取 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-298">Avoid static access to `HttpContext`.</span></span>

> [!NOTE]
> <span data-ttu-id="7d5c0-299">就像所有的建議集，您可能會遇到需要忽略其中之一的情況。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-299">Like all sets of recommendations, you may encounter situations where ignoring one is required.</span></span> <span data-ttu-id="7d5c0-300">我們發現例外狀況很少見 - 主要是架構本身內非常特殊的情況。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-300">We have found exceptions to be rare -- mostly very special cases within the framework itself.</span></span>

<span data-ttu-id="7d5c0-301">請記住，相依性插入是靜態/全域物件存取模式的「替代」選項。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-301">Remember, dependency injection is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="7d5c0-302">如果您將 DI 與靜態物件存取混合，則將無法實現 DI 的優點。</span><span class="sxs-lookup"><span data-stu-id="7d5c0-302">You won't be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7d5c0-303">其他資源</span><span class="sxs-lookup"><span data-stu-id="7d5c0-303">Additional resources</span></span>

* [<span data-ttu-id="7d5c0-304">應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="7d5c0-304">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="7d5c0-305">測試</span><span class="sxs-lookup"><span data-stu-id="7d5c0-305">Testing</span></span>](xref:testing/index)
* [<span data-ttu-id="7d5c0-306">Factory 式中介軟體啟用</span><span class="sxs-lookup"><span data-stu-id="7d5c0-306">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="7d5c0-307">在 ASP.NET Core 使用 Dependency Injection 撰寫簡潔的程式碼 (MSDN)</span><span class="sxs-lookup"><span data-stu-id="7d5c0-307">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* <span data-ttu-id="7d5c0-308">[Container-Managed Application Design, Prelude: Where does the Container Belong?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/) (容器管理的應用程式設計，序曲：容器屬於何處？)</span><span class="sxs-lookup"><span data-stu-id="7d5c0-308">[Container-Managed Application Design, Prelude: Where does the Container Belong?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)</span></span>
* [<span data-ttu-id="7d5c0-309">明確相依性準則</span><span class="sxs-lookup"><span data-stu-id="7d5c0-309">Explicit Dependencies Principle</span></span>](http://deviq.com/explicit-dependencies-principle/)
* <span data-ttu-id="7d5c0-310">[Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html) (逆轉控制容器和相依性插入模式) (Fowler)</span><span class="sxs-lookup"><span data-stu-id="7d5c0-310">[Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html) (Fowler)</span></span>
