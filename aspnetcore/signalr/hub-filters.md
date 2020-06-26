---
title: 在 ASP.NET Core 中使用中樞篩選器SignalR
author: brecon
description: 瞭解如何在 ASP.NET Core 中使用中樞篩選器 SignalR 。
monikerRange: '>= aspnetcore-5.0'
ms.author: brecon
ms.custom: mvc
ms.date: 05/22/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/hub-filters
ms.openlocfilehash: c7ba0fff8bca53e2d6d12add693ee391ffa789ca
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408561"
---
# <a name="use-hub-filters-in-aspnet-core-signalr"></a><span data-ttu-id="7cbc2-103">在 ASP.NET Core 中使用中樞篩選器SignalR</span><span class="sxs-lookup"><span data-stu-id="7cbc2-103">Use hub filters in ASP.NET Core SignalR</span></span>

<span data-ttu-id="7cbc2-104">中樞篩選器：</span><span class="sxs-lookup"><span data-stu-id="7cbc2-104">Hub filters:</span></span>

* <span data-ttu-id="7cbc2-105">適用于 ASP.NET Core 5.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-105">Are available in ASP.NET Core 5.0 or later.</span></span>
* <span data-ttu-id="7cbc2-106">允許邏輯在用戶端叫用中樞方法之前和之後執行。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-106">Allow logic to run before and after hub methods are invoked by clients.</span></span>

<span data-ttu-id="7cbc2-107">本文提供撰寫和使用中樞篩選器的指導方針。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-107">This article provides guidance for writing and using hub filters.</span></span>

## <a name="configure-hub-filters"></a><span data-ttu-id="7cbc2-108">設定中樞篩選器</span><span class="sxs-lookup"><span data-stu-id="7cbc2-108">Configure hub filters</span></span>

<span data-ttu-id="7cbc2-109">中樞篩選器可以全域套用或依中樞類型套用。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-109">Hub filters can be applied globally or per hub type.</span></span> <span data-ttu-id="7cbc2-110">篩選準則的加入順序是篩選準則的執行順序。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-110">The order in which filters are added is the order in which the filters run.</span></span> <span data-ttu-id="7cbc2-111">全域中樞篩選器會在本機中樞篩選器之前執行。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-111">Global hub filters run before local hub filters.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(options =>
    {
        // Global filters will run first
        options.AddFilter<CustomFilter>();
    }).AddHubOptions<ChatHub>(options =>
    {
        // Local filters will run second
        options.AddFilter<CustomFilter2>();
    });
}
```

<span data-ttu-id="7cbc2-112">您可以使用下列其中一種方式來新增中樞篩選器：</span><span class="sxs-lookup"><span data-stu-id="7cbc2-112">A hub filter can be added in one of the following ways:</span></span>

* <span data-ttu-id="7cbc2-113">依具體類型新增篩選：</span><span class="sxs-lookup"><span data-stu-id="7cbc2-113">Add a filter by concrete type:</span></span>

    ```csharp
    hubOptions.AddFilter<TFilter>();
    ```

    <span data-ttu-id="7cbc2-114">這會從相依性插入（DI）或啟動類型中解析。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-114">This will be resolved from dependency injection (DI) or type activated.</span></span>

* <span data-ttu-id="7cbc2-115">依執行時間類型新增篩選準則：</span><span class="sxs-lookup"><span data-stu-id="7cbc2-115">Add a filter by runtime type:</span></span>

    ```csharp
    hubOptions.AddFilter(typeof(TFilter));
    ```

    <span data-ttu-id="7cbc2-116">這會從 DI 或啟動類型解析。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-116">This will be resolved from DI or type activated.</span></span>

* <span data-ttu-id="7cbc2-117">依實例新增篩選：</span><span class="sxs-lookup"><span data-stu-id="7cbc2-117">Add a filter by instance:</span></span>

    ```csharp
    hubOptions.AddFilter(new MyFilter());
    ```

    <span data-ttu-id="7cbc2-118">這個實例將會使用，就像 singleton 一樣。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-118">This instance will be used like a singleton.</span></span> <span data-ttu-id="7cbc2-119">所有中樞方法調用都會使用相同的實例。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-119">All hub method invocations will use the same instance.</span></span>

<span data-ttu-id="7cbc2-120">中樞篩選器會針對每個中樞叫用而建立和處置。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-120">Hub filters are created and disposed per hub invocation.</span></span> <span data-ttu-id="7cbc2-121">如果您想要將全域狀態儲存在篩選準則中，或沒有狀態，請將中樞篩選類型新增至 DI 做為單一，以獲得更好的效能。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-121">If you want to store global state in the filter, or no state, add the hub filter type to DI as a singleton for better performance.</span></span> <span data-ttu-id="7cbc2-122">或者，如果可以，請將篩選加入為實例。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-122">Alternatively, add the filter as an instance if you can.</span></span>

## <a name="create-hub-filters"></a><span data-ttu-id="7cbc2-123">建立中樞篩選器</span><span class="sxs-lookup"><span data-stu-id="7cbc2-123">Create hub filters</span></span>

<span data-ttu-id="7cbc2-124">藉由宣告繼承自的類別來建立篩選 `IHubFilter` ，然後新增 `InvokeMethodAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-124">Create a filter by declaring a class that inherits from `IHubFilter`, and add the `InvokeMethodAsync` method.</span></span> <span data-ttu-id="7cbc2-125">此外，也 `OnConnectedAsync` `OnDisconnectedAsync` 可以選擇性地執行，以 `OnConnectedAsync` 分別包裝和 `OnDisconnectedAsync` 中樞方法。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-125">There is also `OnConnectedAsync` and `OnDisconnectedAsync` that can optionally be implemented to wrap the `OnConnectedAsync` and `OnDisconnectedAsync` hub methods respectively.</span></span>

