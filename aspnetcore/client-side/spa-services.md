---
title: 使用 JavaScript 服務在 ASP.NET核心中建立單頁應用程式
author: scottaddie
description: 瞭解使用 JavaScript 服務建立由 ASP.NET Core 支援的單頁應用程式 (SPA) 的好處。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 09/06/2019
uid: client-side/spa-services
ms.openlocfilehash: c0c73882afd579510ad9cdf5b485c1d6fbeadd1c
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78663776"
---
# <a name="use-javascript-services-to-create-single-page-applications-in-aspnet-core"></a>使用 JavaScript 服務在 ASP.NET核心中建立單頁應用程式

由[斯科特·阿迪](https://github.com/scottaddie)和[菲亞茲·哈桑](https://fiyazhasan.me/)

單頁應用程式 (SPA) 是一種流行的 Web 應用程式類型,由於其固有的豐富用戶體驗。 將用戶端 SPA 框架或庫(如["角"](https://angular.io/)或[「React」)](https://facebook.github.io/react/)與伺服器端框架(如 ASP.NET Core)整合可能很困難。 JavaScript 服務是為減少整合過程中的摩擦而開發的。 它支援不同用戶端和伺服器技術堆疊之間的無縫操作。

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> 本文中描述的功能從 ASP.NET酷 3.0 起已過時。 [微軟](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices.Extensions)提供了更簡單的SPA框架集成機制。 有關詳細資訊,請參閱[[公告] 將微軟(AspNetCore.SpaServices)和微軟(Microsoft.AspNetCore.NodeServices)淘汰](https://github.com/dotnet/AspNetCore/issues/12890)。

::: moniker-end

## <a name="what-is-javascript-services"></a>什麼是 JavaScript 服務

JavaScript 服務是ASP.NET核心的用戶端技術的集合。 其目標是將ASP.NET核心定位為開發人員首選的構建SPA的伺服器端平臺。

JavaScript 服務由兩個不同的 NuGet 套件組成:

* [微軟.AspNetCore.節點服務](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/)(節點服務)
* [微軟.AspNetCore.Spa服務](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/)(Spa服務)

這些包在以下方案中非常有用:

* 在伺服器上執行 JavaScript
* 使用 SPA 架構或庫
* 使用 Webpack 建構客戶端資產

本文的大部分重點是使用 SpaServices 包。

## <a name="what-is-spaservices"></a>什麼是水療服務

創建 SpaServices 是為了將 ASP.NET Core 定位為開發人員首選的建構 SpaA 的伺服器端平臺。 SpaServices 不需要使用 ASP.NET 酷睿開發 SpaA,也不會將開發人員鎖定到特定的用戶端框架中。

SpaServices 提供有用的基礎結構,例如:

* [伺服器端預像](#server-side-prerendering)
* [Webpack 開發人員的中間件](#webpack-dev-middleware)
* [熱模組更換](#hot-module-replacement)
* [路由說明器](#routing-helpers)

總之,這些基礎結構元件既增強了開發工作流,又增強了運行時體驗。 元件可以單獨採用。

## <a name="prerequisites-for-using-spaservices"></a>使用 Spa 服務的先決條件

要使用 SpaServices,請安裝以下內容:

* [Node.js(](https://nodejs.org/)版本 6 或更高版本),帶 npm

  * 要驗證這些元件已安裝並可以找到,請從命令列執行以下內容:

    ```console
    node -v && npm -v
    ```

  * 如果部署到 Azure 網站,&mdash;則不需要安裝 Node.js 並在伺服器環境中可用。

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * 在使用 Visual Studio 2017 的 Windows 上,通過選擇 **.NET Core 跨平台開發**工作負載來安裝 SDK。

* [微軟.AspNetCore.Spa服務](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/)NuGet包

## <a name="server-side-prerendering"></a>伺服器端預像

通用(也稱為同構)應用程式是能夠在伺服器和用戶端上運行的 JavaScript 應用程式。 角、反應和其他流行的框架為此應用程式開發風格提供了一個通用平臺。 其理念是首先通過 Node.js 呈現伺服器上的框架元件,然後將進一步執行委託給用戶端。

ASP.NET SpaServices 提供的核心[標記説明器](xref:mvc/views/tag-helpers/intro)通過在伺服器上調用 JavaScript 函數來簡化伺服器端預呈現的實現。

### <a name="server-side-prerendering-prerequisites"></a>伺服器端預先呈現先決條件

安裝[阿斯普奈特預成成](https://www.npmjs.com/package/aspnet-prerendering)npm 套件:

```console
npm i -S aspnet-prerendering
```

### <a name="server-side-prerendering-configuration"></a>伺服器端預先渲染設定

標記說明器可透過項目的 *_ViewImports.cshtml*檔案中的命名空間註冊進行發現:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

這些標記說明器利用 Razor 檢視中類似 HTML 的語法,抽象出與低級 API 直接通訊的複雜性:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="asp-prerender-module-tag-helper"></a>asp 預先成模組標記說明程式

在`asp-prerender-module`上述代碼示例中使用的標記説明程式透過 Node.js 在伺服器上執行*客戶端應用/dist/main-server.js。* 為了清楚起見,*主伺服器.js*檔是[Webpack](https://webpack.github.io/)生成過程中 TypeScript 到 JavaScript 溢出任務的專案。 Webpack`main-server`定義的入口點別名 。並且,此別名的依賴項圖遍曆從*用戶端應用/引導伺服器.ts*檔開始:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

在下面的 Angular 範例中 *,ClientApp/boot-server.ts*檔案利用`createServerRenderer``RenderResult``aspnet-prerendering`npm 包的功能和類型透過 Node.js 設定伺服器呈現。 用於伺服器端呈現的 HTML 標記將傳遞到解析函數調用,該函數調用套件在強類型 JavaScript`Promise`物件中。 對`Promise`象的意義是,它非同步地將 HTML 標記提供到頁以在 DOM 的占位符元素中注入。

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="asp-prerender-data-tag-helper"></a>asp 預先成資料標記說明程式

與`asp-prerender-module`標籤幫助程式結合時,`asp-prerender-data`標記説明程式可用於將上下文資訊從 Razor 視圖傳遞到伺服器端 JavaScript。 例如,以下標記將使用者資料傳遞給`main-server`模組:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

接收`UserName`的參數使用內置的 JSON 序列化器進行序列化,並存儲`params.data`在物件 中。 在下面的 Angular 範例中,資料`h1`用於在 元素中建構個人化問候語:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

標記説明器中傳遞的屬性名稱用**PascalCase**表示法表示。 對比 JAVAScript,其中相同的屬性名稱用**camelCase**表示。 默認 JSON 序列化配置負責此差異。

要在上述代碼示例中展開,可以通過對提供`globals``resolve`給 函數的屬性進行水化,將數據從伺服器傳遞到視圖:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

在`postList``globals`物件內部定義的陣列附加到瀏覽器的全域`window`物件。 此變數提升到全域範圍可消除重複工作,特別是因為它涉及在伺服器上再次在用戶端上載入相同的數據。

![附至視窗物件的全域後列表變數](spa-services/_static/global_variable.png)

## <a name="webpack-dev-middleware"></a>Webpack 開發人員的中間件

[Webpack 開發人員中間件](https://webpack.js.org/guides/development/#using-webpack-dev-middleware)引入了簡化的開發工作流,Webpack 可按需構建資源。 當頁面在瀏覽器中重新載入時,中間件會自動編譯和提供客戶端資源。 另一種方法是在第三方依賴項或自定義代碼更改時,通過專案的 npm 生成腳本手動調用 Webpack。 *在包.json*檔中的 npm 產生文稿顯示在以下範例中:

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="webpack-dev-middleware-prerequisites"></a>Webpack 開發人員的元件先決條件

安裝[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm 套件:

```console
npm i -D aspnet-webpack
```

### <a name="webpack-dev-middleware-configuration"></a>Webpack 開發人員的元件設定

Webpack 開發人員的額外*Startup.cs*Startup.cs`Configure`檔案方法中的以下代碼註冊到 HTTP 要求導管中:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

在`UseWebpackDevMiddleware`從擴充方法[註冊靜態檔託管](xref:fundamentals/static-files)`UseStaticFiles`之前, 必須調用擴充方法。 出於安全原因,僅當應用在開發模式下運行時才註冊中間件。

*Webpack.config.js*`output.publicPath`檔案 的屬性告訴中間`dist`件注意 資料夾的更改:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

## <a name="hot-module-replacement"></a>熱模組更換

將 Webpack[的熱模組取代](https://webpack.js.org/concepts/hot-module-replacement/)(HMR) 功能視為[Webpack 開發人員中間件](#webpack-dev-middleware)的演變。 HMR 引入了所有相同的優勢,但它在編譯更改后自動更新頁面內容,從而進一步簡化了開發工作流。 不要混淆這與瀏覽器的更新,這將干擾 SPA 的當前記憶體狀態和調試會話。 Webpack 開發人員中間件服務和瀏覽器之間有一個即時連結,這意味著更改被推送到瀏覽器。

### <a name="hot-module-replacement-prerequisites"></a>熱模組更換先決條件

安裝[Webpack 熱中間件](https://www.npmjs.com/package/webpack-hot-middleware)npm 套件:

```console
npm i -D webpack-hot-middleware
```

### <a name="hot-module-replacement-configuration"></a>熱模組更換配置

HMR 元件必須註冊到`Configure`MVC 的 HTTP 請求導管中:

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

與[Webpack 開發人員中間件一](#webpack-dev-middleware)樣,`UseWebpackDevMiddleware``UseStaticFiles`擴充方法必須在擴展方法之前調用。 出於安全原因,僅當應用在開發模式下運行時才註冊中間件。

*Webpack.config.js*檔必須定義陣`plugins`列 ,即使它留空:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

在瀏覽器中載入應用後,開發人員工具的主控台選項卡提供 HMR 啟動的確認:

![熱模組更換連接的消息](spa-services/_static/hmr_connected.png)

## <a name="routing-helpers"></a>路由說明器

在大多數基於核心的ASP.NET SA 中,除了伺服器端路由之外,通常還需要用戶端路由。 SPA 和 MVC 路由系統可以獨立工作,不受干擾。 但是,有一個邊緣案例帶來了挑戰:識別 404 個 HTTP 回應。

請考慮使用的無`/some/page`擴展路由的情況。 假設請求與伺服器端路由不模式匹配,但其模式確實與用戶端路由匹配。 現在考慮一個傳入的請求`/images/user-512.png`,它通常期望在伺服器上找到圖像檔。 如果請求的資源路徑與任何伺服器端路由或靜態檔不匹配,則用戶端應用程式不太可能處理它&mdash;,通常需要返回 404 HTTP 狀態代碼。

### <a name="routing-helpers-prerequisites"></a>路由說明器先決條件

安裝用戶端路由 npm 包。 以「角」為例:

```console
npm i -S @angular/router
```

### <a name="routing-helpers-configuration"></a>路由說明器設定

方法中使用了名為`MapSpaFallbackRoute`的`Configure`擴充方法:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

路由按其配置順序計算。 因此,`default`前面的代碼示例中的路由首先用於模式匹配。

## <a name="create-a-new-project"></a>建立新專案

JavaScript 服務提供預先配置的應用程式範本。 SpaServices 與不同的框架和庫(如角、回應和雷杜)結合使用。

可以通過 .NET Core CLI 安裝這些範本,透過執行以下命令:

```dotnetcli
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

將顯示可用 SPA 樣本的清單:

| 範本                                 | 簡短名稱 | Language | Tags        |
| ------------------------------------------| :--------: | :------: | :---------: |
| MVC ASP.NET帶角的內核             | angular    | [C#]     | Web/MVC/SPA |
| MVC ASP.NET核心與 React.js            | react      | [C#]     | Web/MVC/SPA |
| MVC ASP.NET核心與 React.js 和 Redux  | reactredux | [C#]     | Web/MVC/SPA |

要使用 SPA 樣本的一建立新項目,請在[dotnet 新](/dotnet/core/tools/dotnet-new)指令中包括樣本**的短名稱**。 以下指令建立角度應用程式,該應用程式為伺服器端設定了ASP.NET核心 MVC:

```dotnetcli
dotnet new angular
```

### <a name="set-the-runtime-configuration-mode"></a>設定執行時設定模式

存在兩種主要執行時設定模式:

* **發展**:
  * 包括源映射,以方便調試。
  * 無法優化用戶端代碼的性能。
* **生產**:
  * 排除源映射。
  * 通過捆綁和最小化優化客戶端代碼。

ASP.NET Core 使用名為`ASPNETCORE_ENVIRONMENT`的環境 變數來儲存配置模式。 有關詳細資訊,請參閱[設定環境](xref:fundamentals/environments#set-the-environment)。

### <a name="run-with-net-core-cli"></a>使用 .NET 核心 CLI 執行

以專案根處執行以下指令來回復的 NuGet 和 npm 套件:

```dotnetcli
dotnet restore && npm i
```

產生並執行應用程式:

```dotnetcli
dotnet run
```

應用程式根據[運行時配置模式](#set-the-runtime-configuration-mode)在本地主機上啟動。 導航到`http://localhost:5000`瀏覽器將顯示著陸頁。

### <a name="run-with-visual-studio-2017"></a>與視覺工作室運行 2017

打開[dotnet 新](/dotnet/core/tools/dotnet-new)指令產生的 *.csproj*檔。 項目打開後,將自動還原所需的 NuGet 和 npm 包。 此還原過程可能需要幾分鐘時間,應用程式可在完成後運行。 按下綠色運行按鈕或按`Ctrl + F5`,瀏覽器將打開到應用程式的著陸頁。 應用程式根據[運行時配置模式](#set-the-runtime-configuration-mode)在本地主機上運行。

## <a name="test-the-app"></a>測試應用程式

SpaServices 範本已預先配置為使用[Karma](https://karma-runner.github.io/1.0/index.html)和[茉莉花](https://jasmine.github.io/)運行客戶端測試。 茉莉花是JAVAScript的熱門單元測試框架,而Karma是這些測試的測試運行者。 Karma 設定為使用[Webpack 開發人員中間件](#webpack-dev-middleware),這樣開發人員不需要在每次進行更改時停止並運行測試。 無論是針對測試用例運行的代碼還是測試用例本身,測試都會自動運行。

以 Angular 應用程式為例,已在*計數器.元件.spec.ts*`CounterComponent`檔中為 提供兩個 Jasmine 測試用例:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

在*用戶端應用*目錄中打開命令提示符。 執行以下命令：

```console
npm test
```

該文本啟動 Karma 測試運行程式,該測試串流程式讀取*karma.conf.js*檔中定義的設定。 在其他設定中 *,karma.conf.js*識別要`files`透過新群組執行的測試檔:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

## <a name="publish-the-app"></a>發佈應用程式

有關發佈到 Azure 的詳細資訊,請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/12474)。

將生成的用戶端資產和已發佈的ASP.NET Core 專案組合到即部署的包中可能很麻煩。 謝天謝地,SpaServices 使用名為`RunWebpack`的 自定義 MSBuild 目標協調整個發佈過程:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

MSBuild 目標具有以下職責:

1. 還原 npm 包。
1. 創建第三方客戶端資產的生產級構建。
1. 創建自定義客戶端資產的生產級生成。
1. 將 Webpack 產生的資源複製到發佈資料夾。

執行時將呼叫 MSBuild 目標:

```dotnetcli
dotnet publish -c Release
```

## <a name="additional-resources"></a>其他資源

* [角文件](https://angular.io/docs)
