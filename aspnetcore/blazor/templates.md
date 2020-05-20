---
標題： ' ASP.NET Core Blazor 範本 ' 作者：描述：「瞭解 ASP.NET Core Blazor 應用程式範本和 Blazor 專案結構」。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

---
# <a name="aspnet-core-blazor-templates"></a>ASP.NET Core Blazor 範本

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

此 Blazor 架構會提供範本來為每個裝載模型開發應用程式 Blazor ：

* BlazorWebAssembly （ `blazorwasm` ）
* Blazor伺服器（ `blazorserver` ）

如需裝載模型的詳細資訊 Blazor ，請參閱 <xref:blazor/hosting-models> 。

如需從範本建立應用程式的逐步指示 Blazor ，請參閱 <xref:blazor/get-started> 。

藉由將 `--help` 選項傳遞至[dotnet new](/dotnet/core/tools/dotnet-new) CLI 命令，即可使用範本選項：

```dotnetcli
dotnet new blazorwasm --help
dotnet new blazorserver --help
```

## <a name="blazor-project-structure"></a>Blazor專案結構

下列檔案和資料夾組成 Blazor 從範本產生的應用程式 Blazor ：

* *Program.cs* &ndash; 應用程式的進入點以設定：

  * ASP.NET Core[主機](xref:fundamentals/host/generic-host)（ Blazor 伺服器）
  * WebAssembly 主機（ Blazor WebAssembly） &ndash; 此檔案中的程式碼對於從 Blazor WebAssembly 範本（）建立的應用程式而言是唯一的 `blazorwasm` 。
    * `App`元件（也就是應用程式的根元件）會指定為方法的 `app` DOM 元素 `Add` 。
    * 服務可以使用主機產生器 `ConfigureServices` 上的方法來設定（例如， `builder.Services.AddSingleton<IMyDependency, MyDependency>();` ）。
    * 您可以透過主機產生器（）提供設定 `builder.Configuration` 。

* *Startup.cs* （ Blazor 伺服器） &ndash; 包含應用程式的啟動邏輯。 `Startup`類別會定義兩個方法：

  * `ConfigureServices`設定 &ndash; 應用程式的相依性[插入（DI）](xref:fundamentals/dependency-injection)服務。 在 Blazor 伺服器應用程式中，會藉由呼叫來新增服務 <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor%2A> ，並將 `WeatherForecastService` 新增至服務容器，以供範例 `FetchData` 元件使用。
  * `Configure`設定 &ndash; 應用程式的要求處理管線：
    * <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub%2A>呼叫來設定端點，以與瀏覽器進行即時連接。 連接是使用建立的 [SignalR](xref:signalr/introduction) ，這是將即時 web 功能新增至應用程式的架構。
    * 呼叫[MapFallbackToPage （"/_Host"）](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*)來設定應用程式的根頁面（*Pages/_Host. cshtml*）並啟用導覽。

* *wwwroot/index.html* （ Blazor WebAssembly）實 &ndash; 作為 html 網頁之應用程式的根頁面：
  * 一開始要求應用程式的任何頁面時，會轉譯此頁面並在回應中傳回。
  * 頁面會指定呈現根元件的位置 `App` 。 `App`元件（*razor*）會指定為 `app` 中方法的 DOM 元素 `AddComponent` `Startup.Configure` 。
  * `_framework/blazor.webassembly.js`已載入 JavaScript 檔案，其：
    * 下載 .NET 執行時間、應用程式和應用程式的相依性。
    * 初始化執行時間以執行應用程式。

* *Razor*應用 &ndash; 程式的根元件，其會使用元件來設定用戶端路由 <xref:Microsoft.AspNetCore.Components.Routing.Router> 。 `Router`元件會攔截瀏覽器導覽，並呈現符合所要求位址的頁面。

* *Pages*資料夾 &ndash; 包含可路由傳送的元件/頁面（*razor*），其組成 Blazor 應用程式和 Razor Blazor 伺服器應用程式的根頁面。 每個頁面的路由都是使用指示詞來指定 [`@page`](xref:mvc/views/razor#page) 。 此範本包含下列各項：
  * *_Host. cshtml* （ Blazor 伺服器） &ndash; 應用程式的根頁面實作為 Razor 頁面：
    * 一開始要求應用程式的任何頁面時，會轉譯此頁面並在回應中傳回。
    * `_framework/blazor.server.js`會載入 JavaScript 檔案，以設定 SignalR 瀏覽器與伺服器之間的即時連線。
    * [主機] 頁面會指定要 `App` 呈現根元件（*razor*）的位置。
  * `Counter`（*Razor*）會 &ndash; 實行計數器頁面。
  * `Error`（*錯誤。 razor*， Blazor僅限伺服器應用程式）在 &ndash; 應用程式中發生未處理的例外狀況時呈現。
  * `FetchData`（*FetchData razor*）會執行 &ndash; 提取資料頁面。
  * `Index`（*Index. razor*） &ndash; 實行首頁。

* *共用*資料夾 &ndash; 包含應用程式所使用的其他 UI 元件（*razor*）：
  * `MainLayout`（*MainLayout razor*） &ndash; 應用程式的版面配置元件。
  * `NavMenu`（*Navmenu.cshtml razor*）會執行 &ndash; 提要欄位導覽。 包含[NavLink 元件](xref:blazor/routing#navlink-component)（ <xref:Microsoft.AspNetCore.Components.Routing.NavLink> ），它會呈現其他元件的導覽連結 Razor 。 元件會在 `NavLink` 載入元件時自動指出選取的狀態，協助使用者瞭解目前顯示的元件。

* *_Imports razor* &ndash;包含 Razor 要包含在應用程式元件（*razor*）中的通用指示詞，例如 [`@using`](xref:mvc/views/razor#using) 命名空間的指示詞。

* [*資料*資料夾（ Blazor 伺服器）] 包含的 &ndash; `WeatherForecast` 類別和執行 `WeatherForecastService` ，可提供範例天氣資料給應用程式的 `FetchData` 元件。

* *wwwroot* &ndash;應用程式的[Web 根](xref:fundamentals/index#web-root)資料夾，其中包含應用程式的公用靜態資產。

* *appsettings* Blazor 應用程式的 json （伺服器） &ndash; 設定。
