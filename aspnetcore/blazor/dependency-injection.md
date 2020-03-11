---
title: ASP.NET Core Blazor 相依性插入
author: guardrex
description: 瞭解 Blazor 應用程式如何將服務插入元件中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/20/2020
no-loc:
- Blazor
- SignalR
uid: blazor/dependency-injection
ms.openlocfilehash: 4cdde9ee8c9fd9adf00894a067d32965b180e5ec
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658071"
---
# <a name="aspnet-core-blazor-dependency-injection"></a>ASP.NET Core Blazor 相依性插入

By [Rainer Stropek](https://www.timecockpit.com)和[Mike Rousos](https://github.com/mjrousos)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor 支援相依性[插入（DI）](xref:fundamentals/dependency-injection)。 應用程式可以使用內建的服務，方法是將它們插入元件中。 應用程式也可以定義和註冊自訂服務，並透過 DI 讓它們可在整個應用程式中使用。

DI 是用來存取集中位置所設定之服務的技術。 在 Blazor 應用程式中，這會很有用：

* 跨許多元件（稱為*單一*服務）共用服務類別的單一實例。
* 使用參考抽象，將元件與具體的服務類別分離。 例如，請考慮使用介面 `IDataAccess` 來存取應用程式中的資料。 介面是由具體的 `DataAccess` 類別來執行，並在應用程式的服務容器中註冊為服務。 當元件使用 DI 來接收 `IDataAccess` 執行時，該元件不會與具象類型結合。 可以交換執行，也許是針對單元測試中的 mock 執行。

## <a name="default-services"></a>預設服務

預設服務會自動新增至應用程式的服務集合。

| 服務 | 存留期 | 描述 |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | 單一 | 提供方法來傳送 HTTP 要求，以及從 URI 所識別的資源接收 HTTP 回應。<br><br>Blazor WebAssembly 應用程式中的 `HttpClient` 實例會使用瀏覽器來處理背景中的 HTTP 流量。<br><br>Blazor 伺服器應用程式預設不會包含設定為服務的 `HttpClient`。 提供 Blazor 伺服器應用程式的 `HttpClient`。<br><br>如需詳細資訊，請參閱 <xref:blazor/call-web-api>。 |
| `IJSRuntime` | Singleton （Blazor WebAssembly）<br>限定範圍（Blazor 伺服器） | 代表在其中分派 JavaScript 呼叫的 JavaScript 執行時間實例。 如需詳細資訊，請參閱 <xref:blazor/call-javascript-from-dotnet>。 |
| `NavigationManager` | Singleton （Blazor WebAssembly）<br>限定範圍（Blazor 伺服器） | 包含使用 Uri 和導覽狀態的協助程式。 如需詳細資訊，請參閱[URI 和流覽狀態](xref:blazor/routing#uri-and-navigation-state-helpers)協助程式。 |

自訂服務提供者不會自動提供表格中所列的預設服務。 如果您使用自訂服務提供者，而且需要資料表中所顯示的任何服務，請將所需的服務新增至新的服務提供者。

## <a name="add-services-to-an-app"></a>將服務新增至應用程式

### <a name="blazor-webassembly"></a>Blazor WebAssembly

在*Program.cs*的 `Main` 方法中，為應用程式的服務集合設定服務。 在下列範例中，會為 `IMyDependency`註冊 `MyDependency` 實作為：

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<IMyDependency, MyDependency>();
        builder.RootComponents.Add<App>("app");

        await builder.Build().RunAsync();
    }
}
```

建立主機之後，您就可以從根 DI 範圍存取服務，然後再呈現任何元件。 這有助於在轉譯內容之前執行初始化邏輯：

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync();

        await host.RunAsync();
    }
}
```