```csharp
public class CustomFilter : IHubFilter
{
    public async ValueTask<object> InvokeMethodAsync(
        HubInvocationContext invocationContext, Func<HubInvocationContext, ValueTask<object>> next)
    {
        Console.WriteLine($"Calling hub method '{invocationContext.HubMethodName}'");
        try
        {
            return await next(invocationContext);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Exception calling '{invocationContext.HubMethodName}'");
            throw ex;
        }
    }

    // Optional method
    public Task OnConnectedAsync(HubLifetimeContext context, Func<HubLifetimeContext, Task> next)
    {
        return next(context);
    }

    // Optional method
    public Task OnDisconnectedAsync(
        HubLifetimeContext context, Exception exception, Func<HubLifetimeContext, Exception, Task> next)
    {
        return next(context, exception);
    }
}
```

<span data-ttu-id="7cbc2-126">篩選與中介軟體非常類似。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-126">Filters are very similar to middleware.</span></span> <span data-ttu-id="7cbc2-127">方法會叫用 `next` 下一個篩選準則。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-127">The `next` method invokes the next filter.</span></span> <span data-ttu-id="7cbc2-128">最終的篩選準則將會叫用中樞方法。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-128">The final filter will invoke the hub method.</span></span> <span data-ttu-id="7cbc2-129">在從傳回結果之前，篩選器也可以儲存等候 `next` 和執行邏輯的結果 `next` 。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-129">Filters can also store the result from awaiting `next` and run logic after the hub method has been called before returning the result from `next`.</span></span>

<span data-ttu-id="7cbc2-130">若要略過篩選中的中樞方法叫用，請擲回類型的例外狀況， `HubException` 而不是呼叫 `next` 。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-130">To skip a hub method invocation in a filter, throw an exception of type `HubException` instead of calling `next`.</span></span> <span data-ttu-id="7cbc2-131">如果用戶端預期結果，則會收到錯誤。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-131">The client will receive an error if it was expecting a result.</span></span>

## <a name="use-hub-filters"></a><span data-ttu-id="7cbc2-132">使用中樞篩選器</span><span class="sxs-lookup"><span data-stu-id="7cbc2-132">Use hub filters</span></span>

