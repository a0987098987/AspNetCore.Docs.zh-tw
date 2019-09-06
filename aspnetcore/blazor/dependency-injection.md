---
title: ASP.NET Core Blazor 相依性插入
author: guardrex
description: 瞭解 Blazor apps 如何將服務插入元件中。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/dependency-injection
ms.openlocfilehash: a2bfa0cbe951e817ed6264f1a151d5a716cd795c
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310360"
---
# <a name="aspnet-core-blazor-dependency-injection"></a><span data-ttu-id="c7a2b-103">ASP.NET Core Blazor 相依性插入</span><span class="sxs-lookup"><span data-stu-id="c7a2b-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="c7a2b-104">依[Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="c7a2b-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="c7a2b-105">Blazor 支援相依性[插入（DI）](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-105">Blazor supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c7a2b-106">應用程式可以使用內建的服務，方法是將它們插入元件中。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="c7a2b-107">應用程式也可以定義和註冊自訂服務，並透過 DI 讓它們可在整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="c7a2b-108">DI 是用來存取集中位置所設定之服務的技術。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="c7a2b-109">在 Blazor 應用程式中，這會很有用：</span><span class="sxs-lookup"><span data-stu-id="c7a2b-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="c7a2b-110">跨許多元件（稱為*單一*服務）共用服務類別的單一實例。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="c7a2b-111">使用參考抽象，將元件與具體的服務類別分離。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="c7a2b-112">例如，請考慮使用介面`IDataAccess`來存取應用程式中的資料。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="c7a2b-113">介面是由具象`DataAccess`類別來執行，並在應用程式的服務容器中註冊為服務。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="c7a2b-114">當元件使用 DI 來接收實作為`IDataAccess`時，該元件不會結合到具體類型。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="c7a2b-115">可以交換執行，也許是針對單元測試中的 mock 執行。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="c7a2b-116">預設服務</span><span class="sxs-lookup"><span data-stu-id="c7a2b-116">Default services</span></span>

<span data-ttu-id="c7a2b-117">預設服務會自動新增至應用程式的服務集合。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="c7a2b-118">服務</span><span class="sxs-lookup"><span data-stu-id="c7a2b-118">Service</span></span> | <span data-ttu-id="c7a2b-119">存留期</span><span class="sxs-lookup"><span data-stu-id="c7a2b-119">Lifetime</span></span> | <span data-ttu-id="c7a2b-120">描述</span><span class="sxs-lookup"><span data-stu-id="c7a2b-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="c7a2b-121">單一</span><span class="sxs-lookup"><span data-stu-id="c7a2b-121">Singleton</span></span> | <span data-ttu-id="c7a2b-122">提供方法來傳送 HTTP 要求，以及從 URI 所識別的資源接收 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span> <span data-ttu-id="c7a2b-123">請注意，這個實例`HttpClient`會使用瀏覽器來處理背景中的 HTTP 流量。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-123">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="c7a2b-124">[HttpClient](xref:System.Net.Http.HttpClient.BaseAddress)會自動設定為應用程式的基底 URI 前置詞。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-124">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="c7a2b-125">如需詳細資訊，請參閱 <xref:blazor/call-web-api>。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-125">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="c7a2b-126">單一</span><span class="sxs-lookup"><span data-stu-id="c7a2b-126">Singleton</span></span> | <span data-ttu-id="c7a2b-127">代表在其中分派 JavaScript 呼叫的 JavaScript 執行時間實例。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-127">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="c7a2b-128">如需詳細資訊，請參閱 <xref:blazor/javascript-interop>。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-128">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `NavigationManager` | <span data-ttu-id="c7a2b-129">單一</span><span class="sxs-lookup"><span data-stu-id="c7a2b-129">Singleton</span></span> | <span data-ttu-id="c7a2b-130">包含使用 Uri 和導覽狀態的協助程式。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-130">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="c7a2b-131">如需詳細資訊，請參閱[URI 和流覽狀態](xref:blazor/routing#uri-and-navigation-state-helpers)協助程式。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-131">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="c7a2b-132">自訂服務提供者不會自動提供表格中所列的預設服務。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-132">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="c7a2b-133">如果您使用自訂服務提供者，而且需要資料表中所顯示的任何服務，請將所需的服務新增至新的服務提供者。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-133">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="c7a2b-134">將服務新增至應用程式</span><span class="sxs-lookup"><span data-stu-id="c7a2b-134">Add services to an app</span></span>

