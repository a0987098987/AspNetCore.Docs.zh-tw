---
title: 除錯ASP.NET核心BlazorWeb 組裝
author: guardrex
description: 瞭解如何調試Blazor應用。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 8b63444ba5c8cd45e64e722c8978ba4e6d90af36
ms.sourcegitcommit: 77c046331f3d633d7cc247ba77e58b89e254f487
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81488744"
---
# <a name="debug-aspnet-core-opno-locblazor-webassembly"></a><span data-ttu-id="339e0-103">除錯ASP.NET核心BlazorWeb 組裝</span><span class="sxs-lookup"><span data-stu-id="339e0-103">Debug ASP.NET Core Blazor WebAssembly</span></span>

[<span data-ttu-id="339e0-104">丹尼爾·羅斯</span><span class="sxs-lookup"><span data-stu-id="339e0-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="339e0-105">可以使用基於鉻瀏覽器(邊緣/Chrome)中的瀏覽器開發工具調試 WebAssembly 應用程式。</span><span class="sxs-lookup"><span data-stu-id="339e0-105"> WebAssembly apps can be debugged using the browser dev tools in Chromium-based browsers (Edge/Chrome).</span></span>  <span data-ttu-id="339e0-106">或者,您可以使用可視化工作室或可視化工作室代碼調試應用。</span><span class="sxs-lookup"><span data-stu-id="339e0-106">Alternatively you can debug your app using Visual Studio or Visual Studio Code.</span></span>

<span data-ttu-id="339e0-107">可用機制:</span><span class="sxs-lookup"><span data-stu-id="339e0-107">Available scenarios include:</span></span>

* <span data-ttu-id="339e0-108">設置並刪除斷點。</span><span class="sxs-lookup"><span data-stu-id="339e0-108">Set and remove breakpoints.</span></span>
* <span data-ttu-id="339e0-109">在可視化工作室和可視化工作室代碼<kbd>(F5</kbd>支援)中使用調試支援運行應用程式。</span><span class="sxs-lookup"><span data-stu-id="339e0-109">Run the app with debugging support in Visual Studio and Visual Studio Code (<kbd>F5</kbd> support).</span></span>
* <span data-ttu-id="339e0-110">通過代碼執行單步執行(<kbd>F10</kbd>)。</span><span class="sxs-lookup"><span data-stu-id="339e0-110">Single-step (<kbd>F10</kbd>) through the code.</span></span>
* <span data-ttu-id="339e0-111">在瀏覽器中使用<kbd>F8</kbd>或可視化工作室代碼中的<kbd>F5</kbd>恢復代碼執行。</span><span class="sxs-lookup"><span data-stu-id="339e0-111">Resume code execution with <kbd>F8</kbd> in a browser or <kbd>F5</kbd> in Visual Studio or Visual Studio Code.</span></span>
* <span data-ttu-id="339e0-112">在 *「局部變數」* 顯示中,觀察局部變數的值。</span><span class="sxs-lookup"><span data-stu-id="339e0-112">In the *Locals* display, observe the values of local variables.</span></span>
* <span data-ttu-id="339e0-113">請參閱調用堆疊,包括從 JAVAScript 到 .NET 並從 .NET 到 JavaScript 的調用鏈。</span><span class="sxs-lookup"><span data-stu-id="339e0-113">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="339e0-114">現在,*你不能*:</span><span class="sxs-lookup"><span data-stu-id="339e0-114">For now, you *can't*:</span></span>

* <span data-ttu-id="339e0-115">檢查陣列。</span><span class="sxs-lookup"><span data-stu-id="339e0-115">Inspect arrays.</span></span>
* <span data-ttu-id="339e0-116">懸停以檢查成員。</span><span class="sxs-lookup"><span data-stu-id="339e0-116">Hover to inspect members.</span></span>
* <span data-ttu-id="339e0-117">步進調試到或從託管代碼中輸入。</span><span class="sxs-lookup"><span data-stu-id="339e0-117">Step debug into or out of managed code.</span></span>
* <span data-ttu-id="339e0-118">完全支援檢查值類型。</span><span class="sxs-lookup"><span data-stu-id="339e0-118">Have full support for inspecting value types.</span></span>
* <span data-ttu-id="339e0-119">中斷未處理的異常。</span><span class="sxs-lookup"><span data-stu-id="339e0-119">Break on unhandled exceptions.</span></span>
* <span data-ttu-id="339e0-120">在應用啟動期間命中斷點。</span><span class="sxs-lookup"><span data-stu-id="339e0-120">Hit breakpoints during app startup.</span></span>
* <span data-ttu-id="339e0-121">使用服務輔助角色調試應用。</span><span class="sxs-lookup"><span data-stu-id="339e0-121">Debug an app with a service worker.</span></span>

<span data-ttu-id="339e0-122">我們將繼續改進即將發佈的調試體驗。</span><span class="sxs-lookup"><span data-stu-id="339e0-122">We will continue to improve the debugging experience in upcoming releases.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="339e0-123">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="339e0-123">Prerequisites</span></span>

<span data-ttu-id="339e0-124">除錯需要以下任瀏覽器:</span><span class="sxs-lookup"><span data-stu-id="339e0-124">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="339e0-125">微軟邊緣(版本80或更高版本)</span><span class="sxs-lookup"><span data-stu-id="339e0-125">Microsoft Edge (version 80 or later)</span></span>
* <span data-ttu-id="339e0-126">谷歌Chrome(版本70或更高版本)</span><span class="sxs-lookup"><span data-stu-id="339e0-126">Google Chrome (version 70 or later)</span></span>

## <a name="enable-debugging-for-visual-studio-and-visual-studio-code"></a><span data-ttu-id="339e0-127">啟用視覺化工作室和視覺化工作室代碼的除錯</span><span class="sxs-lookup"><span data-stu-id="339e0-127">Enable debugging for Visual Studio and Visual Studio Code</span></span>

<span data-ttu-id="339e0-128">對於使用 ASP.NET酷 3.2 預覽 3Blazor或更高版本的 WebAssembly 專案樣本([目前版本為 3.2 預覽 4)](xref:blazor/get-started)創建的新專案,將自動啟用調試。</span><span class="sxs-lookup"><span data-stu-id="339e0-128">Debugging is enabled automatically for new projects that are created using the ASP.NET Core 3.2 Preview 3 or later Blazor WebAssembly project template ([current release is 3.2 Preview 4](xref:blazor/get-started)).</span></span>

<span data-ttu-id="339e0-129">要開啟現有BlazorWebAssembly 應用的除錯,請在啟動項目中更新*啟動設定.json*檔,以便`inspectUri`在每個啟動 設定檔中包括以下屬性:</span><span class="sxs-lookup"><span data-stu-id="339e0-129">To enable debugging for an existing Blazor WebAssembly app, update the *launchSettings.json* file in the startup project to include the following `inspectUri` property in each launch profile:</span></span>

```json
"inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}"
```

<span data-ttu-id="339e0-130">更新後,*啟動設定.json*檔應類似於以下範例:</span><span class="sxs-lookup"><span data-stu-id="339e0-130">Once updated, the *launchSettings.json* file should look similar to the following example:</span></span>

[!code-json[](debug/launchSettings.json?highlight=14,22)]

<span data-ttu-id="339e0-131">屬性`inspectUri`:</span><span class="sxs-lookup"><span data-stu-id="339e0-131">The `inspectUri` property:</span></span>

* <span data-ttu-id="339e0-132">使 IDE 能夠檢測Blazor應用是 Web 裝配應用。</span><span class="sxs-lookup"><span data-stu-id="339e0-132">Enables the IDE to detect that the app is a Blazor WebAssembly app.</span></span>
* <span data-ttu-id="339e0-133">指示腳本調試基礎結構通過Blazor調試代理連接到瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="339e0-133">Instructs the script debugging infrastructure to connect to the browser through Blazor's debugging proxy.</span></span>

## <a name="visual-studio"></a><span data-ttu-id="339e0-134">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="339e0-134">Visual Studio</span></span>

<span data-ttu-id="339e0-135">要在Blazor可視化工作室中調試 Web 組裝應用,可以執行:</span><span class="sxs-lookup"><span data-stu-id="339e0-135">To debug a Blazor WebAssembly app in Visual Studio:</span></span>

1. <span data-ttu-id="339e0-136">確保您[已安裝 Visual Studio 2019 16.6(](https://visualstudio.com/preview)預覽版 2 或更高版本)的最新預覽版本。</span><span class="sxs-lookup"><span data-stu-id="339e0-136">Ensure you have [installed the latest preview release of Visual Studio 2019 16.6](https://visualstudio.com/preview) (Preview 2 or later).</span></span>
1. <span data-ttu-id="339e0-137">創建新ASP.NET核心託管BlazorWeb 組裝應用。</span><span class="sxs-lookup"><span data-stu-id="339e0-137">Create a new ASP.NET Core hosted Blazor WebAssembly app.</span></span>
1. <span data-ttu-id="339e0-138">按<kbd>F5</kbd>在調試器中運行應用。</span><span class="sxs-lookup"><span data-stu-id="339e0-138">Press <kbd>F5</kbd> to run the app in the debugger.</span></span>
1. <span data-ttu-id="339e0-139">在`IncrementCount`方法中的*Counter.razor*中設定斷點。</span><span class="sxs-lookup"><span data-stu-id="339e0-139">Set a breakpoint in *Counter.razor* in the `IncrementCount` method.</span></span>
1. <span data-ttu-id="339e0-140">瀏覽到 **「計數器」** 選項卡並選擇按鈕以命中斷點:</span><span class="sxs-lookup"><span data-stu-id="339e0-140">Browse to the **Counter** tab and select the button to hit the breakpoint:</span></span>

   ![除錯計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-counter.png)

1. <span data-ttu-id="339e0-142">在局部變數視窗中檢視`currentCount`欄位的值:</span><span class="sxs-lookup"><span data-stu-id="339e0-142">Check out the value of the `currentCount` field in the locals window:</span></span>

   ![檢視本地人](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-locals.png)

1. <span data-ttu-id="339e0-144">按<kbd>F5</kbd>繼續執行。</span><span class="sxs-lookup"><span data-stu-id="339e0-144">Press <kbd>F5</kbd> to continue execution.</span></span>

<span data-ttu-id="339e0-145">除錯BlazorWebAssembly 應用程式時,您還可以除錯伺服器碼:</span><span class="sxs-lookup"><span data-stu-id="339e0-145">While debugging your Blazor WebAssembly app, you can also debug your server code:</span></span>

1. <span data-ttu-id="339e0-146">在中的*FetchData.razor*`OnInitializedAsync`頁中 設置斷點。</span><span class="sxs-lookup"><span data-stu-id="339e0-146">Set a breakpoint in the *FetchData.razor* page in `OnInitializedAsync`.</span></span>
1. <span data-ttu-id="339e0-147">在`WeatherForecastController``Get`操作方法中設置斷點。</span><span class="sxs-lookup"><span data-stu-id="339e0-147">Set a breakpoint in the `WeatherForecastController` in the `Get` action method.</span></span>
1. <span data-ttu-id="339e0-148">瀏覽到 **「取得資料**」選項卡,在向`FetchData`伺服器發出 HTTP 請求之前,點擊元件中的第一個斷點:</span><span class="sxs-lookup"><span data-stu-id="339e0-148">Browse to the **Fetch Data** tab to hit the first breakpoint in the `FetchData` component just before it issues an HTTP request to the server:</span></span>

   ![除錯提取資料](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-fetch-data.png)

1. <span data-ttu-id="339e0-150">按<kbd>F5</kbd>繼續執行,然後`WeatherForecastController`在 中 命中伺服器上的斷點:</span><span class="sxs-lookup"><span data-stu-id="339e0-150">Press <kbd>F5</kbd> to continue execution and then hit the breakpoint on the server in the `WeatherForecastController`:</span></span>

   ![除錯伺服器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-server.png)

1. <span data-ttu-id="339e0-152">再次按<kbd>F5</kbd>以允許執行繼續,並查看已呈現的天氣預報表。</span><span class="sxs-lookup"><span data-stu-id="339e0-152">Press <kbd>F5</kbd> again to let execution continue and see the weather forecast table rendered.</span></span>

## <a name="visual-studio-code"></a><span data-ttu-id="339e0-153">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="339e0-153">Visual Studio Code</span></span>

<span data-ttu-id="339e0-154">要在可視化Blazor工作室代碼中調試 Web 組裝應用,請執行:</span><span class="sxs-lookup"><span data-stu-id="339e0-154">To debug a Blazor WebAssembly app in Visual Studio Code:</span></span>
 
1. <span data-ttu-id="339e0-155">安裝[C# 延伸](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)與[JavaScript 除錯器(夜間)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly)延伸`debug.javascript.usePreview`, 設定為`true`。</span><span class="sxs-lookup"><span data-stu-id="339e0-155">Install the [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the [JavaScript Debugger (Nightly)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) extension with `debug.javascript.usePreview` set to `true`.</span></span>

   ![延伸模組](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-extensions.png)

   ![JS 預覽除錯器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-js-use-preview.png)

1. <span data-ttu-id="339e0-158">打開啟用調試Blazor的現有 WebAssembly 應用。</span><span class="sxs-lookup"><span data-stu-id="339e0-158">Open an existing Blazor WebAssembly app with debugging enabled.</span></span>

   * <span data-ttu-id="339e0-159">如果收到以下通知,表示啟用除錯需要額外的設定,請確認已安裝正確的擴展並啟用 JavaScript 預覽除錯,然後重新載入視窗:</span><span class="sxs-lookup"><span data-stu-id="339e0-159">If you get the following notification that additional setup is required to enable debugging, confirm that you have the correct extensions installed and JavaScript preview debugging enabled and then reload the window:</span></span>

     ![附加設定](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-additional-setup.png)

   * <span data-ttu-id="339e0-161">通知提供將所需資產添加到應用以生成和調試。</span><span class="sxs-lookup"><span data-stu-id="339e0-161">A notification offers to add the required assets to the app for building and debugging.</span></span> <span data-ttu-id="339e0-162">選擇 **"是**"</span><span class="sxs-lookup"><span data-stu-id="339e0-162">Select **Yes**:</span></span>

     ![新增所需資產](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-required-assets.png)

1. <span data-ttu-id="339e0-164">在除錯器中啟動應用是一個兩步過程:</span><span class="sxs-lookup"><span data-stu-id="339e0-164">Starting the app in the debugger is a two-step process:</span></span>

   <span data-ttu-id="339e0-165">1\.</span><span class="sxs-lookup"><span data-stu-id="339e0-165">1\.</span></span> <span data-ttu-id="339e0-166">**首先**,使用 **.NETBlazor核心啟動(獨立)啟動**配置啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="339e0-166">**First**, start the app using the **.NET Core Launch (Blazor Standalone)** launch configuration.</span></span>

   <span data-ttu-id="339e0-167">2\.</span><span class="sxs-lookup"><span data-stu-id="339e0-167">2\.</span></span> <span data-ttu-id="339e0-168">**應用啟動後**,在 Chrome 啟動配置**中使用 .NETBlazor核心調試網路程式集**啟動瀏覽器(需要 Chrome)。</span><span class="sxs-lookup"><span data-stu-id="339e0-168">**After the app has started**, start the browser using the **.NET Core Debug Blazor Web Assembly in Chrome** launch configuration (requires Chrome).</span></span> <span data-ttu-id="339e0-169">要使用 Edge 而不是`type`Chrome, 請將 *.vscode/launch.json*中的啟動`pwa-chrome`設定`pwa-msedge`從 更改為 。</span><span class="sxs-lookup"><span data-stu-id="339e0-169">To use Edge instead of Chrome, change the `type` of the launch configuration in *.vscode/launch.json* from `pwa-chrome` to `pwa-msedge`.</span></span>

1. <span data-ttu-id="339e0-170">在`IncrementCount``Counter`元件中的方法中設定斷點,然後選擇按鈕以命中斷點:</span><span class="sxs-lookup"><span data-stu-id="339e0-170">Set a breakpoint in the `IncrementCount` method in the `Counter` component and then select the button to hit the breakpoint:</span></span>

   ![VS 代碼中的除錯計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-debug-counter.png)

## <a name="debug-in-the-browser"></a><span data-ttu-id="339e0-172">在瀏覽器中除錯</span><span class="sxs-lookup"><span data-stu-id="339e0-172">Debug in the browser</span></span>

1. <span data-ttu-id="339e0-173">在開發環境中運行應用的調試生成。</span><span class="sxs-lookup"><span data-stu-id="339e0-173">Run a Debug build of the app in the Development environment.</span></span>

1. <span data-ttu-id="339e0-174">按<kbd>移位</kbd>+<kbd>Alt</kbd>+<kbd>D</kbd>.</span><span class="sxs-lookup"><span data-stu-id="339e0-174">Press <kbd>Shift</kbd>+<kbd>Alt</kbd>+<kbd>D</kbd>.</span></span>

1. <span data-ttu-id="339e0-175">必須在啟用遠端調試後運行瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="339e0-175">The browser must be run with remote debugging enabled.</span></span> <span data-ttu-id="339e0-176">如果關閉遠端除錯,將產生 **「無法搜尋可除錯的瀏覽器選項卡**錯誤」 頁。</span><span class="sxs-lookup"><span data-stu-id="339e0-176">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated.</span></span> <span data-ttu-id="339e0-177">錯誤頁包含在打開除錯埠時執行瀏覽器的說明,以便Blazor除錯代理可以連接到應用。</span><span class="sxs-lookup"><span data-stu-id="339e0-177">The error page contains instructions for running the browser with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="339e0-178">*關閉所有瀏覽器實例*,並按照指示重新啟動瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="339e0-178">*Close all browser instances* and restart the browser as instructed.</span></span>

<span data-ttu-id="339e0-179">啟用遠端除錯後,瀏覽器運行後,除錯鍵盤快捷方式將打開一個新的除錯器選項卡。片刻後,「**源**」選項卡顯示應用中 .NET 程式集的清單。</span><span class="sxs-lookup"><span data-stu-id="339e0-179">Once the browser is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="339e0-180">展開每個程式集並找到可用於除錯的 *.cs.razor*/*.razor*源檔。</span><span class="sxs-lookup"><span data-stu-id="339e0-180">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="339e0-181">設置斷點,切換回應用的選項卡,並在代碼執行時命中斷點。</span><span class="sxs-lookup"><span data-stu-id="339e0-181">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="339e0-182">命中斷點後,透過代碼執行單步(<kbd>F10</kbd>) 或正常恢復 (<kbd>F8</kbd>) 程式碼執行.</span><span class="sxs-lookup"><span data-stu-id="339e0-182">After a breakpoint is hit, single-step (<kbd>F10</kbd>) through the code or resume (<kbd>F8</kbd>) code execution normally.</span></span>

Blazor<span data-ttu-id="339e0-183">提供了一個調試代理,用於實現[Chrome 開發人員工具協定](https://chromedevtools.github.io/devtools-protocol/),並使用 來增強協定。特定於 NET 的資訊。</span><span class="sxs-lookup"><span data-stu-id="339e0-183"> provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="339e0-184">按下調試鍵盤快捷鍵時,Blazor將 Chrome 開發人員工具指向代理。</span><span class="sxs-lookup"><span data-stu-id="339e0-184">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="339e0-185">代理連接到您要除錯的瀏覽器視窗(因此需要啟用遠端除錯)。</span><span class="sxs-lookup"><span data-stu-id="339e0-185">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="339e0-186">瀏覽器來源映射</span><span class="sxs-lookup"><span data-stu-id="339e0-186">Browser source maps</span></span>

<span data-ttu-id="339e0-187">瀏覽器源映射允許瀏覽器將編譯的檔映射回其原始源檔,並通常用於用戶端調試。</span><span class="sxs-lookup"><span data-stu-id="339e0-187">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="339e0-188">但是,Blazor當前不會將 C# 直接映射到 JAVAScript/WASM。</span><span class="sxs-lookup"><span data-stu-id="339e0-188">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="339e0-189">相反,ILBlazor在瀏覽器中解釋,因此源映射不相關。</span><span class="sxs-lookup"><span data-stu-id="339e0-189">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="339e0-190">疑難排解</span><span class="sxs-lookup"><span data-stu-id="339e0-190">Troubleshoot</span></span>

<span data-ttu-id="339e0-191">如果您遇到錯誤,以下提示可能會有所説明:</span><span class="sxs-lookup"><span data-stu-id="339e0-191">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="339e0-192">在 **「除錯器」** 選項卡中,在瀏覽器中打開開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="339e0-192">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="339e0-193">在控制台中,執行`localStorage.clear()`以刪除任何斷點。</span><span class="sxs-lookup"><span data-stu-id="339e0-193">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