<span data-ttu-id="7cbc2-133">撰寫篩選邏輯時，請嘗試使用中樞方法上的屬性，而不是檢查中樞方法名稱，將它設為泛型。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-133">When writing the filter logic, try to make it generic by using attributes on hub methods instead of checking for hub method names.</span></span>

<span data-ttu-id="7cbc2-134">假設有一個篩選準則會檢查禁用片語的中樞方法引數，並將找到的任何片語取代為 `***` 。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-134">Consider a filter that will check a hub method argument for banned phrases and replace any phrases it finds with `***`.</span></span>
<span data-ttu-id="7cbc2-135">在此範例中，假設已 `LanguageFilterAttribute` 定義類別。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-135">For this example, assume a `LanguageFilterAttribute` class is defined.</span></span> <span data-ttu-id="7cbc2-136">類別具有名為的屬性 `FilterArgument` ，可以在使用屬性時設定。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-136">The class has a property named `FilterArgument` that can be set when using the attribute.</span></span>

1. <span data-ttu-id="7cbc2-137">將屬性放在具有要清除之字串引數的中樞方法上：</span><span class="sxs-lookup"><span data-stu-id="7cbc2-137">Place the attribute on the hub method that has a string argument to be cleaned:</span></span>

    ```csharp
    public class ChatHub
    {
        [LanguageFilter(filterArgument: 0)]
        public async Task SendMessage(string message, string username)
        {
            await Clients.All.SendAsync("SendMessage", $"{username} says: {message}");
        }
    }
    ```

1. <span data-ttu-id="7cbc2-138">定義中樞篩選器以檢查屬性，並將中樞方法引數中的禁用片語取代為 `***` ：</span><span class="sxs-lookup"><span data-stu-id="7cbc2-138">Define a hub filter to check for the attribute and replace banned phrases in a hub method argument with `***`:</span></span>

    ```csharp
    public class LanguageFilter : IHubFilter
    {
        // populated from a file or inline
        private List<string> bannedPhrases = new List<string> { "async void", ".Result" };

        public async ValueTask<object> InvokeMethodAsync(HubInvocationContext invocationContext, 
            Func<HubInvocationContext, ValueTask<object>> next)
        {
            var languageFilter = (LanguageFilterAttribute)Attribute.GetCustomAttribute(
                invocationContext.HubMethod, typeof(LanguageFilterAttribute));
            if (languageFilter != null &&
                invocationContext.HubMethodArguments.Count > languageFilter.FilterArgument &&
                invocationContext.HubMethodArguments[languageFilter.FilterArgument] is string str)
            {
                foreach (var bannedPhrase in bannedPhrases)
                {
                    str = str.Replace(bannedPhrase, "***");
                }

                arguments = invocationContext.HubMethodArguments.ToArray();
                arguments[languageFilter.FilterArgument] = str;
                invocationContext = new HubInvocationContext(invocationContext.Context,
                    invocationContext.ServiceProvider,
                    invocationContext.Hub,
                    invocationContext.HubMethod,
                    arguments);
            }

            return await next(invocationContext);
        }
    }
    ```

