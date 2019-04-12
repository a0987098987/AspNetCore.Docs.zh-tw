---
title: Razor 元件相依性插入
author: guardrex
description: 請參閱如何 Blazor 和 Razor 元件的應用程式時，可以將它們插入元件，以便使用服務。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
uid: razor-components/dependency-injection
ms.openlocfilehash: 40aec2e3a5032039c7d921f67d7d333b03c07fb1
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/30/2019
ms.locfileid: "59515401"
---
# <a name="razor-components-dependency-injection"></a><span data-ttu-id="0b0f9-103">Razor 元件相依性插入</span><span class="sxs-lookup"><span data-stu-id="0b0f9-103">Razor Components dependency injection</span></span>

<span data-ttu-id="0b0f9-104">藉由[Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="0b0f9-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="0b0f9-105">Razor 元件支援[相依性插入 (DI)](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-105">Razor Components supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="0b0f9-106">應用程式可以使用內建的服務，藉由插入元件。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="0b0f9-107">應用程式也可以定義和註冊自訂的服務並提供給全透過 DI 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="0b0f9-108">相依性插入</span><span class="sxs-lookup"><span data-stu-id="0b0f9-108">Dependency injection</span></span>

<span data-ttu-id="0b0f9-109">DI 是一種技術來存取中央位置中所設定的服務。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-109">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="0b0f9-110">這可用在 Razor 元件的應用程式：</span><span class="sxs-lookup"><span data-stu-id="0b0f9-110">This can be useful in Razor Components apps to:</span></span>

* <span data-ttu-id="0b0f9-111">服務類別的單一執行個體共用許多元件，稱為*singleton*服務。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-111">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="0b0f9-112">使用參考抽象概念中分離從具象服務類別的元件。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-112">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="0b0f9-113">例如，請考慮介面`IDataAccess`來存取應用程式中的資料。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-113">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="0b0f9-114">介面由具體實作`DataAccess`類別，並註冊為應用程式的服務容器中的服務。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-114">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="0b0f9-115">當元件會使用 DI 來接收`IDataAccess`元件的實作不具象型別結合。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-115">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="0b0f9-116">實作可以交換，或許在單元測試中模擬 （mock） 實作。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-116">The implementation can be swapped, perhaps to a mock implementation in unit tests.</span></span>

<span data-ttu-id="0b0f9-117">如需詳細資訊，請參閱<xref:fundamentals/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-117">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="add-services-to-di"></a><span data-ttu-id="0b0f9-118">將服務新增至 DI</span><span class="sxs-lookup"><span data-stu-id="0b0f9-118">Add services to DI</span></span>

<span data-ttu-id="0b0f9-119">建立新的應用程式之後, 檢查`Startup.ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="0b0f9-119">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="0b0f9-120">`ConfigureServices`傳遞給方法<xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>，這是一份服務描述元物件 (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>)。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-120">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="0b0f9-121">服務會藉由提供服務集合的服務描述元加入。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-121">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="0b0f9-122">下列範例示範使用概念`IDataAccess`介面和其具象實作`DataAccess`:</span><span class="sxs-lookup"><span data-stu-id="0b0f9-122">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="0b0f9-123">服務可以使用下表所示的存留期設定。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-123">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="0b0f9-124">存留期</span><span class="sxs-lookup"><span data-stu-id="0b0f9-124">Lifetime</span></span> | <span data-ttu-id="0b0f9-125">描述</span><span class="sxs-lookup"><span data-stu-id="0b0f9-125">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="0b0f9-126">建立 DI*單一執行個體*的服務。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-126">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="0b0f9-127">需要的所有元件`Singleton`服務接收相同的服務執行個體。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-127">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="0b0f9-128">每當元件取得的執行個體`Transient`服務從服務容器，它會接收*新的執行個體*的服務。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-128">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | <span data-ttu-id="0b0f9-129">用戶端 Blazor 目前沒有 DI 領域的概念。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-129">Client-side Blazor doesn't currently have the concept of DI scopes.</span></span> <span data-ttu-id="0b0f9-130">`Scoped` 行為類似`Singleton`。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-130">`Scoped` behaves like `Singleton`.</span></span> <span data-ttu-id="0b0f9-131">不過，ASP.NET Core Razor 元件支援`Scoped`存留期。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-131">However, ASP.NET Core Razor Components support the `Scoped` lifetime.</span></span> <span data-ttu-id="0b0f9-132">在 Razor 元件中，連接範圍已設定領域的服務註冊。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-132">In a Razor Component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="0b0f9-133">基於這個理由，使用已設定領域的服務是慣用的服務，應受限於目前的使用者，即使目前的目的是要執行用戶端瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-133">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |

<span data-ttu-id="0b0f9-134">DI 系統是以 ASP.NET Core 中的 DI 系統為基礎。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-134">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="0b0f9-135">如需詳細資訊，請參閱<xref:fundamentals/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-135">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="default-services"></a><span data-ttu-id="0b0f9-136">預設服務</span><span class="sxs-lookup"><span data-stu-id="0b0f9-136">Default services</span></span>

<span data-ttu-id="0b0f9-137">預設的服務會自動新增至應用程式的服務集合。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-137">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="0b0f9-138">服務</span><span class="sxs-lookup"><span data-stu-id="0b0f9-138">Service</span></span> | <span data-ttu-id="0b0f9-139">描述</span><span class="sxs-lookup"><span data-stu-id="0b0f9-139">Description</span></span> |
| ------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="0b0f9-140">提供方法來傳送 HTTP 要求，以及從 URI (singleton) 所識別的資源接收 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-140">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI (singleton).</span></span> <span data-ttu-id="0b0f9-141">請注意，這個執行個體`HttpClient`處理 HTTP 流量，在背景中的使用瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-141">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="0b0f9-142">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress)會自動設為應用程式的基底 URI 前置詞。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-142">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="0b0f9-143">`HttpClient` 僅提供給用戶端 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-143">`HttpClient` is only provided to client-side Blazor apps.</span></span> |
| `IJSRuntime` | <span data-ttu-id="0b0f9-144">表示可能會分派呼叫的 JavaScript 執行階段的執行個體。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-144">Represents an instance of a JavaScript runtime to which calls may be dispatched.</span></span> <span data-ttu-id="0b0f9-145">如需詳細資訊，請參閱<xref:razor-components/javascript-interop>。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-145">For more information, see <xref:razor-components/javascript-interop>.</span></span> |
| `IUriHelper` | <span data-ttu-id="0b0f9-146">使用 Uri 和瀏覽狀態 (singleton) 包含協助程式。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-146">Contains helpers for working with URIs and navigation state (singleton).</span></span> <span data-ttu-id="0b0f9-147">`IUriHelper` 會提供給 Blazor 和 Razor 元件應用程式。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-147">`IUriHelper` is provided to both Blazor and Razor Components apps.</span></span> |

<span data-ttu-id="0b0f9-148">可以使用自訂服務提供者，而新增的預設範本的預設服務提供者。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-148">It's possible to use a custom service provider instead of the default service provider added by the default template.</span></span> <span data-ttu-id="0b0f9-149">自訂服務提供者不會自動提供資料表中列出的預設服務。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-149">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="0b0f9-150">如果您使用自訂服務提供者，而且需要的任何服務表所示，加入新的服務提供者所需的服務。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-150">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="0b0f9-151">要求中元件的服務</span><span class="sxs-lookup"><span data-stu-id="0b0f9-151">Request a service in a component</span></span>

<span data-ttu-id="0b0f9-152">服務集合中加入的服務之後，則將服務插入元件的 Razor 範本使用[ @inject ](xref:mvc/views/razor#section-4) Razor 指示詞。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-152">After services are added to the service collection, inject the services into the components' Razor templates using the [@inject](xref:mvc/views/razor#section-4) Razor directive.</span></span> <span data-ttu-id="0b0f9-153">`@inject` 有兩個參數：</span><span class="sxs-lookup"><span data-stu-id="0b0f9-153">`@inject` has two parameters:</span></span>

* <span data-ttu-id="0b0f9-154">類型名稱：要插入的服務型別。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-154">Type name: The type of the service to inject.</span></span>
* <span data-ttu-id="0b0f9-155">屬性名稱：接收插入應用程式服務的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-155">Property name: The name of the property receiving the injected app service.</span></span> <span data-ttu-id="0b0f9-156">請注意，屬性不需要手動建立。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-156">Note that the property doesn't require manual creation.</span></span> <span data-ttu-id="0b0f9-157">編譯器會建立屬性。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-157">The compiler creates the property.</span></span>

<span data-ttu-id="0b0f9-158">如需詳細資訊，請參閱<xref:mvc/views/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-158">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="0b0f9-159">使用多個`@inject`陳述式來插入不同的服務。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-159">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="0b0f9-160">下列範例示範如何使用 `@inject`。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-160">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="0b0f9-161">服務實作`Services.IDataAccess`元件的屬性不會插入`DataRepository`。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-161">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="0b0f9-162">請注意程式碼方式只使用`IDataAccess`抽象概念：</span><span class="sxs-lookup"><span data-stu-id="0b0f9-162">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.cshtml?highlight=2-3,23)]

<span data-ttu-id="0b0f9-163">就內部而言，產生的屬性 (`DataRepository`) 以裝飾`InjectAttribute`屬性。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-163">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="0b0f9-164">一般而言，這個屬性不是直接使用。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-164">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="0b0f9-165">如果基底類別是必要元件，而且插入的屬性也是必要的基底類別，如`InjectAttribute`可以手動新增：</span><span class="sxs-lookup"><span data-stu-id="0b0f9-165">If a base class is required for components and injected properties are also required for the base class, `InjectAttribute` can be manually added:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // Dependency injection works even if using the
    // InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="0b0f9-166">在衍生自基底類別中，元件`@inject`指示詞不需要。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-166">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="0b0f9-167">`InjectAttribute`基底類別已足夠：</span><span class="sxs-lookup"><span data-stu-id="0b0f9-167">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="dependency-injection-in-services"></a><span data-ttu-id="0b0f9-168">在服務中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="0b0f9-168">Dependency injection in services</span></span>

<span data-ttu-id="0b0f9-169">複雜的服務可能需要額外的服務。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-169">Complex services might require additional services.</span></span> <span data-ttu-id="0b0f9-170">在先前範例中，`DataAccess`可能需要`HttpClient`預設服務。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-170">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="0b0f9-171">`@inject` (或`InjectAttribute`) 不適用於在 services 中使用。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-171">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="0b0f9-172">*建構函式插入*必須改為使用。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-172">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="0b0f9-173">將參數加入至服務的建構函式時，會新增必要的服務。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-173">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="0b0f9-174">當相依性插入建立服務時，它會辨識它需要在建構函式，並據以提供它們的服務。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-174">When dependency injection creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
}
```

<span data-ttu-id="0b0f9-175">建構函式插入的必要條件：</span><span class="sxs-lookup"><span data-stu-id="0b0f9-175">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="0b0f9-176">必須要有一個建構函式，其引數可以而完成的相依性插入。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-176">There must be one constructor whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="0b0f9-177">請注意，如果他們指定預設值允許使用 DI 未涵蓋的其他參數。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-177">Note that additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="0b0f9-178">適用的建構函式必須是*公開*。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-178">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="0b0f9-179">只能有一個適用的建構函式。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-179">There must only be one applicable constructor.</span></span> <span data-ttu-id="0b0f9-180">發生模稜兩可，DI 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0b0f9-180">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0b0f9-181">其他資源</span><span class="sxs-lookup"><span data-stu-id="0b0f9-181">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
