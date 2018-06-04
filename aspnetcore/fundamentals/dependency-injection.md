---
title: .NET Core 中的相依性插入
author: ardalis
description: 了解 ASP.NET Core 如何實作相依性插入以及如何使用它。
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/dependency-injection
ms.openlocfilehash: 14c3d464773fe78a563a27776bfcd124c22df134
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2018
ms.locfileid: "34566954"
---
# <a name="dependency-injection-in-aspnet-core"></a>.NET Core 中的相依性插入

<a name="fundamentals-dependency-injection"></a>

作者：[Steve Smith](https://ardalis.com/) 和 [Scott Addie](https://scottaddie.com)

ASP.NET Core 的設計完全是為了支援並利用相依性插入。 ASP.NET Core 應用程式可以藉由讓內建的架構服務插入至 Startup 類別中的方法來利用它們，也可以設定應用程式服務以便插入。 ASP.NET Core 所提供的預設服務容器會提供最少的功能集，並不旨在取代其他容器。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-dependency-injection"></a>相依性插入是什麼？

相依性插入 (DI) 是為了達到物件和共同作業者之間鬆散結合 (也就是相依性) 的技術。 類別要執行其動作所需的物件，會以某些方式提供給類別，而不是直接具現化共同作業者，或使用靜態參考。 大多數情況下，類別會透過其建構函式宣告它們的相依性，讓它們能遵循[明確相依性準則](http://deviq.com/explicit-dependencies-principle/)。 這種方法稱為「建構函式插入」。

當類別專為 DI 設計時，它們會較為鬆散結合，因為它們對於其共同作業者沒有硬式編碼的直接相依性。 這遵循了[相依性反向準則](http://deviq.com/dependency-inversion-principle/)，其指出「高層級模組不應該相依於低層級模組；兩者都應該相依於抽象概念」。 類別會要求在建構類別時提供給它們的抽象概念 (通常是 `interfaces`)，而不是參考特定實作。 將相依性擷取成介面，並提供這些介面的實作作為參數，也是[策略設計模式](http://deviq.com/strategy-design-pattern/)的範例。

當系統設計為使用 DI，並且有許多類別透過其建構函式 (或屬性) 要求相依性時，讓類別專門用來建立這些類別及其相關聯相依性會很有助益。 這些類別統稱為「容器」，或是更具體來說，稱為[逆轉控制 (IoC)](http://deviq.com/inversion-of-control/) 容器或相依性插入 (DI) 容器。 容器基本上是負責提供向它要求的類別之執行個體的 actory。 如果指定的類型已經宣告它有相依性，且容器已設定為提供相依性類型，則它會在建立所要求的執行個體時建立相依性。 如此一來，可以提供類別複雜的相依性圖形，而不需要任何硬式編碼的物件建構。 除了建立具有相依性的物件之外，容器通常也會管理應用程式中的物件存留期。

ASP.NET Core 包含簡單的內建容器 (由 `IServiceProvider` 介面代表)，它預設會支援建構函式插入，而 ASP.NET 則透過 DI 提供特定服務。 ASP.NET 的容器將其管理的類型稱為「服務」。 本文的其餘部分中，「服務」將會指 ASP.NET Core IoC 容器所管理的類型。 您在應用程式 `Startup` 類別的 `ConfigureServices` 方法中設定內建容器的服務。

> [!NOTE]
> Martin Fowler 撰寫了關於 [Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html) (逆轉控制容器和相依性插入模式) 的詳盡文章。 Microsoft Patterns and Practices 也對[相依性插入](https://msdn.microsoft.com/library/hh323705.aspx)有很棒的描述。

> [!NOTE]
> 本文介紹相依性插入適用於所有 ASP.NET 應用程式的方面。 MVC 控制器內的相依性插入，於[相依性插入和控制器](../mvc/controllers/dependency-injection.md)中介紹。

### <a name="constructor-injection-behavior"></a>建構函式插入行為

建構函式插入需要討論中的建構函式為「公用」。 否則，您的應用程式將會擲回 `InvalidOperationException`：

> 找不到類型 'YourType' 的合適建構函式。 請確定類型是具象類型，且已針對公用建構函式的所有參數註冊服務。

建構函式插入需要僅存在一個適用的建構函式。 支援建構函式多載，但只能有一個多載存在，其引數可以藉由相依性插入而完成。 如果有多個存在，您的應用程式將會擲回 `InvalidOperationException`：

> 類型 'YourType' 中已找到接受所有指定引數類型的多個建構函式。 只能有一個適用的建構函式。

建構函式可以接受不是由相依性插入提供的引數，但這些引數必須支援預設值。 例如: 

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

## <a name="using-framework-provided-services"></a>使用架構提供的服務

`Startup` 類別中的 `ConfigureServices` 方法負責定義應用程式將使用的服務，包括像是 Entity Framework Core 與 ASP.NET Core MVC 等平台功能。 一開始，提供給 `ConfigureServices` 的 `IServiceCollection` 具有下列已定義的服務 (取決於[主機的設定方式](xref:fundamentals/host/index))：

| 服務類型 | 存留期 |
| ----- | ------- |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | 單一 |
| [Microsoft.Extensions.Logging.ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | 單一 |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](/dotnet/api/microsoft.extensions.logging.ilogger) | 單一 |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | 暫時性 |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | 暫時性 |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.ioptions-1) | 單一 |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | 單一 |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | 單一 |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | 暫時性 |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | 單一 |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | 暫時性 |
| [Microsoft.AspNetCore.Hosting.Server.IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | 單一 |
| [Microsoft.AspNetCore.Hosting.IStartup](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | 單一 |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | 單一 |

以下範例說明如何使用許多擴充方法，例如 `AddDbContext`、`AddIdentity` 和 `AddMvc`，將其他服務新增至容器。

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

ASP.NET 提供的功能與中介軟體，例如 MVC，會遵循使用單一 Add*ServiceName* 的擴充方法註冊該功能所需之所有服務的慣例。

> [!TIP]
> 您可以透過 `Startup` 方法的參數清單在方法內要求特定的架構提供服務 - 如需詳細資料，請參閱[應用程式啟動](startup.md)。

## <a name="registering-services"></a>註冊服務

您可以註冊自己的應用程式服務，如下所示。 第一個泛型型別代表將從容器要求的類型 (通常是介面)。 第二個泛型型別代表容器將具現化且用來完成這類要求的具象類型。

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> 每個 `services.Add<ServiceName>` 擴充方法都會新增 (並且可能會設定) 服務。 例如，`services.AddMvc()` 會新增 MVC 要求的服務。 建議您遵循這個慣例，並將擴充方法放在 `Microsoft.Extensions.DependencyInjection` 命名空間中，來封裝服務註冊的群組。

`AddTransient` 方法用來針對每個需要它的物件，將抽象類型對應至個別具現化的具象服務。 這稱為服務的「存留期」，其他的存留期選項描述如下。 請務必為每個您註冊的服務選擇適當的存留期。 服務的新執行個體應該提供給每個要求它的類別嗎？ 一個執行個體應該用於整個給定 Web 要求嗎？ 或是單一執行個體應該用於應用程式的存留期嗎？

在本文的範例中，有顯示字元名稱的簡單控制器，稱為 `CharactersController`。 其 `Index` 方法會顯示目前已儲存在應用程式中的字元清單，並初始化少數字元的集合，如果尚無集合存在的話。 請注意，雖然此應用程式為了持續性使用 Entity Framework Core 和 `ApplicationDbContext` 類別，但那在控制器中都不明顯。 相反地，在介面背後已抽象化特定資料存取機制 `ICharacterRepository`，它遵循[儲存機制模式](http://deviq.com/repository-pattern/)。 已透過建構函式要求 `ICharacterRepository` 的執行個體，並指派給私用欄位，然後視需要使用該欄位來存取字元。

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

`ICharacterRepository` 定義了控制器搭配 `Character` 執行個體所需要的兩個方法。

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

接著這個介面由具象類型 `CharacterRepository` 實作，用於執行階段。

> [!NOTE]
> DI 搭配 `CharacterRepository` 類別使用的方式是一個一般模型，您可以針對應用程式服務遵循該模型，而不只是在「儲存機制」或資料存取類別中。

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

請注意，`CharacterRepository` 在其建構函式中會要求 `ApplicationDbContext`。 相依性插入用於像這樣的鏈結方式並不少見，每個要求的相依性接著都會要求它自己的相依性。 容器負責解決圖形中的所有相依性，並傳回完全解析的服務。

> [!NOTE]
> 建立所要求的物件、它需要的所有物件，以及那些物件需要的所有物件，有時稱為「物件圖形」。 同樣地，必須先解析的相依性集合組通常稱為「相依性樹狀結構」或「相依性圖形」。

在此情況下，`ICharacterRepository` 以及 `ApplicationDbContext` 都必須向 `Startup` 中 `ConfigureServices` 的服務容器註冊。 `ApplicationDbContext` 的設定是藉由呼叫擴充方法 `AddDbContext<T>` 進行。 下列程式碼顯示 `CharacterRepository` 類型的註冊。

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

Entity Framework 內容應該使用 `Scoped` 存留期新增至服務容器。 如果您使用上述的協助程式方法，便會自動處理這點。 將利用 Entity Framework 的儲存機制應該使用相同的存留期。

> [!WARNING]
> 要小心的主要危險是從單一項目解析 `Scoped` 服務。 很可能在此情況下，處理後續要求時，服務會有不正確的狀態。

具有相依性的服務應該在容器中註冊它們。 如果服務的建構函式需要基本類型，例如 `string`，這可以藉由使用[設定](xref:fundamentals/configuration/index)和[選項模式](xref:fundamentals/configuration/options)來插入。

## <a name="service-lifetimes-and-registration-options"></a>服務存留期和註冊選項

ASP.NET 服務可以使用下列存留期進行設定：

**暫時性**

每次要求暫時性存留期服務時都會建立它們。 此存留期最適合用於輕量型的無狀態服務。

**具範圍**

具範圍存留期服務會針對每個要求建立一次。

> [!WARNING]
> 若要在中介軟體中使用範圍服務，諘將該服務入 `Invoke` 或 `InvokeAsync` 方法中。 因為其會強制服務執行單一服務的行為，所以請勿透過建構函式插入進行插入。

**單一**

單一存留期服務會在第一次要求它們時建立 (或是當執行 `ConfigureServices` 時，如果您指定了執行個體)，然後每個後續的要求將會使用相同的執行個體。 如果您的應用程式需要單一行為，則建議允許服務容器管理服務的存留期，而不是實作單一設計模式，並自行管理類別中的物件存留期。

有數種方法可以向容器註冊服務。 我們已經看過如何藉由指定要使用的具象類型，註冊具有指定類型的服務實作。 此外，可以指定 Factory，然後將它用來視需要建立執行個體。 第三種方法是直接指定要使用之類型的執行個體，在這種情況下，容器將永遠不會嘗試建立執行個體 (也不會處置執行個體)。

為了示範這些存留期和註冊選項之間的差異，請考慮一個簡單的介面，以具有唯一識別碼 `OperationId` 的「作業」代表一或多個工作。 根據我們設定此服務存留期的方式，容器會提供相同或不同的服務執行個體給要求的類別。 為了清楚表示所要求的存留期，我們將為每個存留期選項建立一個類型：

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

我們會使用單一類別 `Operation` 來實作這些介面，它在其建構函式接受 `Guid`，如果未提供則使用新的 `Guid`。

接下來，在 `ConfigureServices` 中，每個類型會根據其具名存留期新增至容器：

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

請注意，`IOperationSingletonInstance` 服務使用特定執行個體，它具有已知識別碼 `Guid.Empty`，因此可以清楚知道此類型在使用中的時候 (其 Guid 會是全部為零)。 我們也已註冊 `OperationService`，它相依於每一個其他 `Operation` 類型，因此可以清楚知道在要求內這個服務針對每個作業類型是獲得與控制器相同的執行個體，還是新的執行個體。 此服務所做的就是將其相依性公開為屬性，以便在檢視中顯示。

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

為了示範個別應用程式要求內與之間的物件存留期，範例包含了 `OperationsController`，它會要求每種 `IOperation` 類型，以及 `OperationService`。 然後 `Index` 動作會顯示控制器和服務的所有 `OperationId` 值。

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

現在，對此控制器動作進行了兩個不同的要求：

![Microsoft Edge 中執行的相依性插入範例 Web 應用程式 [作業] 檢視，並在第一個要求顯示暫時性、具範圍、單一和執行個體控制器和作業服務作業的作業識別碼值 (GUID)。](dependency-injection/_static/lifetimes_request1.png)

![顯示第二個要求的作業識別碼值的 [作業] 檢視。](dependency-injection/_static/lifetimes_request2.png)

觀察哪些 `OperationId` 值在要求內以及要求之間不同。

* 「暫時性」物件一定會不同；會提供新的執行個體給每個控制器與每個服務。

* 「具範圍」物件在要求內相同，但跨不同的要求時會不同

* 「單一」物件對於每個物件與每個要求都相同 (不論執行個體是否提供於 `ConfigureServices`)

## <a name="resolve-a-scoped-service-within-the-application-scope"></a>解析應用程式範圍中的範圍服務

使用 [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) 建立 [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope)，以解析應用程 式範圍中的範圍服務。 此法可用於在開機時存取範圍服務，以執行初始化工作。 下列範例示範如何在 `Program.Main` 中取得 `MyScopedService`：

```csharp
public static void Main(string[] args)
{
    var host = BuildWebHost(args);

    using (var serviceScope = host.Services.CreateScope())
    {
        var services = serviceScope.ServiceProvider;

        try
        {
            var serviceContext = services.GetRequiredService<MyScopedService>();
            // Use the context here
        }
        catch (Exception ex)
        {
            var logger = services.GetRequiredService<ILogger<Program>>();
            logger.LogError(ex, "An error occurred.");
        }
    }

    host.Run();
}
```

## <a name="scope-validation"></a>範圍驗證

當應用程式在 ASP.NET Core 2.0 或更新版本的開發環境中執行時，預設服務提供者會確認：

* 範圍服務不是直接或間接由開機服務提供者解析。
* 範圍服務不是直接或間接插入至單一服務。

根服務提供者會在呼叫 [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) 時建立。 當提供者啟動應用程式時，根服務提供者的存留期與應用程式/伺服器的存留期一致，並會在應用程式關閉時處置。

範圍服務會由建立這些服務的容器處置。 若是在根容器中建立範圍服務，因為當應用程式/伺服器關機時，服務只會由根容器處理，所以服務的存留期會提升為單一服務等級。 當呼叫 `BuildServiceProvider` 時，驗證服務範圍會攔截到這些情況。

如需詳細資訊，請參閱 [Web 主機 主題中的範圍驗證](xref:fundamentals/host/web-host#scope-validation)。

## <a name="request-services"></a>要求服務

來自 `HttpContext`，在 ASP.NET 要求內提供的服務是透過 `RequestServices` 集合公開。

![HttpContext 要求服務 Intellisense 的內容對話方塊，指出要求服務會取得或設定提供要求之服務容器存取權的 IServiceProvider。](dependency-injection/_static/request-services.png)

要求服務代表您在應用程式中設定和要求的服務。 當您的物件指定相依性時，這些會由 `RequestServices` 中找到的類型來滿足，而非 `ApplicationServices`。

一般而言，您不應該直接使用這些屬性，而是最好改為透過您的類別建構函式要求需要的類別類型，並讓架構插入這些相依性。 如此產生的類別將更較容易測試 (請參閱[測試與偵錯](xref:test/index))，而且更鬆散結合。

> [!NOTE]
> 偏好要求相依性作為建構函式參數，而不要存取 `RequestServices` 集合。

## <a name="designing-services-for-dependency-injection"></a>針對相依性插入設計服務

您應該設計服務，以使用相依性插入取得其共同作業者。 這表示避免使用可設定狀態的靜態方法呼叫 (這會導致稱為[靜態黏貼](http://deviq.com/static-cling/)的程式碼特徵) 以及直接具現化您服務內的相依類別。 當您選擇是要具現化類型，還是透過相依性插入要求它時，記住 [New is Glue](https://ardalis.com/new-is-glue) 這句話可能有用。 藉由遵循[物件導向設計的 SOLID 準則](http://deviq.com/solid/)，您的類別自然而然會傾向小型、構造良好且輕鬆地測試。

如果您發現類別通常插入太多相依性怎麼辦？ 這通常是表示您的類別嘗試完成太多事項，並且可能違反 SRP - [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/) (單一責任準則)。 將類別負責的某些部分移到新的類別，看看是否能夠重構類別。 請記住，您的 `Controller` 類別應該專注於 UI 考量，因此商務規則和資料存取實作詳細資料應該保存在適合這些 [Separation of Concerns](http://deviq.com/separation-of-concerns/) (不同考量) 的類別中。

具體來說，關於資料存取，您可以將 `DbContext` 插入至您的控制器 (假設您已在 `ConfigureServices` 中將 EF 新增到服務容器)。 有些開發人員偏好使用資料庫的儲存機制介面，而不是直接插入 `DbContext`。 使用介面將資料存取邏輯封裝在一個地方，可以在資料庫變更時將您必須變更的位置數目降至最低。

### <a name="disposing-of-services"></a>處置服務

容器會為它建立的 `IDisposable` 類型呼叫 `Dispose`。 不過，如果您自行將執行個體新增到容器，將不會處置它。

範例：

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
> 在 1.0 版中，容器會對「所有」`IDisposable` 物件呼叫 dispose，包括它未建立的物件。

## <a name="replacing-the-default-services-container"></a>取代預設服務容器

內建的服務容器是要服務架構和建置在其上的大部分取用者應用程式的基本需求。 不過，開發人員可以使用其偏好的容器取代內建的容器。 `ConfigureServices` 方法通常會傳回 `void`，但如果其簽章變更為傳回 `IServiceProvider`，則可以設定與傳回不同的容器。 .NET 有許多 IOC 容器可用。 在此範例中，使用了 [Autofac](https://autofac.org/) 套件。

首先，安裝適當的容器套件：

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

接下來，在 `ConfigureServices` 中設定容器並傳回 `IServiceProvider`：

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
> 當使用協力廠商 DI 容器時，您必須變更 `ConfigureServices`，使它傳回 `IServiceProvider` 而不是 `void`。

最後，像平常一樣在 `DefaultModule` 設定 Autofac：

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

在執行階段，Autofac 將用於解析類型和插入相依性。 [深入了解使用 Autofac 和 ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html)。

### <a name="thread-safety"></a>執行緒安全

單一服務需要具備執行緒安全。 如果單一服務相依於暫時性服務，暫時性服務可能也需要具備執行緒安全，視它如何由單一項目使用。

## <a name="recommendations"></a>建議

使用相依性插入時，請記住下列建議：

* DI 適用於具有複雜相依性的物件。 控制器、服務、配接器以及儲存機制全都是可能新增至 DI 的物件範例。

* 避免直接在 DI 中儲存資料和組態。 例如，使用者的購物車通常不應該新增至服務容器。 組態應該使用[選項模式](xref:fundamentals/configuration/options)。 同樣地，請避免只存在以允許存取某個其他物件的「資料持有者」物件。 最好是盡可能透過 DI 要求所需的實際項目。

* 避免靜態存取服務。

* 避免應用程式程式碼中的服務位置。

* 避免靜態存取 `HttpContext`。

就像所有的建議集，您可能會遇到需要忽略其中之一的情況。 我們發現例外狀況很少見 - 主要是架構本身內非常特殊的情況。

相依性插入是靜態/全域物件存取模式的「替代」選項。 如果您將 DI 與靜態物件存取混合，則可能無法實現 DI 的優點。

## <a name="additional-resources"></a>其他資源

* [在檢視中插入相依性](xref:mvc/views/dependency-injection)
* [在控制器中插入相依性](xref:mvc/controllers/dependency-injection)
* [要求處理常式中的相依性插入](xref:security/authorization/dependencyinjection)
* [應用程式啟動](xref:fundamentals/startup)
* [測試及偵錯](xref:test/index)
* [Factory 式中介軟體啟用](xref:fundamentals/middleware/extensibility)
* [在 ASP.NET Core 使用 Dependency Injection 撰寫簡潔的程式碼 (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [Container-Managed Application Design, Prelude: Where does the Container Belong?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/) (容器管理的應用程式設計，序曲：容器屬於何處？)
* [明確相依性準則](http://deviq.com/explicit-dependencies-principle/)
* [Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html) (逆轉控制容器和相依性插入模式) (Fowler)
