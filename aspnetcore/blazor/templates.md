---
title: ASP.NET Core Blazor 範本
author: guardrex
description: 深入瞭解 ASP.NET Core Blazor 應用程式範本和 Blazor 專案結構。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/25/2019
no-loc:
- Blazor
- SignalR
uid: blazor/templates
ms.openlocfilehash: e82f28afdac8517f72538094d97f28bdcfe46102
ms.sourcegitcommit: 918d7000b48a2892750264b852bad9e96a1165a7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2019
ms.locfileid: "74551582"
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

* *Program.cs* &ndash; 設定 ASP.NET Core[主機](xref:fundamentals/host/generic-host)的應用程式進入點。 此檔案中的程式碼通用於從 ASP.NET Core 範本產生的所有 ASP.NET Core 應用程式。

* *Startup.cs* &ndash; 包含應用程式的啟動邏輯。 `Startup` 類別會定義兩個方法：

  * `ConfigureServices` &ndash; 會設定應用程式的相依性[插入（DI）](xref:fundamentals/dependency-injection)服務。 在 Blazor 伺服器應用程式中，會藉由呼叫 <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor*>來新增服務，並將 `WeatherForecastService` 新增至服務容器，以供範例 `FetchData` 元件使用。
  * `Configure` &ndash; 設定應用程式的要求處理管線：
    * Blazor WebAssembly &ndash; 會將 `App` 元件（指定為 `app` DOM 元素）新增至 `AddComponent` 方法），也就是應用程式的根元件。
    * Blazor 伺服器
      * 呼叫 <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*> 來設定端點，以與瀏覽器進行即時連接。 連接是使用[SignalR](xref:signalr/introduction)建立的，這是將即時 web 功能新增至應用程式的架構。
      * 呼叫[MapFallbackToPage （"/_Host"）](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*)來設定應用程式的根頁面（*Pages/_Host. cshtml*）並啟用導覽。

* *wwwroot/index.html* （Blazor WebAssembly） &ndash; 實作為 html 網頁之應用程式的根頁面：
  * 一開始要求應用程式的任何頁面時，會轉譯此頁面並在回應中傳回。
  * 此頁面會指定呈現根 `App` 元件的位置。 `App` 元件（*app.config*）會指定為 `Startup.Configure`中 `AddComponent` 方法的 `app` DOM 元素。
  * *_Framework/Blazor.webassembly.js* JavaScript 檔案已載入，其：
    * 下載 .NET 執行時間、應用程式和應用程式的相依性。
    * 初始化執行時間以執行應用程式。

* *Pages/_Host. cshtml* （Blazor Server） &ndash; 實作為 Razor 頁面的應用程式的根頁面：
  * 一開始要求應用程式的任何頁面時，會轉譯此頁面並在回應中傳回。
  * *_Framework/Blazor.server.js* JavaScript 檔案已載入，這會在瀏覽器與伺服器之間設定即時 SignalR 連線。
  * [主機] 頁面會指定要呈現根 `App` 元件（*razor*）的位置。

* *App.config* &ndash; 應用程式的根元件，其會使用 <xref:Microsoft.AspNetCore.Components.Routing.Router> 元件來設定用戶端路由。 `Router` 元件會攔截瀏覽器導覽，並呈現符合所要求位址的頁面。

* *Pages*資料夾 &ndash; 包含組成 Blazor 應用程式的可路由元件/頁面（*razor*）。 每個頁面的路由都是使用[@page](xref:mvc/views/razor#page)指示詞來指定。 此範本包含下列元件：
  * `Index` （*Index. razor*） &ndash; 會執行首頁。
  * `Counter` （*razor*） &ndash; 會實行 [計數器] 頁面。
  * `Error` （只有在應用程式中發生未處理的例外狀況時，才會轉譯） &ndash; 轉譯的*錯誤（razor*、Blazor 伺服器應用程式）。
  * `FetchData` （*FetchData razor*） &ndash; 會執行提取資料頁面。

* *共用*資料夾 &ndash; 包含應用程式所使用的其他 UI 元件（*razor*）：
  * `MainLayout` （*MainLayout razor*） &ndash; 應用程式的版面配置元件。
  * `NavMenu` （*navmenu.cshtml razor*） &ndash; 會執行提要欄位導覽。 包含[NavLink 元件](xref:blazor/routing#navlink-component)（<xref:Microsoft.AspNetCore.Components.Routing.NavLink>），它會呈現其他 Razor 元件的導覽連結。 [`NavLink`] 元件會在載入元件時自動指出選取的狀態，協助使用者瞭解目前顯示的元件。

* *_Imports razor* &ndash; 包含常見的 razor 指示詞，以包含在應用程式的元件（*razor*）中，例如命名空間的[@using](xref:mvc/views/razor#using)指示詞。

* *資料*資料夾（Blazor Server） &ndash; 包含 `WeatherForecastService` 的 `WeatherForecast` 類別和執行，可提供範例天氣資料給應用程式的 `FetchData` 元件。

* *wwwroot* &ndash; 應用程式的[Web 根](xref:fundamentals/index#web-root)資料夾，其中包含應用程式的公用靜態資產。

* 應用程式的*appsettings* （Blazor Server） &ndash; 設定。
