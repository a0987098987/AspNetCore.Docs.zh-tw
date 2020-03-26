---
title: .NET Core 中的相依性插入
author: rick-anderson
description: 了解 ASP.NET Core 如何實作相依性插入以及如何使用它。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
uid: fundamentals/dependency-injection
ms.openlocfilehash: d24807d1ea675448536ab865d41ae46f80043666
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306514"
---
# <a name="dependency-injection-in-aspnet-core"></a>.NET Core 中的相依性插入

作者：[Steve Smith](https://ardalis.com/) 和 [Scott Addie](https://scottaddie.com)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core 支援相依性插入 (DI) 軟體設計模式，這是用來在類別與其相依性之間達成[控制權反轉 (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) \(英文\) 的技術。

如需有關 MVC 控制器內相依性插入的特定詳細資訊，請參閱 <xref:mvc/controllers/dependency-injection>。

[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="overview-of-dependency-injection"></a>相依性插入概觀

「相依性」是另一個物件所需的任何物件。 檢查具有 `MyDependency` 方法的下列 `WriteMessage` 物件，應用程式中的其他類別相依於此物件：

```csharp
public class MyDependency
{
    public MyDependency()
    {
    }

    public Task WriteMessage(string message)
    {
        Console.WriteLine(
            $"MyDependency.WriteMessage called. Message: {message}");

        return Task.FromResult(0);
    }
}
```

您可以建立 `MyDependency` 類別的執行個體，讓 `WriteMessage` 方法可供類別使用。 `MyDependency` 類別是 `IndexModel` 類別的相依性：

```csharp
public class IndexModel : PageModel
{
    MyDependency _dependency = new MyDependency();

    public async Task OnGetAsync()
    {
        await _dependency.WriteMessage(
            "IndexModel.OnGetAsync created this message.");
    }
}
```

該類別會建立 `MyDependency` 執行個體並直接相依於該執行個體。 程式碼相依性 (例如前一個範例) 有問題，因此基於下列原因應該避免使用：

* 若要將 `MyDependency` 取代為不同的實作，必須修改該類別。
* 若 `MyDependency` 有相依性，那些相依性必須由該類別設定。 在具有多個相依於 `MyDependency` 之多個類別的大型專案中，設定程式碼在不同的應用程式之間會變得鬆散。
* 此實作難以進行單元測試。 應用程式應該使用模擬 (Mock) 或虛設常式 (Stub) `MyDependency` 類別，這在使用此方法時無法使用。

相依性插入可透過下列方式解決這些問題：

* 使用介面或基底類別來將相依性資訊抽象化。
* 在服務容器中註冊相依性。 ASP.NET Core 提供內建服務容器 <xref:System.IServiceProvider>。 服務會在應用程式的 `Startup.ConfigureServices` 方法中註冊。
* 將服務「插入」到服務使用位置之類別的建構函式。 架構會負責建立相依性的執行個體，並在不再需要時將它捨棄。

在[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)中，`IMyDependency` 介面定義了服務提供給應用程式的方法：

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

這個介面是由具象型別 `MyDependency` 所實作：

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

`MyDependency` 在其建構函式中會要求 <xref:Microsoft.Extensions.Logging.ILogger`1>。 以鏈結方式使用相依性插入並非不尋常。 每個要求的相依性接著會要求其自己的相依性。 容器會解決圖形中的相依性，並傳回完全解析的服務。 必須先解析的相依性集合組通常稱為「相依性樹狀結構」、「相依性圖形」或「物件圖形」。

`IMyDependency` 與 `ILogger<TCategoryName>` 必須在服務容器中註冊。 `IMyDependency` 是在 `Startup.ConfigureServices` 中註冊。 `ILogger<TCategoryName>` 是由記錄抽象基礎結構所註冊，所以它是預設由架構所註冊的[架構提供的服務](#framework-provided-services)。

容器會利用 `ILogger<TCategoryName>`(泛型) 開放類型[ 解析 ](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types)，讓您不需註冊每個 [(泛型) 建構的型別](/dotnet/csharp/language-reference/language-specification/types#constructed-types)：

```csharp
services.AddSingleton(typeof(ILogger<>), typeof(Logger<>));
```

在範例應用程式中，`IMyDependency` 服務是使用具象型別 `MyDependency` 所註冊。 註冊會將服務的存留期範圍限制為單一要求的存留期。 將在此主題稍後將說明[服務存留期](#service-lifetimes)。

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> 每個 `services.Add{SERVICE_NAME}` 擴充方法都會新增 (並且可能會設定) 服務。 例如，`services.AddMvc()` 會新增 Razor Pages 與 MVC 要求的服務。 建議應用程式遵循此慣例。 在 [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) 命名空間中放置擴充方法，以封裝服務註冊群組。

如果服務的建構函式需要[內建類型](/dotnet/csharp/language-reference/keywords/built-in-types-table)，例如 `string`，可以使用[組態](xref:fundamentals/configuration/index)或[選項模式](xref:fundamentals/configuration/options)插入該類型：

```csharp
public class MyDependency : IMyDependency
{
    public MyDependency(IConfiguration config)
    {
        var myStringValue = config["MyStringKey"];

        // Use myStringValue
    }

    ...
}
```

服務執行個體是透過服務使用所在之類別建構函式所要求，而且會指派給私人欄位。 該欄位會視需要用來存取整個類別中的服務。

在範例應用程式中，會要求 `IMyDependency` 執行個體並使用它來呼叫服務的 `WriteMessage` 方法：

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="services-injected-into-startup"></a>插入至啟動的服務

使用泛型主機（<xref:Microsoft.Extensions.Hosting.IHostBuilder>）時，只能將下列服務類型插入 `Startup` 的函數：

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

服務可以插入 `Startup.Configure`：

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

如需詳細資訊，請參閱 <xref:fundamentals/startup>。

## <a name="framework-provided-services"></a>架構提供的服務

`Startup.ConfigureServices` 方法負責定義應用程式所使用的服務，包括平臺功能，例如 Entity Framework Core 和 ASP.NET Core MVC。 一開始，根據[主機的設定方式](xref:fundamentals/index#host)，提供給 `ConfigureServices` 的 `IServiceCollection` 具有架構所定義的服務。 以 ASP.NET Core 範本為基礎的應用程式，在架構中註冊數百項服務並不常見。 下表列出架構註冊服務的小型範例。

| 服務類型 | 存留期 |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | 暫時性 |
| `IHostApplicationLifetime` | 單一 |
| `IWebHostEnvironment` | 單一 |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | 單一 |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | 暫時性 |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | 單一 |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | 暫時性 |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | 單一 |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | 單一 |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | 單一 |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | 暫時性 |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | 單一 |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | 單一 |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | 單一 |

## <a name="register-additional-services-with-extension-methods"></a>以擴充方法註冊其他服務

當可以使用服務集合擴充方法來註冊服務 (如果需要，也可以註冊其相依服務) 時，慣例是使用單一 `Add{SERVICE_NAME}` 擴充方法來註冊該服務要求的所有服務。 下列程式碼範例說明如何使用擴充方法[AddDbCoNtext\<TCoNtext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)和 <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>，將其他服務新增至容器：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    ...
}
```

如需詳細資訊，請參閱 API 文件中的 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> 類別。

## <a name="service-lifetimes"></a>執行個體存留期

為每個已註冊的服務選擇適當的存留期。 ASP.NET Core 服務可以使用下列存留期進行設定：

### <a name="transient"></a>暫時性

每次從服務容器要求暫時性存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) 時都會建立它們。 此存留期最適合用於輕量型的無狀態服務。

### <a name="scoped"></a>具範圍

具範圍存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) 會在每次用戶端要求 (連線) 時建立一次。

