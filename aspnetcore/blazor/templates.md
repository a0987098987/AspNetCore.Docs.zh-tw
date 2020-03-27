---
title: ASP.NET Core Blazor 範本
author: guardrex
description: 深入瞭解 ASP.NET Core Blazor 應用程式範本和 Blazor 專案結構。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/templates
ms.openlocfilehash: 71a9d9eee8637dda0b3cecac82ff96a0c3bfedb5
ms.sourcegitcommit: f3b1bcfd108e5d53f73abc0bf2555890869d953b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80320986"
---
# <a name="aspnet-core-opno-locblazor-templates"></a>ASP.NET Core Blazor 範本

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor 架構會提供範本來為每個 Blazor 裝載模型開發應用程式：

* Blazor WebAssembly （`blazorwasm`）
* Blazor Server （`blazorserver`）

如需 Blazor裝載模型的詳細資訊，請參閱 <xref:blazor/hosting-models>。

如需從範本建立 Blazor 應用程式的逐步指示，請參閱 <xref:blazor/get-started>。

## <a name="opno-locblazor-project-structure"></a>Blazor 專案結構

下列檔案和資料夾組成從 Blazor 範本產生的 Blazor 應用程式：

* *Program.cs* &ndash; 應用程式的進入點來設定：

  * ASP.NET Core[主機](xref:fundamentals/host/generic-host)（Blazor 伺服器）
  * WebAssembly 主機（Blazor WebAssembly） &ndash; 此檔案中的程式碼對於從 Blazor WebAssembly 範本（`blazorwasm`）所建立的應用程式而言是唯一的。
    * `App` 元件（也就是應用程式的根元件）會指定為 `Add` 方法的 `app` DOM 元素。
    * 服務可以使用主機產生器上的 `ConfigureServices` 方法來設定（例如，`builder.Services.AddSingleton<IMyDependency, MyDependency>();`）。
    * 您可以透過主機產生器（`builder.Configuration`）來提供設定。

* *Startup.cs* （Blazor Server） &ndash; 包含應用程式的啟動邏輯。 `Startup` 類別會定義兩個方法：

  * `ConfigureServices` &ndash; 會設定應用程式的相依性[插入（DI）](xref:fundamentals/dependency-injection)服務。 在 Blazor 伺服器應用程式中，會藉由呼叫 <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor*>來新增服務，並將 `WeatherForecastService` 新增至服務容器，以供範例 `FetchData` 元件使用。
  * `Configure` &ndash; 設定應用程式的要求處理管線：
    * 呼叫 <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*> 來設定端點，以與瀏覽器進行即時連接。 連接是使用[SignalR](xref:signalr/introduction)建立的，這是將即時 web 功能新增至應用程式的架構。
    * 呼叫[MapFallbackToPage （"/_Host"）](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*)來設定應用程式的根頁面（*Pages/_Host. cshtml*）並啟用導覽。

* *wwwroot/index.html* （Blazor WebAssembly） &ndash; 實作為 html 網頁之應用程式的根頁面：
  * 一開始要求應用程式的任何頁面時，會轉譯此頁面並在回應中傳回。
  * 此頁面會指定呈現根 `App` 元件的位置。 `App` 元件（*app.config*）會指定為 `Startup.Configure`中 `AddComponent` 方法的 `app` DOM 元素。
  * 載入 `_framework/blazor.webassembly.js` JavaScript 檔案，其：
    * 下載 .NET 執行時間、應用程式和應用程式的相依性。
    * 初始化執行時間以執行應用程式。

* *App.config* &ndash; 應用程式的根元件，其會使用 <xref:Microsoft.AspNetCore.Components.Routing.Router> 元件來設定用戶端路由。 `Router` 元件會攔截瀏覽器導覽，並呈現符合所要求位址的頁面。

* *Pages*資料夾 &ndash; 包含組成 Blazor 應用程式的可路由元件/頁面（*razor*），以及 Blazor 伺服器應用程式的根 razor 頁面。 每個頁面的路由都是使用[`@page`](xref:mvc/views/razor#page)指示詞來指定。 此範本包含下列各項：
  * *_Host. cshtml* （Blazor 伺服器） &ndash; 實作為 Razor 頁面的應用程式的根頁面：
    * 一開始要求應用程式的任何頁面時，會轉譯此頁面並在回應中傳回。
    * 載入 `_framework/blazor.server.js` JavaScript 檔案，這會在瀏覽器與伺服器之間設定即時 SignalR 連接。
    * [主機] 頁面會指定要呈現根 `App` 元件（*razor*）的位置。
  * `Counter` （*razor*） &ndash; 會實行 [計數器] 頁面。
  * `Error` （只有在應用程式中發生未處理的例外狀況時，才會轉譯） &ndash; 轉譯的*錯誤（razor*、Blazor 伺服器應用程式）。
  * `FetchData` （*FetchData razor*） &ndash; 會執行提取資料頁面。
  * `Index` （*Index. razor*） &ndash; 會執行首頁。

* *共用*資料夾 &ndash; 包含應用程式所使用的其他 UI 元件（*razor*）：
  * `MainLayout` （*MainLayout razor*） &ndash; 應用程式的版面配置元件。
  * `NavMenu` （*navmenu.cshtml razor*） &ndash; 會執行提要欄位導覽。 包含[NavLink 元件](xref:blazor/routing#navlink-component)（<xref:Microsoft.AspNetCore.Components.Routing.NavLink>），它會呈現其他 Razor 元件的導覽連結。 [`NavLink`] 元件會在載入元件時自動指出選取的狀態，協助使用者瞭解目前顯示的元件。

* *_Imports razor* &ndash; 包含常見的 razor 指示詞，以包含在應用程式的元件（*razor*）中，例如命名空間的[`@using`](xref:mvc/views/razor#using)指示詞。

* *資料*資料夾（Blazor Server） &ndash; 包含 `WeatherForecastService` 的 `WeatherForecast` 類別和執行，可提供範例天氣資料給應用程式的 `FetchData` 元件。

* *wwwroot* &ndash; 應用程式的[Web 根](xref:fundamentals/index#web-root)資料夾，其中包含應用程式的公用靜態資產。

* 應用程式的*appsettings* （Blazor Server） &ndash; 設定。
