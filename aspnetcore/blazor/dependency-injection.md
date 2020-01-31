---
title: ASP.NET Core Blazor 相依性插入
author: guardrex
description: 瞭解 Blazor 應用程式如何將服務插入元件中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2020
no-loc:
- Blazor
- SignalR
uid: blazor/dependency-injection
ms.openlocfilehash: fa6762522c831c7fbe2742dbfe4e25a377988e1e
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2020
ms.locfileid: "76869560"
---
# <a name="aspnet-core-blazor-dependency-injection"></a><span data-ttu-id="2e044-103">ASP.NET Core Blazor 相依性插入</span><span class="sxs-lookup"><span data-stu-id="2e044-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="2e044-104">依[Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="2e044-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="2e044-105">Blazor 支援相依性[插入（DI）](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="2e044-105">Blazor supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="2e044-106">應用程式可以使用內建的服務，方法是將它們插入元件中。</span><span class="sxs-lookup"><span data-stu-id="2e044-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="2e044-107">應用程式也可以定義和註冊自訂服務，並透過 DI 讓它們可在整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="2e044-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="2e044-108">DI 是用來存取集中位置所設定之服務的技術。</span><span class="sxs-lookup"><span data-stu-id="2e044-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="2e044-109">在 Blazor 應用程式中，這會很有用：</span><span class="sxs-lookup"><span data-stu-id="2e044-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="2e044-110">跨許多元件（稱為*單一*服務）共用服務類別的單一實例。</span><span class="sxs-lookup"><span data-stu-id="2e044-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="2e044-111">使用參考抽象，將元件與具體的服務類別分離。</span><span class="sxs-lookup"><span data-stu-id="2e044-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="2e044-112">例如，請考慮使用介面 `IDataAccess` 來存取應用程式中的資料。</span><span class="sxs-lookup"><span data-stu-id="2e044-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="2e044-113">介面是由具體的 `DataAccess` 類別來執行，並在應用程式的服務容器中註冊為服務。</span><span class="sxs-lookup"><span data-stu-id="2e044-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="2e044-114">當元件使用 DI 來接收 `IDataAccess` 執行時，該元件不會與具象類型結合。</span><span class="sxs-lookup"><span data-stu-id="2e044-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="2e044-115">可以交換執行，也許是針對單元測試中的 mock 執行。</span><span class="sxs-lookup"><span data-stu-id="2e044-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="2e044-116">預設服務</span><span class="sxs-lookup"><span data-stu-id="2e044-116">Default services</span></span>

