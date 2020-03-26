---
title: Debug ASP.NET Core Blazor WebAssembly
author: guardrex
description: 瞭解如何 Blazor 應用程式進行 debug。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 5dbc900ab68682068a7f9e3ffdaabef89a0c7798
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306480"
---
# <a name="debug-aspnet-core-opno-locblazor-webassembly"></a><span data-ttu-id="6ee3b-103">Debug ASP.NET Core Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="6ee3b-103">Debug ASP.NET Core Blazor WebAssembly</span></span>

[<span data-ttu-id="6ee3b-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="6ee3b-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="6ee3b-105"> WebAssembly 應用程式可以使用 Chromium 式瀏覽器中的瀏覽器開發工具（邊緣/Chrome）進行調試。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-105"> WebAssembly apps can be debugged using the browser dev tools in Chromium-based browsers (Edge/Chrome).</span></span>  <span data-ttu-id="6ee3b-106">或者，您可以使用 Visual Studio 或 Visual Studio Code 來對應用程式進行 debug 錯。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-106">Alternatively you can debug your app using Visual Studio or Visual Studio Code.</span></span>

<span data-ttu-id="6ee3b-107">可用的案例包括：</span><span class="sxs-lookup"><span data-stu-id="6ee3b-107">Available scenarios include:</span></span>

* <span data-ttu-id="6ee3b-108">設定和移除中斷點。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-108">Set and remove breakpoints.</span></span>
* <span data-ttu-id="6ee3b-109">在 Visual Studio 和 Visual Studio Code （<kbd>F5</kbd>支援）中，以支援偵錯工具的方式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-109">Run the app with debugging support in Visual Studio and Visual Studio Code (<kbd>F5</kbd> support).</span></span>
* <span data-ttu-id="6ee3b-110">透過程式碼的單一步驟（<kbd>F10</kbd>）。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-110">Single-step (<kbd>F10</kbd>) through the code.</span></span>
* <span data-ttu-id="6ee3b-111">在瀏覽器中使用<kbd>F8</kbd>或 Visual Studio 或 Visual Studio Code 中的<kbd>F5</kbd>繼續執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-111">Resume code execution with <kbd>F8</kbd> in a browser or <kbd>F5</kbd> in Visual Studio or Visual Studio Code.</span></span>
* <span data-ttu-id="6ee3b-112">在 [*區域變數*] 顯示中，觀察本機變數的值。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-112">In the *Locals* display, observe the values of local variables.</span></span>
* <span data-ttu-id="6ee3b-113">查看呼叫堆疊，包括從 JavaScript 轉換成 .NET，以及從 .NET 到 JavaScript 的呼叫鏈。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-113">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="6ee3b-114">目前，您*無法*：</span><span class="sxs-lookup"><span data-stu-id="6ee3b-114">For now, you *can't*:</span></span>

* <span data-ttu-id="6ee3b-115">檢查陣列。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-115">Inspect arrays.</span></span>
* <span data-ttu-id="6ee3b-116">將滑鼠暫留以檢查成員。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-116">Hover to inspect members.</span></span>
* <span data-ttu-id="6ee3b-117">逐步執行 managed 程式碼的偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-117">Step debug into or out of managed code.</span></span>
* <span data-ttu-id="6ee3b-118">具有檢查實數值型別的完整支援。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-118">Have full support for inspecting value types.</span></span>
* <span data-ttu-id="6ee3b-119">中斷未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-119">Break on unhandled exceptions.</span></span>
* <span data-ttu-id="6ee3b-120">在應用程式啟動期間叫用中斷點。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-120">Hit breakpoints during app startup.</span></span>
* <span data-ttu-id="6ee3b-121">使用服務工作者來對應用程式進行 Debug。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-121">Debug an app with a service worker.</span></span>

<span data-ttu-id="6ee3b-122">我們將繼續改善即將發行的版本中的調試過程。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-122">We will continue to improve the debugging experience in upcoming releases.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6ee3b-123">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="6ee3b-123">Prerequisites</span></span>

<span data-ttu-id="6ee3b-124">調試需要下列其中一個瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="6ee3b-124">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="6ee3b-125">Microsoft Edge （版本80或更新版本）</span><span class="sxs-lookup"><span data-stu-id="6ee3b-125">Microsoft Edge (version 80 or later)</span></span>
* <span data-ttu-id="6ee3b-126">Google Chrome （版本70或更新版本）</span><span class="sxs-lookup"><span data-stu-id="6ee3b-126">Google Chrome (version 70 or later)</span></span>

## <a name="enable-debugging-for-visual-studio-and-visual-studio-code"></a><span data-ttu-id="6ee3b-127">啟用 Visual Studio 和 Visual Studio Code 的偵錯工具</span><span class="sxs-lookup"><span data-stu-id="6ee3b-127">Enable debugging for Visual Studio and Visual Studio Code</span></span>

<span data-ttu-id="6ee3b-128">使用 ASP.NET Core 3.2 Preview 3 或更新版本 Blazor WebAssembly 專案範本所建立的新專案，會自動啟用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-128">Debugging is enabled automatically for new projects that are created using the ASP.NET Core 3.2 Preview 3 or later Blazor WebAssembly project template.</span></span>

<span data-ttu-id="6ee3b-129">若要為現有的 Blazor WebAssembly 應用程式啟用偵錯工具，請更新啟始專案中的*launchsettings.json* ，以在每個啟動設定檔中包含下列 `inspectUri` 屬性：</span><span class="sxs-lookup"><span data-stu-id="6ee3b-129">To enable debugging for an existing Blazor WebAssembly app, update the *launchSettings.json* file in the startup project to include the following `inspectUri` property in each launch profile:</span></span>

```json
"inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}"
```

<span data-ttu-id="6ee3b-130">更新之後， *launchsettings.json*看起來應該類似下列範例：</span><span class="sxs-lookup"><span data-stu-id="6ee3b-130">Once updated, the *launchSettings.json* file should look similar to the following example:</span></span>

[!code-json[](debug/launchSettings.json?highlight=14,22)]

<span data-ttu-id="6ee3b-131">`inspectUri` 屬性：</span><span class="sxs-lookup"><span data-stu-id="6ee3b-131">The `inspectUri` property:</span></span>

* <span data-ttu-id="6ee3b-132">可讓 IDE 偵測應用程式是 Blazor WebAssembly 應用程式。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-132">Enables the IDE to detect that the app is a Blazor WebAssembly app.</span></span>
* <span data-ttu-id="6ee3b-133">指示腳本的偵錯工具，透過 Blazor的偵錯工具 proxy 連接到瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-133">Instructs the script debugging infrastructure to connect to the browser through Blazor's debugging proxy.</span></span>

## <a name="visual-studio"></a><span data-ttu-id="6ee3b-134">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6ee3b-134">Visual Studio</span></span>

<span data-ttu-id="6ee3b-135">若要在 Visual Studio 中進行 Blazor WebAssembly 應用程式的 debug：</span><span class="sxs-lookup"><span data-stu-id="6ee3b-135">To debug a Blazor WebAssembly app in Visual Studio:</span></span>

1. <span data-ttu-id="6ee3b-136">請確定您已安裝 Visual Studio 2019 16.6 （preview 2 或更新版本）的[最新預覽版本](https://visualstudio.com/preview)。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-136">Ensure you have [installed the latest preview release of Visual Studio 2019 16.6](https://visualstudio.com/preview) (Preview 2 or later).</span></span>
1. <span data-ttu-id="6ee3b-137">建立 Blazor WebAssembly 應用程式託管的新 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-137">Create a new ASP.NET Core hosted Blazor WebAssembly app.</span></span>
1. <span data-ttu-id="6ee3b-138">按<kbd>F5</kbd>以在偵錯工具中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-138">Press <kbd>F5</kbd> to run the app in the debugger.</span></span>
1. <span data-ttu-id="6ee3b-139">在 `IncrementCount` 方法中的 [ *razor* ] 中設定中斷點。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-139">Set a breakpoint in *Counter.razor* in the `IncrementCount` method.</span></span>
1. <span data-ttu-id="6ee3b-140">流覽至 [**計數器**] 索引標籤，然後選取按鈕以點擊中斷點：</span><span class="sxs-lookup"><span data-stu-id="6ee3b-140">Browse to the **Counter** tab and select the button to hit the breakpoint:</span></span>

   ![Debug 計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-counter.png)

1. <span data-ttu-id="6ee3b-142">查看 [區域變數] 視窗中 [`currentCount`] 欄位的值：</span><span class="sxs-lookup"><span data-stu-id="6ee3b-142">Check out the value of the `currentCount` field in the locals window:</span></span>

   ![View 區域變數](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-locals.png)

1. <span data-ttu-id="6ee3b-144">按<kbd>F5</kbd>繼續執行。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-144">Press <kbd>F5</kbd> to continue execution.</span></span>

<span data-ttu-id="6ee3b-145">在對您的 Blazor WebAssembly 應用程式進行調試時，您也可以對伺服器程式碼進行 debug：</span><span class="sxs-lookup"><span data-stu-id="6ee3b-145">While debugging your Blazor WebAssembly app, you can also debug your server code:</span></span>

1. <span data-ttu-id="6ee3b-146">在 `OnInitializedAsync`的*FetchData razor*頁面中設定中斷點。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-146">Set a breakpoint in the *FetchData.razor* page in `OnInitializedAsync`.</span></span>
1. <span data-ttu-id="6ee3b-147">在 `Get` 動作方法 的 `WeatherForecastController` 中設定中斷點。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-147">Set a breakpoint in the `WeatherForecastController` in the `Get` action method.</span></span>
1. <span data-ttu-id="6ee3b-148">流覽至 [**提取資料**] 索引標籤，在發出 HTTP 要求給伺服器之前，先叫用 `FetchData` 元件中的第一個中斷點：</span><span class="sxs-lookup"><span data-stu-id="6ee3b-148">Browse to the **Fetch Data** tab to hit the first breakpoint in the `FetchData` component just before it issues an HTTP request to the server:</span></span>

   ![Debug Fetch 資料](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-fetch-data.png)

1. <span data-ttu-id="6ee3b-150">按<kbd>F5</kbd>繼續執行，然後在 `WeatherForecastController`中的伺服器上按下中斷點：</span><span class="sxs-lookup"><span data-stu-id="6ee3b-150">Press <kbd>F5</kbd> to continue execution and then hit the breakpoint on the server in the `WeatherForecastController`:</span></span>

   ![Debug server](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-server.png)

1. <span data-ttu-id="6ee3b-152">再按一次<kbd>F5</kbd> ，讓執行繼續，並查看所轉譯的氣象預測資料表。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-152">Press <kbd>F5</kbd> again to let execution continue and see the weather forecast table rendered.</span></span>

## <a name="visual-studio-code"></a><span data-ttu-id="6ee3b-153">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6ee3b-153">Visual Studio Code</span></span>

<span data-ttu-id="6ee3b-154">若要在 Visual Studio Code 中進行 Blazor WebAssembly 應用程式的 debug：</span><span class="sxs-lookup"><span data-stu-id="6ee3b-154">To debug a Blazor WebAssembly app in Visual Studio Code:</span></span>
 
1. <span data-ttu-id="6ee3b-155">安裝[ C#擴充](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)功能和[JavaScript 偵錯工具（夜間）](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly)延伸模組，並將 `debug.javascript.usePreview` 設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-155">Install the [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the [JavaScript Debugger (Nightly)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) extension with `debug.javascript.usePreview` set to `true`.</span></span>

   ![延伸模組](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-extensions.png)

   ![JS 預覽偵錯工具](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-js-use-preview.png)

1. <span data-ttu-id="6ee3b-158">開啟已啟用偵測的現有 Blazor WebAssembly 應用程式。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-158">Open an existing Blazor WebAssembly app with debugging enabled.</span></span>

   * <span data-ttu-id="6ee3b-159">如果您收到下列通知，表示需要額外的設定才能啟用偵錯工具，請確認您已安裝正確的延伸模組，並已啟用 JavaScript 預覽偵測，然後重載視窗：</span><span class="sxs-lookup"><span data-stu-id="6ee3b-159">If you get the following notification that additional setup is required to enable debugging, confirm that you have the correct extensions installed and JavaScript preview debugging enabled and then reload the window:</span></span>

     ![其他安裝必要](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-additional-setup.png)

   * <span data-ttu-id="6ee3b-161">通知會提供將所需的資產新增至應用程式，以進行建立和調試。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-161">A notification offers to add the required assets to the app for building and debugging.</span></span> <span data-ttu-id="6ee3b-162">選取 **[是]** ：</span><span class="sxs-lookup"><span data-stu-id="6ee3b-162">Select **Yes**:</span></span>

     ![新增必要的資產](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-required-assets.png)

1. <span data-ttu-id="6ee3b-164">在偵錯工具中啟動應用程式是兩個步驟的進程：</span><span class="sxs-lookup"><span data-stu-id="6ee3b-164">Starting the app in the debugger is a two-step process:</span></span>

   <span data-ttu-id="6ee3b-165">1\.</span><span class="sxs-lookup"><span data-stu-id="6ee3b-165">1\.</span></span> <span data-ttu-id="6ee3b-166">**首先**，使用 **.net Core 啟動（Blazor 獨立）** 啟動設定來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-166">**First**, start the app using the **.NET Core Launch (Blazor Standalone)** launch configuration.</span></span>

   <span data-ttu-id="6ee3b-167">2\.</span><span class="sxs-lookup"><span data-stu-id="6ee3b-167">2\.</span></span> <span data-ttu-id="6ee3b-168">**啟動應用程式之後**，請使用 chrome 啟動設定（需要 chrome）**中的 .net Core Debug Blazor Web 元件**來啟動瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-168">**After the app has started**, start the browser using the **.NET Core Debug Blazor Web Assembly in Chrome** launch configuration (requires Chrome).</span></span> <span data-ttu-id="6ee3b-169">若要使用 Edge 而不是 Chrome，請將*vscode/啟動*中的啟動設定 `type` 從 `pwa-chrome` 變更為 `pwa-edge`。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-169">To use Edge instead of Chrome, change the `type` of the launch configuration in *.vscode/launch.json* from `pwa-chrome` to `pwa-edge`.</span></span>

1. <span data-ttu-id="6ee3b-170">在 `Counter` 元件的 `IncrementCount` 方法中設定中斷點，然後選取按鈕以叫用中斷點：</span><span class="sxs-lookup"><span data-stu-id="6ee3b-170">Set a breakpoint in the `IncrementCount` method in the `Counter` component and then select the button to hit the breakpoint:</span></span>

   ![VS Code 中的 Debug 計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-debug-counter.png)

## <a name="debug-in-the-browser"></a><span data-ttu-id="6ee3b-172">瀏覽器中的 Debug</span><span class="sxs-lookup"><span data-stu-id="6ee3b-172">Debug in the browser</span></span>

1. <span data-ttu-id="6ee3b-173">在開發環境中執行應用程式的 Debug 組建。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-173">Run a Debug build of the app in the Development environment.</span></span>

1. <span data-ttu-id="6ee3b-174">按<kbd>Shift</kbd>+<kbd>Alt</kbd>+<kbd>D</kbd>。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-174">Press <kbd>Shift</kbd>+<kbd>Alt</kbd>+<kbd>D</kbd>.</span></span>

1. <span data-ttu-id="6ee3b-175">瀏覽器必須在啟用遠端偵錯功能的情況下執行。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-175">The browser must be run with remote debugging enabled.</span></span> <span data-ttu-id="6ee3b-176">如果已停用遠端偵錯程式，就會產生 [找**不到可調試的瀏覽器]** 索引標籤錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-176">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated.</span></span> <span data-ttu-id="6ee3b-177">錯誤頁面包含在開啟偵錯工具的情況之下執行瀏覽器的指示，讓 Blazor 的偵錯工具 proxy 可以連接到應用程式。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-177">The error page contains instructions for running the browser with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="6ee3b-178">*關閉所有瀏覽器實例*，然後依照指示重新開機瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-178">*Close all browser instances* and restart the browser as instructed.</span></span>

<span data-ttu-id="6ee3b-179">當瀏覽器在啟用遠端偵錯程式的情況下執行時，[偵錯工具] 鍵盤快速鍵會開啟新的 [偵錯工具]經過一段時間之後，[**來源**] 索引標籤會顯示應用程式中的 .net 元件清單。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-179">Once the browser is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="6ee3b-180">展開每個元件，並找出可用來進行偵錯工具的 *.cs*/ *. razor*原始程式檔。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-180">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="6ee3b-181">設定中斷點、切換回應用程式的索引標籤，並在程式碼執行時叫用中斷點。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-181">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="6ee3b-182">叫用中斷點之後，以單一步驟（<kbd>F10</kbd>），透過程式碼或繼續（<kbd>F8</kbd>）程式碼執行正常。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-182">After a breakpoint is hit, single-step (<kbd>F10</kbd>) through the code or resume (<kbd>F8</kbd>) code execution normally.</span></span>

Blazor<span data-ttu-id="6ee3b-183"> 提供可執行[Chrome DevTools 通訊協定](https://chromedevtools.github.io/devtools-protocol/)，並以擴充通訊協定的偵錯工具 proxy。NET 特定資訊。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-183"> provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="6ee3b-184">當您按下 [調試鍵盤快速鍵] 時，Blazor 會指向位於 proxy 的 Chrome DevTools。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-184">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="6ee3b-185">Proxy 會連線到您想要進行調試的瀏覽器視窗（因此需要啟用遠端偵錯）。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-185">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="6ee3b-186">瀏覽器來源對應</span><span class="sxs-lookup"><span data-stu-id="6ee3b-186">Browser source maps</span></span>

<span data-ttu-id="6ee3b-187">瀏覽器來源對應可讓瀏覽器將已編譯的檔案對應回原始來源檔案，而且通常會用於用戶端的偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-187">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="6ee3b-188">不過，Blazor 目前不會C#直接對應至 JAVASCRIPT/WASM。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-188">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="6ee3b-189">相反地，Blazor 會在瀏覽器中執行 IL 轉譯，因此來源對應不相關。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-189">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="6ee3b-190">疑難排解</span><span class="sxs-lookup"><span data-stu-id="6ee3b-190">Troubleshoot</span></span>

<span data-ttu-id="6ee3b-191">如果您遇到錯誤，下列秘訣可能會有説明：</span><span class="sxs-lookup"><span data-stu-id="6ee3b-191">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="6ee3b-192">在 [**偵錯工具**] 索引標籤中，開啟瀏覽器中的開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-192">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="6ee3b-193">在主控台中，執行 `localStorage.clear()` 以移除任何中斷點。</span><span class="sxs-lookup"><span data-stu-id="6ee3b-193">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