<span data-ttu-id="c7a2b-135">建立新的應用程式之後，請`Startup.ConfigureServices`檢查方法：</span><span class="sxs-lookup"><span data-stu-id="c7a2b-135">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="c7a2b-136">會傳遞方法，這是服務描述元物件（<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>）的清單。 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> `ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="c7a2b-136">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="c7a2b-137">服務會藉由將服務描述項提供給服務集合來新增。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-137">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="c7a2b-138">下列範例示範`IDataAccess`介面和其具體執行`DataAccess`的概念：</span><span class="sxs-lookup"><span data-stu-id="c7a2b-138">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="c7a2b-139">您可以使用下表所示的存留期來設定服務。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-139">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="c7a2b-140">存留期</span><span class="sxs-lookup"><span data-stu-id="c7a2b-140">Lifetime</span></span> | <span data-ttu-id="c7a2b-141">描述</span><span class="sxs-lookup"><span data-stu-id="c7a2b-141">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | <span data-ttu-id="c7a2b-142">Blazor 用戶端目前沒有 DI 範圍的概念。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-142">Blazor client-side doesn't currently have a concept of DI scopes.</span></span> <span data-ttu-id="c7a2b-143">`Scoped`註冊的服務的行為`Singleton`就像服務一樣。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-143">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="c7a2b-144">不過，伺服器端裝載模型支援`Scoped`存留期。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-144">However, the server-side hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="c7a2b-145">在 Razor 元件中，限定範圍的服務註冊的範圍是連接。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-145">In a Razor component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="c7a2b-146">因此，即使目前的意圖是在瀏覽器中執行用戶端，使用範圍服務也適用于應該範圍設定為目前使用者的服務。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-146">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="c7a2b-147">DI 會建立服務的*單一實例*。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-147">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="c7a2b-148">所有需要`Singleton`服務的元件都會收到相同服務的實例。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-148">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="c7a2b-149">每當元件從服務容器取得`Transient`服務的實例時，就會收到服務的*新實例*。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-149">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="c7a2b-150">DI 系統是以 ASP.NET Core 中的 DI 系統為基礎。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-150">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="c7a2b-151">如需詳細資訊，請參閱 <xref:fundamentals/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-151">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="c7a2b-152">要求元件中的服務</span><span class="sxs-lookup"><span data-stu-id="c7a2b-152">Request a service in a component</span></span>

<span data-ttu-id="c7a2b-153">將服務新增至服務集合之後，請使用[ \@插入](xref:mvc/views/razor#inject)Razor 指示詞將服務插入元件中。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-153">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="c7a2b-154">`@inject`有兩個參數：</span><span class="sxs-lookup"><span data-stu-id="c7a2b-154">`@inject` has two parameters:</span></span>

* <span data-ttu-id="c7a2b-155">輸入&ndash;要插入之服務的類型。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-155">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="c7a2b-156">屬性&ndash;接收插入的應用程式服務之屬性的名稱。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-156">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="c7a2b-157">屬性不需要手動建立。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-157">The property doesn't require manual creation.</span></span> <span data-ttu-id="c7a2b-158">編譯器會建立屬性。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-158">The compiler creates the property.</span></span>

<span data-ttu-id="c7a2b-159">如需詳細資訊，請參閱 <xref:mvc/views/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-159">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="c7a2b-160">使用多`@inject`個語句來插入不同的服務。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-160">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="c7a2b-161">下列範例示範如何使用 `@inject`。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-161">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="c7a2b-162">執行`Services.IDataAccess`的服務會插入元件的屬性`DataRepository`中。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-162">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="c7a2b-163">請注意程式碼如何使用`IDataAccess`抽象概念：</span><span class="sxs-lookup"><span data-stu-id="c7a2b-163">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="c7a2b-164">就內部`DataRepository` `InjectAttribute`而言，產生的屬性（）會以屬性裝飾。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-164">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="c7a2b-165">通常不會直接使用這個屬性。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-165">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="c7a2b-166">如果元件需要基類，而且基類也需要插入的`InjectAttribute`屬性，請手動新增：</span><span class="sxs-lookup"><span data-stu-id="c7a2b-166">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="c7a2b-167">在衍生自基類的元件中， `@inject`不需要指示詞。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-167">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="c7a2b-168">`InjectAttribute`基類的已足夠：</span><span class="sxs-lookup"><span data-stu-id="c7a2b-168">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="c7a2b-169">在服務中使用 DI</span><span class="sxs-lookup"><span data-stu-id="c7a2b-169">Use DI in services</span></span>

<span data-ttu-id="c7a2b-170">複雜的服務可能需要額外的服務。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-170">Complex services might require additional services.</span></span> <span data-ttu-id="c7a2b-171">在先前的範例中`DataAccess` ，可能`HttpClient`需要預設服務。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-171">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="c7a2b-172">`@inject`（或`InjectAttribute`）無法在服務中使用。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-172">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="c7a2b-173">必須改為使用*函數插入*。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-173">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="c7a2b-174">將參數新增至服務的函式，即可加入必要的服務。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-174">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="c7a2b-175">當 DI 建立服務時，它會辨識它在此函式中所需的服務，並據以提供它們。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-175">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="c7a2b-176">函式插入的必要條件：</span><span class="sxs-lookup"><span data-stu-id="c7a2b-176">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="c7a2b-177">其中一個函式必須存在，且其引數可由 DI 完成。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-177">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="c7a2b-178">如果指定預設值，則允許 DI 未涵蓋的其他參數。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-178">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="c7a2b-179">適用的函式必須是*公用*的。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-179">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="c7a2b-180">其中一個適用的函數必須存在。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-180">One applicable constructor must exist.</span></span> <span data-ttu-id="c7a2b-181">如果發生不明確的情況，DI 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c7a2b-181">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c7a2b-182">其他資源</span><span class="sxs-lookup"><span data-stu-id="c7a2b-182">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