主機也會提供應用程式的中央設定實例。 以先前的範例為基礎，氣象服務的 URL 會從預設設定來源（例如， *appsettings*）傳遞至 `InitializeWeatherAsync`：

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync(
            host.Configuration["WeatherServiceUrl"]);

        await host.RunAsync();
    }
}
```

### <a name="blazor-server"></a>Blazor 伺服器

建立新的應用程式之後，請檢查 `Startup.ConfigureServices` 方法：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

`ConfigureServices` 方法會傳遞 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>，這是服務描述元物件（<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>）的清單。 服務會藉由將服務描述項提供給服務集合來新增。 下列範例示範 `IDataAccess` 介面和其具體執行 `DataAccess`的概念：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

### <a name="service-lifetime"></a>服務存留期

您可以使用下表所示的存留期來設定服務。

| 存留期 | 描述 |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor WebAssembly 應用程式目前沒有 DI 範圍的概念。 `Scoped`註冊的服務行為就像 `Singleton` 服務一樣。 不過，Blazor 伺服器裝載模型支援 `Scoped` 存留期。 在 Blazor 伺服器應用程式中，限定範圍的服務註冊的範圍是*連接*。 因此，即使目前的意圖是在瀏覽器中執行用戶端，使用範圍服務也適用于應該範圍設定為目前使用者的服務。 |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | DI 會建立服務的*單一實例*。 所有需要 `Singleton` 服務的元件都會收到相同服務的實例。 |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | 每當元件從服務容器取得 `Transient` 服務的實例時，就會收到服務的*新實例*。 |

DI 系統是以 ASP.NET Core 中的 DI 系統為基礎。 如需詳細資訊，請參閱 <xref:fundamentals/dependency-injection>。

## <a name="request-a-service-in-a-component"></a>要求元件中的服務

將服務新增至服務集合之後，請使用[\@插入](xref:mvc/views/razor#inject)Razor 指示詞，將服務插入元件中。 `@inject` 有兩個參數：

* 輸入 &ndash; 要插入之服務的類型。
* 屬性 &ndash; 接收插入的應用程式服務之屬性的名稱。 屬性不需要手動建立。 編譯器會建立屬性。

如需詳細資訊，請參閱 <xref:mvc/views/dependency-injection>。

使用多個 `@inject` 語句來插入不同的服務。

下列範例示範如何使用 `@inject`。 執行 `Services.IDataAccess` 的服務會插入元件的屬性 `DataRepository`中。 請注意程式碼如何使用 `IDataAccess` 抽象：

[!code-razor[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

就內部而言，產生的屬性（`DataRepository`）會使用 `InjectAttribute` 屬性。 通常不會直接使用這個屬性。 如果元件需要基類，而且基類也需要插入的屬性，請手動加入 `InjectAttribute`：

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

在衍生自基類的元件中，不需要 `@inject` 指示詞。 基類的 `InjectAttribute` 已足夠：

```razor
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a>在服務中使用 DI

複雜的服務可能需要額外的服務。 在先前的範例中，`DataAccess` 可能需要 `HttpClient` 預設服務。 `@inject` （或 `InjectAttribute`）無法在服務中使用。 必須改為使用*函數插入*。 將參數新增至服務的函式，即可加入必要的服務。 當 DI 建立服務時，它會辨識它在此函式中所需的服務，並據以提供它們。

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

在 ASP.NET Core 應用程式中，限域服務的範圍通常是目前的要求。 要求完成之後，DI 系統會處置任何範圍或暫時性的服務。 在 Blazor 伺服器應用程式中，要求範圍會在用戶端連線期間持續進行，這可能會導致暫時性和範圍內的服務生活得比預期的長。 在 Blazor WebAssembly 應用程式中，以限定範圍存留期註冊的服務會被視為單次個體，因此其存留時間比一般 ASP.NET Core 應用程式中的範圍服務長。

限制 Blazor 應用程式中服務存留期的方法是使用 `OwningComponentBase` 類型。 `OwningComponentBase` 是衍生自 `ComponentBase` 的抽象類別型，它會建立與元件存留期相對應的 DI 範圍。 使用此範圍時，您可以使用具有限定範圍存留期的 DI 服務，而且只要元件，就可以讓它們正常運作。 當元件損毀時，來自元件範圍服務提供者的服務也會一併處置。 這適用于下列服務：

* 應該在元件內重複使用，因為暫時性存留期並不適當。
* 不應跨元件共用，因為單一存留期並不適當。

有兩種版本的 `OwningComponentBase` 類型可供使用：

* `OwningComponentBase` 是 `ComponentBase` 類型的抽象、可處置子系，具有 `IServiceProvider`類型的受保護 `ScopedServices` 屬性。 這個提供者可以用來解析範圍設定為元件存留期的服務。

  使用 `@inject` 或 `InjectAttribute` （`[Inject]`）插入元件中的 DI 服務，不會在元件的範圍內建立。 若要使用元件的範圍，必須使用 `ScopedServices.GetRequiredService` 或 `ScopedServices.GetService`來解析服務。 使用 `ScopedServices` 提供者解決的任何服務都會從該相同範圍提供其相依性。

  ```razor
  @page "/preferences"
  @using Microsoft.Extensions.DependencyInjection
  @inherits OwningComponentBase

  <h1>User (@UserService.Name)</h1>

  <ul>
      @foreach (var setting in SettingService.GetSettings())
      {
          <li>@setting.SettingName: @setting.SettingValue</li>
      }
  </ul>

  @code {
      private IUserService UserService { get; set; }
      private ISettingService SettingService { get; set; }

      protected override void OnInitialized()
      {
          UserService = ScopedServices.GetRequiredService<IUserService>();
          SettingService = ScopedServices.GetRequiredService<ISettingService>();
      }
  }
  ```

