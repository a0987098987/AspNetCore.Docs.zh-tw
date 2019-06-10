---
title: 使用 JavaScript 服務來建立 ASP.NET Core 中的單一頁面應用程式
author: scottaddie
description: 深入了解使用 JavaScript 的服務建立單一頁面應用程式 (SPA) ASP.NET Core 所支援的權益。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 05/28/2019
uid: client-side/spa-services
ms.openlocfilehash: c7cd35865c5bddf0e5efaa9e616832b6755d9227
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750125"
---
# <a name="use-javascript-services-to-create-single-page-applications-in-aspnet-core"></a>使用 JavaScript 服務來建立 ASP.NET Core 中的單一頁面應用程式

藉由[Scott Addie](https://github.com/scottaddie)和[Fiyaz Hasan](http://fiyazhasan.me/)

單一頁面應用程式 (SPA) 是熱門的 web 應用程式，因為其本身的豐富使用者經驗類型。 整合用戶端 SPA 架構或程式庫，例如[Angular](https://angular.io/)或是[React](https://facebook.github.io/react/)，伺服器端架構，例如 ASP.NET Core 可能很困難。 若要減少摩擦整合程序中的開發 JavaScript 服務。 它可讓不同的用戶端和伺服器技術堆疊之間的無縫式作業。

## <a name="what-is-javascript-services"></a>什麼是 JavaScript 服務

JavaScript 服務是 ASP.NET Core 的用戶端技術的集合。 其目標是要做為開發人員的慣用伺服器端平台建置 Spa 置於 ASP.NET Core。

JavaScript 服務是由兩個不同的 NuGet 套件所組成：

* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)

這些封裝可用於下列案例：

* 在伺服器上執行 JavaScript
* 使用 SPA 架構或程式庫
* 建置用戶端資產 Webpack

在這篇文章的焦點會放在使用 SpaServices 套件。

## <a name="what-is-spaservices"></a>什麼是 SpaServices

SpaServices 已建立以位置為開發人員的慣用伺服器端的平台建置 Spa 的 ASP.NET Core。 SpaServices 不一定要開發使用 ASP.NET Core 的 Spa，它並不會鎖定至特定用戶端架構的開發人員。

SpaServices 提供有用的基礎結構如下所示：

* [伺服器端預呈現](#server-side-prerendering)
* [Webpack 開發中介軟體](#webpack-dev-middleware)
* [熱門的模組更換](#hot-module-replacement)
* [路由的協助程式](#routing-helpers)

整體而言，這些基礎結構元件增強開發工作流程和執行階段體驗。 元件可以個別地採用。

## <a name="prerequisites-for-using-spaservices"></a>使用 SpaServices 的必要條件

若要使用 SpaServices，安裝下列項目：

* [Node.js](https://nodejs.org/) （版本 6 或更新版本） 與 npm

  * 若要確認已安裝這些元件，而且可以找到，從命令列執行下列命令：

    ```console
    node -v && npm -v
    ```

  * 如果部署至 Azure 網站時，不需要任何動作&mdash;Node.js 已安裝並可在伺服器環境中。

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * 在使用 Visual Studio 2017 的 Windows，選取安裝的 SDK **.NET Core 跨平台開發**工作負載。

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet 套件

## <a name="server-side-prerendering"></a>伺服器端渲染

通用 (也稱為 isomorphic) 應用程式是 JavaScript 應用程式能夠執行同時在伺服器和用戶端上。 Angular、 React 和其他常用架構提供此應用程式的開發樣式通用平台。 做法是先呈現架構上的元件透過 Node.js 伺服器，然後進一步委派給用戶端執行。

ASP.NET Core[標籤協助程式](xref:mvc/views/tag-helpers/intro)提供 SpaServices 簡化伺服器端預呈現的實作，藉由叫用伺服器上的 JavaScript 函式。

### <a name="server-side-prerendering-prerequisites"></a>伺服器端預先呈現的必要條件

安裝[aspnet 預呈現](https://www.npmjs.com/package/aspnet-prerendering)npm 套件：

```console
npm i -S aspnet-prerendering
```

### <a name="server-side-prerendering-configuration"></a>伺服器端預先呈現設定

在專案中，標籤協助程式會變成可探索透過命名空間註冊 *_ViewImports.cshtml*檔案：

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

這些標籤協助程式抽離利用 HTML 的類似語法，在 Razor 檢視內直接使用低層級的 Api 進行通訊的複雜性：

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="asp-prerender-module-tag-helper"></a>asp prerender 模組標籤協助程式

`asp-prerender-module`標籤協助程式，用於上述程式碼範例中，執行*ClientApp/dist/main-server.js*上透過 Node.js 伺服器。 為了清楚起見*主要 server.js*檔案是中的 TypeScript-JavaScript 的轉譯工作的成品[Webpack](http://webpack.github.io/)建置程序。 Webpack 定義的項目點別名`main-server`; 並在開始此別名的相依性圖形周遊*ClientApp/開機-server.ts*檔案：

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

在下列的 Angular 範例中， *ClientApp/開機-server.ts*檔案會利用`createServerRenderer`函式和`RenderResult`類型`aspnet-prerendering`設定伺服器轉譯透過 Node.js 的 npm 封裝。 預定要給伺服器端轉譯會傳遞至解析函式呼叫，包裝在強型別在 JavaScript 中的 HTML 標記`Promise`物件。 `Promise`物件的意義是，它會以非同步方式提供至 DOM 的預留位置項目中的插入頁面的 HTML 標記。

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="asp-prerender-data-tag-helper"></a>asp prerender 資料標籤協助程式

如果結合`asp-prerender-module`標籤協助程式，`asp-prerender-data`標籤協助程式可用來從 Razor 檢視中將伺服器端 JavaScript 的內容資訊。 例如，下列標記使用者會將資料傳遞至`main-server`模組：

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

接收`UserName`引數會使用內建的 JSON 序列化程式序列化並儲存在`params.data`物件。 在下列的 Angular 範例中，資料用來建構內的個人化的問候語`h1`項目：

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

標籤協助程式中傳遞的屬性名稱以表示**PascalCase**標記法。 Javascript 中，相同的屬性名稱以表示的相反**camelCase**。 預設值的 JSON 序列化組態會負責這項差異。

若要展開在前述的程式碼範例時，資料可以傳遞從伺服器至檢視的 hydrating`globals`屬性提供給`resolve`函式：

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

`postList`內定義的陣列`globals`物件附加至瀏覽器的全域`window`物件。 此變數的 hoisting 至全域範圍就不重複的工作，，特別是當它適用於載入相同的資料一次在伺服器上，另一次在用戶端上。

![附加到視窗物件的全域 postList 變數](spa-services/_static/global_variable.png)

## <a name="webpack-dev-middleware"></a>Webpack 開發中介軟體

[Webpack 開發中介軟體](https://webpack.js.org/guides/development/#using-webpack-dev-middleware)導入了簡化的開發工作流程，藉此讓 Webpack 是根據資源需求。 中介軟體會自動編譯並在瀏覽器中重新載入頁面時，提供用戶端的資源。 替代方法是以手動方式時要叫用 Webpack 透過專案的 npm 建置指令碼的第三方相依性或自訂程式碼變更。 Npm 建置指令碼*package.json*檔案在下列範例所示：

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="webpack-dev-middleware-prerequisites"></a>Webpack 開發中介軟體必要條件

安裝[aspnet webpack](https://www.npmjs.com/package/aspnet-webpack) npm 套件：

```console
npm i -D aspnet-webpack
```

### <a name="webpack-dev-middleware-configuration"></a>Webpack 開發中介軟體組態

Webpack 開發中介軟體會註冊到 HTTP 要求管線中的下列程式碼透過*Startup.cs*檔案的`Configure`方法：

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

`UseWebpackDevMiddleware`擴充方法必須先呼叫才能[註冊靜態檔案裝載](xref:fundamentals/static-files)透過`UseStaticFiles`擴充方法。 基於安全性理由，請在開發模式中執行的應用程式時，才註冊中介軟體。

*Webpack.config.js*檔案的`output.publicPath`屬性會告知中介軟體觀看`dist`資料夾變更：

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

## <a name="hot-module-replacement"></a>熱門的模組更換

Webpack 的想像[熱模組替換](https://webpack.js.org/concepts/hot-module-replacement/)視為的進化版 (HMR) 功能[Webpack 開發中介軟體](#webpack-dev-middleware)。 HMR 完全相同帶來的好處，但它藉由自動編譯所做的變更之後更新頁面內容，進一步簡化開發工作流程。 請勿混淆這與重新整理瀏覽器中，這會干擾的 SPA 的偵錯工作階段的目前記憶體中狀態。 沒有 Webpack 開發中介軟體服務與瀏覽器中，這表示變更推送至瀏覽器之間的即時連結。

### <a name="hot-module-replacement-prerequisites"></a>最忙碌的更換模組必要條件

安裝[webpack 經常性存取-中介軟體](https://www.npmjs.com/package/webpack-hot-middleware)npm 套件：

```console
npm i -D webpack-hot-middleware
```

### <a name="hot-module-replacement-configuration"></a>最忙碌的更換模組組態

HMR 元件必須註冊至 MVC 的 HTTP 要求管線`Configure`方法：

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

做為已使用，則為 true [Webpack 開發中介軟體](#webpack-dev-middleware)，則`UseWebpackDevMiddleware`擴充方法必須先呼叫才能`UseStaticFiles`擴充方法。 基於安全性理由，請在開發模式中執行的應用程式時，才註冊中介軟體。

*Webpack.config.js*檔案必須定義`plugins`陣列，即使其為空白：

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

載入瀏覽器中的應用程式之後, 的開發人員工具主控台 索引標籤會提供 HMR 啟用的確認：

![熱門的模組取代連接的訊息](spa-services/_static/hmr_connected.png)

## <a name="routing-helpers"></a>路由的協助程式

在大多數 ASP.NET Core 為基礎的 Spa，用戶端路由是通常需要除了伺服器端路由。 SPA 和 MVC 的路由系統可以獨立運作，不受干擾。 沒有，不過，一個邊緣案例提出的挑戰： 找出 404 HTTP 回應。

請考慮案例，其中的無副檔名的路由`/some/page`用。 假設要求不模式比對伺服器端路由，但其模式符合的用戶端的路由。 現在，假設連入要求`/images/user-512.png`，這通常會預期找到伺服器上的影像檔。 如果該要求的資源路徑不符合任何伺服器端路由或靜態檔案，也不太可能用戶端應用程式會處理它&mdash;想要使用通常傳回 「 404 的 HTTP 狀態碼。

### <a name="routing-helpers-prerequisites"></a>路由的協助程式先決條件

安裝用戶端的路由 npm 套件。 使用 Angular 做為範例：

```console
npm i -S @angular/router
```

### <a name="routing-helpers-configuration"></a>協助程式的路由組態

名為擴充方法`MapSpaFallbackRoute`用於`Configure`方法：

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

路由會評估已設定的順序。 因此，`default`進行模式比對第一次使用上述的程式碼範例中的路由。

## <a name="create-a-new-project"></a>建立新專案

JavaScript 服務會提供預先設定的應用程式範本。 SpaServices 用於搭配不同的架構和程式庫，例如 Angular、 React 和 Redux 這些範本。

這些範本可以透過.NET Core CLI 安裝，執行下列命令：

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

會顯示一份可用的 SPA 範本：

| 範本                                 | 簡短名稱 | 語言 | Tags        |
| ------------------------------------------| :--------: | :------: | :---------: |
| 將 ASP.NET Core MVC 與 Angular             | angular    | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core 與 React.js            | react      | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core 與 React.js 和 Redux  | reactredux | [C#]     | Web/MVC/SPA |

若要建立新專案使用其中一個 SPA 範本時，包含**簡短名稱**中的範本[dotnet 新](/dotnet/core/tools/dotnet-new)命令。 下列命令會使用伺服器端設定的 ASP.NET Core MVC 建立 Angular 應用程式：

```console
dotnet new angular
```

### <a name="set-the-runtime-configuration-mode"></a>設定執行階段組態模式

有兩種主要的執行階段組態模式：

* **開發**:
  * 包含來源對應，以便偵錯。
  * 不最佳化效能的用戶端程式碼。
* **生產**:
  * 排除來源對應。
  * 最佳化透過統合和縮製的用戶端程式碼。

ASP.NET Core 會使用環境變數，名為`ASPNETCORE_ENVIRONMENT`來儲存組態模式。 如需詳細資訊，請參閱 <<c0> [ 將環境設定](xref:fundamentals/environments#set-the-environment)。

### <a name="run-with-net-core-cli"></a>執行使用.NET Core CLI

在專案的根目錄執行下列命令來還原必要的 NuGet 和 npm 套件：

```console
dotnet restore && npm i
```

建置和執行應用程式：

```console
dotnet run
```

根據本機主機上的應用程式啟動[執行階段組態模式](#set-the-runtime-configuration-mode)。 瀏覽至`http://localhost:5000`瀏覽器中顯示的登陸頁面。

### <a name="run-with-visual-studio-2017"></a>執行使用 Visual Studio 2017

開啟 *.csproj*所產生的檔案[dotnet 新](/dotnet/core/tools/dotnet-new)命令。 在專案開啟時，會自動還原必要的 NuGet 和 npm 套件。 此還原程序可能需要幾分鐘的時間，以及應用程式已準備好執行完成時。 按一下綠色 [執行] 按鈕或按下`Ctrl + F5`，而且瀏覽器會開啟到應用程式的登陸頁面。 根據本機主機上執行的應用程式[執行階段組態模式](#set-the-runtime-configuration-mode)。

## <a name="test-the-app"></a>測試應用程式

SpaServices 範本已預先設定為執行用戶端使用的測試[Karma](https://karma-runner.github.io/1.0/index.html)並[Jasmine](https://jasmine.github.io/)。 Jasmine 是熱門的單元測試架構的 JavaScript，而 Karma 是這些測試的測試執行器。 Karma 設定為搭配[Webpack 開發中介軟體](#webpack-dev-middleware)使開發人員不需要停止並執行測試，每次進行變更。 不論是針對測試案例或測試案例本身所執行的程式碼，測試將會自動執行。

使用 Angular 的應用程式，例如，兩個 Jasmine 測試案例已提供`CounterComponent`中*counter.component.spec.ts*檔案：

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

開啟命令提示字元*ClientApp*目錄。 執行下列命令：

```console
npm test
```

指令碼啟動 Karma 測試執行器，其內容中定義的設定*karma.conf.js*檔案。 在其他設定中， *karma.conf.js*識別的測試檔案，以透過執行其`files`陣列：

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

## <a name="publish-the-app"></a>發行應用程式

將產生的用戶端資產和已發行的 ASP.NET Core 成品結合成已準備好部署的封裝可能會相當繁雜。 幸好 SpaServices 協調該整個發行程序名為的自訂 MSBuild 目標與`RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

MSBuild 目標須擔負下列責任：

1. 還原 npm 套件。
1. 建立生產等級的組建的協力廠商的用戶端資產。
1. 建立生產等級的組建的自訂用戶端資產。
1. Webpack 產生資產複製到發佈資料夾。

執行時，會叫用 MSBuild 目標：

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>其他資源

* [Angular Docs](https://angular.io/docs)
