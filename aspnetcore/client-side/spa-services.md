---
title: 使用 JavaScriptServices 單一頁面應用程式建立 ASP.NET Core
author: scottaddie
description: 深入了解使用 JavaScriptServices 建立單一頁面應用程式 (SPA) ASP.NET Core 所支援的權益。
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
uid: client-side/spa-services
ms.openlocfilehash: ee772e67ef14608bcc6e3498ade00424ff6090e5
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64893945"
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="40de5-103">使用 JavaScriptServices 單一頁面應用程式建立 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40de5-103">Use JavaScriptServices to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="40de5-104">藉由[Scott Addie](https://github.com/scottaddie)和[Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="40de5-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="40de5-105">單一頁面應用程式 (SPA) 是熱門的 web 應用程式，因為其本身的豐富使用者經驗類型。</span><span class="sxs-lookup"><span data-stu-id="40de5-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="40de5-106">整合用戶端 SPA 架構或程式庫，例如[Angular](https://angular.io/)或[React](https://facebook.github.io/react/)，與伺服器端架構，像 ASP.NET Core 可能相當困難。 </span><span class="sxs-lookup"><span data-stu-id="40de5-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="40de5-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices)特別開發來減少摩擦整合程序中的。</span><span class="sxs-lookup"><span data-stu-id="40de5-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="40de5-108">它可讓不同的用戶端和伺服器技術堆疊之間的無縫式作業。</span><span class="sxs-lookup"><span data-stu-id="40de5-108">It enables seamless operation between the different client and server technology stacks.</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="40de5-109">什麼是 JavaScriptServices</span><span class="sxs-lookup"><span data-stu-id="40de5-109">What is JavaScriptServices</span></span>

<span data-ttu-id="40de5-110">JavaScriptServices 是 ASP.NET Core 的用戶端技術的集合。</span><span class="sxs-lookup"><span data-stu-id="40de5-110">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="40de5-111">其目標是要做為開發人員的慣用伺服器端平台建置 Spa 置於 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="40de5-111">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="40de5-112">JavaScriptServices 是由三個不同的 NuGet 套件所組成：</span><span class="sxs-lookup"><span data-stu-id="40de5-112">JavaScriptServices consists of three distinct NuGet packages:</span></span>

* <span data-ttu-id="40de5-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="40de5-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="40de5-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="40de5-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="40de5-115">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="40de5-115">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="40de5-116">這些套件裝很有用如果您：</span><span class="sxs-lookup"><span data-stu-id="40de5-116">These packages are useful if you:</span></span>

* <span data-ttu-id="40de5-117">在伺服器上執行 JavaScript</span><span class="sxs-lookup"><span data-stu-id="40de5-117">Run JavaScript on the server</span></span>
* <span data-ttu-id="40de5-118">使用 SPA 架構或程式庫</span><span class="sxs-lookup"><span data-stu-id="40de5-118">Use a SPA framework or library</span></span>
* <span data-ttu-id="40de5-119">建置用戶端資產 Webpack</span><span class="sxs-lookup"><span data-stu-id="40de5-119">Build client-side assets with Webpack</span></span>

<span data-ttu-id="40de5-120">在這篇文章的焦點會放在使用 SpaServices 套件。</span><span class="sxs-lookup"><span data-stu-id="40de5-120">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="40de5-121">什麼是 SpaServices</span><span class="sxs-lookup"><span data-stu-id="40de5-121">What is SpaServices</span></span>

<span data-ttu-id="40de5-122">SpaServices 已建立以位置為開發人員的慣用伺服器端的平台建置 Spa 的 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="40de5-122">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="40de5-123">SpaServices 不一定要開發使用 ASP.NET Core 的 Spa，它並不會鎖定您到特定用戶端架構。</span><span class="sxs-lookup"><span data-stu-id="40de5-123">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="40de5-124">SpaServices 提供有用的基礎結構如下所示：</span><span class="sxs-lookup"><span data-stu-id="40de5-124">SpaServices provides useful infrastructure such as:</span></span>