<span data-ttu-id="2e044-117">預設服務會自動新增至應用程式的服務集合。</span><span class="sxs-lookup"><span data-stu-id="2e044-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="2e044-118">服務</span><span class="sxs-lookup"><span data-stu-id="2e044-118">Service</span></span> | <span data-ttu-id="2e044-119">存留期</span><span class="sxs-lookup"><span data-stu-id="2e044-119">Lifetime</span></span> | <span data-ttu-id="2e044-120">描述</span><span class="sxs-lookup"><span data-stu-id="2e044-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="2e044-121">單一</span><span class="sxs-lookup"><span data-stu-id="2e044-121">Singleton</span></span> | <span data-ttu-id="2e044-122">提供方法來傳送 HTTP 要求，以及從 URI 所識別的資源接收 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="2e044-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span><br><br><span data-ttu-id="2e044-123">Blazor WebAssembly 應用程式中的 `HttpClient` 實例會使用瀏覽器來處理背景中的 HTTP 流量。</span><span class="sxs-lookup"><span data-stu-id="2e044-123">The instance of `HttpClient` in a Blazor WebAssembly app uses the browser for handling the HTTP traffic in the background.</span></span><br><br><span data-ttu-id="2e044-124">Blazor 伺服器應用程式預設不會包含設定為服務的 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="2e044-124">Blazor Server apps don't include an `HttpClient` configured as a service by default.</span></span> <span data-ttu-id="2e044-125">提供 Blazor 伺服器應用程式的 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="2e044-125">Provide an `HttpClient` to a Blazor Server app.</span></span><br><br><span data-ttu-id="2e044-126">如需詳細資訊，請參閱<xref:blazor/call-web-api>。</span><span class="sxs-lookup"><span data-stu-id="2e044-126">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="2e044-127">Singleton （Blazor WebAssembly）</span><span class="sxs-lookup"><span data-stu-id="2e044-127">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="2e044-128">限定範圍（Blazor 伺服器）</span><span class="sxs-lookup"><span data-stu-id="2e044-128">Scoped (Blazor Server)</span></span> | <span data-ttu-id="2e044-129">代表在其中分派 JavaScript 呼叫的 JavaScript 執行時間實例。</span><span class="sxs-lookup"><span data-stu-id="2e044-129">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="2e044-130">如需詳細資訊，請參閱<xref:blazor/javascript-interop>。</span><span class="sxs-lookup"><span data-stu-id="2e044-130">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `NavigationManager` | <span data-ttu-id="2e044-131">Singleton （Blazor WebAssembly）</span><span class="sxs-lookup"><span data-stu-id="2e044-131">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="2e044-132">限定範圍（Blazor 伺服器）</span><span class="sxs-lookup"><span data-stu-id="2e044-132">Scoped (Blazor Server)</span></span> | <span data-ttu-id="2e044-133">包含使用 Uri 和導覽狀態的協助程式。</span><span class="sxs-lookup"><span data-stu-id="2e044-133">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="2e044-134">如需詳細資訊，請參閱[URI 和流覽狀態](xref:blazor/routing#uri-and-navigation-state-helpers)協助程式。</span><span class="sxs-lookup"><span data-stu-id="2e044-134">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="2e044-135">自訂服務提供者不會自動提供表格中所列的預設服務。</span><span class="sxs-lookup"><span data-stu-id="2e044-135">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="2e044-136">如果您使用自訂服務提供者，而且需要資料表中所顯示的任何服務，請將所需的服務新增至新的服務提供者。</span><span class="sxs-lookup"><span data-stu-id="2e044-136">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="2e044-137">將服務新增至應用程式</span><span class="sxs-lookup"><span data-stu-id="2e044-137">Add services to an app</span></span>

<span data-ttu-id="2e044-138">建立新的應用程式之後，請檢查 `Startup.ConfigureServices` 方法：</span><span class="sxs-lookup"><span data-stu-id="2e044-138">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="2e044-139">`ConfigureServices` 方法會傳遞 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>，這是服務描述元物件（<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>）的清單。</span><span class="sxs-lookup"><span data-stu-id="2e044-139">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="2e044-140">服務會藉由將服務描述項提供給服務集合來新增。</span><span class="sxs-lookup"><span data-stu-id="2e044-140">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="2e044-141">下列範例示範 `IDataAccess` 介面和其具體執行 `DataAccess`的概念：</span><span class="sxs-lookup"><span data-stu-id="2e044-141">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="2e044-142">您可以使用下表所示的存留期來設定服務。</span><span class="sxs-lookup"><span data-stu-id="2e044-142">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="2e044-143">存留期</span><span class="sxs-lookup"><span data-stu-id="2e044-143">Lifetime</span></span> | <span data-ttu-id="2e044-144">描述</span><span class="sxs-lookup"><span data-stu-id="2e044-144">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor<span data-ttu-id="2e044-145"> WebAssembly 應用程式目前沒有 DI 範圍的概念。</span><span class="sxs-lookup"><span data-stu-id="2e044-145"> WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="2e044-146">`Scoped`註冊的服務行為就像 `Singleton` 服務一樣。</span><span class="sxs-lookup"><span data-stu-id="2e044-146">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="2e044-147">不過，Blazor 伺服器裝載模型支援 `Scoped` 存留期。</span><span class="sxs-lookup"><span data-stu-id="2e044-147">However, the Blazor Server hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="2e044-148">在 Blazor 伺服器應用程式中，限定範圍的服務註冊的範圍是*連接*。</span><span class="sxs-lookup"><span data-stu-id="2e044-148">In Blazor Server apps, a scoped service registration is scoped to the *connection*.</span></span> <span data-ttu-id="2e044-149">因此，即使目前的意圖是在瀏覽器中執行用戶端，使用範圍服務也適用于應該範圍設定為目前使用者的服務。</span><span class="sxs-lookup"><span data-stu-id="2e044-149">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="2e044-150">DI 會建立服務的*單一實例*。</span><span class="sxs-lookup"><span data-stu-id="2e044-150">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="2e044-151">所有需要 `Singleton` 服務的元件都會收到相同服務的實例。</span><span class="sxs-lookup"><span data-stu-id="2e044-151">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="2e044-152">每當元件從服務容器取得 `Transient` 服務的實例時，就會收到服務的*新實例*。</span><span class="sxs-lookup"><span data-stu-id="2e044-152">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="2e044-153">DI 系統是以 ASP.NET Core 中的 DI 系統為基礎。</span><span class="sxs-lookup"><span data-stu-id="2e044-153">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="2e044-154">如需詳細資訊，請參閱<xref:fundamentals/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="2e044-154">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="2e044-155">要求元件中的服務</span><span class="sxs-lookup"><span data-stu-id="2e044-155">Request a service in a component</span></span>

