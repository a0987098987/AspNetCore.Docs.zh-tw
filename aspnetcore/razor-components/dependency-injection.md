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
# <a name="razor-components-dependency-injection"></a>Razor 元件相依性插入

藉由[Rainer Stropek](https://www.timecockpit.com)

Razor 元件支援[相依性插入 (DI)](xref:fundamentals/dependency-injection)。 應用程式可以使用內建的服務，藉由插入元件。 應用程式也可以定義和註冊自訂的服務並提供給全透過 DI 的應用程式。

## <a name="dependency-injection"></a>相依性插入

DI 是一種技術來存取中央位置中所設定的服務。 這可用在 Razor 元件的應用程式：

* 服務類別的單一執行個體共用許多元件，稱為*singleton*服務。
* 使用參考抽象概念中分離從具象服務類別的元件。 例如，請考慮介面`IDataAccess`來存取應用程式中的資料。 介面由具體實作`DataAccess`類別，並註冊為應用程式的服務容器中的服務。 當元件會使用 DI 來接收`IDataAccess`元件的實作不具象型別結合。 實作可以交換，或許在單元測試中模擬 （mock） 實作。

如需詳細資訊，請參閱<xref:fundamentals/dependency-injection>。

## <a name="add-services-to-di"></a>將服務新增至 DI

建立新的應用程式之後, 檢查`Startup.ConfigureServices`方法：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

`ConfigureServices`傳遞給方法<xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>，這是一份服務描述元物件 (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>)。 服務會藉由提供服務集合的服務描述元加入。 下列範例示範使用概念`IDataAccess`介面和其具象實作`DataAccess`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

服務可以使用下表所示的存留期設定。

| 存留期 | 描述 |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | 建立 DI*單一執行個體*的服務。 需要的所有元件`Singleton`服務接收相同的服務執行個體。 |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | 每當元件取得的執行個體`Transient`服務從服務容器，它會接收*新的執行個體*的服務。 |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | 用戶端 Blazor 目前沒有 DI 領域的概念。 `Scoped` 行為類似`Singleton`。 不過，ASP.NET Core Razor 元件支援`Scoped`存留期。 在 Razor 元件中，連接範圍已設定領域的服務註冊。 基於這個理由，使用已設定領域的服務是慣用的服務，應受限於目前的使用者，即使目前的目的是要執行用戶端瀏覽器中。 |

DI 系統是以 ASP.NET Core 中的 DI 系統為基礎。 如需詳細資訊，請參閱<xref:fundamentals/dependency-injection>。

## <a name="default-services"></a>預設服務

預設的服務會自動新增至應用程式的服務集合。

| 服務 | 描述 |
| ------- | ----------- |
| <xref:System.Net.Http.HttpClient> | 提供方法來傳送 HTTP 要求，以及從 URI (singleton) 所識別的資源接收 HTTP 回應。 請注意，這個執行個體`HttpClient`處理 HTTP 流量，在背景中的使用瀏覽器。 [HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress)會自動設為應用程式的基底 URI 前置詞。 `HttpClient` 僅提供給用戶端 Blazor 應用程式。 |
| `IJSRuntime` | 表示可能會分派呼叫的 JavaScript 執行階段的執行個體。 如需詳細資訊，請參閱<xref:razor-components/javascript-interop>。 |
| `IUriHelper` | 使用 Uri 和瀏覽狀態 (singleton) 包含協助程式。 `IUriHelper` 會提供給 Blazor 和 Razor 元件應用程式。 |

可以使用自訂服務提供者，而新增的預設範本的預設服務提供者。 自訂服務提供者不會自動提供資料表中列出的預設服務。 如果您使用自訂服務提供者，而且需要的任何服務表所示，加入新的服務提供者所需的服務。

## <a name="request-a-service-in-a-component"></a>要求中元件的服務

服務集合中加入的服務之後，則將服務插入元件的 Razor 範本使用[ @inject ](xref:mvc/views/razor#section-4) Razor 指示詞。 `@inject` 有兩個參數：

* 類型名稱：要插入的服務型別。
* 屬性名稱：接收插入應用程式服務的屬性名稱。 請注意，屬性不需要手動建立。 編譯器會建立屬性。

如需詳細資訊，請參閱<xref:mvc/views/dependency-injection>。

使用多個`@inject`陳述式來插入不同的服務。

下列範例示範如何使用 `@inject`。 服務實作`Services.IDataAccess`元件的屬性不會插入`DataRepository`。 請注意程式碼方式只使用`IDataAccess`抽象概念：

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.cshtml?highlight=2-3,23)]

就內部而言，產生的屬性 (`DataRepository`) 以裝飾`InjectAttribute`屬性。 一般而言，這個屬性不是直接使用。 如果基底類別是必要元件，而且插入的屬性也是必要的基底類別，如`InjectAttribute`可以手動新增：

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

在衍生自基底類別中，元件`@inject`指示詞不需要。 `InjectAttribute`基底類別已足夠：

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="dependency-injection-in-services"></a>在服務中的相依性插入

複雜的服務可能需要額外的服務。 在先前範例中，`DataAccess`可能需要`HttpClient`預設服務。 `@inject` (或`InjectAttribute`) 不適用於在 services 中使用。 *建構函式插入*必須改為使用。 將參數加入至服務的建構函式時，會新增必要的服務。 當相依性插入建立服務時，它會辨識它需要在建構函式，並據以提供它們的服務。

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

建構函式插入的必要條件：

* 必須要有一個建構函式，其引數可以而完成的相依性插入。 請注意，如果他們指定預設值允許使用 DI 未涵蓋的其他參數。
* 適用的建構函式必須是*公開*。
* 只能有一個適用的建構函式。 發生模稜兩可，DI 會擲回例外狀況。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