> [!WARNING]
> 在中介軟體中使用具範圍服務時，請將該服務插入 `Invoke` 或 `InvokeAsync` 方法中。 因為其會強制服務執行單一服務的行為，所以請勿透過建構函式插入進行插入。 如需詳細資訊，請參閱 <xref:fundamentals/middleware/write#per-request-middleware-dependencies>。

### <a name="singleton"></a>單一

當第一次收到有關單一資料庫存留期服務的要求時 (或是當執行 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*> 且隨著服務註冊指定執行個體時)，即會建立單一資料庫存留期服務 (`Startup.ConfigureServices`)。 每個後續要求都會使用相同的執行個體。 若應用程式要求單一資料庫行為，建議您允許服務容器管理服務的存留期。 不要實作單一資料庫設計模式並為使用者提供可用來管理類別中物件存留期的程式碼。

> [!WARNING]
> 從單一資料庫解析具範圍服務是非常危險的。 處理後續要求時，它可能會導致服務有不正確的狀態。

## <a name="service-registration-methods"></a>服務註冊方法

服務註冊擴充方法提供在特定案例中很有用的多載。

| 方法 | 自動<br>物件 (object)<br>處置 | 多個<br>實作 | 傳遞引數 |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br>範例：<br>`services.AddSingleton<IMyDep, MyDep>();` | 是 | 是 | 否 |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br>範例：<br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br>`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));` | 是 | 是 | 是 |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br>範例：<br>`services.AddSingleton<MyDep>();` | 是 | 否 | 否 |
| `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`<br>範例：<br>`services.AddSingleton<IMyDep>(new MyDep());`<br>`services.AddSingleton<IMyDep>(new MyDep("A string!"));` | 否 | 是 | 是 |
| `AddSingleton(new {IMPLEMENTATION})`<br>範例：<br>`services.AddSingleton(new MyDep());`<br>`services.AddSingleton(new MyDep("A string!"));` | 否 | 否 | 是 |

