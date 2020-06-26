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
# <a name="use-hub-filters-in-aspnet-core-signalr"></a>在 ASP.NET Core 中使用中樞篩選器SignalR

中樞篩選器：

* 適用于 ASP.NET Core 5.0 或更新版本。
* 允許邏輯在用戶端叫用中樞方法之前和之後執行。

本文提供撰寫和使用中樞篩選器的指導方針。

## <a name="configure-hub-filters"></a>設定中樞篩選器

中樞篩選器可以全域套用或依中樞類型套用。 篩選準則的加入順序是篩選準則的執行順序。 全域中樞篩選器會在本機中樞篩選器之前執行。

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

您可以使用下列其中一種方式來新增中樞篩選器：

* 依具體類型新增篩選：

    ```csharp
    hubOptions.AddFilter<TFilter>();
    ```

    這會從相依性插入（DI）或啟動類型中解析。

* 依執行時間類型新增篩選準則：

    ```csharp
    hubOptions.AddFilter(typeof(TFilter));
    ```

    這會從 DI 或啟動類型解析。

* 依實例新增篩選：

    ```csharp
    hubOptions.AddFilter(new MyFilter());
    ```

    這個實例將會使用，就像 singleton 一樣。 所有中樞方法調用都會使用相同的實例。

中樞篩選器會針對每個中樞叫用而建立和處置。 如果您想要將全域狀態儲存在篩選準則中，或沒有狀態，請將中樞篩選類型新增至 DI 做為單一，以獲得更好的效能。 或者，如果可以，請將篩選加入為實例。

## <a name="create-hub-filters"></a>建立中樞篩選器

藉由宣告繼承自的類別來建立篩選 `IHubFilter` ，然後新增 `InvokeMethodAsync` 方法。 此外，也 `OnConnectedAsync` `OnDisconnectedAsync` 可以選擇性地執行，以 `OnConnectedAsync` 分別包裝和 `OnDisconnectedAsync` 中樞方法。

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

篩選與中介軟體非常類似。 方法會叫用 `next` 下一個篩選準則。 最終的篩選準則將會叫用中樞方法。 在從傳回結果之前，篩選器也可以儲存等候 `next` 和執行邏輯的結果 `next` 。

若要略過篩選中的中樞方法叫用，請擲回類型的例外狀況， `HubException` 而不是呼叫 `next` 。 如果用戶端預期結果，則會收到錯誤。

## <a name="use-hub-filters"></a>使用中樞篩選器

撰寫篩選邏輯時，請嘗試使用中樞方法上的屬性，而不是檢查中樞方法名稱，將它設為泛型。

假設有一個篩選準則會檢查禁用片語的中樞方法引數，並將找到的任何片語取代為 `***` 。
在此範例中，假設已 `LanguageFilterAttribute` 定義類別。 類別具有名為的屬性 `FilterArgument` ，可以在使用屬性時設定。

1. 將屬性放在具有要清除之字串引數的中樞方法上：

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

1. 定義中樞篩選器以檢查屬性，並將中樞方法引數中的禁用片語取代為 `***` ：

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

1. 在方法中註冊中樞篩選準則 `Startup.ConfigureServices` 。 若要避免重新初始化每個調用的禁用片語清單，中樞篩選會註冊為 singleton：

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

## <a name="the-hubinvocationcontext-object"></a>HubInvocationCoNtext 物件

`HubInvocationContext`包含目前中樞方法叫用的資訊。

| 屬性 | 描述 | 類型 |
| ------ | ------ | ----------- |
| `Context ` | `HubCallerContext`包含連接的相關資訊。 | `HubCallerContext` |
| `Hub` | 用於此中樞方法叫用的中樞實例。 | `Hub` |
| `HubMethodName` | 所叫用之中樞方法的名稱。 | `string` |
| `HubMethodArguments` | 要傳遞至中樞方法的引數清單。 | `IReadOnlyList<string>` |
| `ServiceProvider` | 此中樞方法叫用的範圍服務提供者。 | `IServiceProvider` |
| `HubMethod` | 中樞方法資訊。 | `MethodInfo` |

## <a name="the-hublifetimecontext-object"></a>HubLifetimeCoNtext 物件

`HubLifetimeContext`包含 `OnConnectedAsync` 和中樞方法的資訊 `OnDisconnectedAsync` 。

| 屬性 | 描述 | 類型 |
| ------ | ------ | ----------- |
| `Context ` | `HubCallerContext`包含連接的相關資訊。 | `HubCallerContext` |
| `Hub` | 用於此中樞方法叫用的中樞實例。 | `Hub` |
| `ServiceProvider` | 此中樞方法叫用的範圍服務提供者。 | `IServiceProvider` |

## <a name="authorization-and-filters"></a>授權和篩選

在中樞篩選之前執行[的中樞方法上的屬性授權](xref:signalr/authn-and-authz#use-authorization-handlers-to-customize-hub-method-authorization)。
