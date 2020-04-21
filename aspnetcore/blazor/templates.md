---
title: ASP.NET核心Blazor範本
author: guardrex
description: 瞭解ASP.NET核心Blazor應用範本和Blazor專案結構。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/19/2020
no-loc:
- Blazor
- SignalR
uid: blazor/templates
ms.openlocfilehash: 0a4a508beeae3d7bc665372d925989aa4e34ad52
ms.sourcegitcommit: 5547d920f322e5a823575c031529e4755ab119de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/21/2020
ms.locfileid: "81661720"
---
# <a name="aspnet-core-opno-locblazor-templates"></a>ASP.NET核心Blazor範本

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

該Blazor框架提供樣本,用於為每個Blazor託管模型開發應用:

* Blazor網路彙編`blazorwasm`( )
* Blazor伺服器`blazorserver`( )

有關Blazor託管模型的詳細資訊,請參閱<xref:blazor/hosting-models>。

有關從範本創建Blazor應用的分步說明,請參閱<xref:blazor/get-started>。

樣本選項可透過選項`--help`傳遞給[dotnet 新](/dotnet/core/tools/dotnet-new)CLI 命令:

```dotnetcli
dotnet new blazorwasm --help
dotnet new blazorserver --help
```

## <a name="opno-locblazor-project-structure"></a>Blazor專案結構

以下檔案與資料夾構成從BlazorBlazor樣本產生的應用:

* *Program.cs*&ndash;設定以下功能的應用程式點:

  * ASP.NET核心[主機](xref:fundamentals/host/generic-host)(Blazor伺服器 )
  * Web組裝主機Blazor&ndash;(WebAssembly) 此檔案中的代碼對於從 WebAssembly 範本Blazor()`blazorwasm`創建的應用是唯一的。
    * 元件`App`(是應用的根元件)被指定為`app``Add`方法的 DOM 元素。
    * 可以使用主機產生器上`ConfigureServices`的方法配置服務(例如`builder.Services.AddSingleton<IMyDependency, MyDependency>();`, 。
    * 配置可以透過主機生成器`builder.Configuration`() 提供。

* *Startup.cs(*Blazor伺服器&ndash;) 包含應用程式的啟動邏輯。 該`Startup`類別定義兩種方法:

  * `ConfigureServices`&ndash;配置應用的依賴[項注入 (DI)](xref:fundamentals/dependency-injection)服務。 在Blazor「伺服器」應用中,服務透過呼<xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor*>叫添加`WeatherForecastService`,並將添加到服務容器中,供範`FetchData`例元件使用。
  * `Configure`&ndash;設定應用的要求處理導管:
    * <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*>調用 以設置與瀏覽器的即時連接的終結點。 該連接與[SignalR](xref:signalr/introduction)創建,這是一個框架,用於向應用添加即時 Web 功能。
    * [MapFallbackToPage("/_Host")](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*)被調用來設置應用程式的根頁 (*頁面/ _Host.cshtml)* 並啟用導航。

* *wwwroot/index.html* wwwroot/index.html(WebAssembly)&ndash;Blazor作為 HTML 頁面實現的應用程式的根頁:
  * 當最初請求應用的任何頁面時,此頁將呈現並在回應中返回。
  * 該頁指定根`App`元件的呈現位置。 `App`元件 *(App.razor*) 被`app`指定為`Startup.Configure`方法`AddComponent`中的 DOM 元素。
  * 載入`_framework/blazor.webassembly.js`JavaScript 檔,其中:
    * 下載 .NET 運行時、應用和應用的依賴項。
    * 初始化運行時以運行應用。

* *App.razor*&ndash;<xref:Microsoft.AspNetCore.Components.Routing.Router>使用 元件設置用戶端路由的應用的根元件。 元件`Router`攔截瀏覽器導航並呈現與請求的位址匹配的頁面。

* *頁面*&ndash;資料夾Blazor包含構成 應用的可路由元件Blazor/頁面 *(.razor)* 和伺服器應用的根 Razor 頁面。 使用指令指定每個頁面的[`@page`](xref:mvc/views/razor#page)路由。 該樣本包括以下內容:
  * *_Host.cshtml(*Blazor伺服器&ndash;) 作為 Razor 頁面實現的應用程式的根頁:
    * 當最初請求應用的任何頁面時,此頁將呈現並在回應中返回。
    * 載入`_framework/blazor.server.js`JavaScript 檔案,該檔設定瀏覽器和伺服器SignalR之間的即時 連接。
    * 主機"頁指定呈現根`App`元件 (*App.razor*) 的位置。
  * `Counter`(*Counter.razor*反&ndash;. razor ) 實現計數器頁。
  * `Error`(*錯誤.剃鬚刀*,Blazor僅限伺服器應用 )&ndash;在應用中發生未處理的異常時呈現。
  * `FetchData`(*提取資料.razor*)&ndash;實現提取數據頁。
  * `Index`(*索引.razor*)&ndash;實現主頁。

* *分享*&ndash;資料夾 包含應用程式使用的其他 UI 元件 (*.razor*):
  * `MainLayout`(*主佈局.razor*)&ndash;應用程式的佈局元件。
  * `NavMenu`(*NavMenu.razor*)&ndash;實現側邊欄導航。 包括[NavLink](xref:blazor/routing#navlink-component)<xref:Microsoft.AspNetCore.Components.Routing.NavLink>元件 ( ),該元件呈現指向其他 Razor 元件的導航連結。 元件`NavLink`在載入其元件時自動指示所選狀態,這有助於使用者瞭解當前顯示的元件。

* *_Imports.razor*&ndash;包括要包含在應用元件 *(.razor)* 中的常見 Razor 指令[`@using`](xref:mvc/views/razor#using),例如命名空間的指令。

* *數據*資料夾Blazor(伺服器&ndash;)`WeatherForecast`包含向應用`FetchData`元件 提供範`WeatherForecastService`例天氣 資料的類和實現。

* *wwwroot*&ndash;包含應用的公共靜態資產的應用的[Web 根](xref:fundamentals/index#web-root)資料夾。

* *應用設置.json* Blazor (&ndash;伺服器 ) 應用程式的配置設置。