如需類型處置的詳細資訊，請參閱[＜服務處置＞](#disposal-of-services)一節。 多個實作的常見案例是[模擬測試類型](xref:test/integration-tests#inject-mock-services)。

如果還未註冊任何實作，則 `TryAdd{LIFETIME}` 方法只會註冊服務。

在下列範例中，第一行會為 `MyDependency` 註冊 `IMyDependency`。 第二行則沒有任何作用，因為 `IMyDependency` 已具有註冊的實作：

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

如需詳細資訊，請參閱

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

如果還沒有 [相同類型的](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) 實作，則 *TryAddEnumerable(ServiceDescriptor)* 方法只會註冊服務。 多個服務會透過 `IEnumerable<{SERVICE}>` 解析。 註冊服務時，如果尚未新增相同類型的執行個體，則開發人員只會希望新增執行個體。 程式庫作者通常會使用此方法來避免註冊容器中某個執行個體的兩個複本。

在下列範例中，第一行會為 `MyDep` 註冊 `IMyDep1`。 第二行會為 `MyDep` 註冊 `IMyDep2`。 第三行則沒有任何作用，因為 `IMyDep1` 已具有 `MyDep` 的已註冊實作：

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a>建構函式插入行為

服務可以透過兩個機制來解析：

* <xref:System.IServiceProvider>
* <xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; 允許在相依性插入容器中不進行服務註冊，即可建立物件。 搭配使用者面向抽象 (例如標籤協助程式、MVC 控制器與模型繫結器) 使用 `ActivatorUtilities`。

建構函式可以接受不是由相依性插入提供的引數，但引數必須指派預設值。

當服務由 `IServiceProvider` 或 `ActivatorUtilities` 解析時，建構函式插入會要求 *public*建構函式。

當服務由 `ActivatorUtilities` 解析時，建構函式插入只要求只能有一個適用的建構函式存在。 支援建構函式多載，但只能有一個多載存在，其引數可以藉由相依性插入而完成。

## <a name="entity-framework-contexts"></a>Entity Framework 內容

因為一般會將 Web 應用程式資料庫作業範圍設定為用戶端要求，所以通常會使用[具範圍存留期](#service-lifetimes)將 Entity Framework 內容新增至服務容器。 註冊資料庫內容時，如果 [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) 多載未指定留存期，則會將範圍設定為預設存留期。 指定存留期的服務不應該使用存留期比服務還短的資料庫內容。

## <a name="lifetime-and-registration-options"></a>留期和註冊選項

為了示範這些存留期和註冊選項之間的差異，請考慮下列介面，以具有唯一識別碼 `OperationId` 的「作業」代表工作。 視如何針對下列介面設定作業服務存留期而定，當類別要求時，容器會提供相同或不同的服務執行個體：

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

介面是在 `Operation` 類別中實作。 若未提供 GUID，`Operation` 建構函式會產生 GUID：

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

會註冊相依於每種其他 `OperationService` 類型的 `Operation`。 當透過相依性插入要求 `OperationService` 時，它會根據相依服務的存留期擷取每個服務的新執行個體或現有執行個體。

* 當因收到來自容器的要求而建立暫時性服務時，`OperationId` 服務的 `IOperationTransient` 會與 `OperationId` 的 `OperationService` 不同。 `OperationService` 會收到 `IOperationTransient` 類別的新執行個體。 新執行個體會產生不同的 `OperationId`。
* 當因用戶端要求而建立具範圍的服務時，`OperationId` 服務與用戶端要求中 `IOperationScoped` 的 `OperationService` 皆相同。 在不同的用戶端要求中，兩個服務的 `OperationId` 值都不同。
* 當建立一次 singleton 與 singleton 執行個體服務，並在所有用戶端要求與所有服務中使用時，`OperationId` 在所有服務要求中會固定不變。

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

在 `Startup.ConfigureServices` 中，每個類型都會根據其具名存留期新增至容器：

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

`IOperationSingletonInstance` 服務使用具有 `Guid.Empty` 已知識別碼的特定執行個體。 很明顯此型別是使用中 (其 GUID 是所有區域)。

範例應用程式示範個別要求內與個別要求之間的物件存留期。 範例應用程式的 `IndexModel` 會要求每種 `IOperation` 型別與 `OperationService`。 頁面接著會透過屬性指派來顯示所有頁面模型類別與服務的 `OperationId` 值：

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

下面兩個輸出顯示兩個要求的結果：

**第一個要求：**

控制器作業：

暫時性： d233e165-f417-469b-a866-1cf1935d2518  
具範圍： 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9  
執行個體： 00000000-0000-0000-0000-000000000000

`OperationService` 作業：

暫時性： c6b049eb-1318-4e31-90f1-eb2dd849ff64  
具範圍： 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9  
執行個體： 00000000-0000-0000-0000-000000000000

**:第二個要求：**

控制器作業：

暫時性： b63bd538-0a37-4ff1-90ba-081c5138dda0  
具範圍： 31e820c5-4834-4d22-83fc-a60118acb9f4  
單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9  
執行個體： 00000000-0000-0000-0000-000000000000

`OperationService` 作業：

暫時性： c4cbacb8-36a2-436d-81c8-8c1b78808aaf  
具範圍： 31e820c5-4834-4d22-83fc-a60118acb9f4  
單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9  
執行個體： 00000000-0000-0000-0000-000000000000

觀察哪些 `OperationId` 值在要求內以及要求之間不同：

* 「暫時性」 物件一律不同。 第一個與第二個用戶端要求的暫時性 `OperationId` 值在兩個 `OperationService` 作業之間與各用戶端要求之間都不同。 新執行個體會提供給每個服務要求以及用戶端要求。
* 「具範圍」物件在同一個用戶端要求內皆相同，但在不同的用戶端要求之間則不同。
* 「單一資料庫」物件對於每個物件與每個要求都相同 (不論 `Operation` 執行個體是否提供於 `Startup.ConfigureServices`)。

## <a name="call-services-from-main"></a>從主要呼叫服務

使用 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>IServiceScopeFactory.CreateScope[ 建立 ](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*)，以解析應用程式範圍中的範圍服務。 此法可用於在開機時存取範圍服務，以執行初始化工作。 下列範例示範如何在 `MyScopedService` 中取得 `Program.Main`：

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static async Task Main(string[] args)
    {
        var host = CreateHostBuilder(args).Build();

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
    
        await host.RunAsync();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
}
```

## <a name="scope-validation"></a>範圍驗證

當應用程式在開發環境中執行並呼叫[CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings)來建立主機時，預設服務提供者會執行檢查以確認：

* 範圍服務不是直接或間接由根服務提供者解析。
* 範圍服務不是直接或間接插入至單一服務。

根服務提供者會在呼叫 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> 時建立。 當提供者啟動應用程式時，根服務提供者的存留期與應用程式/伺服器的存留期一致，並會在應用程式關閉時處置。

範圍服務會由建立這些服務的容器處置。 若是在根容器中建立範圍服務，因為當應用程式/伺服器關機時，服務只會由根容器處理，所以服務的存留期會提升為單一服務等級。 當呼叫 `BuildServiceProvider` 時，驗證服務範圍會攔截到這些情況。

如需詳細資訊，請參閱 <xref:fundamentals/host/web-host#scope-validation>。

## <a name="request-services"></a>要求服務

來自 `HttpContext`，在 ASP.NET Core 要求內提供的服務是透過 [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) 集合公開。

要求服務代表您在應用程式中設定和要求的服務。 當物件指定相依性時，這些會由在 `RequestServices` 中找到的類型來滿足，而非 `ApplicationServices`。

一般而言，應用程式不應該直接使用這些屬性。 相反地，透過類別建構函式要求類別所需的型別，以及允許架構插入相依性。 這會產生容易測試的類別。

> [!NOTE]
> 偏好要求相依性作為建構函式參數，而不要存取 `RequestServices` 集合。

## <a name="design-services-for-dependency-injection"></a>針對相依性插入設計服務

最佳做法是：

* 設計服務以使用相依性插入來取得其相依性。
* 避免具狀態的靜態類別和成員。 將應用程式設計成使用單一服務，以避免建立全域狀態。
* 避免直接在服務內具現化相依類別。 直接具現化會將程式碼耦合到特定實作。
* 讓應用程式類別維持在小型、情況良好且可輕鬆測試的狀態。

若類別有太多插入的相依性，這通常表示類別有太多責任而且正違反[單一責任原則 (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility) \(英文\)。 將類別負責的某些部分移到新的類別，以嘗試重構類別。 請記住，Razor Pages 頁面模型類別與 MVC 控制器類別應該專注於 UI 考量。 商務規則和資料存取實作詳細資料應該保存在適合這些[分開考量](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) (Separation of Concerns) 類別中。

### <a name="disposal-of-services"></a>處置服務

容器會為它建立的 <xref:System.IDisposable.Dispose*> 類型呼叫 <xref:System.IDisposable>。 若執行個體由使用者程式碼新增到容器中，它將不會自動處置。

在下列範例中，服務會建立服務容器並自動處置：

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public interface IService3 {}
public class Service3 : IService3, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<IService3>(sp => new Service3());
}
```

在下例中︰

* 服務容器不會建立服務實例。
* 架構不知道預期的服務存留期。
* 架構不會自動處置服務。
* 如果服務未在開發人員程式碼中明確處置，它們會持續存在，直到應用程式關閉為止。

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<Service1>(new Service1());
    services.AddSingleton(new Service2());
}
```

如需服務處置選項的討論，請參閱[在 ASP.NET Core 中處置 IDisposables 的四種方式](https://andrewlock.net/four-ways-to-dispose-idisposables-in-asp-net-core/)。

## <a name="default-service-container-replacement"></a>預設服務容器取代

內建的服務容器是設計用來滿足架構和大部分取用者應用程式的需求。 我們建議使用內建容器，除非您需要內建容器不支援的特定功能，例如：

* 屬性插入
* 根據名稱插入
* 子容器
* 自訂生命週期管理
* `Func<T>` 支援延遲初始設定
* 以慣例為基礎的註冊

下列協力廠商容器可以與 ASP.NET Core 應用程式搭配使用：

* [Autofac](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
* [DryIoc](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
* [寬限](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
* [LightInject](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
* [Lamar](https://jasperfx.github.io/lamar/)
* [Stashbox](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
* [Unity](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a>執行緒安全性

建立具備執行緒安全性的 singleton 服務。 如果 singleton 服務相依於暫時性服務，暫時性服務可能也需要具備執行緒安全性，取決於 singleton 如何使用它。

單一服務的 Factory 方法 (例如 [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*) 的第二個引數) 不需要是安全執行緒。 就像型別 (`static`) 建構函式一樣，它一定會被單一執行緒呼叫一次。

## <a name="recommendations"></a>建議

* 不支援以 `async/await` 與 `Task` 為基礎的服務解析。 C# 不支援非同步建構函式，因此建議的模式是以同步方式解析服務後使用非同步方法。

* 避免直接在服務容器中儲存資料與設定。 例如，使用者的購物車通常不應該新增至服務容器。 組態應該使用[選項模式](xref:fundamentals/configuration/options)。 同樣地，請避免只存在以允許存取某個其他物件的「資料持有者」物件。 最好是透過 DI 要求實際項目。

* 避免以靜態方式存取服務 (例如，以靜態方式設定 [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) 型別以四處使用)。

* 避免使用「服務定位器模式」。 例如，當您可以改用 DI 時，請勿叫用 <xref:System.IServiceProvider.GetService*> 來取得服務執行個體：

  **不正確：**

  ```csharp
  public class MyClass()
  {
      public void MyMethod()
      {
          var optionsMonitor = 
              _services.GetService<IOptionsMonitor<MyOptions>>();
          var option = optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

  **正確**：

  ```csharp
  public class MyClass
  {
      private readonly IOptionsMonitor<MyOptions> _optionsMonitor;

      public MyClass(IOptionsMonitor<MyOptions> optionsMonitor)
      {
          _optionsMonitor = optionsMonitor;
      }

      public void MyMethod()
      {
          var option = _optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

* 另一個要避免的服務定位器變化是插入在執行階段解析相依性的處理站。 這兩種做法都會混用[控制反轉](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion)策略。

* 避免以靜態方式存取 `HttpContext` (例如 [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext))。

就像所有的建議集，您可能會遇到需要忽略建議的情況。 例外狀況很少見&mdash;大部分是架構本身內的特殊案例。

DI 是靜態/全域物件存取模式的「替代」選項。 如果您將 DI 與靜態物件存取混合，則可能無法實現 DI 的優點。

## <a name="additional-resources"></a>其他資源

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [在 ASP.NET Core 使用 Dependency Injection 撰寫簡潔的程式碼 (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [明確相依性準則](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [逆轉控制容器和相依性插入模式 (Martin Fowler)](https://www.martinfowler.com/articles/injection.html) \(英文\)
* [How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/) (如何使用 ASP.NET Core DI 中的多個介面註冊服務)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core 支援相依性插入 (DI) 軟體設計模式，這是用來在類別與其相依性之間達成[控制權反轉 (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) \(英文\) 的技術。

如需有關 MVC 控制器內相依性插入的特定詳細資訊，請參閱 <xref:mvc/controllers/dependency-injection>。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="overview-of-dependency-injection"></a>相依性插入概觀

「相依性」是另一個物件所需的任何物件。 檢查具有 `MyDependency` 方法的下列 `WriteMessage` 物件，應用程式中的其他類別相依於此物件：

```csharp
public class MyDependency
{
    public MyDependency()
    {
    }

    public Task WriteMessage(string message)
    {
        Console.WriteLine(
            $"MyDependency.WriteMessage called. Message: {message}");

        return Task.FromResult(0);
    }
}
```

您可以建立 `MyDependency` 類別的執行個體，讓 `WriteMessage` 方法可供類別使用。 `MyDependency` 類別是 `IndexModel` 類別的相依性：

```csharp
public class IndexModel : PageModel
{
    MyDependency _dependency = new MyDependency();

    public async Task OnGetAsync()
    {
        await _dependency.WriteMessage(
            "IndexModel.OnGetAsync created this message.");
    }
}
```

該類別會建立 `MyDependency` 執行個體並直接相依於該執行個體。 程式碼相依性 (例如前一個範例) 有問題，因此基於下列原因應該避免使用：

* 若要將 `MyDependency` 取代為不同的實作，必須修改該類別。
* 若 `MyDependency` 有相依性，那些相依性必須由該類別設定。 在具有多個相依於 `MyDependency` 之多個類別的大型專案中，設定程式碼在不同的應用程式之間會變得鬆散。
* 此實作難以進行單元測試。 應用程式應該使用模擬 (Mock) 或虛設常式 (Stub) `MyDependency` 類別，這在使用此方法時無法使用。

相依性插入可透過下列方式解決這些問題：

* 使用介面或基底類別來將相依性資訊抽象化。
* 在服務容器中註冊相依性。 ASP.NET Core 提供內建服務容器 <xref:System.IServiceProvider>。 服務會在應用程式的 `Startup.ConfigureServices` 方法中註冊。
* 將服務「插入」到服務使用位置之類別的建構函式。 架構會負責建立相依性的執行個體，並在不再需要時將它捨棄。

在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)中，`IMyDependency` 介面定義了服務提供給應用程式的方法：

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

這個介面是由具象型別 `MyDependency` 所實作：

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

`MyDependency` 在其建構函式中會要求 <xref:Microsoft.Extensions.Logging.ILogger`1>。 以鏈結方式使用相依性插入並非不尋常。 每個要求的相依性接著會要求其自己的相依性。 容器會解決圖形中的相依性，並傳回完全解析的服務。 必須先解析的相依性集合組通常稱為「相依性樹狀結構」、「相依性圖形」或「物件圖形」。

`IMyDependency` 與 `ILogger<TCategoryName>` 必須在服務容器中註冊。 `IMyDependency` 是在 `Startup.ConfigureServices` 中註冊。 `ILogger<TCategoryName>` 是由記錄抽象基礎結構所註冊，所以它是預設由架構所註冊的[架構提供的服務](#framework-provided-services)。

容器會利用 `ILogger<TCategoryName>`(泛型) 開放類型[ 解析 ](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types)，讓您不需註冊每個 [(泛型) 建構的型別](/dotnet/csharp/language-reference/language-specification/types#constructed-types)：

```csharp
services.AddSingleton(typeof(ILogger<>), typeof(Logger<>));
```

在範例應用程式中，`IMyDependency` 服務是使用具象型別 `MyDependency` 所註冊。 註冊會將服務的存留期範圍限制為單一要求的存留期。 將在此主題稍後將說明[服務存留期](#service-lifetimes)。

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> 每個 `services.Add{SERVICE_NAME}` 擴充方法都會新增 (並且可能會設定) 服務。 例如，`services.AddMvc()` 會新增 Razor Pages 與 MVC 要求的服務。 建議應用程式遵循此慣例。 在 [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) 命名空間中放置擴充方法，以封裝服務註冊群組。

如果服務的建構函式需要[內建類型](/dotnet/csharp/language-reference/keywords/built-in-types-table)，例如 `string`，可以使用[組態](xref:fundamentals/configuration/index)或[選項模式](xref:fundamentals/configuration/options)插入該類型：

```csharp
public class MyDependency : IMyDependency
{
    public MyDependency(IConfiguration config)
    {
        var myStringValue = config["MyStringKey"];

        // Use myStringValue
    }

    ...
}
```

服務執行個體是透過服務使用所在之類別建構函式所要求，而且會指派給私人欄位。 該欄位會視需要用來存取整個類別中的服務。

在範例應用程式中，會要求 `IMyDependency` 執行個體並使用它來呼叫服務的 `WriteMessage` 方法：

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="services-injected-into-startup"></a>插入至啟動的服務

使用泛型主機（<xref:Microsoft.Extensions.Hosting.IHostBuilder>）時，只能將下列服務類型插入 `Startup` 的函數：

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

服務可以插入 `Startup.Configure`：

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

如需詳細資訊，請參閱 <xref:fundamentals/startup>。

## <a name="framework-provided-services"></a>架構提供的服務

`Startup.ConfigureServices` 方法負責定義應用程式所使用的服務，包括平臺功能，例如 Entity Framework Core 和 ASP.NET Core MVC。 一開始，根據[主機的設定方式](xref:fundamentals/index#host)，提供給 `ConfigureServices` 的 `IServiceCollection` 具有架構所定義的服務。 以 ASP.NET Core 範本為基礎的應用程式，在架構中註冊數百項服務並不常見。 下表列出架構註冊服務的小型範例。

| 服務類型 | 存留期 |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | 暫時性 |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | 單一 |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | 單一 |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | 單一 |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | 暫時性 |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | 單一 |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | 暫時性 |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | 單一 |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | 單一 |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | 單一 |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | 暫時性 |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | 單一 |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | 單一 |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | 單一 |

## <a name="register-additional-services-with-extension-methods"></a>以擴充方法註冊其他服務

當可以使用服務集合擴充方法來註冊服務 (如果需要，也可以註冊其相依服務) 時，慣例是使用單一 `Add{SERVICE_NAME}` 擴充方法來註冊該服務要求的所有服務。 下列程式碼範例說明如何使用擴充方法[AddDbCoNtext\<TCoNtext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)和 <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>，將其他服務新增至容器：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    ...
}
```

如需詳細資訊，請參閱 API 文件中的 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> 類別。

## <a name="service-lifetimes"></a>執行個體存留期

為每個已註冊的服務選擇適當的存留期。 ASP.NET Core 服務可以使用下列存留期進行設定：

### <a name="transient"></a>暫時性

每次從服務容器要求暫時性存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) 時都會建立它們。 此存留期最適合用於輕量型的無狀態服務。

### <a name="scoped"></a>具範圍

具範圍存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) 會在每次用戶端要求 (連線) 時建立一次。

> [!WARNING]
> 在中介軟體中使用具範圍服務時，請將該服務插入 `Invoke` 或 `InvokeAsync` 方法中。 因為其會強制服務執行單一服務的行為，所以請勿透過建構函式插入進行插入。 如需詳細資訊，請參閱 <xref:fundamentals/middleware/write#per-request-middleware-dependencies>。

### <a name="singleton"></a>單一

當第一次收到有關單一資料庫存留期服務的要求時 (或是當執行 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*> 且隨著服務註冊指定執行個體時)，即會建立單一資料庫存留期服務 (`Startup.ConfigureServices`)。 每個後續要求都會使用相同的執行個體。 若應用程式要求單一資料庫行為，建議您允許服務容器管理服務的存留期。 不要實作單一資料庫設計模式並為使用者提供可用來管理類別中物件存留期的程式碼。

> [!WARNING]
> 從單一資料庫解析具範圍服務是非常危險的。 處理後續要求時，它可能會導致服務有不正確的狀態。

## <a name="service-registration-methods"></a>服務註冊方法

服務註冊擴充方法提供在特定案例中很有用的多載。

| 方法 | 自動<br>物件 (object)<br>處置 | 多個<br>實作 | 傳遞引數 |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br>範例：<br>`services.AddSingleton<IMyDep, MyDep>();` | 是 | 是 | 否 |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br>範例：<br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br>`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));` | 是 | 是 | 是 |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br>範例：<br>`services.AddSingleton<MyDep>();` | 是 | 否 | 否 |
| `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`<br>範例：<br>`services.AddSingleton<IMyDep>(new MyDep());`<br>`services.AddSingleton<IMyDep>(new MyDep("A string!"));` | 否 | 是 | 是 |
| `AddSingleton(new {IMPLEMENTATION})`<br>範例：<br>`services.AddSingleton(new MyDep());`<br>`services.AddSingleton(new MyDep("A string!"));` | 否 | 否 | 是 |

如需類型處置的詳細資訊，請參閱[＜服務處置＞](#disposal-of-services)一節。 多個實作的常見案例是[模擬測試類型](xref:test/integration-tests#inject-mock-services)。

如果還未註冊任何實作，則 `TryAdd{LIFETIME}` 方法只會註冊服務。

在下列範例中，第一行會為 `MyDependency` 註冊 `IMyDependency`。 第二行則沒有任何作用，因為 `IMyDependency` 已具有註冊的實作：

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

如需詳細資訊，請參閱

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

如果還沒有[「相同類型的」](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*)實作，則 *TryAddEnumerable(ServiceDescriptor)* 方法只會註冊服務。 多個服務會透過 `IEnumerable<{SERVICE}>` 解析。 註冊服務時，如果尚未新增相同類型的執行個體，則開發人員只會希望新增執行個體。 程式庫作者通常會使用此方法來避免註冊容器中某個執行個體的兩個複本。

在下列範例中，第一行會為 `MyDep` 註冊 `IMyDep1`。 第二行會為 `MyDep` 註冊 `IMyDep2`。 第三行則沒有任何作用，因為 `IMyDep1` 已具有 `MyDep` 的已註冊實作：

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a>建構函式插入行為

服務可以透過兩個機制來解析：

* <xref:System.IServiceProvider>
* <xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; 允許在相依性插入容器中不進行服務註冊，即可建立物件。 搭配使用者面向抽象 (例如標籤協助程式、MVC 控制器與模型繫結器) 使用 `ActivatorUtilities`。

建構函式可以接受不是由相依性插入提供的引數，但引數必須指派預設值。

當服務由 `IServiceProvider` 或 `ActivatorUtilities` 解析時，建構函式插入會要求 *public*建構函式。

當服務由 `ActivatorUtilities` 解析時，建構函式插入只要求只能有一個適用的建構函式存在。 支援建構函式多載，但只能有一個多載存在，其引數可以藉由相依性插入而完成。

## <a name="entity-framework-contexts"></a>Entity Framework 內容

因為一般會將 Web 應用程式資料庫作業範圍設定為用戶端要求，所以通常會使用[具範圍存留期](#service-lifetimes)將 Entity Framework 內容新增至服務容器。 註冊資料庫內容時，如果 [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) 多載未指定留存期，則會將範圍設定為預設存留期。 指定存留期的服務不應該使用存留期比服務還短的資料庫內容。

## <a name="lifetime-and-registration-options"></a>留期和註冊選項

為了示範這些存留期和註冊選項之間的差異，請考慮下列介面，以具有唯一識別碼 `OperationId` 的「作業」代表工作。 視如何針對下列介面設定作業服務存留期而定，當類別要求時，容器會提供相同或不同的服務執行個體：

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

介面是在 `Operation` 類別中實作。 若未提供 GUID，`Operation` 建構函式會產生 GUID：

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

會註冊相依於每種其他 `OperationService` 類型的 `Operation`。 當透過相依性插入要求 `OperationService` 時，它會根據相依服務的存留期擷取每個服務的新執行個體或現有執行個體。

* 當因收到來自容器的要求而建立暫時性服務時，`OperationId` 服務的 `IOperationTransient` 會與 `OperationId` 的 `OperationService` 不同。 `OperationService` 會收到 `IOperationTransient` 類別的新執行個體。 新執行個體會產生不同的 `OperationId`。
* 當因用戶端要求而建立具範圍的服務時，`OperationId` 服務與用戶端要求中 `IOperationScoped` 的 `OperationService` 皆相同。 在不同的用戶端要求中，兩個服務的 `OperationId` 值都不同。
* 當建立一次 singleton 與 singleton 執行個體服務，並在所有用戶端要求與所有服務中使用時，`OperationId` 在所有服務要求中會固定不變。

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

在 `Startup.ConfigureServices` 中，每個類型都會根據其具名存留期新增至容器：

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

`IOperationSingletonInstance` 服務使用具有 `Guid.Empty` 已知識別碼的特定執行個體。 很明顯此型別是使用中 (其 GUID 是所有區域)。

範例應用程式示範個別要求內與個別要求之間的物件存留期。 範例應用程式的 `IndexModel` 會要求每種 `IOperation` 型別與 `OperationService`。 頁面接著會透過屬性指派來顯示所有頁面模型類別與服務的 `OperationId` 值：

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

下面兩個輸出顯示兩個要求的結果：

**第一個要求：**

控制器作業：

暫時性： d233e165-f417-469b-a866-1cf1935d2518  
具範圍： 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9  
執行個體： 00000000-0000-0000-0000-000000000000

`OperationService` 作業：

暫時性： c6b049eb-1318-4e31-90f1-eb2dd849ff64  
具範圍： 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9  
執行個體： 00000000-0000-0000-0000-000000000000

**:第二個要求：**

控制器作業：

暫時性： b63bd538-0a37-4ff1-90ba-081c5138dda0  
具範圍： 31e820c5-4834-4d22-83fc-a60118acb9f4  
單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9  
執行個體： 00000000-0000-0000-0000-000000000000

`OperationService` 作業：

暫時性： c4cbacb8-36a2-436d-81c8-8c1b78808aaf  
具範圍： 31e820c5-4834-4d22-83fc-a60118acb9f4  
單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9  
執行個體： 00000000-0000-0000-0000-000000000000

觀察哪些 `OperationId` 值在要求內以及要求之間不同：

* 「暫時性」 物件一律不同。 第一個與第二個用戶端要求的暫時性 `OperationId` 值在兩個 `OperationService` 作業之間與各用戶端要求之間都不同。 新執行個體會提供給每個服務要求以及用戶端要求。
* 「具範圍」物件在同一個用戶端要求內皆相同，但在不同的用戶端要求之間則不同。
* 「單一資料庫」物件對於每個物件與每個要求都相同 (不論 `Operation` 執行個體是否提供於 `Startup.ConfigureServices`)。

## <a name="call-services-from-main"></a>從主要呼叫服務

使用 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>IServiceScopeFactory.CreateScope[ 建立 ](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*)，以解析應用程式範圍中的範圍服務。 此法可用於在開機時存取範圍服務，以執行初始化工作。 下列範例示範如何在 `MyScopedService` 中取得 `Program.Main`：

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;

public class Program
{
    public static async Task Main(string[] args)
    {
        var host = CreateWebHostBuilder(args).Build();

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
    
        await host.RunAsync();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

## <a name="scope-validation"></a>範圍驗證

當應用程式在開發環境中執行時，預設服務提供者會執行檢查以確認：

* 範圍服務不是直接或間接由根服務提供者解析。
* 範圍服務不是直接或間接插入至單一服務。

根服務提供者會在呼叫 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> 時建立。 當提供者啟動應用程式時，根服務提供者的存留期與應用程式/伺服器的存留期一致，並會在應用程式關閉時處置。

範圍服務會由建立這些服務的容器處置。 若是在根容器中建立範圍服務，因為當應用程式/伺服器關機時，服務只會由根容器處理，所以服務的存留期會提升為單一服務等級。 當呼叫 `BuildServiceProvider` 時，驗證服務範圍會攔截到這些情況。

如需詳細資訊，請參閱 <xref:fundamentals/host/web-host#scope-validation>。

## <a name="request-services"></a>要求服務

來自 `HttpContext`，在 ASP.NET Core 要求內提供的服務是透過 [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) 集合公開。

要求服務代表您在應用程式中設定和要求的服務。 當物件指定相依性時，這些會由在 `RequestServices` 中找到的類型來滿足，而非 `ApplicationServices`。

一般而言，應用程式不應該直接使用這些屬性。 相反地，透過類別建構函式要求類別所需的型別，以及允許架構插入相依性。 這會產生容易測試的類別。

> [!NOTE]
> 偏好要求相依性作為建構函式參數，而不要存取 `RequestServices` 集合。

## <a name="design-services-for-dependency-injection"></a>針對相依性插入設計服務

最佳做法是：

* 設計服務以使用相依性插入來取得其相依性。
* 避免具狀態的靜態類別和成員。 將應用程式設計成使用單一服務，以避免建立全域狀態。
* 避免直接在服務內具現化相依類別。 直接具現化會將程式碼耦合到特定實作。
* 讓應用程式類別維持在小型、情況良好且可輕鬆測試的狀態。

若類別有太多插入的相依性，這通常表示類別有太多責任而且正違反[單一責任原則 (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility) \(英文\)。 將類別負責的某些部分移到新的類別，以嘗試重構類別。 請記住，Razor Pages 頁面模型類別與 MVC 控制器類別應該專注於 UI 考量。 商務規則和資料存取實作詳細資料應該保存在適合這些[分開考量](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) (Separation of Concerns) 類別中。

### <a name="disposal-of-services"></a>處置服務

容器會為它建立的 <xref:System.IDisposable.Dispose*> 類型呼叫 <xref:System.IDisposable>。 若執行個體由使用者程式碼新增到容器中，它將不會自動處置。

在下列範例中，服務會建立服務容器並自動處置：

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public interface IService3 {}
public class Service3 : IService3, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<IService3>(sp => new Service3());
}
```

在下例中︰

* 服務容器不會建立服務實例。
* 架構不知道預期的服務存留期。
* 架構不會自動處置服務。
* 如果服務未在開發人員程式碼中明確處置，它們會持續存在，直到應用程式關閉為止。

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<Service1>(new Service1());
    services.AddSingleton(new Service2());
}
```

## <a name="default-service-container-replacement"></a>預設服務容器取代

內建的服務容器是設計用來滿足架構和大部分取用者應用程式的需求。 我們建議使用內建容器，除非您需要內建容器不支援的特定功能，例如：

* 屬性插入
* 根據名稱插入
* 子容器
* 自訂生命週期管理
* `Func<T>` 支援延遲初始設定
* 以慣例為基礎的註冊

下列協力廠商容器可以與 ASP.NET Core 應用程式搭配使用：

* [Autofac](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
* [DryIoc](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
* [寬限](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
* [LightInject](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
* [Lamar](https://jasperfx.github.io/lamar/)
* [Stashbox](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
* [Unity](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a>執行緒安全性

建立具備執行緒安全性的 singleton 服務。 如果 singleton 服務相依於暫時性服務，暫時性服務可能也需要具備執行緒安全性，取決於 singleton 如何使用它。

單一服務的 Factory 方法 (例如 [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*) 的第二個引數) 不需要是安全執行緒。 就像型別 (`static`) 建構函式一樣，它一定會被單一執行緒呼叫一次。

## <a name="recommendations"></a>建議

* 不支援以 `async/await` 與 `Task` 為基礎的服務解析。 C# 不支援非同步建構函式，因此建議的模式是以同步方式解析服務後使用非同步方法。

* 避免直接在服務容器中儲存資料與設定。 例如，使用者的購物車通常不應該新增至服務容器。 組態應該使用[選項模式](xref:fundamentals/configuration/options)。 同樣地，請避免只存在以允許存取某個其他物件的「資料持有者」物件。 最好是透過 DI 要求實際項目。

* 避免以靜態方式存取服務 (例如，以靜態方式設定 [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) 型別以四處使用)。

* 避免使用*服務定位器模式*，這會混用控制策略的[反轉](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion)。

  * 當您可以改用 DI 時，請勿叫用 <xref:System.IServiceProvider.GetService*> 來取得服務實例：

    **不正確：**

    ```csharp
    public class MyClass()
    {
        public void MyMethod()
        {
            var optionsMonitor = 
                _services.GetService<IOptionsMonitor<MyOptions>>();
            var option = optionsMonitor.CurrentValue.Option;

            ...
        }
    }
    ```

    **正確**：

    ```csharp
    public class MyClass
    {
        private readonly IOptionsMonitor<MyOptions> _optionsMonitor;

        public MyClass(IOptionsMonitor<MyOptions> optionsMonitor)
        {
            _optionsMonitor = optionsMonitor;
        }

        public void MyMethod()
        {
            var option = _optionsMonitor.CurrentValue.Option;

            ...
        }
    }
    ```

  * 避免在執行時間使用 <xref:System.IServiceProvider.GetService*>來插入解析相依性的 factory。

* 避免以靜態方式存取 `HttpContext` (例如 [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext))。

就像所有的建議集，您可能會遇到需要忽略建議的情況。 例外狀況很少見&mdash;大部分是架構本身內的特殊案例。

DI 是靜態/全域物件存取模式的「替代」選項。 如果您將 DI 與靜態物件存取混合，則可能無法實現 DI 的優點。

## <a name="additional-resources"></a>其他資源

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [在 ASP.NET Core 使用 Dependency Injection 撰寫簡潔的程式碼 (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [明確相依性準則](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [逆轉控制容器和相依性插入模式 (Martin Fowler)](https://www.martinfowler.com/articles/injection.html) \(英文\)
* [How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/) (如何使用 ASP.NET Core DI 中的多個介面註冊服務)

::: moniker-end