* `OwningComponentBase<T>` 衍生自 `OwningComponentBase` 並加入屬性 `Service`，從範圍 DI 提供者傳回 `T` 的實例。 當應用程式需要使用元件範圍的 DI 容器中的一個主要服務時，此類型可方便存取已設定範圍的服務，而不需使用 `IServiceProvider` 的實例。 `ScopedServices` 屬性可供使用，因此應用程式可以取得其他類型的服務（如有需要）。

  ```razor
  @page "/users"
  @attribute [Authorize]
  @inherits OwningComponentBase<AppDbContext>

  <h1>Users (@Service.Users.Count())</h1>

  <ul>
      @foreach (var user in Service.Users)
      {
          <li>@user.UserName</li>
      }
  </ul>
  ```

## <a name="use-of-entity-framework-dbcontext-from-di"></a>從 DI 使用 Entity Framework DbCoNtext

從 web 應用程式中的 DI 取出的一個常見服務類型是 Entity Framework （EF） `DbContext` 物件。 使用 `IServiceCollection.AddDbContext` 註冊 EF 服務時，預設會將 `DbContext` 新增為範圍服務。 註冊為範圍服務可能會導致 Blazor 應用程式發生問題，因為這會導致 `DbContext` 實例長期存留，並在應用程式間共用。 `DbContext` 不是安全線程，而且不得同時使用。

根據應用程式而定，使用 `OwningComponentBase` 將 `DbContext` 的範圍限制為單一元件，*可能會*解決此問題。 如果元件未平行使用 `DbContext`，從 `OwningComponentBase` 衍生元件，並從 `ScopedServices` 中抓取 `DbContext` 就已足夠，因為它可確保：

* 個別元件不會共用 `DbContext`。
* `DbContext` 的存留時間只會取決於元件。

如果單一元件可能會同時使用 `DbContext` （例如，每次使用者選取按鈕時），即使使用 `OwningComponentBase` 也不會避免並行 EF 作業的問題。 在此情況下，請針對每個邏輯 EF 作業使用不同的 `DbContext`。 請使用下列其中一種方法：

* 使用 `DbContextOptions<TContext>` 做為引數，直接建立 `DbContext`，這可以從 DI 抓取，而且是安全線程。

    ```razor
    @page "/example"
    @inject DbContextOptions<AppDbContext> DbContextOptions

    <ul>
        @foreach (var item in _data)
        {
            <li>@item</li>
        }
    </ul>

    <button @onclick="LoadData">Load Data</button>

    @code {
        private List<string> _data = new List<string>();

        private async Task LoadData()
        {
            _data = await GetAsync();
            StateHasChanged();
        }

        public async Task<List<string>> GetAsync()
        {
            using (var context = new AppDbContext(DbContextOptions))
            {
                return await context.Products.Select(p => p.Name).ToListAsync();
            }
        }
    }
    ```

* 在服務容器中，以暫時性存留期註冊 `DbContext`：
  * 註冊內容時，請使用 `ServiceLifetime.Transient`。 `AddDbContext` 擴充方法會接受 `ServiceLifetime`類型的兩個選擇性參數。 若要使用此方法，只需要 `ServiceLifetime.Transient``contextLifetime` 參數。 `optionsLifetime` 可以保留其預設值 `ServiceLifetime.Scoped`。

    ```csharp
    services.AddDbContext<AppDbContext>(options =>
         options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")),
         ServiceLifetime.Transient);
    ```  

  * 暫時性 `DbContext` 可以像平常一樣（使用 `@inject`）插入不會平行執行多個 EF 作業的元件。 可能同時執行多個 EF 作業的人員可以使用 `IServiceProvider.GetRequiredService`，針對每個平行作業要求個別的 `DbContext` 物件。

    ```razor
    @page "/example"
    @using Microsoft.Extensions.DependencyInjection
    @inject IServiceProvider ServiceProvider

    <ul>
        @foreach (var item in _data)
        {
            <li>@item</li>
        }
    </ul>

    <button @onclick="LoadData">Load Data</button>

    @code {
        private List<string> _data = new List<string>();

        private async Task LoadData()
        {
            _data = await GetAsync();
            StateHasChanged();
        }

        public async Task<List<string>> GetAsync()
        {
            using (var context = ServiceProvider.GetRequiredService<AppDbContext>())
            {
                return await context.Products.Select(p => p.Name).ToListAsync();
            }
        }
    }
    ```

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