1. <span data-ttu-id="7cbc2-139">在方法中註冊中樞篩選準則 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-139">Register the hub filter in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="7cbc2-140">若要避免重新初始化每個調用的禁用片語清單，中樞篩選會註冊為 singleton：</span><span class="sxs-lookup"><span data-stu-id="7cbc2-140">To avoid reinitializing the banned phrases list for every invocation, the hub filter is registered as a singleton:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSignalR(hubOptions =>
        {
            hubOptions.AddFilter<LanguageFilter>();
        });
    
        services.AddSingleton<LanguageFilter>();
    }
    ```

## <a name="the-hubinvocationcontext-object"></a><span data-ttu-id="7cbc2-141">HubInvocationCoNtext 物件</span><span class="sxs-lookup"><span data-stu-id="7cbc2-141">The HubInvocationContext object</span></span>

<span data-ttu-id="7cbc2-142">`HubInvocationContext`包含目前中樞方法叫用的資訊。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-142">The `HubInvocationContext` contains information for the current hub method invocation.</span></span>

| <span data-ttu-id="7cbc2-143">屬性</span><span class="sxs-lookup"><span data-stu-id="7cbc2-143">Property</span></span> | <span data-ttu-id="7cbc2-144">描述</span><span class="sxs-lookup"><span data-stu-id="7cbc2-144">Description</span></span> | <span data-ttu-id="7cbc2-145">類型</span><span class="sxs-lookup"><span data-stu-id="7cbc2-145">Type</span></span> |
| ------ | ------ | ----------- |
| `Context ` | <span data-ttu-id="7cbc2-146">`HubCallerContext`包含連接的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-146">The `HubCallerContext` contains information about the connection.</span></span> | `HubCallerContext` |
| `Hub` | <span data-ttu-id="7cbc2-147">用於此中樞方法叫用的中樞實例。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-147">The instance of the Hub being used for this hub method invocation.</span></span> | `Hub` |
| `HubMethodName` | <span data-ttu-id="7cbc2-148">所叫用之中樞方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-148">The name of the hub method being invoked.</span></span> | `string` |
| `HubMethodArguments` | <span data-ttu-id="7cbc2-149">要傳遞至中樞方法的引數清單。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-149">The list of arguments being passed to the hub method.</span></span> | `IReadOnlyList<string>` |
| `ServiceProvider` | <span data-ttu-id="7cbc2-150">此中樞方法叫用的範圍服務提供者。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-150">The scoped service provider for this hub method invocation.</span></span> | `IServiceProvider` |
| `HubMethod` | <span data-ttu-id="7cbc2-151">中樞方法資訊。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-151">The hub method information.</span></span> | `MethodInfo` |

## <a name="the-hublifetimecontext-object"></a><span data-ttu-id="7cbc2-152">HubLifetimeCoNtext 物件</span><span class="sxs-lookup"><span data-stu-id="7cbc2-152">The HubLifetimeContext object</span></span>

<span data-ttu-id="7cbc2-153">`HubLifetimeContext`包含 `OnConnectedAsync` 和中樞方法的資訊 `OnDisconnectedAsync` 。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-153">The `HubLifetimeContext` contains information for the `OnConnectedAsync` and `OnDisconnectedAsync` hub methods.</span></span>

| <span data-ttu-id="7cbc2-154">屬性</span><span class="sxs-lookup"><span data-stu-id="7cbc2-154">Property</span></span> | <span data-ttu-id="7cbc2-155">描述</span><span class="sxs-lookup"><span data-stu-id="7cbc2-155">Description</span></span> | <span data-ttu-id="7cbc2-156">類型</span><span class="sxs-lookup"><span data-stu-id="7cbc2-156">Type</span></span> |
| ------ | ------ | ----------- |
| `Context ` | <span data-ttu-id="7cbc2-157">`HubCallerContext`包含連接的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-157">The `HubCallerContext` contains information about the connection.</span></span> | `HubCallerContext` |
| `Hub` | <span data-ttu-id="7cbc2-158">用於此中樞方法叫用的中樞實例。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-158">The instance of the Hub being used for this hub method invocation.</span></span> | `Hub` |
| `ServiceProvider` | <span data-ttu-id="7cbc2-159">此中樞方法叫用的範圍服務提供者。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-159">The scoped service provider for this hub method invocation.</span></span> | `IServiceProvider` |

## <a name="authorization-and-filters"></a><span data-ttu-id="7cbc2-160">授權和篩選</span><span class="sxs-lookup"><span data-stu-id="7cbc2-160">Authorization and filters</span></span>

<span data-ttu-id="7cbc2-161">在中樞篩選之前執行[的中樞方法上的屬性授權](xref:signalr/authn-and-authz#use-authorization-handlers-to-customize-hub-method-authorization)。</span><span class="sxs-lookup"><span data-stu-id="7cbc2-161">[Authorize attributes on hub methods](xref:signalr/authn-and-authz#use-authorization-handlers-to-customize-hub-method-authorization) run before hub filters.</span></span>
