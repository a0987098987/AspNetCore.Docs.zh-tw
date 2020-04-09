---
title: ASP.NET核心Blazor相依項
author: guardrex
description: 瞭解應用Blazor如何將服務注入元件。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/20/2020
no-loc:
- Blazor
- SignalR
uid: blazor/dependency-injection
ms.openlocfilehash: 4cdde9ee8c9fd9adf00894a067d32965b180e5ec
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78658071"
---
# <a name="aspnet-core-blazor-dependency-injection"></a>ASP.NET核心布拉佐爾依賴注入

由[雷納·斯特羅佩克](https://www.timecockpit.com)和[邁克·魯索斯](https://github.com/mjrousos)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

布拉佐爾支持[依賴注入 (DI)。](xref:fundamentals/dependency-injection) 應用可以通過將它們注入元件來使用內置服務。 應用還可以定義和註冊自定義服務,並通過 DI 在整個應用中提供這些服務。

DI 是一種用於訪問在中心位置配置的服務的技術。 這在 Blazor 應用中非常有用,可用於:

* 跨多個元件(稱為*單例*服務)共用服務類的單個實例。
* 通過使用引用抽象將元件與具體服務類分離。 例如,請考慮用於訪問應用中`IDataAccess`數據的介面。 該介面由具體`DataAccess`類實現,並在應用的服務容器中註冊為服務。 當元件使用 DI 接收`IDataAccess`實現 時,元件不會耦合到具體類型。 可以交換實現,也許是為了單元測試中的模擬實現。

## <a name="default-services"></a>預設服務

默認服務將自動添加到應用的服務集合中。

| 服務 | 存留期 | 描述 |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | 單一 | 提供了從 URI 識別的資源發送 HTTP 請求和接收 HTTP 回應的方法。<br><br>Blazor `HttpClient` WebAssembly 應用中的實例使用瀏覽器處理後台的 HTTP 流量。<br><br>默認情況下,Blazor `HttpClient` Server 應用不包括配置為服務的應用。 向`HttpClient`Blazor 伺服器應用提供 。<br><br>如需詳細資訊，請參閱 <xref:blazor/call-web-api>。 |
| `IJSRuntime` | 單頓(布拉佐網路大會)<br>範圍 (布拉佐伺服器) | 表示調度 JavaScript 呼叫的 JavaScript 執行時的實例。 如需詳細資訊，請參閱 <xref:blazor/call-javascript-from-dotnet>。 |
| `NavigationManager` | 單頓(布拉佐網路大會)<br>範圍 (布拉佐伺服器) | 包含用於處理URI和導航狀態的幫助器。 有關詳細資訊,請參閱[URI 和瀏覽狀態說明器](xref:blazor/routing#uri-and-navigation-state-helpers)。 |

自定義服務提供者不會自動提供表中列出的預設服務。 如果使用自定義服務提供者並且需要表中顯示的任何服務,則向新的服務提供者添加所需的服務。

## <a name="add-services-to-an-app"></a>新增服務新增到應用程式

### <a name="blazor-webassembly"></a>Blazor WebAssembly

在`Main`*Program.cs*方法中為應用的服務集合配置服務。 在下面的範例中,`MyDependency`實現註冊`IMyDependency`為 :

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

生成主機后,可以在呈現任何元件之前從根 DI 作用域訪問服務。 這對於在呈現內容之前執行初始化邏輯非常有用:

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

主機還為應用提供中央配置實例。 基於前面的範例,天氣服務的網址從預設設定來源 (例如*appsettings.json)* 傳遞到`InitializeWeatherAsync`:

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

建立新應用程式後,請檢查以下`Startup.ConfigureServices`方法:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

方法`ConfigureServices`傳遞的是<xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, 它是服務描述符<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>物件 () 的清單。 通過向服務集合提供服務描述符來添加服務。 下面的範例展示該概念與`IDataAccess`介面與介面與`DataAccess`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

### <a name="service-lifetime"></a>服務壽命

可以使用下表中顯示的存留期配置服務。

| 存留期 | 描述 |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | BlazorWeb組裝應用當前沒有 DI 作用域的概念。 `Scoped`-註冊服務就像服務一`Singleton`樣。 但是,Blazor伺服器託管模型支援`Scoped`存留期。 在Blazor伺服器應用中,作用網域服務註冊的範圍限定到*連接*。 因此,對於應限定為當前用戶的服務,使用作用域服務是首選,即使當前意圖是在瀏覽器中運行用戶端。 |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | DI 建立服務的*單一實體 。* 需要`Singleton`服務的所有元件都接收同一服務的實例。 |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | 每當元件從`Transient`服務容器取得服務的實體時,它都會收到服務*的新實體 。* |

DI 系統基於 ASP.NET 核心中的 DI 系統。 如需詳細資訊，請參閱 <xref:fundamentals/dependency-injection>。

## <a name="request-a-service-in-a-component"></a>要求元件中的服務

將服務添加到服務集合後,使用[\@注入](xref:mvc/views/razor#inject)Razor 指令將服務注入到元件中。 `@inject`有兩個參數:

* 鍵入&ndash;要注入的服務類型。
* 屬性&ndash;接收注入的應用服務的屬性的名稱。 屬性不需要手動創建。 編譯器創建該屬性。

如需詳細資訊，請參閱 <xref:mvc/views/dependency-injection>。

使用多個`@inject`語句注入不同的服務。

下列範例示範如何使用 `@inject`。 服務實現`Services.IDataAccess`被注入到元件的屬性`DataRepository`中。 請注意代碼如何只使用`IDataAccess`抽象:

[!code-razor[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

在內部,生成的屬性`DataRepository`(`InjectAttribute`) 使用 屬性。 通常,此屬性不會直接使用。 如果元件需要基數,並且基類別也需要注入屬性,則手動添加 : `InjectAttribute`

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

在從基類派生的元件中,`@inject`不需要該指令。 基`InjectAttribute`類別的已足夠:

```razor
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a>在服務中使用 DI

複雜的服務可能需要其他服務。 在上一個示例中`DataAccess`,可能`HttpClient`需要 默認服務。 `@inject`(或`InjectAttribute`) 不適用於服務。 必須改用*建構函式注入*。 通過向服務的構造函數添加參數來添加所需的服務。 當 DI 創建服務時,它會識別構造函數中所需的服務,並相應地提供這些服務。

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

建構函數注入的先決條件:

* 一個構造函數必須存在,其參數都可以由 DI 實現。 如果 DI 未涵蓋的其他參數指定預設值,則允許它們。
* 適用的建構函數必須是*公共的*。
* 必須存在一個適用的構造函數。 如果模稜兩可,DI 將引發異常。

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a>管理 DI 作用網域的實用程式基礎元件類

在 ASP.NET 核心應用中,作用域服務通常可限定為當前請求。 請求完成後,任何作用域或瞬態服務將由 DI 系統處理。 在BlazorServer 應用中,請求範圍持續於用戶端連接的持續時間,這可能導致瞬態和作用域服務的壽命比預期長得多。 在BlazorWebAssembly 應用中,以作用域存留期註冊的服務被視爲單例,因此它們比典型 ASP.NET Core 應用中的作用域服務活得更長。

限制Blazor套用的服務存留期的方法`OwningComponentBase`是使用類型 。 `OwningComponentBase`是從創建與元件存留期`ComponentBase`對應的 DI 作用域派生的抽象類型。 使用此作用域,可以使用具有作用域存留期的 DI 服務,並使它們像元件一樣存活。 銷毀元件后,將釋放來自元件作用域服務提供者的服務。 這對於以下服務非常有用:

* 應在元件中重複使用,因為瞬態存留期不合適。
* 不應跨元件共用,因為單例存留期不合適。

`OwningComponentBase`該類型的兩個版本可用:

* `OwningComponentBase`是具有類型`ComponentBase``ScopedServices``IServiceProvider`受保護屬性的抽象的一次性子級。 此提供程式可用於解析範圍為元件存留期的服務。

  使用`@inject``InjectAttribute`或`[Inject]`( ) 注入到元件中的 DI 服務不是在元件的作用域中創建的。 要使用元件的範圍,必須使用`ScopedServices.GetRequiredService`或`ScopedServices.GetService`解析服務。 使用`ScopedServices`提供程式解析的任何服務都提供來自同一作用域的依賴項。

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

* `OwningComponentBase<T>`派生並`OwningComponentBase`添加從作用域`Service`DI 提供`T`程式返回 的 實例的屬性。 此類型是存取作用域服務的便捷方法`IServiceProvider`, 無需使用應用使用元件作用域從 DI 容器中需要一個主服務的實例。 該`ScopedServices`屬性可用,因此如有必要,應用可以獲取其他類型的服務。

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

## <a name="use-of-entity-framework-dbcontext-from-di"></a>使用來自 DI 的實體框架 DbContext

在 Web 應用中從 DI 檢索的一種`DbContext`常見服務類型是實體框架 (EF) 物件。 預設情況下,使用`IServiceCollection.AddDbContext`將添加`DbContext`作為作用域服務的 EF 服務註冊。 註冊為作用域服務可能會導致Blazor應用中出現問題,因為它`DbContext`會導致 實例在應用中長壽命且共用。 `DbContext`不帶線程安全,不能同時使用。

根據應用的不同,使用`OwningComponentBase`將的範圍`DbContext`限制為單個元件*可能會*解決此問題。 `DbContext`如果元件不並行 使用 , 派生`OwningComponentBase`元件 並從 中`DbContext`派生`ScopedServices`並 檢索 就足夠了,因為它可確保:

* 單獨的元件不分享 。 `DbContext`
* 只`DbContext`活只要元件取決於它。

如果單個元件可能同時使用一`DbContext`個元件(例如,每次用戶選擇按鈕時),即使使用`OwningComponentBase`也不會避免併發 EF 操作的問題。 在這種情況下,`DbContext`對每個邏輯 EF 操作使用不同的操作。 使用以下任一方法:

* 直接使用`DbContext``DbContextOptions<TContext>`參數創建參數,可以從 DI 檢索,並且線程安全。

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

* 在具有`DbContext`瞬態存留期的服務容器中註冊:
  * 註冊上文時,請使用`ServiceLifetime.Transient`。 擴展`AddDbContext`方法採用兩個類型的`ServiceLifetime`可選參數。 要使用此方法,只需要參數`contextLifetime`為`ServiceLifetime.Transient`。 `optionsLifetime`可以保留預設值`ServiceLifetime.Scoped`。

    ```csharp
    services.AddDbContext<AppDbContext>(options =>
         options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")),
         ServiceLifetime.Transient);
    ```  

  * 瞬態`DbContext`可以像正常一樣(`@inject`使用 )注入不會並行執行多個 EF 操作的元件中。 可以同時執行多個 EF 操作的使用者`DbContext``IServiceProvider.GetRequiredService`可以使用 請求每個並行操作的單獨物件。

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
