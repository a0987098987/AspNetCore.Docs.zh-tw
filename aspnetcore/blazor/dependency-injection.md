---
title: ASP.NET Core Blazor 相依性插入
author: guardrex
description: 瞭解 Blazor apps 如何將服務插入元件中。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: blazor/dependency-injection
ms.openlocfilehash: 074d7a669c900eb242c8329147b28d1c50652915
ms.sourcegitcommit: e5a74f882c14eaa0e5639ff082355e130559ba83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2019
ms.locfileid: "71168083"
---
# <a name="aspnet-core-blazor-dependency-injection"></a>ASP.NET Core Blazor 相依性插入

依[Rainer Stropek](https://www.timecockpit.com)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor 支援相依性[插入（DI）](xref:fundamentals/dependency-injection)。 應用程式可以使用內建的服務，方法是將它們插入元件中。 應用程式也可以定義和註冊自訂服務，並透過 DI 讓它們可在整個應用程式中使用。

DI 是用來存取集中位置所設定之服務的技術。 在 Blazor 應用程式中，這會很有用：

* 跨許多元件（稱為*單一*服務）共用服務類別的單一實例。
* 使用參考抽象，將元件與具體的服務類別分離。 例如，請考慮使用介面`IDataAccess`來存取應用程式中的資料。 介面是由具象`DataAccess`類別來執行，並在應用程式的服務容器中註冊為服務。 當元件使用 DI 來接收實作為`IDataAccess`時，該元件不會結合到具體類型。 可以交換執行，也許是針對單元測試中的 mock 執行。

## <a name="default-services"></a>預設服務

預設服務會自動新增至應用程式的服務集合。

| 服務 | 存留期 | 描述 |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | 單一 | 提供方法來傳送 HTTP 要求，以及從 URI 所識別的資源接收 HTTP 回應。 請注意，這個實例`HttpClient`會使用瀏覽器來處理背景中的 HTTP 流量。 [HttpClient](xref:System.Net.Http.HttpClient.BaseAddress)會自動設定為應用程式的基底 URI 前置詞。 如需詳細資訊，請參閱<xref:blazor/call-web-api>。 |
| `IJSRuntime` | 單一 | 代表在其中分派 JavaScript 呼叫的 JavaScript 執行時間實例。 如需詳細資訊，請參閱<xref:blazor/javascript-interop>。 |
| `NavigationManager` | 單一 | 包含使用 Uri 和導覽狀態的協助程式。 如需詳細資訊，請參閱[URI 和流覽狀態](xref:blazor/routing#uri-and-navigation-state-helpers)協助程式。 |

自訂服務提供者不會自動提供表格中所列的預設服務。 如果您使用自訂服務提供者，而且需要資料表中所顯示的任何服務，請將所需的服務新增至新的服務提供者。

## <a name="add-services-to-an-app"></a>將服務新增至應用程式

建立新的應用程式之後，請`Startup.ConfigureServices`檢查方法：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

會傳遞方法，這是服務描述元物件（<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>）的清單。 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> `ConfigureServices` 服務會藉由將服務描述項提供給服務集合來新增。 下列範例示範`IDataAccess`介面和其具體執行`DataAccess`的概念：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

您可以使用下表所示的存留期來設定服務。

| 存留期 | 描述 |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor WebAssembly apps 目前不具有 DI 範圍的概念。 `Scoped`註冊的服務的行為`Singleton`就像服務一樣。 不過，Blazor 伺服器裝載模型支援`Scoped`存留期。 在 Blazor 伺服器應用程式中，限定範圍的服務註冊的範圍是*連接*。 因此，即使目前的意圖是在瀏覽器中執行用戶端，使用範圍服務也適用于應該範圍設定為目前使用者的服務。 |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | DI 會建立服務的*單一實例*。 所有需要`Singleton`服務的元件都會收到相同服務的實例。 |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | 每當元件從服務容器取得`Transient`服務的實例時，就會收到服務的*新實例*。 |

DI 系統是以 ASP.NET Core 中的 DI 系統為基礎。 如需詳細資訊，請參閱<xref:fundamentals/dependency-injection>。

## <a name="request-a-service-in-a-component"></a>要求元件中的服務

將服務新增至服務集合之後，請使用[ \@插入](xref:mvc/views/razor#inject)Razor 指示詞將服務插入元件中。 `@inject`有兩個參數：

* 輸入&ndash;要插入之服務的類型。
* 屬性&ndash;接收插入的應用程式服務之屬性的名稱。 屬性不需要手動建立。 編譯器會建立屬性。

如需詳細資訊，請參閱<xref:mvc/views/dependency-injection>。

使用多`@inject`個語句來插入不同的服務。

下列範例示範如何使用 `@inject`。 執行`Services.IDataAccess`的服務會插入元件的屬性`DataRepository`中。 請注意程式碼如何使用`IDataAccess`抽象概念：

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

就內部`DataRepository` `InjectAttribute`而言，產生的屬性（）會以屬性裝飾。 通常不會直接使用這個屬性。 如果元件需要基類，而且基類也需要插入的`InjectAttribute`屬性，請手動新增：

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

在衍生自基類的元件中， `@inject`不需要指示詞。 `InjectAttribute`基類的已足夠：

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a>在服務中使用 DI

複雜的服務可能需要額外的服務。 在先前的範例中`DataAccess` ，可能`HttpClient`需要預設服務。 `@inject`（或`InjectAttribute`）無法在服務中使用。 必須改為使用*函數插入*。 將參數新增至服務的函式，即可加入必要的服務。 當 DI 建立服務時，它會辨識它在此函式中所需的服務，並據以提供它們。

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

函式插入的必要條件：

* 其中一個函式必須存在，且其引數可由 DI 完成。 如果指定預設值，則允許 DI 未涵蓋的其他參數。
* 適用的函式必須是*公用*的。
* 其中一個適用的函數必須存在。 如果發生不明確的情況，DI 會擲回例外狀況。

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a>用來管理 DI 範圍的公用程式基底元件類別

在 ASP.NET Core 應用程式中，限域服務的範圍通常是目前的要求。 要求完成之後，DI 系統會處置任何範圍或暫時性的服務。 在 Blazor 伺服器應用程式中，要求範圍會持續存在用戶端連線的持續時間，這可能會導致暫時性和範圍內的服務存留得比預期的長。

若要將服務的範圍設為元件的存留期， `OwningComponentBase`可以`OwningComponentBase<TService>`使用和基類。 這些基類會公開類型`ScopedServices` `IServiceProvider`的屬性，其會解析範圍設定為元件存留期的服務。 若要撰寫繼承自 Razor 基類的元件，請使用`@inherits`指示詞。

```cshtml
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
> 使用`@inject`或插入元件的`InjectAttribute`服務不會建立在元件的範圍內，而且會系結至要求範圍。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
