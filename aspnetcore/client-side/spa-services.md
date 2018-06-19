---
title: 若要建立單一頁面應用程式中 ASP.NET Core 使用 JavaScriptServices
author: scottaddie
description: 深入了解使用 JavaScriptServices 建立單一頁面應用程式 (SPA) 支援的 ASP.NET Core 的優點。
manager: wpickett
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/spa-services
ms.openlocfilehash: fd893b7c62f38442bf5633a956786983763e6f9f
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483548"
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a>若要建立單一頁面應用程式中 ASP.NET Core 使用 JavaScriptServices

由[Scott Addie](https://github.com/scottaddie)和[Fiyaz Hasan](http://fiyazhasan.me/)

單一頁面應用程式 (SPA) 是熱門的 web 應用程式，因為其本身的豐富使用者經驗類型。 整合用戶端 SPA 架構或程式庫，例如[Angular](https://angular.io/)或[React](https://facebook.github.io/react/)，與伺服器端架構，像 ASP.NET Core 可能相當困難。  [JavaScriptServices](https://github.com/aspnet/JavaScriptServices)特別開發來減少摩擦整合程序中的。 它可讓不同的用戶端和伺服器技術堆疊之間的無縫式作業。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a>什麼是 JavaScriptServices？

JavaScriptServices 是 ASP.NET Core 的用戶端技術的集合。 其目標是要為開發人員的慣用伺服器端平台建置 SPAs 定位 ASP.NET Core。

JavaScriptServices 包含三個相異的 NuGet 封裝：
* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)
* [Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)

這些封裝會很有用如果您：
* 在伺服器上執行的 JavaScript
* 使用 SPA 架構或程式庫
* 建置用戶端 Webpack 資產

在這篇文章的焦點會放在使用 SpaServices 封裝。

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a>什麼是 SpaServices？

SpaServices 建立位置為開發人員的慣用伺服器端平台建置 SPAs ASP.NET Core。 SpaServices 不需要開發與 ASP.NET Core SPAs，它並不會鎖定您的特定用戶端架構。

SpaServices 提供有用的基礎結構，例如：
* [伺服器端尚未](#server-prerendering)
* [Webpack Dev 中介軟體](#webpack-dev-middleware)
* [熱模組更換](#hot-module-replacement)
* [路由的協助程式](#routing-helpers)

整體而言，這些基礎結構元件可強化開發工作流程和執行階段經驗。 元件可以個別採用。

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a>使用 SpaServices 的必要條件

若要使用 SpaServices，安裝下列項目：
* [Node.js](https://nodejs.org/) （版本 6 或更新版本） 與 npm
  * 若要確認已安裝這些元件，並可以找到，從命令列執行下列命令：

    ```console
    node -v && npm -v
    ```

注意： 如果您要部署至 Azure 的網站，您不需要執行以下任何動作&mdash;Node.js 已安裝並可在伺服器環境中。

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * 如果您使用 Visual Studio 2017 在 Windows 上，選取已安裝的 SDK **.NET Core 跨平台開發**工作負載。

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet 封裝

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a>伺服器端渲染

通用 (也稱為 isomorphic) 應用程式是 JavaScript 應用程式能夠執行同時在伺服器和用戶端上。 Angular、 React 和其他常用架構提供此應用程式的開發樣式通用平台。 做法是先呈現架構上的元件透過 Node.js 伺服器，然後進一步委派給用戶端執行。

ASP.NET Core[標記協助程式](xref:mvc/views/tag-helpers/intro)提供 SpaServices 簡化的伺服器端尚未實作叫用伺服器上的 JavaScript 函式。

### <a name="prerequisites"></a>必要條件

安裝下列項目：
* [aspnet 尚未](https://www.npmjs.com/package/aspnet-prerendering)npm 封裝：

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a>組態

標記協助程式會在專案的命名空間註冊透過 *_ViewImports.cshtml*檔案：

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

這些標記協助程式立即抽象複雜的通訊直接與低階 Api 運用在 Razor 檢視內部類似 HTML 的語法：

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a>`asp-prerender-module`標記協助程式

`asp-prerender-module`標記協助程式，使用在上述程式碼範例中，執行*ClientApp/dist/main-server.js*透過 Node.js 伺服器上。 為了清楚起見， *main 立即轉譯 server.js*檔案是 TypeScript-JavaScript transpilation 工作中的成品[Webpack](http://webpack.github.io/)建置程序。 Webpack 定義的項目點別名`main-server`; 和此別名的相依性圖表的周遊開始*ClientApp/開機-server.ts*檔案：

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

在下列的 Angular 範例中， *ClientApp/開機-server.ts*檔案會利用`createServerRenderer`函式和`RenderResult`類型`aspnet-prerendering`設定伺服器轉譯透過 Node.js 的 npm 封裝。 預定要給伺服器端轉譯會傳遞至解析函式呼叫，包裝強型別在 JavaScript 中的 HTML 標記`Promise`物件。 `Promise`物件的重要性是會以非同步方式提供要在 DOM 的預留位置項目插入頁面的 HTML 標記。

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a>`asp-prerender-data`標記協助程式

如果結合`asp-prerender-module`標記 Helper`asp-prerender-data`標記協助程式可以用來從 Razor 檢視的內容資訊傳遞至伺服器端 JavaScript。 例如，下列標記使用者會將資料傳遞至`main-server`模組：

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

接收`UserName`引數會使用內建的 JSON 序列化程式序列化並儲存在`params.data`物件。 在下列的角度範例中，資料用來建構內的個人化的問候語`h1`項目：

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

注意： 使用表示屬性名稱中標記協助程式傳遞**PascalCase**標記法。 Javascript 中，會使用以表示相同的屬性名稱相**camelCase**。 預設的 JSON 序列化組態會負責這項差異。

若要展開在上述程式碼範例時，資料可傳遞從伺服器到檢視的 hydrating`globals`屬性提供給`resolve`函式：

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

`postList`陣列內定義`globals`物件附加至瀏覽器的全域`window`物件。 此變數 hoisting 全域範圍排除重複的工作，尤其是它適用於載入相同的資料一次在伺服器上一次用戶端上。

![附加至視窗物件的全域 postList 變數](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a>Webpack Dev 中介軟體

[中介軟體開發人員 」 Webpack](https://webpack.github.io/docs/webpack-dev-middleware.html)引入 Webpack 隨組建資源的藉此簡化的開發工作流程。 中介軟體會自動編譯和瀏覽器中重新載入頁面時，提供用戶端的資源。 替代方法是手動透過叫用 Webpack 專案的 npm 組建指令碼的第三方相依性或自訂程式碼變更時。 Npm 建置指令碼*package.json*檔案在下列範例所示：

[!code-json[](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a>必要條件

安裝下列項目：
* [aspnet webpack](https://www.npmjs.com/package/aspnet-webpack) npm 封裝：

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a>組態

Webpack Dev 中介軟體已註冊至 HTTP 要求管線中的下列程式碼透過*Startup.cs*檔案的`Configure`方法：

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

`UseWebpackDevMiddleware`之前必須先呼叫擴充方法[註冊靜態檔案裝載](xref:fundamentals/static-files)透過`UseStaticFiles`擴充方法。 基於安全性理由，在開發模式中執行的應用程式時，才登錄中介軟體。

*Webpack.config.js*檔案的`output.publicPath`屬性會告知監看的中介軟體`dist`資料夾是否有變更：

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a>熱模組更換

想像 Webpack 的[熱模組更換](https://webpack.js.org/concepts/hot-module-replacement/)(HMR) 功能的進化[Webpack Dev 中介軟體](#webpack-dev-middleware)。 HMR 完全相同的優點，但它進一步簡化開發工作流程自動編譯所做的變更之後更新頁面內容。 請勿混淆這個與重新整理瀏覽器中，這會干擾 SPA 的偵錯工作階段與目前記憶體中狀態。 沒有 Webpack Dev 中介軟體服務與瀏覽器中，這表示變更推送至瀏覽器之間的即時連結。

### <a name="prerequisites"></a>必要條件

安裝下列項目：
* [webpack 熱的中介軟體](https://www.npmjs.com/package/webpack-hot-middleware)npm 封裝：

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a>組態

HMR 元件必須註冊到 MVC 的 HTTP 要求管線中`Configure`方法：

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

為已具有 true [Webpack Dev 中介軟體](#webpack-dev-middleware)、`UseWebpackDevMiddleware`之前必須先呼叫擴充方法`UseStaticFiles`擴充方法。 基於安全性理由，在開發模式中執行的應用程式時，才登錄中介軟體。

*Webpack.config.js*檔案必須定義`plugins`陣列，即使它空白：

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

載入瀏覽器中的應用程式之後, 的開發人員工具主控台 索引標籤會提供確認 HMR 啟用：

![熱模組更換連接的訊息](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a>路由的協助程式

在大部分的 ASP.NET 核心 SPAs，您需要路由除了路由伺服器端的用戶端。 SPA 和 MVC 路由系統可以不受干擾的獨立工作。 不過，一個邊緣案例一起集思廣益面臨的挑戰，是： 識別 404 HTTP 回應。

請考慮案例，其中無副檔名的路由`/some/page`用。 假設要求不會對伺服器端路由，但它的模式比對用戶端路由。 現在請思考一下的連入要求`/images/user-512.png`，這通常會預期找到映像檔在伺服器上的。 如果該要求的資源路徑不符合任何伺服器端路由或靜態檔案，也不太可能用戶端應用程式會處理它，您通常想要傳回 404 HTTP 狀態碼。

### <a name="prerequisites"></a>必要條件

安裝下列項目：
* 用戶端路由 npm 封裝。 使用 Angular 做為範例：

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a>組態

擴充方法，名為`MapSpaFallbackRoute`用於`Configure`方法：

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

提示： 路由設定的順序會進行評估。 因此，`default`用於模式比對第一次使用上述的程式碼範例中的路由。

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a>建立新的專案

JavaScriptServices 提供預先設定的應用程式範本。 在這些範本，搭配不同的架構和程式庫，例如 Angular、 React 和 Redux 用於 SpaServices。

這些範本可以透過.NET Core CLI 安裝，執行下列命令：

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

會顯示可用 SPA 範本的清單：

| 範本                                 | 簡短名稱 | 語言 | Tags        |
|:------------------------------------------|:-----------|:---------|:------------|
| MVC ASP.NET Core 與角度             | angular    | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core 與 React.js            | react      | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core React.js 和 Redux  | reactredux | [C#]     | Web/MVC/SPA |

若要建立新專案使用其中一個 SPA 範本時，包含**簡短名稱**中的範本[dotnet 新](/dotnet/core/tools/dotnet-new)命令。 下列命令會建立與伺服器端設定的 ASP.NET Core MVC 角度的應用程式：

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a>設定執行階段組態模式

有兩種主要的執行階段組態模式：
* **開發**:
    * 包含來源對應，以便偵錯。
    * 不最佳化效能的用戶端程式碼。
* **生產**:
    * 排除來源對應。
    * 最佳化透過組合和縮製的用戶端程式碼。

ASP.NET Core 使用名為環境變數`ASPNETCORE_ENVIRONMENT`來儲存組態模式。 請參閱**[設定環境](xref:fundamentals/environments#setting-the-environment)** 如需詳細資訊。

### <a name="running-with-net-core-cli"></a>執行.NET Core CLI

還原所需的 NuGet 及 npm 封裝在專案的根目錄執行下列命令：

```console
dotnet restore && npm i
```

建置並執行應用程式：

```console
dotnet run
```

根據本機主機上的應用程式啟動[執行階段組態模式](#runtime-config-mode)。 瀏覽至`http://localhost:5000`瀏覽器中顯示 登陸頁面。

### <a name="running-with-visual-studio-2017"></a>使用 Visual Studio 2017 執行

開啟 *.csproj*所產生的檔案[dotnet 新](/dotnet/core/tools/dotnet-new)命令。 在專案開啟時自動還原必要的 NuGet 及 npm 封裝。 此還原程序可能需要幾分鐘的時間，和應用程式已準備好完成時執行。 按一下綠色執行的按鈕或按`Ctrl + F5`，和瀏覽器會開啟應用程式的登陸頁面。 根據本機主機上執行的應用程式[執行階段組態模式](#runtime-config-mode)。 

<a name="app-testing"></a>

## <a name="testing-the-app"></a>測試應用程式

SpaServices 範本是預先設定為執行用戶端測試使用[Karma](https://karma-runner.github.io/1.0/index.html)和[Jasmine](https://jasmine.github.io/)。 Jasmine 是熱門單元測試架構適用 JavaScript 的而 Karma 是這些測試的測試執行器。 Karma 設為搭配[Webpack Dev 中介軟體](#webpack-dev-middleware)，開發人員不需要停止並執行測試，每次進行變更。 是否為測試案例或測試案例本身上執行的程式碼，會自動執行測試。

使用 Angular 的應用程式，例如，兩個 Jasmine 測試案例已提供`CounterComponent`中*counter.component.spec.ts*檔案：

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

開啟命令提示字元中*ClientApp*目錄。 執行下列命令：

```console
npm test
```

指令碼會啟動 Karma 測試執行器，讀取中定義的設定*karma.conf.js*檔案。 在其他設定， *karma.conf.js*識別測試檔案執行透過其`files`陣列：

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a>發行應用程式

將產生的用戶端資產和已發行的 ASP.NET Core 成品結合成已備妥部署的封裝可能會相當繁雜。 幸好 SpaServices 協調整個發行集程序與自訂的 MSBuild 目標，名為`RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

MSBuild 目標具有下列責任：
1. 還原 npm 封裝
1. 建立生產等級的組建的協力廠商的用戶端資產
1. 建立自訂用戶端資產的實際執行等級的組建
1. 複製到發佈資料夾的 Webpack 產生的資產

執行時，會叫用 MSBuild 目標：

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>其他資源

* [角度的文件](https://angular.io/docs)