* [<span data-ttu-id="40de5-125">伺服器端預呈現</span><span class="sxs-lookup"><span data-stu-id="40de5-125">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="40de5-126">Webpack 開發中介軟體</span><span class="sxs-lookup"><span data-stu-id="40de5-126">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="40de5-127">熱門的模組更換</span><span class="sxs-lookup"><span data-stu-id="40de5-127">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="40de5-128">路由的協助程式</span><span class="sxs-lookup"><span data-stu-id="40de5-128">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="40de5-129">整體而言，這些基礎結構元件增強開發工作流程和執行階段體驗。</span><span class="sxs-lookup"><span data-stu-id="40de5-129">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="40de5-130">元件可以個別地採用。</span><span class="sxs-lookup"><span data-stu-id="40de5-130">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="40de5-131">使用 SpaServices 的必要條件</span><span class="sxs-lookup"><span data-stu-id="40de5-131">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="40de5-132">若要使用 SpaServices，安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="40de5-132">To work with SpaServices, install the following:</span></span>

* <span data-ttu-id="40de5-133">[Node.js](https://nodejs.org/) （版本 6 或更新版本） 與 npm</span><span class="sxs-lookup"><span data-stu-id="40de5-133">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
  * <span data-ttu-id="40de5-134">若要確認已安裝這些元件，而且可以找到，從命令列執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="40de5-134">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="40de5-135">注意:如果您要部署至 Azure 網站，您不需要執行以下任何動作&mdash;Node.js 已安裝並可在伺服器環境中。</span><span class="sxs-lookup"><span data-stu-id="40de5-135">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="40de5-136">如果您是使用 Visual Studio 2017 的 Windows 上，選取已安裝 SDK **.NET Core 跨平台開發**工作負載。</span><span class="sxs-lookup"><span data-stu-id="40de5-136">If you're on Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="40de5-137">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="40de5-137">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="40de5-138">伺服器端渲染</span><span class="sxs-lookup"><span data-stu-id="40de5-138">Server-side prerendering</span></span>

<span data-ttu-id="40de5-139">通用 (也稱為 isomorphic) 應用程式是 JavaScript 應用程式能夠執行同時在伺服器和用戶端上。</span><span class="sxs-lookup"><span data-stu-id="40de5-139">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="40de5-140">Angular、 React 和其他常用架構提供此應用程式的開發樣式通用平台。</span><span class="sxs-lookup"><span data-stu-id="40de5-140">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="40de5-141">做法是先呈現架構上的元件透過 Node.js 伺服器，然後進一步委派給用戶端執行。</span><span class="sxs-lookup"><span data-stu-id="40de5-141">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="40de5-142">ASP.NET Core[標籤協助程式](xref:mvc/views/tag-helpers/intro)提供 SpaServices 簡化伺服器端預呈現的實作，藉由叫用伺服器上的 JavaScript 函式。</span><span class="sxs-lookup"><span data-stu-id="40de5-142">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="40de5-143">必要條件</span><span class="sxs-lookup"><span data-stu-id="40de5-143">Prerequisites</span></span>

<span data-ttu-id="40de5-144">安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="40de5-144">Install the following:</span></span>

* <span data-ttu-id="40de5-145">[aspnet 預呈現](https://www.npmjs.com/package/aspnet-prerendering)npm 套件：</span><span class="sxs-lookup"><span data-stu-id="40de5-145">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="40de5-146">組態</span><span class="sxs-lookup"><span data-stu-id="40de5-146">Configuration</span></span>

<span data-ttu-id="40de5-147">在專案中，標籤協助程式會變成可探索透過命名空間註冊 *_ViewImports.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="40de5-147">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="40de5-148">這些標籤協助程式抽離利用 HTML 的類似語法，在 Razor 檢視內直接使用低層級的 Api 進行通訊的複雜性：</span><span class="sxs-lookup"><span data-stu-id="40de5-148">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="40de5-149">`asp-prerender-module`標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="40de5-149">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="40de5-150">`asp-prerender-module`標籤協助程式，用於上述程式碼範例中，執行*ClientApp/dist/main-server.js*上透過 Node.js 伺服器。</span><span class="sxs-lookup"><span data-stu-id="40de5-150">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="40de5-151">為了清楚起見*主要 server.js*檔案是中的 TypeScript-JavaScript 的轉譯工作的成品[Webpack](http://webpack.github.io/)建置程序。</span><span class="sxs-lookup"><span data-stu-id="40de5-151">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="40de5-152">Webpack 定義的項目點別名`main-server`; 並在開始此別名的相依性圖形周遊*ClientApp/開機-server.ts*檔案：</span><span class="sxs-lookup"><span data-stu-id="40de5-152">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="40de5-153">在下列的 Angular 範例中， *ClientApp/開機-server.ts*檔案會利用`createServerRenderer`函式和`RenderResult`類型`aspnet-prerendering`設定伺服器轉譯透過 Node.js 的 npm 封裝。</span><span class="sxs-lookup"><span data-stu-id="40de5-153">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="40de5-154">預定要給伺服器端轉譯會傳遞至解析函式呼叫，包裝在強型別在 JavaScript 中的 HTML 標記`Promise`物件。</span><span class="sxs-lookup"><span data-stu-id="40de5-154">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="40de5-155">`Promise`物件的意義是，它會以非同步方式提供至 DOM 的預留位置項目中的插入頁面的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="40de5-155">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="40de5-156">`asp-prerender-data`標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="40de5-156">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="40de5-157">如果結合`asp-prerender-module`標籤協助程式，`asp-prerender-data`標籤協助程式可用來從 Razor 檢視中將伺服器端 JavaScript 的內容資訊。</span><span class="sxs-lookup"><span data-stu-id="40de5-157">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="40de5-158">例如，下列標記使用者會將資料傳遞至`main-server`模組：</span><span class="sxs-lookup"><span data-stu-id="40de5-158">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="40de5-159">接收`UserName`引數會使用內建的 JSON 序列化程式序列化並儲存在`params.data`物件。</span><span class="sxs-lookup"><span data-stu-id="40de5-159">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="40de5-160">在下列的 Angular 範例中，資料用來建構內的個人化的問候語`h1`項目：</span><span class="sxs-lookup"><span data-stu-id="40de5-160">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="40de5-161">注意:標籤協助程式中傳遞的屬性名稱以表示**PascalCase**標記法。</span><span class="sxs-lookup"><span data-stu-id="40de5-161">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="40de5-162">Javascript 中，相同的屬性名稱以表示的相反**camelCase**。</span><span class="sxs-lookup"><span data-stu-id="40de5-162">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="40de5-163">預設值的 JSON 序列化組態會負責這項差異。</span><span class="sxs-lookup"><span data-stu-id="40de5-163">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="40de5-164">若要展開在前述的程式碼範例時，資料可以傳遞從伺服器至檢視的 hydrating`globals`屬性提供給`resolve`函式：</span><span class="sxs-lookup"><span data-stu-id="40de5-164">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="40de5-165">`postList`內定義的陣列`globals`物件附加至瀏覽器的全域`window`物件。</span><span class="sxs-lookup"><span data-stu-id="40de5-165">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="40de5-166">此變數的 hoisting 至全域範圍就不重複的工作，，特別是當它適用於載入相同的資料一次在伺服器上，另一次在用戶端上。</span><span class="sxs-lookup"><span data-stu-id="40de5-166">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![附加到視窗物件的全域 postList 變數](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="40de5-168">Webpack 開發中介軟體</span><span class="sxs-lookup"><span data-stu-id="40de5-168">Webpack Dev Middleware</span></span>

<span data-ttu-id="40de5-169">[Webpack 開發中介軟體](https://webpack.github.io/docs/webpack-dev-middleware.html)導入了簡化的開發工作流程，藉此讓 Webpack 是根據資源需求。</span><span class="sxs-lookup"><span data-stu-id="40de5-169">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="40de5-170">中介軟體會自動編譯並在瀏覽器中重新載入頁面時，提供用戶端的資源。</span><span class="sxs-lookup"><span data-stu-id="40de5-170">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="40de5-171">替代方法是以手動方式時要叫用 Webpack 透過專案的 npm 建置指令碼的第三方相依性或自訂程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="40de5-171">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="40de5-172">Npm 建置指令碼*package.json*檔案在下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="40de5-172">An npm build script in the *package.json* file is shown in the following example:</span></span>

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="prerequisites"></a><span data-ttu-id="40de5-173">必要條件</span><span class="sxs-lookup"><span data-stu-id="40de5-173">Prerequisites</span></span>

<span data-ttu-id="40de5-174">安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="40de5-174">Install the following:</span></span>

* <span data-ttu-id="40de5-175">[aspnet webpack](https://www.npmjs.com/package/aspnet-webpack) npm 套件：</span><span class="sxs-lookup"><span data-stu-id="40de5-175">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="40de5-176">組態</span><span class="sxs-lookup"><span data-stu-id="40de5-176">Configuration</span></span>

<span data-ttu-id="40de5-177">Webpack 開發中介軟體會註冊到 HTTP 要求管線中的下列程式碼透過*Startup.cs*檔案的`Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="40de5-177">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

<span data-ttu-id="40de5-178">`UseWebpackDevMiddleware`擴充方法必須先呼叫才能[註冊靜態檔案裝載](xref:fundamentals/static-files)透過`UseStaticFiles`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="40de5-178">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="40de5-179">基於安全性理由，請在開發模式中執行的應用程式時，才註冊中介軟體。</span><span class="sxs-lookup"><span data-stu-id="40de5-179">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="40de5-180">*Webpack.config.js*檔案的`output.publicPath`屬性會告知中介軟體觀看`dist`資料夾變更：</span><span class="sxs-lookup"><span data-stu-id="40de5-180">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="40de5-181">熱門的模組更換</span><span class="sxs-lookup"><span data-stu-id="40de5-181">Hot Module Replacement</span></span>

<span data-ttu-id="40de5-182">Webpack 的想像[熱模組替換](https://webpack.js.org/concepts/hot-module-replacement/)視為的進化版 (HMR) 功能[Webpack 開發中介軟體](#webpack-dev-middleware)。</span><span class="sxs-lookup"><span data-stu-id="40de5-182">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="40de5-183">HMR 完全相同帶來的好處，但它藉由自動編譯所做的變更之後更新頁面內容，進一步簡化開發工作流程。</span><span class="sxs-lookup"><span data-stu-id="40de5-183">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="40de5-184">請勿混淆這與重新整理瀏覽器中，這會干擾的 SPA 的偵錯工作階段的目前記憶體中狀態。</span><span class="sxs-lookup"><span data-stu-id="40de5-184">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="40de5-185">沒有 Webpack 開發中介軟體服務與瀏覽器中，這表示變更推送至瀏覽器之間的即時連結。</span><span class="sxs-lookup"><span data-stu-id="40de5-185">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="40de5-186">必要條件</span><span class="sxs-lookup"><span data-stu-id="40de5-186">Prerequisites</span></span>

<span data-ttu-id="40de5-187">安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="40de5-187">Install the following:</span></span>

* <span data-ttu-id="40de5-188">[webpack 經常性存取-中介軟體](https://www.npmjs.com/package/webpack-hot-middleware)npm 套件：</span><span class="sxs-lookup"><span data-stu-id="40de5-188">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="40de5-189">組態</span><span class="sxs-lookup"><span data-stu-id="40de5-189">Configuration</span></span>

<span data-ttu-id="40de5-190">HMR 元件必須註冊至 MVC 的 HTTP 要求管線`Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="40de5-190">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="40de5-191">做為已使用，則為 true [Webpack 開發中介軟體](#webpack-dev-middleware)，則`UseWebpackDevMiddleware`擴充方法必須先呼叫才能`UseStaticFiles`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="40de5-191">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="40de5-192">基於安全性理由，請在開發模式中執行的應用程式時，才註冊中介軟體。</span><span class="sxs-lookup"><span data-stu-id="40de5-192">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="40de5-193">*Webpack.config.js*檔案必須定義`plugins`陣列，即使其為空白：</span><span class="sxs-lookup"><span data-stu-id="40de5-193">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="40de5-194">載入瀏覽器中的應用程式之後, 的開發人員工具主控台 索引標籤會提供 HMR 啟用的確認：</span><span class="sxs-lookup"><span data-stu-id="40de5-194">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![熱門的模組取代連接的訊息](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="40de5-196">路由的協助程式</span><span class="sxs-lookup"><span data-stu-id="40de5-196">Routing helpers</span></span>

<span data-ttu-id="40de5-197">在大多數 ASP.NET Core 為基礎的 Spa，您需要用戶端除了伺服器端路由的路由。</span><span class="sxs-lookup"><span data-stu-id="40de5-197">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="40de5-198">SPA 和 MVC 的路由系統可以獨立運作，不受干擾。</span><span class="sxs-lookup"><span data-stu-id="40de5-198">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="40de5-199">沒有，不過，一個邊緣案例提出的挑戰： 找出 404 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="40de5-199">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="40de5-200">請考慮案例，其中的無副檔名的路由`/some/page`用。</span><span class="sxs-lookup"><span data-stu-id="40de5-200">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="40de5-201">假設要求不模式比對伺服器端路由，但其模式符合的用戶端的路由。</span><span class="sxs-lookup"><span data-stu-id="40de5-201">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="40de5-202">現在，假設連入要求`/images/user-512.png`，這通常會預期找到伺服器上的影像檔。</span><span class="sxs-lookup"><span data-stu-id="40de5-202">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="40de5-203">如果該要求的資源路徑不符合任何伺服器端路由或靜態檔案，也不太可能用戶端應用程式會處理它，您通常想要傳回 404 的 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="40de5-203">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="40de5-204">必要條件</span><span class="sxs-lookup"><span data-stu-id="40de5-204">Prerequisites</span></span>

<span data-ttu-id="40de5-205">安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="40de5-205">Install the following:</span></span>

* <span data-ttu-id="40de5-206">用戶端路由 npm 套件。</span><span class="sxs-lookup"><span data-stu-id="40de5-206">The client-side routing npm package.</span></span> <span data-ttu-id="40de5-207">使用 Angular 做為範例：</span><span class="sxs-lookup"><span data-stu-id="40de5-207">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="40de5-208">組態</span><span class="sxs-lookup"><span data-stu-id="40de5-208">Configuration</span></span>

<span data-ttu-id="40de5-209">名為擴充方法`MapSpaFallbackRoute`用於`Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="40de5-209">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

<span data-ttu-id="40de5-210">提示：路由會評估已設定的順序。</span><span class="sxs-lookup"><span data-stu-id="40de5-210">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="40de5-211">因此，`default`進行模式比對第一次使用上述的程式碼範例中的路由。</span><span class="sxs-lookup"><span data-stu-id="40de5-211">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="40de5-212">建立新的專案</span><span class="sxs-lookup"><span data-stu-id="40de5-212">Creating a new project</span></span>

<span data-ttu-id="40de5-213">JavaScriptServices 提供預先設定的應用程式範本。</span><span class="sxs-lookup"><span data-stu-id="40de5-213">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="40de5-214">在這些範本，搭配不同的架構和程式庫，例如 Angular、 React 和 Redux 用於 SpaServices。</span><span class="sxs-lookup"><span data-stu-id="40de5-214">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="40de5-215">這些範本可以透過.NET Core CLI 安裝，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="40de5-215">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="40de5-216">會顯示一份可用的 SPA 範本：</span><span class="sxs-lookup"><span data-stu-id="40de5-216">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="40de5-217">範本</span><span class="sxs-lookup"><span data-stu-id="40de5-217">Templates</span></span>                                 | <span data-ttu-id="40de5-218">簡短名稱</span><span class="sxs-lookup"><span data-stu-id="40de5-218">Short Name</span></span> | <span data-ttu-id="40de5-219">語言</span><span class="sxs-lookup"><span data-stu-id="40de5-219">Language</span></span> | <span data-ttu-id="40de5-220">Tags</span><span class="sxs-lookup"><span data-stu-id="40de5-220">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="40de5-221">將 ASP.NET Core MVC 與 Angular</span><span class="sxs-lookup"><span data-stu-id="40de5-221">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="40de5-222">angular</span><span class="sxs-lookup"><span data-stu-id="40de5-222">angular</span></span>    | <span data-ttu-id="40de5-223">[C#]</span><span class="sxs-lookup"><span data-stu-id="40de5-223">[C#]</span></span>     | <span data-ttu-id="40de5-224">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="40de5-224">Web/MVC/SPA</span></span> |
| <span data-ttu-id="40de5-225">MVC ASP.NET Core 與 React.js</span><span class="sxs-lookup"><span data-stu-id="40de5-225">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="40de5-226">react</span><span class="sxs-lookup"><span data-stu-id="40de5-226">react</span></span>      | <span data-ttu-id="40de5-227">[C#]</span><span class="sxs-lookup"><span data-stu-id="40de5-227">[C#]</span></span>     | <span data-ttu-id="40de5-228">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="40de5-228">Web/MVC/SPA</span></span> |
| <span data-ttu-id="40de5-229">MVC ASP.NET Core 與 React.js 和 Redux</span><span class="sxs-lookup"><span data-stu-id="40de5-229">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="40de5-230">reactredux</span><span class="sxs-lookup"><span data-stu-id="40de5-230">reactredux</span></span> | <span data-ttu-id="40de5-231">[C#]</span><span class="sxs-lookup"><span data-stu-id="40de5-231">[C#]</span></span>     | <span data-ttu-id="40de5-232">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="40de5-232">Web/MVC/SPA</span></span> |

<span data-ttu-id="40de5-233">若要建立新專案使用其中一個 SPA 範本時，包含**簡短名稱**中的範本[dotnet 新](/dotnet/core/tools/dotnet-new)命令。</span><span class="sxs-lookup"><span data-stu-id="40de5-233">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="40de5-234">下列命令會使用伺服器端設定的 ASP.NET Core MVC 建立 Angular 應用程式：</span><span class="sxs-lookup"><span data-stu-id="40de5-234">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="40de5-235">設定執行階段組態模式</span><span class="sxs-lookup"><span data-stu-id="40de5-235">Set the runtime configuration mode</span></span>

<span data-ttu-id="40de5-236">有兩種主要的執行階段組態模式：</span><span class="sxs-lookup"><span data-stu-id="40de5-236">Two primary runtime configuration modes exist:</span></span>

* <span data-ttu-id="40de5-237">**開發**:</span><span class="sxs-lookup"><span data-stu-id="40de5-237">**Development**:</span></span>
  * <span data-ttu-id="40de5-238">包含來源對應，以便偵錯。</span><span class="sxs-lookup"><span data-stu-id="40de5-238">Includes source maps to ease debugging.</span></span>
  * <span data-ttu-id="40de5-239">不最佳化效能的用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="40de5-239">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="40de5-240">**生產**:</span><span class="sxs-lookup"><span data-stu-id="40de5-240">**Production**:</span></span>
  * <span data-ttu-id="40de5-241">排除來源對應。</span><span class="sxs-lookup"><span data-stu-id="40de5-241">Excludes source maps.</span></span>
  * <span data-ttu-id="40de5-242">最佳化透過統合和縮製的用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="40de5-242">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="40de5-243">ASP.NET Core 會使用環境變數，名為`ASPNETCORE_ENVIRONMENT`來儲存組態模式。</span><span class="sxs-lookup"><span data-stu-id="40de5-243">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="40de5-244">請參閱 **[設定的環境](xref:fundamentals/environments#set-the-environment)** 如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="40de5-244">See **[Set the environment](xref:fundamentals/environments#set-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="40de5-245">執行使用.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="40de5-245">Running with .NET Core CLI</span></span>

<span data-ttu-id="40de5-246">在專案的根目錄執行下列命令來還原必要的 NuGet 和 npm 套件：</span><span class="sxs-lookup"><span data-stu-id="40de5-246">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="40de5-247">建置和執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="40de5-247">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="40de5-248">根據本機主機上的應用程式啟動[執行階段組態模式](#runtime-config-mode)。</span><span class="sxs-lookup"><span data-stu-id="40de5-248">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="40de5-249">瀏覽至`http://localhost:5000`瀏覽器中顯示的登陸頁面。</span><span class="sxs-lookup"><span data-stu-id="40de5-249">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="40de5-250">執行使用 Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="40de5-250">Running with Visual Studio 2017</span></span>

<span data-ttu-id="40de5-251">開啟 *.csproj*所產生的檔案[dotnet 新](/dotnet/core/tools/dotnet-new)命令。</span><span class="sxs-lookup"><span data-stu-id="40de5-251">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="40de5-252">在專案開啟時，會自動還原必要的 NuGet 和 npm 套件。</span><span class="sxs-lookup"><span data-stu-id="40de5-252">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="40de5-253">此還原程序可能需要幾分鐘的時間，以及應用程式已準備好執行完成時。</span><span class="sxs-lookup"><span data-stu-id="40de5-253">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="40de5-254">按一下綠色 [執行] 按鈕或按下`Ctrl + F5`，而且瀏覽器會開啟到應用程式的登陸頁面。</span><span class="sxs-lookup"><span data-stu-id="40de5-254">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="40de5-255">根據本機主機上執行的應用程式[執行階段組態模式](#runtime-config-mode)。</span><span class="sxs-lookup"><span data-stu-id="40de5-255">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span>

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="40de5-256">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="40de5-256">Testing the app</span></span>

<span data-ttu-id="40de5-257">SpaServices 範本已預先設定為執行用戶端使用的測試[Karma](https://karma-runner.github.io/1.0/index.html)並[Jasmine](https://jasmine.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="40de5-257">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="40de5-258">Jasmine 是熱門的單元測試架構的 JavaScript，而 Karma 是這些測試的測試執行器。</span><span class="sxs-lookup"><span data-stu-id="40de5-258">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="40de5-259">Karma 設定為搭配[Webpack 開發中介軟體](#webpack-dev-middleware)使開發人員不需要停止並執行測試，每次進行變更。</span><span class="sxs-lookup"><span data-stu-id="40de5-259">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="40de5-260">不論是針對測試案例或測試案例本身所執行的程式碼，測試將會自動執行。</span><span class="sxs-lookup"><span data-stu-id="40de5-260">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="40de5-261">使用 Angular 的應用程式，例如，兩個 Jasmine 測試案例已提供`CounterComponent`中*counter.component.spec.ts*檔案：</span><span class="sxs-lookup"><span data-stu-id="40de5-261">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="40de5-262">開啟命令提示字元*ClientApp*目錄。</span><span class="sxs-lookup"><span data-stu-id="40de5-262">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="40de5-263">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="40de5-263">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="40de5-264">指令碼啟動 Karma 測試執行器，其內容中定義的設定*karma.conf.js*檔案。</span><span class="sxs-lookup"><span data-stu-id="40de5-264">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="40de5-265">在其他設定中， *karma.conf.js*識別的測試檔案，以透過執行其`files`陣列：</span><span class="sxs-lookup"><span data-stu-id="40de5-265">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="40de5-266">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="40de5-266">Publishing the application</span></span>

<span data-ttu-id="40de5-267">將產生的用戶端資產和已發行的 ASP.NET Core 成品結合成已準備好部署的封裝可能會相當繁雜。</span><span class="sxs-lookup"><span data-stu-id="40de5-267">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="40de5-268">幸好 SpaServices 協調該整個發行程序名為的自訂 MSBuild 目標與`RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="40de5-268">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="40de5-269">MSBuild 目標須擔負下列責任：</span><span class="sxs-lookup"><span data-stu-id="40de5-269">The MSBuild target has the following responsibilities:</span></span>

1. <span data-ttu-id="40de5-270">還原 npm 套件</span><span class="sxs-lookup"><span data-stu-id="40de5-270">Restore the npm packages</span></span>
1. <span data-ttu-id="40de5-271">建立生產等級的組建的協力廠商的用戶端資產</span><span class="sxs-lookup"><span data-stu-id="40de5-271">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="40de5-272">建立自訂用戶端資產的生產等級的組建</span><span class="sxs-lookup"><span data-stu-id="40de5-272">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="40de5-273">Webpack 產生資產複製到發佈資料夾</span><span class="sxs-lookup"><span data-stu-id="40de5-273">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="40de5-274">執行時，會叫用 MSBuild 目標：</span><span class="sxs-lookup"><span data-stu-id="40de5-274">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="40de5-275">其他資源</span><span class="sxs-lookup"><span data-stu-id="40de5-275">Additional resources</span></span>

* [<span data-ttu-id="40de5-276">Angular Docs</span><span class="sxs-lookup"><span data-stu-id="40de5-276">Angular Docs</span></span>](https://angular.io/docs)
