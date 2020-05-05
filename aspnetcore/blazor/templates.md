---
title: ASP.NET Core Blazor範本
author: guardrex
description: 深入瞭解 ASP.NET Core Blazor應用程式範本Blazor和專案結構。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/19/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/templates
ms.openlocfilehash: c84d6415728bf56836d98cfa66d1b9d46d2eadc8
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82770897"
---
# <a name="aspnet-core-blazor-templates"></a>ASP.NET Core Blazor範本

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

此Blazor架構會提供範本來為每個Blazor裝載模型開發應用程式：

* BlazorWebAssembly （`blazorwasm`）
* Blazor伺服器（`blazorserver`）

如需裝載模型Blazor的詳細資訊，請<xref:blazor/hosting-models>參閱。

如需從範本建立Blazor應用程式的逐步指示，請參閱。 <xref:blazor/get-started>

藉由將`--help`選項傳遞至[dotnet new](/dotnet/core/tools/dotnet-new) CLI 命令，即可使用範本選項：

```dotnetcli
dotnet new blazorwasm --help
dotnet new blazorserver --help
```

## <a name="blazor-project-structure"></a>Blazor專案結構

下列檔案和資料夾組成從Blazor Blazor範本產生的應用程式：

* *Program.cs* &ndash;應用程式的進入點以設定：

  * ASP.NET Core[主機](xref:fundamentals/host/generic-host)（Blazor伺服器）
  * WebAssembly 主機（Blazor WebAssembly） &ndash;此檔案中的程式碼對於從Blazor WebAssembly 範本（`blazorwasm`）建立的應用程式而言是唯一的。
    * `App`元件（也就是應用程式的根元件）會指定為`Add`方法的`app` DOM 元素。
    * 服務可以使用主機產生器`ConfigureServices`上的方法來設定（例如， `builder.Services.AddSingleton<IMyDependency, MyDependency>();`）。
    * 您可以透過主機產生器（`builder.Configuration`）提供設定。

* *Startup.cs* （Blazor伺服器） &ndash;包含應用程式的啟動邏輯。 `Startup`類別會定義兩個方法：

  * `ConfigureServices`&ndash;設定應用程式的相依性[插入（DI）](xref:fundamentals/dependency-injection)服務。 在Blazor伺服器應用程式中，會藉由<xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor*>呼叫來新增`WeatherForecastService`服務，並將新增至服務容器，以供`FetchData`範例元件使用。
  * `Configure`&ndash;設定應用程式的要求處理管線：
    * <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*>呼叫來設定端點，以與瀏覽器進行即時連接。 連接是使用[SignalR](xref:signalr/introduction)建立的，這是將即時 web 功能新增至應用程式的架構。
    * 呼叫[MapFallbackToPage （"/_Host"）](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*)來設定應用程式的根頁面（*Pages/_Host. cshtml*）並啟用導覽。

* *wwwroot/index.html* （Blazor WebAssembly） &ndash;實作為 html 網頁之應用程式的根頁面：
  * 一開始要求應用程式的任何頁面時，會轉譯此頁面並在回應中傳回。
  * 頁面會指定呈現根`App`元件的位置。 `App`元件（*razor*）會指定為中`app` `AddComponent` `Startup.Configure`方法的 DOM 元素。
  * 已`_framework/blazor.webassembly.js`載入 JavaScript 檔案，其：
    * 下載 .NET 執行時間、應用程式和應用程式的相依性。
    * 初始化執行時間以執行應用程式。

* *Razor* &ndash;應用程式的根元件，其會使用<xref:Microsoft.AspNetCore.Components.Routing.Router>元件來設定用戶端路由。 `Router`元件會攔截瀏覽器導覽，並呈現符合所要求位址的頁面。

* *Pages*資料夾&ndash;包含可路由傳送的元件/頁面（*razor*），其組成Blazor應用程式和Razor Blazor伺服器應用程式的根頁面。 每個頁面的路由都是使用[`@page`](xref:mvc/views/razor#page)指示詞來指定。 此範本包含下列各項：
  * *_Host. cshtml* （Blazor伺服器） &ndash;應用程式的根頁面實作為Razor頁面：
    * 一開始要求應用程式的任何頁面時，會轉譯此頁面並在回應中傳回。
    * 會`_framework/blazor.server.js`載入 JavaScript 檔案，以設定瀏覽器與伺服器之間的SignalR即時連線。
    * [主機] 頁面會指定要`App`呈現根元件（*razor*）的位置。
  * `Counter`（*Razor*） &ndash;會實行計數器頁面。
  * `Error`（*錯誤。 razor*， Blazor僅限伺服器應用程式）&ndash;當應用程式中發生未處理的例外狀況時轉譯。
  * `FetchData`（*FetchData razor*） &ndash;會執行提取資料頁面。
  * `Index`（*Index. razor*） &ndash;實行首頁。

* *共用*資料夾&ndash;包含應用程式所使用的其他 UI 元件（*razor*）：
  * `MainLayout`（*MainLayout razor*） &ndash;應用程式的版面配置元件。
  * `NavMenu`（*Navmenu.cshtml razor*） &ndash;會執行提要欄位導覽。 包含[NavLink 元件](xref:blazor/routing#navlink-component)（<xref:Microsoft.AspNetCore.Components.Routing.NavLink>），它會呈現其他Razor元件的導覽連結。 `NavLink`元件會在載入元件時自動指出選取的狀態，協助使用者瞭解目前顯示的元件。

* *_Imports razor* &ndash;包含要包含Razor在應用程式元件（*razor*）中的通用指示詞，例如[`@using`](xref:mvc/views/razor#using)命名空間的指示詞。

* *[資料*資料夾Blazor （伺服器&ndash; ）] `WeatherForecast`包含的類別和執行`WeatherForecastService` ，可提供範例天氣資料給應用程式`FetchData`的元件。

* *wwwroot* &ndash;包含應用程式公用靜態資產之應用程式的[Web 根](xref:fundamentals/index#web-root)資料夾。

* *appsettings*應用程式的Blazor json （ &ndash;伺服器）設定。