<span data-ttu-id="2e044-156">將服務新增至服務集合之後，請使用[\@插入](xref:mvc/views/razor#inject)Razor 指示詞，將服務插入元件中。</span><span class="sxs-lookup"><span data-stu-id="2e044-156">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="2e044-157">`@inject` 有兩個參數：</span><span class="sxs-lookup"><span data-stu-id="2e044-157">`@inject` has two parameters:</span></span>

* <span data-ttu-id="2e044-158">輸入 &ndash; 要插入之服務的類型。</span><span class="sxs-lookup"><span data-stu-id="2e044-158">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="2e044-159">屬性 &ndash; 接收插入的應用程式服務之屬性的名稱。</span><span class="sxs-lookup"><span data-stu-id="2e044-159">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="2e044-160">屬性不需要手動建立。</span><span class="sxs-lookup"><span data-stu-id="2e044-160">The property doesn't require manual creation.</span></span> <span data-ttu-id="2e044-161">編譯器會建立屬性。</span><span class="sxs-lookup"><span data-stu-id="2e044-161">The compiler creates the property.</span></span>

<span data-ttu-id="2e044-162">如需詳細資訊，請參閱<xref:mvc/views/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="2e044-162">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="2e044-163">使用多個 `@inject` 語句來插入不同的服務。</span><span class="sxs-lookup"><span data-stu-id="2e044-163">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="2e044-164">下列範例示範如何使用 `@inject`。</span><span class="sxs-lookup"><span data-stu-id="2e044-164">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="2e044-165">執行 `Services.IDataAccess` 的服務會插入元件的屬性 `DataRepository`中。</span><span class="sxs-lookup"><span data-stu-id="2e044-165">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="2e044-166">請注意程式碼如何使用 `IDataAccess` 抽象：</span><span class="sxs-lookup"><span data-stu-id="2e044-166">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-razor[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="2e044-167">就內部而言，產生的屬性（`DataRepository`）會使用 `InjectAttribute` 屬性。</span><span class="sxs-lookup"><span data-stu-id="2e044-167">Internally, the generated property (`DataRepository`) uses the `InjectAttribute` attribute.</span></span> <span data-ttu-id="2e044-168">通常不會直接使用這個屬性。</span><span class="sxs-lookup"><span data-stu-id="2e044-168">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="2e044-169">如果元件需要基類，而且基類也需要插入的屬性，請手動加入 `InjectAttribute`：</span><span class="sxs-lookup"><span data-stu-id="2e044-169">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="2e044-170">在衍生自基類的元件中，不需要 `@inject` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="2e044-170">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="2e044-171">基類的 `InjectAttribute` 已足夠：</span><span class="sxs-lookup"><span data-stu-id="2e044-171">The `InjectAttribute` of the base class is sufficient:</span></span>

```razor
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="2e044-172">在服務中使用 DI</span><span class="sxs-lookup"><span data-stu-id="2e044-172">Use DI in services</span></span>

<span data-ttu-id="2e044-173">複雜的服務可能需要額外的服務。</span><span class="sxs-lookup"><span data-stu-id="2e044-173">Complex services might require additional services.</span></span> <span data-ttu-id="2e044-174">在先前的範例中，`DataAccess` 可能需要 `HttpClient` 預設服務。</span><span class="sxs-lookup"><span data-stu-id="2e044-174">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="2e044-175">`@inject` （或 `InjectAttribute`）無法在服務中使用。</span><span class="sxs-lookup"><span data-stu-id="2e044-175">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="2e044-176">必須改為使用*函數插入*。</span><span class="sxs-lookup"><span data-stu-id="2e044-176">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="2e044-177">將參數新增至服務的函式，即可加入必要的服務。</span><span class="sxs-lookup"><span data-stu-id="2e044-177">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="2e044-178">當 DI 建立服務時，它會辨識它在此函式中所需的服務，並據以提供它們。</span><span class="sxs-lookup"><span data-stu-id="2e044-178">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="2e044-179">函式插入的必要條件：</span><span class="sxs-lookup"><span data-stu-id="2e044-179">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="2e044-180">其中一個函式必須存在，且其引數可由 DI 完成。</span><span class="sxs-lookup"><span data-stu-id="2e044-180">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="2e044-181">如果指定預設值，則允許 DI 未涵蓋的其他參數。</span><span class="sxs-lookup"><span data-stu-id="2e044-181">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="2e044-182">適用的函式必須是*公用*的。</span><span class="sxs-lookup"><span data-stu-id="2e044-182">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="2e044-183">其中一個適用的函數必須存在。</span><span class="sxs-lookup"><span data-stu-id="2e044-183">One applicable constructor must exist.</span></span> <span data-ttu-id="2e044-184">如果發生不明確的情況，DI 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2e044-184">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="2e044-185">用來管理 DI 範圍的公用程式基底元件類別</span><span class="sxs-lookup"><span data-stu-id="2e044-185">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="2e044-186">在 ASP.NET Core 應用程式中，限域服務的範圍通常是目前的要求。</span><span class="sxs-lookup"><span data-stu-id="2e044-186">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="2e044-187">要求完成之後，DI 系統會處置任何範圍或暫時性的服務。</span><span class="sxs-lookup"><span data-stu-id="2e044-187">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="2e044-188">在 Blazor 伺服器應用程式中，要求範圍會在用戶端連線期間持續進行，這可能會導致暫時性和範圍內的服務生活得比預期的長。</span><span class="sxs-lookup"><span data-stu-id="2e044-188">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span>

<span data-ttu-id="2e044-189">若要將服務的範圍限定在元件的存留期間，您可以使用 `OwningComponentBase` 並 `OwningComponentBase<TService>` 基類。</span><span class="sxs-lookup"><span data-stu-id="2e044-189">To scope services to the lifetime of a component, you can use the `OwningComponentBase` and `OwningComponentBase<TService>` base classes.</span></span> <span data-ttu-id="2e044-190">這些基類會公開類型 `IServiceProvider` 的 `ScopedServices` 屬性，其會解析範圍設定為元件存留期的服務。</span><span class="sxs-lookup"><span data-stu-id="2e044-190">These base classes expose a `ScopedServices` property of type `IServiceProvider` that resolve services that are scoped to the lifetime of the component.</span></span> <span data-ttu-id="2e044-191">若要撰寫繼承自 Razor 基類的元件，請使用 `@inherits` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="2e044-191">To author a component that inherits from a base class in Razor, use the `@inherits` directive.</span></span>

```razor
@page "/users"
@attribute [Authorize]
@inherits OwningComponentBase<Data.ApplicationDbContext>

<h1>Users (@Service.Users.Count())</h1>
<ul>
    @foreach (var user in Service.Users)
    {
        <li>@user.UserName</li>
    }
</ul>
```

> [!NOTE]
> <span data-ttu-id="2e044-192">使用 `@inject` 或 `InjectAttribute` 插入元件中的服務不會在元件的範圍內建立，並且會系結至要求範圍。</span><span class="sxs-lookup"><span data-stu-id="2e044-192">Services injected into the component using `@inject` or the `InjectAttribute` aren't created in the component's scope and are tied to the request scope.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2e044-193">其他資源</span><span class="sxs-lookup"><span data-stu-id="2e044-193">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
