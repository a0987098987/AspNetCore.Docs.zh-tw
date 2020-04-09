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
# <a name="use-javascript-services-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="d1f54-103">使用 JavaScript 服務在 ASP.NET核心中建立單頁應用程式</span><span class="sxs-lookup"><span data-stu-id="d1f54-103">Use JavaScript Services to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="d1f54-104">由[斯科特·阿迪](https://github.com/scottaddie)和[菲亞茲·哈桑](https://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="d1f54-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](https://fiyazhasan.me/)</span></span>

<span data-ttu-id="d1f54-105">單頁應用程式 (SPA) 是一種流行的 Web 應用程式類型,由於其固有的豐富用戶體驗。</span><span class="sxs-lookup"><span data-stu-id="d1f54-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="d1f54-106">將用戶端 SPA 框架或庫(如["角"](https://angular.io/)或[「React」)](https://facebook.github.io/react/)與伺服器端框架(如 ASP.NET Core)整合可能很困難。</span><span class="sxs-lookup"><span data-stu-id="d1f54-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks such as ASP.NET Core can be difficult.</span></span> <span data-ttu-id="d1f54-107">JavaScript 服務是為減少整合過程中的摩擦而開發的。</span><span class="sxs-lookup"><span data-stu-id="d1f54-107">JavaScript Services was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="d1f54-108">它支援不同用戶端和伺服器技術堆疊之間的無縫操作。</span><span class="sxs-lookup"><span data-stu-id="d1f54-108">It enables seamless operation between the different client and server technology stacks.</span></span>

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="d1f54-109">本文中描述的功能從 ASP.NET酷 3.0 起已過時。</span><span class="sxs-lookup"><span data-stu-id="d1f54-109">The features described in this article are obsolete as of ASP.NET Core 3.0.</span></span> <span data-ttu-id="d1f54-110">[微軟](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices.Extensions)提供了更簡單的SPA框架集成機制。</span><span class="sxs-lookup"><span data-stu-id="d1f54-110">A simpler SPA frameworks integration mechanism is available in the [Microsoft.AspNetCore.SpaServices.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices.Extensions) NuGet package.</span></span> <span data-ttu-id="d1f54-111">有關詳細資訊,請參閱[[公告] 將微軟(AspNetCore.SpaServices)和微軟(Microsoft.AspNetCore.NodeServices)淘汰](https://github.com/dotnet/AspNetCore/issues/12890)。</span><span class="sxs-lookup"><span data-stu-id="d1f54-111">For more information, see [[Announcement] Obsoleting Microsoft.AspNetCore.SpaServices and Microsoft.AspNetCore.NodeServices](https://github.com/dotnet/AspNetCore/issues/12890).</span></span>

::: moniker-end

## <a name="what-is-javascript-services"></a><span data-ttu-id="d1f54-112">什麼是 JavaScript 服務</span><span class="sxs-lookup"><span data-stu-id="d1f54-112">What is JavaScript Services</span></span>

<span data-ttu-id="d1f54-113">JavaScript 服務是ASP.NET核心的用戶端技術的集合。</span><span class="sxs-lookup"><span data-stu-id="d1f54-113">JavaScript Services is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="d1f54-114">其目標是將ASP.NET核心定位為開發人員首選的構建SPA的伺服器端平臺。</span><span class="sxs-lookup"><span data-stu-id="d1f54-114">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="d1f54-115">JavaScript 服務由兩個不同的 NuGet 套件組成:</span><span class="sxs-lookup"><span data-stu-id="d1f54-115">JavaScript Services consists of two distinct NuGet packages:</span></span>

* <span data-ttu-id="d1f54-116">[微軟.AspNetCore.節點服務](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/)(節點服務)</span><span class="sxs-lookup"><span data-stu-id="d1f54-116">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="d1f54-117">[微軟.AspNetCore.Spa服務](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/)(Spa服務)</span><span class="sxs-lookup"><span data-stu-id="d1f54-117">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>

<span data-ttu-id="d1f54-118">這些包在以下方案中非常有用:</span><span class="sxs-lookup"><span data-stu-id="d1f54-118">These packages are useful in the following scenarios:</span></span>

* <span data-ttu-id="d1f54-119">在伺服器上執行 JavaScript</span><span class="sxs-lookup"><span data-stu-id="d1f54-119">Run JavaScript on the server</span></span>
* <span data-ttu-id="d1f54-120">使用 SPA 架構或庫</span><span class="sxs-lookup"><span data-stu-id="d1f54-120">Use a SPA framework or library</span></span>
* <span data-ttu-id="d1f54-121">使用 Webpack 建構客戶端資產</span><span class="sxs-lookup"><span data-stu-id="d1f54-121">Build client-side assets with Webpack</span></span>

<span data-ttu-id="d1f54-122">本文的大部分重點是使用 SpaServices 包。</span><span class="sxs-lookup"><span data-stu-id="d1f54-122">Much of the focus in this article is placed on using the SpaServices package.</span></span>

## <a name="what-is-spaservices"></a><span data-ttu-id="d1f54-123">什麼是水療服務</span><span class="sxs-lookup"><span data-stu-id="d1f54-123">What is SpaServices</span></span>

<span data-ttu-id="d1f54-124">創建 SpaServices 是為了將 ASP.NET Core 定位為開發人員首選的建構 SpaA 的伺服器端平臺。</span><span class="sxs-lookup"><span data-stu-id="d1f54-124">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="d1f54-125">SpaServices 不需要使用 ASP.NET 酷睿開發 SpaA,也不會將開發人員鎖定到特定的用戶端框架中。</span><span class="sxs-lookup"><span data-stu-id="d1f54-125">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock developers into a particular client framework.</span></span>

<span data-ttu-id="d1f54-126">SpaServices 提供有用的基礎結構,例如:</span><span class="sxs-lookup"><span data-stu-id="d1f54-126">SpaServices provides useful infrastructure such as:</span></span>

* [<span data-ttu-id="d1f54-127">伺服器端預像</span><span class="sxs-lookup"><span data-stu-id="d1f54-127">Server-side prerendering</span></span>](#server-side-prerendering)
* [<span data-ttu-id="d1f54-128">Webpack 開發人員的中間件</span><span class="sxs-lookup"><span data-stu-id="d1f54-128">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="d1f54-129">熱模組更換</span><span class="sxs-lookup"><span data-stu-id="d1f54-129">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="d1f54-130">路由說明器</span><span class="sxs-lookup"><span data-stu-id="d1f54-130">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="d1f54-131">總之,這些基礎結構元件既增強了開發工作流,又增強了運行時體驗。</span><span class="sxs-lookup"><span data-stu-id="d1f54-131">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="d1f54-132">元件可以單獨採用。</span><span class="sxs-lookup"><span data-stu-id="d1f54-132">The components can be adopted individually.</span></span>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="d1f54-133">使用 Spa 服務的先決條件</span><span class="sxs-lookup"><span data-stu-id="d1f54-133">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="d1f54-134">要使用 SpaServices,請安裝以下內容:</span><span class="sxs-lookup"><span data-stu-id="d1f54-134">To work with SpaServices, install the following:</span></span>

* <span data-ttu-id="d1f54-135">[Node.js(](https://nodejs.org/)版本 6 或更高版本),帶 npm</span><span class="sxs-lookup"><span data-stu-id="d1f54-135">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>

  * <span data-ttu-id="d1f54-136">要驗證這些元件已安裝並可以找到,請從命令列執行以下內容:</span><span class="sxs-lookup"><span data-stu-id="d1f54-136">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

  * <span data-ttu-id="d1f54-137">如果部署到 Azure 網站,&mdash;則不需要安裝 Node.js 並在伺服器環境中可用。</span><span class="sxs-lookup"><span data-stu-id="d1f54-137">If deploying to an Azure web site, no action is required&mdash;Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="d1f54-138">在使用 Visual Studio 2017 的 Windows 上,通過選擇 **.NET Core 跨平台開發**工作負載來安裝 SDK。</span><span class="sxs-lookup"><span data-stu-id="d1f54-138">On Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="d1f54-139">[微軟.AspNetCore.Spa服務](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/)NuGet包</span><span class="sxs-lookup"><span data-stu-id="d1f54-139">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

## <a name="server-side-prerendering"></a><span data-ttu-id="d1f54-140">伺服器端預像</span><span class="sxs-lookup"><span data-stu-id="d1f54-140">Server-side prerendering</span></span>

<span data-ttu-id="d1f54-141">通用(也稱為同構)應用程式是能夠在伺服器和用戶端上運行的 JavaScript 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d1f54-141">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="d1f54-142">角、反應和其他流行的框架為此應用程式開發風格提供了一個通用平臺。</span><span class="sxs-lookup"><span data-stu-id="d1f54-142">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="d1f54-143">其理念是首先通過 Node.js 呈現伺服器上的框架元件,然後將進一步執行委託給用戶端。</span><span class="sxs-lookup"><span data-stu-id="d1f54-143">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="d1f54-144">ASP.NET SpaServices 提供的核心[標記説明器](xref:mvc/views/tag-helpers/intro)通過在伺服器上調用 JavaScript 函數來簡化伺服器端預呈現的實現。</span><span class="sxs-lookup"><span data-stu-id="d1f54-144">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="server-side-prerendering-prerequisites"></a><span data-ttu-id="d1f54-145">伺服器端預先呈現先決條件</span><span class="sxs-lookup"><span data-stu-id="d1f54-145">Server-side prerendering prerequisites</span></span>

<span data-ttu-id="d1f54-146">安裝[阿斯普奈特預成成](https://www.npmjs.com/package/aspnet-prerendering)npm 套件:</span><span class="sxs-lookup"><span data-stu-id="d1f54-146">Install the [aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

```console
npm i -S aspnet-prerendering
```

### <a name="server-side-prerendering-configuration"></a><span data-ttu-id="d1f54-147">伺服器端預先渲染設定</span><span class="sxs-lookup"><span data-stu-id="d1f54-147">Server-side prerendering configuration</span></span>

<span data-ttu-id="d1f54-148">標記說明器可透過項目的 *_ViewImports.cshtml*檔案中的命名空間註冊進行發現:</span><span class="sxs-lookup"><span data-stu-id="d1f54-148">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="d1f54-149">這些標記說明器利用 Razor 檢視中類似 HTML 的語法,抽象出與低級 API 直接通訊的複雜性:</span><span class="sxs-lookup"><span data-stu-id="d1f54-149">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="asp-prerender-module-tag-helper"></a><span data-ttu-id="d1f54-150">asp 預先成模組標記說明程式</span><span class="sxs-lookup"><span data-stu-id="d1f54-150">asp-prerender-module Tag Helper</span></span>

<span data-ttu-id="d1f54-151">在`asp-prerender-module`上述代碼示例中使用的標記説明程式透過 Node.js 在伺服器上執行*客戶端應用/dist/main-server.js。*</span><span class="sxs-lookup"><span data-stu-id="d1f54-151">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="d1f54-152">為了清楚起見,*主伺服器.js*檔是[Webpack](https://webpack.github.io/)生成過程中 TypeScript 到 JavaScript 溢出任務的專案。</span><span class="sxs-lookup"><span data-stu-id="d1f54-152">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](https://webpack.github.io/) build process.</span></span> <span data-ttu-id="d1f54-153">Webpack`main-server`定義的入口點別名 。並且,此別名的依賴項圖遍曆從*用戶端應用/引導伺服器.ts*檔開始:</span><span class="sxs-lookup"><span data-stu-id="d1f54-153">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="d1f54-154">在下面的 Angular 範例中 *,ClientApp/boot-server.ts*檔案利用`createServerRenderer``RenderResult``aspnet-prerendering`npm 包的功能和類型透過 Node.js 設定伺服器呈現。</span><span class="sxs-lookup"><span data-stu-id="d1f54-154">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="d1f54-155">用於伺服器端呈現的 HTML 標記將傳遞到解析函數調用,該函數調用套件在強類型 JavaScript`Promise`物件中。</span><span class="sxs-lookup"><span data-stu-id="d1f54-155">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="d1f54-156">對`Promise`象的意義是,它非同步地將 HTML 標記提供到頁以在 DOM 的占位符元素中注入。</span><span class="sxs-lookup"><span data-stu-id="d1f54-156">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="asp-prerender-data-tag-helper"></a><span data-ttu-id="d1f54-157">asp 預先成資料標記說明程式</span><span class="sxs-lookup"><span data-stu-id="d1f54-157">asp-prerender-data Tag Helper</span></span>

<span data-ttu-id="d1f54-158">與`asp-prerender-module`標籤幫助程式結合時,`asp-prerender-data`標記説明程式可用於將上下文資訊從 Razor 視圖傳遞到伺服器端 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="d1f54-158">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="d1f54-159">例如,以下標記將使用者資料傳遞給`main-server`模組:</span><span class="sxs-lookup"><span data-stu-id="d1f54-159">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="d1f54-160">接收`UserName`的參數使用內置的 JSON 序列化器進行序列化,並存儲`params.data`在物件 中。</span><span class="sxs-lookup"><span data-stu-id="d1f54-160">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="d1f54-161">在下面的 Angular 範例中,資料`h1`用於在 元素中建構個人化問候語:</span><span class="sxs-lookup"><span data-stu-id="d1f54-161">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="d1f54-162">標記説明器中傳遞的屬性名稱用**PascalCase**表示法表示。</span><span class="sxs-lookup"><span data-stu-id="d1f54-162">Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="d1f54-163">對比 JAVAScript,其中相同的屬性名稱用**camelCase**表示。</span><span class="sxs-lookup"><span data-stu-id="d1f54-163">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="d1f54-164">默認 JSON 序列化配置負責此差異。</span><span class="sxs-lookup"><span data-stu-id="d1f54-164">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="d1f54-165">要在上述代碼示例中展開,可以通過對提供`globals``resolve`給 函數的屬性進行水化,將數據從伺服器傳遞到視圖:</span><span class="sxs-lookup"><span data-stu-id="d1f54-165">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="d1f54-166">在`postList``globals`物件內部定義的陣列附加到瀏覽器的全域`window`物件。</span><span class="sxs-lookup"><span data-stu-id="d1f54-166">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="d1f54-167">此變數提升到全域範圍可消除重複工作,特別是因為它涉及在伺服器上再次在用戶端上載入相同的數據。</span><span class="sxs-lookup"><span data-stu-id="d1f54-167">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![附至視窗物件的全域後列表變數](spa-services/_static/global_variable.png)

## <a name="webpack-dev-middleware"></a><span data-ttu-id="d1f54-169">Webpack 開發人員的中間件</span><span class="sxs-lookup"><span data-stu-id="d1f54-169">Webpack Dev Middleware</span></span>

<span data-ttu-id="d1f54-170">[Webpack 開發人員中間件](https://webpack.js.org/guides/development/#using-webpack-dev-middleware)引入了簡化的開發工作流,Webpack 可按需構建資源。</span><span class="sxs-lookup"><span data-stu-id="d1f54-170">[Webpack Dev Middleware](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="d1f54-171">當頁面在瀏覽器中重新載入時,中間件會自動編譯和提供客戶端資源。</span><span class="sxs-lookup"><span data-stu-id="d1f54-171">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="d1f54-172">另一種方法是在第三方依賴項或自定義代碼更改時,通過專案的 npm 生成腳本手動調用 Webpack。</span><span class="sxs-lookup"><span data-stu-id="d1f54-172">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="d1f54-173">*在包.json*檔中的 npm 產生文稿顯示在以下範例中:</span><span class="sxs-lookup"><span data-stu-id="d1f54-173">An npm build script in the *package.json* file is shown in the following example:</span></span>

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="webpack-dev-middleware-prerequisites"></a><span data-ttu-id="d1f54-174">Webpack 開發人員的元件先決條件</span><span class="sxs-lookup"><span data-stu-id="d1f54-174">Webpack Dev Middleware prerequisites</span></span>

<span data-ttu-id="d1f54-175">安裝[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm 套件:</span><span class="sxs-lookup"><span data-stu-id="d1f54-175">Install the [aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

```console
npm i -D aspnet-webpack
```

### <a name="webpack-dev-middleware-configuration"></a><span data-ttu-id="d1f54-176">Webpack 開發人員的元件設定</span><span class="sxs-lookup"><span data-stu-id="d1f54-176">Webpack Dev Middleware configuration</span></span>

<span data-ttu-id="d1f54-177">Webpack 開發人員的額外*Startup.cs*Startup.cs`Configure`檔案方法中的以下代碼註冊到 HTTP 要求導管中:</span><span class="sxs-lookup"><span data-stu-id="d1f54-177">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

<span data-ttu-id="d1f54-178">在`UseWebpackDevMiddleware`從擴充方法[註冊靜態檔託管](xref:fundamentals/static-files)`UseStaticFiles`之前, 必須調用擴充方法。</span><span class="sxs-lookup"><span data-stu-id="d1f54-178">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="d1f54-179">出於安全原因,僅當應用在開發模式下運行時才註冊中間件。</span><span class="sxs-lookup"><span data-stu-id="d1f54-179">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="d1f54-180">*Webpack.config.js*`output.publicPath`檔案 的屬性告訴中間`dist`件注意 資料夾的更改:</span><span class="sxs-lookup"><span data-stu-id="d1f54-180">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

## <a name="hot-module-replacement"></a><span data-ttu-id="d1f54-181">熱模組更換</span><span class="sxs-lookup"><span data-stu-id="d1f54-181">Hot Module Replacement</span></span>

<span data-ttu-id="d1f54-182">將 Webpack[的熱模組取代](https://webpack.js.org/concepts/hot-module-replacement/)(HMR) 功能視為[Webpack 開發人員中間件](#webpack-dev-middleware)的演變。</span><span class="sxs-lookup"><span data-stu-id="d1f54-182">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="d1f54-183">HMR 引入了所有相同的優勢,但它在編譯更改后自動更新頁面內容,從而進一步簡化了開發工作流。</span><span class="sxs-lookup"><span data-stu-id="d1f54-183">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="d1f54-184">不要混淆這與瀏覽器的更新,這將干擾 SPA 的當前記憶體狀態和調試會話。</span><span class="sxs-lookup"><span data-stu-id="d1f54-184">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="d1f54-185">Webpack 開發人員中間件服務和瀏覽器之間有一個即時連結,這意味著更改被推送到瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="d1f54-185">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="hot-module-replacement-prerequisites"></a><span data-ttu-id="d1f54-186">熱模組更換先決條件</span><span class="sxs-lookup"><span data-stu-id="d1f54-186">Hot Module Replacement prerequisites</span></span>

<span data-ttu-id="d1f54-187">安裝[Webpack 熱中間件](https://www.npmjs.com/package/webpack-hot-middleware)npm 套件:</span><span class="sxs-lookup"><span data-stu-id="d1f54-187">Install the [webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

```console
npm i -D webpack-hot-middleware
```

### <a name="hot-module-replacement-configuration"></a><span data-ttu-id="d1f54-188">熱模組更換配置</span><span class="sxs-lookup"><span data-stu-id="d1f54-188">Hot Module Replacement configuration</span></span>

<span data-ttu-id="d1f54-189">HMR 元件必須註冊到`Configure`MVC 的 HTTP 請求導管中:</span><span class="sxs-lookup"><span data-stu-id="d1f54-189">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="d1f54-190">與[Webpack 開發人員中間件一](#webpack-dev-middleware)樣,`UseWebpackDevMiddleware``UseStaticFiles`擴充方法必須在擴展方法之前調用。</span><span class="sxs-lookup"><span data-stu-id="d1f54-190">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="d1f54-191">出於安全原因,僅當應用在開發模式下運行時才註冊中間件。</span><span class="sxs-lookup"><span data-stu-id="d1f54-191">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="d1f54-192">*Webpack.config.js*檔必須定義陣`plugins`列 ,即使它留空:</span><span class="sxs-lookup"><span data-stu-id="d1f54-192">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="d1f54-193">在瀏覽器中載入應用後,開發人員工具的主控台選項卡提供 HMR 啟動的確認:</span><span class="sxs-lookup"><span data-stu-id="d1f54-193">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![熱模組更換連接的消息](spa-services/_static/hmr_connected.png)

## <a name="routing-helpers"></a><span data-ttu-id="d1f54-195">路由說明器</span><span class="sxs-lookup"><span data-stu-id="d1f54-195">Routing helpers</span></span>

<span data-ttu-id="d1f54-196">在大多數基於核心的ASP.NET SA 中,除了伺服器端路由之外,通常還需要用戶端路由。</span><span class="sxs-lookup"><span data-stu-id="d1f54-196">In most ASP.NET Core-based SPAs, client-side routing is often desired in addition to server-side routing.</span></span> <span data-ttu-id="d1f54-197">SPA 和 MVC 路由系統可以獨立工作,不受干擾。</span><span class="sxs-lookup"><span data-stu-id="d1f54-197">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="d1f54-198">但是,有一個邊緣案例帶來了挑戰:識別 404 個 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="d1f54-198">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="d1f54-199">請考慮使用的無`/some/page`擴展路由的情況。</span><span class="sxs-lookup"><span data-stu-id="d1f54-199">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="d1f54-200">假設請求與伺服器端路由不模式匹配,但其模式確實與用戶端路由匹配。</span><span class="sxs-lookup"><span data-stu-id="d1f54-200">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="d1f54-201">現在考慮一個傳入的請求`/images/user-512.png`,它通常期望在伺服器上找到圖像檔。</span><span class="sxs-lookup"><span data-stu-id="d1f54-201">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="d1f54-202">如果請求的資源路徑與任何伺服器端路由或靜態檔不匹配,則用戶端應用程式不太可能處理它&mdash;,通常需要返回 404 HTTP 狀態代碼。</span><span class="sxs-lookup"><span data-stu-id="d1f54-202">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it&mdash;generally returning a 404 HTTP status code is desired.</span></span>

### <a name="routing-helpers-prerequisites"></a><span data-ttu-id="d1f54-203">路由說明器先決條件</span><span class="sxs-lookup"><span data-stu-id="d1f54-203">Routing helpers prerequisites</span></span>

<span data-ttu-id="d1f54-204">安裝用戶端路由 npm 包。</span><span class="sxs-lookup"><span data-stu-id="d1f54-204">Install the client-side routing npm package.</span></span> <span data-ttu-id="d1f54-205">以「角」為例:</span><span class="sxs-lookup"><span data-stu-id="d1f54-205">Using Angular as an example:</span></span>

```console
npm i -S @angular/router
```

### <a name="routing-helpers-configuration"></a><span data-ttu-id="d1f54-206">路由說明器設定</span><span class="sxs-lookup"><span data-stu-id="d1f54-206">Routing helpers configuration</span></span>

<span data-ttu-id="d1f54-207">方法中使用了名為`MapSpaFallbackRoute`的`Configure`擴充方法:</span><span class="sxs-lookup"><span data-stu-id="d1f54-207">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

<span data-ttu-id="d1f54-208">路由按其配置順序計算。</span><span class="sxs-lookup"><span data-stu-id="d1f54-208">Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="d1f54-209">因此,`default`前面的代碼示例中的路由首先用於模式匹配。</span><span class="sxs-lookup"><span data-stu-id="d1f54-209">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="d1f54-210">建立新專案</span><span class="sxs-lookup"><span data-stu-id="d1f54-210">Create a new project</span></span>

<span data-ttu-id="d1f54-211">JavaScript 服務提供預先配置的應用程式範本。</span><span class="sxs-lookup"><span data-stu-id="d1f54-211">JavaScript Services provide pre-configured application templates.</span></span> <span data-ttu-id="d1f54-212">SpaServices 與不同的框架和庫(如角、回應和雷杜)結合使用。</span><span class="sxs-lookup"><span data-stu-id="d1f54-212">SpaServices is used in these templates in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="d1f54-213">可以通過 .NET Core CLI 安裝這些範本,透過執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="d1f54-213">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```dotnetcli
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="d1f54-214">將顯示可用 SPA 樣本的清單:</span><span class="sxs-lookup"><span data-stu-id="d1f54-214">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="d1f54-215">範本</span><span class="sxs-lookup"><span data-stu-id="d1f54-215">Templates</span></span>                                 | <span data-ttu-id="d1f54-216">簡短名稱</span><span class="sxs-lookup"><span data-stu-id="d1f54-216">Short Name</span></span> | <span data-ttu-id="d1f54-217">Language</span><span class="sxs-lookup"><span data-stu-id="d1f54-217">Language</span></span> | <span data-ttu-id="d1f54-218">Tags</span><span class="sxs-lookup"><span data-stu-id="d1f54-218">Tags</span></span>        |
| ------------------------------------------| :--------: | :------: | :---------: |
| <span data-ttu-id="d1f54-219">MVC ASP.NET帶角的內核</span><span class="sxs-lookup"><span data-stu-id="d1f54-219">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="d1f54-220">angular</span><span class="sxs-lookup"><span data-stu-id="d1f54-220">angular</span></span>    | <span data-ttu-id="d1f54-221">[C#]</span><span class="sxs-lookup"><span data-stu-id="d1f54-221">[C#]</span></span>     | <span data-ttu-id="d1f54-222">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="d1f54-222">Web/MVC/SPA</span></span> |
| <span data-ttu-id="d1f54-223">MVC ASP.NET核心與 React.js</span><span class="sxs-lookup"><span data-stu-id="d1f54-223">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="d1f54-224">react</span><span class="sxs-lookup"><span data-stu-id="d1f54-224">react</span></span>      | <span data-ttu-id="d1f54-225">[C#]</span><span class="sxs-lookup"><span data-stu-id="d1f54-225">[C#]</span></span>     | <span data-ttu-id="d1f54-226">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="d1f54-226">Web/MVC/SPA</span></span> |
| <span data-ttu-id="d1f54-227">MVC ASP.NET核心與 React.js 和 Redux</span><span class="sxs-lookup"><span data-stu-id="d1f54-227">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="d1f54-228">reactredux</span><span class="sxs-lookup"><span data-stu-id="d1f54-228">reactredux</span></span> | <span data-ttu-id="d1f54-229">[C#]</span><span class="sxs-lookup"><span data-stu-id="d1f54-229">[C#]</span></span>     | <span data-ttu-id="d1f54-230">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="d1f54-230">Web/MVC/SPA</span></span> |

<span data-ttu-id="d1f54-231">要使用 SPA 樣本的一建立新項目,請在[dotnet 新](/dotnet/core/tools/dotnet-new)指令中包括樣本**的短名稱**。</span><span class="sxs-lookup"><span data-stu-id="d1f54-231">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="d1f54-232">以下指令建立角度應用程式,該應用程式為伺服器端設定了ASP.NET核心 MVC:</span><span class="sxs-lookup"><span data-stu-id="d1f54-232">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```dotnetcli
dotnet new angular
```

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="d1f54-233">設定執行時設定模式</span><span class="sxs-lookup"><span data-stu-id="d1f54-233">Set the runtime configuration mode</span></span>

<span data-ttu-id="d1f54-234">存在兩種主要執行時設定模式:</span><span class="sxs-lookup"><span data-stu-id="d1f54-234">Two primary runtime configuration modes exist:</span></span>

* <span data-ttu-id="d1f54-235">**發展**:</span><span class="sxs-lookup"><span data-stu-id="d1f54-235">**Development**:</span></span>
  * <span data-ttu-id="d1f54-236">包括源映射,以方便調試。</span><span class="sxs-lookup"><span data-stu-id="d1f54-236">Includes source maps to ease debugging.</span></span>
  * <span data-ttu-id="d1f54-237">無法優化用戶端代碼的性能。</span><span class="sxs-lookup"><span data-stu-id="d1f54-237">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="d1f54-238">**生產**:</span><span class="sxs-lookup"><span data-stu-id="d1f54-238">**Production**:</span></span>
  * <span data-ttu-id="d1f54-239">排除源映射。</span><span class="sxs-lookup"><span data-stu-id="d1f54-239">Excludes source maps.</span></span>
  * <span data-ttu-id="d1f54-240">通過捆綁和最小化優化客戶端代碼。</span><span class="sxs-lookup"><span data-stu-id="d1f54-240">Optimizes the client-side code via bundling and minification.</span></span>

<span data-ttu-id="d1f54-241">ASP.NET Core 使用名為`ASPNETCORE_ENVIRONMENT`的環境 變數來儲存配置模式。</span><span class="sxs-lookup"><span data-stu-id="d1f54-241">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="d1f54-242">有關詳細資訊,請參閱[設定環境](xref:fundamentals/environments#set-the-environment)。</span><span class="sxs-lookup"><span data-stu-id="d1f54-242">For more information, see [Set the environment](xref:fundamentals/environments#set-the-environment).</span></span>

### <a name="run-with-net-core-cli"></a><span data-ttu-id="d1f54-243">使用 .NET 核心 CLI 執行</span><span class="sxs-lookup"><span data-stu-id="d1f54-243">Run with .NET Core CLI</span></span>

<span data-ttu-id="d1f54-244">以專案根處執行以下指令來回復的 NuGet 和 npm 套件:</span><span class="sxs-lookup"><span data-stu-id="d1f54-244">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```dotnetcli
dotnet restore && npm i
```

<span data-ttu-id="d1f54-245">產生並執行應用程式:</span><span class="sxs-lookup"><span data-stu-id="d1f54-245">Build and run the application:</span></span>

```dotnetcli
dotnet run
```

<span data-ttu-id="d1f54-246">應用程式根據[運行時配置模式](#set-the-runtime-configuration-mode)在本地主機上啟動。</span><span class="sxs-lookup"><span data-stu-id="d1f54-246">The application starts on localhost according to the [runtime configuration mode](#set-the-runtime-configuration-mode).</span></span> <span data-ttu-id="d1f54-247">導航到`http://localhost:5000`瀏覽器將顯示著陸頁。</span><span class="sxs-lookup"><span data-stu-id="d1f54-247">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="run-with-visual-studio-2017"></a><span data-ttu-id="d1f54-248">與視覺工作室運行 2017</span><span class="sxs-lookup"><span data-stu-id="d1f54-248">Run with Visual Studio 2017</span></span>

<span data-ttu-id="d1f54-249">打開[dotnet 新](/dotnet/core/tools/dotnet-new)指令產生的 *.csproj*檔。</span><span class="sxs-lookup"><span data-stu-id="d1f54-249">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="d1f54-250">項目打開後,將自動還原所需的 NuGet 和 npm 包。</span><span class="sxs-lookup"><span data-stu-id="d1f54-250">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="d1f54-251">此還原過程可能需要幾分鐘時間,應用程式可在完成後運行。</span><span class="sxs-lookup"><span data-stu-id="d1f54-251">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="d1f54-252">按下綠色運行按鈕或按`Ctrl + F5`,瀏覽器將打開到應用程式的著陸頁。</span><span class="sxs-lookup"><span data-stu-id="d1f54-252">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="d1f54-253">應用程式根據[運行時配置模式](#set-the-runtime-configuration-mode)在本地主機上運行。</span><span class="sxs-lookup"><span data-stu-id="d1f54-253">The application runs on localhost according to the [runtime configuration mode](#set-the-runtime-configuration-mode).</span></span>

## <a name="test-the-app"></a><span data-ttu-id="d1f54-254">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="d1f54-254">Test the app</span></span>

<span data-ttu-id="d1f54-255">SpaServices 範本已預先配置為使用[Karma](https://karma-runner.github.io/1.0/index.html)和[茉莉花](https://jasmine.github.io/)運行客戶端測試。</span><span class="sxs-lookup"><span data-stu-id="d1f54-255">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="d1f54-256">茉莉花是JAVAScript的熱門單元測試框架,而Karma是這些測試的測試運行者。</span><span class="sxs-lookup"><span data-stu-id="d1f54-256">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="d1f54-257">Karma 設定為使用[Webpack 開發人員中間件](#webpack-dev-middleware),這樣開發人員不需要在每次進行更改時停止並運行測試。</span><span class="sxs-lookup"><span data-stu-id="d1f54-257">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="d1f54-258">無論是針對測試用例運行的代碼還是測試用例本身,測試都會自動運行。</span><span class="sxs-lookup"><span data-stu-id="d1f54-258">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="d1f54-259">以 Angular 應用程式為例,已在*計數器.元件.spec.ts*`CounterComponent`檔中為 提供兩個 Jasmine 測試用例:</span><span class="sxs-lookup"><span data-stu-id="d1f54-259">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="d1f54-260">在*用戶端應用*目錄中打開命令提示符。</span><span class="sxs-lookup"><span data-stu-id="d1f54-260">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="d1f54-261">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="d1f54-261">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="d1f54-262">該文本啟動 Karma 測試運行程式,該測試串流程式讀取*karma.conf.js*檔中定義的設定。</span><span class="sxs-lookup"><span data-stu-id="d1f54-262">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="d1f54-263">在其他設定中 *,karma.conf.js*識別要`files`透過新群組執行的測試檔:</span><span class="sxs-lookup"><span data-stu-id="d1f54-263">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

## <a name="publish-the-app"></a><span data-ttu-id="d1f54-264">發佈應用程式</span><span class="sxs-lookup"><span data-stu-id="d1f54-264">Publish the app</span></span>

<span data-ttu-id="d1f54-265">有關發佈到 Azure 的詳細資訊,請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/12474)。</span><span class="sxs-lookup"><span data-stu-id="d1f54-265">See [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/12474) for more information on publishing to Azure.</span></span>

<span data-ttu-id="d1f54-266">將生成的用戶端資產和已發佈的ASP.NET Core 專案組合到即部署的包中可能很麻煩。</span><span class="sxs-lookup"><span data-stu-id="d1f54-266">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="d1f54-267">謝天謝地,SpaServices 使用名為`RunWebpack`的 自定義 MSBuild 目標協調整個發佈過程:</span><span class="sxs-lookup"><span data-stu-id="d1f54-267">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="d1f54-268">MSBuild 目標具有以下職責:</span><span class="sxs-lookup"><span data-stu-id="d1f54-268">The MSBuild target has the following responsibilities:</span></span>

1. <span data-ttu-id="d1f54-269">還原 npm 包。</span><span class="sxs-lookup"><span data-stu-id="d1f54-269">Restore the npm packages.</span></span>
1. <span data-ttu-id="d1f54-270">創建第三方客戶端資產的生產級構建。</span><span class="sxs-lookup"><span data-stu-id="d1f54-270">Create a production-grade build of the third-party, client-side assets.</span></span>
1. <span data-ttu-id="d1f54-271">創建自定義客戶端資產的生產級生成。</span><span class="sxs-lookup"><span data-stu-id="d1f54-271">Create a production-grade build of the custom client-side assets.</span></span>
1. <span data-ttu-id="d1f54-272">將 Webpack 產生的資源複製到發佈資料夾。</span><span class="sxs-lookup"><span data-stu-id="d1f54-272">Copy the Webpack-generated assets to the publish folder.</span></span>

<span data-ttu-id="d1f54-273">執行時將呼叫 MSBuild 目標:</span><span class="sxs-lookup"><span data-stu-id="d1f54-273">The MSBuild target is invoked when running:</span></span>

```dotnetcli
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="d1f54-274">其他資源</span><span class="sxs-lookup"><span data-stu-id="d1f54-274">Additional resources</span></span>

* [<span data-ttu-id="d1f54-275">角文件</span><span class="sxs-lookup"><span data-stu-id="d1f54-275">Angular Docs</span></span>](https://angular.io/docs)
