---
<span data-ttu-id="72c0f-101">標題： ' Debug ASP.NET Core Blazor WebAssembly ' 作者：描述：「瞭解如何調試 Blazor 程式」。</span><span class="sxs-lookup"><span data-stu-id="72c0f-101">title: 'Debug ASP.NET Core Blazor WebAssembly' author: description: 'Learn how to debug Blazor apps.'</span></span>
<span data-ttu-id="72c0f-102">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="72c0f-102">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="72c0f-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="72c0f-103">'Blazor'</span></span>
- <span data-ttu-id="72c0f-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="72c0f-104">'Identity'</span></span>
- <span data-ttu-id="72c0f-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="72c0f-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="72c0f-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="72c0f-106">'Razor'</span></span>
- <span data-ttu-id="72c0f-107">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="72c0f-107">'SignalR' uid:</span></span> 

---
# <a name="debug-aspnet-core-blazor-webassembly"></a><span data-ttu-id="72c0f-108">Debug ASP.NET Core Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="72c0f-108">Debug ASP.NET Core Blazor WebAssembly</span></span>

[<span data-ttu-id="72c0f-109">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="72c0f-109">Daniel Roth</span></span>](https://github.com/danroth27)

Blazor<span data-ttu-id="72c0f-110">WebAssembly 應用程式可以使用 Chromium 式瀏覽器中的瀏覽器開發工具（邊緣/Chrome）進行調試。</span><span class="sxs-lookup"><span data-stu-id="72c0f-110"> WebAssembly apps can be debugged using the browser dev tools in Chromium-based browsers (Edge/Chrome).</span></span>  <span data-ttu-id="72c0f-111">或者，您可以使用 Visual Studio 或 Visual Studio Code 來對應用程式進行 debug 錯。</span><span class="sxs-lookup"><span data-stu-id="72c0f-111">Alternatively you can debug your app using Visual Studio or Visual Studio Code.</span></span>

<span data-ttu-id="72c0f-112">可用的案例包括：</span><span class="sxs-lookup"><span data-stu-id="72c0f-112">Available scenarios include:</span></span>

* <span data-ttu-id="72c0f-113">設定和移除中斷點。</span><span class="sxs-lookup"><span data-stu-id="72c0f-113">Set and remove breakpoints.</span></span>
* <span data-ttu-id="72c0f-114">在 Visual Studio 和 Visual Studio Code （<kbd>F5</kbd>支援）中，以支援偵錯工具的方式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="72c0f-114">Run the app with debugging support in Visual Studio and Visual Studio Code (<kbd>F5</kbd> support).</span></span>
* <span data-ttu-id="72c0f-115">透過程式碼的單一步驟（<kbd>F10</kbd>）。</span><span class="sxs-lookup"><span data-stu-id="72c0f-115">Single-step (<kbd>F10</kbd>) through the code.</span></span>
* <span data-ttu-id="72c0f-116">在瀏覽器中使用<kbd>F8</kbd>或 Visual Studio 或 Visual Studio Code 中的<kbd>F5</kbd>繼續執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="72c0f-116">Resume code execution with <kbd>F8</kbd> in a browser or <kbd>F5</kbd> in Visual Studio or Visual Studio Code.</span></span>
* <span data-ttu-id="72c0f-117">在 [*區域變數*] 顯示中，觀察本機變數的值。</span><span class="sxs-lookup"><span data-stu-id="72c0f-117">In the *Locals* display, observe the values of local variables.</span></span>
* <span data-ttu-id="72c0f-118">查看呼叫堆疊，包括從 JavaScript 轉換成 .NET，以及從 .NET 到 JavaScript 的呼叫鏈。</span><span class="sxs-lookup"><span data-stu-id="72c0f-118">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="72c0f-119">目前，您*無法*：</span><span class="sxs-lookup"><span data-stu-id="72c0f-119">For now, you *can't*:</span></span>

* <span data-ttu-id="72c0f-120">檢查陣列。</span><span class="sxs-lookup"><span data-stu-id="72c0f-120">Inspect arrays.</span></span>
* <span data-ttu-id="72c0f-121">將滑鼠暫留以檢查成員。</span><span class="sxs-lookup"><span data-stu-id="72c0f-121">Hover to inspect members.</span></span>
* <span data-ttu-id="72c0f-122">逐步執行 managed 程式碼的偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="72c0f-122">Step debug into or out of managed code.</span></span>
* <span data-ttu-id="72c0f-123">具有檢查實數值型別的完整支援。</span><span class="sxs-lookup"><span data-stu-id="72c0f-123">Have full support for inspecting value types.</span></span>
* <span data-ttu-id="72c0f-124">中斷未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="72c0f-124">Break on unhandled exceptions.</span></span>
* <span data-ttu-id="72c0f-125">在應用程式啟動期間叫用中斷點。</span><span class="sxs-lookup"><span data-stu-id="72c0f-125">Hit breakpoints during app startup.</span></span>
* <span data-ttu-id="72c0f-126">使用服務工作者來對應用程式進行 Debug。</span><span class="sxs-lookup"><span data-stu-id="72c0f-126">Debug an app with a service worker.</span></span>

<span data-ttu-id="72c0f-127">我們將繼續改善即將發行的版本中的調試過程。</span><span class="sxs-lookup"><span data-stu-id="72c0f-127">We will continue to improve the debugging experience in upcoming releases.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="72c0f-128">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="72c0f-128">Prerequisites</span></span>

<span data-ttu-id="72c0f-129">調試需要下列其中一個瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="72c0f-129">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="72c0f-130">Microsoft Edge （版本80或更新版本）</span><span class="sxs-lookup"><span data-stu-id="72c0f-130">Microsoft Edge (version 80 or later)</span></span>
* <span data-ttu-id="72c0f-131">Google Chrome （版本70或更新版本）</span><span class="sxs-lookup"><span data-stu-id="72c0f-131">Google Chrome (version 70 or later)</span></span>

## <a name="enable-debugging-for-visual-studio-and-visual-studio-code"></a><span data-ttu-id="72c0f-132">啟用 Visual Studio 和 Visual Studio Code 的偵錯工具</span><span class="sxs-lookup"><span data-stu-id="72c0f-132">Enable debugging for Visual Studio and Visual Studio Code</span></span>

<span data-ttu-id="72c0f-133">若要啟用現有 Blazor WebAssembly 應用程式的偵測，請更新啟始專案中的*launchsettings.json* ，以 `inspectUri` 在每個啟動設定檔中包含下列屬性：</span><span class="sxs-lookup"><span data-stu-id="72c0f-133">To enable debugging for an existing Blazor WebAssembly app, update the *launchSettings.json* file in the startup project to include the following `inspectUri` property in each launch profile:</span></span>

```json
"inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}"
```

<span data-ttu-id="72c0f-134">更新之後， *launchsettings.json*看起來應該類似下列範例：</span><span class="sxs-lookup"><span data-stu-id="72c0f-134">Once updated, the *launchSettings.json* file should look similar to the following example:</span></span>

[!code-json[](debug/launchSettings.json?highlight=14,22)]

<span data-ttu-id="72c0f-135">`inspectUri`屬性：</span><span class="sxs-lookup"><span data-stu-id="72c0f-135">The `inspectUri` property:</span></span>

* <span data-ttu-id="72c0f-136">可讓 IDE 偵測應用程式是否為 Blazor WebAssembly 應用程式。</span><span class="sxs-lookup"><span data-stu-id="72c0f-136">Enables the IDE to detect that the app is a Blazor WebAssembly app.</span></span>
* <span data-ttu-id="72c0f-137">指示腳本的偵錯工具，透過的偵錯工具 proxy 連接到瀏覽器 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="72c0f-137">Instructs the script debugging infrastructure to connect to the browser through Blazor's debugging proxy.</span></span>

## <a name="visual-studio"></a><span data-ttu-id="72c0f-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="72c0f-138">Visual Studio</span></span>

<span data-ttu-id="72c0f-139">若要 Blazor 在 Visual Studio 中進行 WebAssembly 應用程式的 debug：</span><span class="sxs-lookup"><span data-stu-id="72c0f-139">To debug a Blazor WebAssembly app in Visual Studio:</span></span>

1. <span data-ttu-id="72c0f-140">建立新的 ASP.NET Core 託管 Blazor WebAssembly 應用程式。</span><span class="sxs-lookup"><span data-stu-id="72c0f-140">Create a new ASP.NET Core hosted Blazor WebAssembly app.</span></span>
1. <span data-ttu-id="72c0f-141">按<kbd>F5</kbd>以在偵錯工具中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="72c0f-141">Press <kbd>F5</kbd> to run the app in the debugger.</span></span>
1. <span data-ttu-id="72c0f-142">在方法的 [ *razor* ] 中設定中斷點 `IncrementCount` 。</span><span class="sxs-lookup"><span data-stu-id="72c0f-142">Set a breakpoint in *Counter.razor* in the `IncrementCount` method.</span></span>
1. <span data-ttu-id="72c0f-143">流覽至 [**計數器**] 索引標籤，然後選取按鈕以點擊中斷點：</span><span class="sxs-lookup"><span data-stu-id="72c0f-143">Browse to the **Counter** tab and select the button to hit the breakpoint:</span></span>

   ![Debug 計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-counter.png)

1. <span data-ttu-id="72c0f-145">查看 `currentCount` [區域變數] 視窗中的欄位值：</span><span class="sxs-lookup"><span data-stu-id="72c0f-145">Check out the value of the `currentCount` field in the locals window:</span></span>

   ![View 區域變數](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-locals.png)

1. <span data-ttu-id="72c0f-147">按<kbd>F5</kbd>繼續執行。</span><span class="sxs-lookup"><span data-stu-id="72c0f-147">Press <kbd>F5</kbd> to continue execution.</span></span>

<span data-ttu-id="72c0f-148">在偵測 Blazor WebAssembly 應用程式時，您也可以對伺服器程式碼進行偵錯工具：</span><span class="sxs-lookup"><span data-stu-id="72c0f-148">While debugging your Blazor WebAssembly app, you can also debug your server code:</span></span>

1. <span data-ttu-id="72c0f-149">在的 [ *FetchData* ] 頁面中設定中斷點 `OnInitializedAsync` 。</span><span class="sxs-lookup"><span data-stu-id="72c0f-149">Set a breakpoint in the *FetchData.razor* page in `OnInitializedAsync`.</span></span>
1. <span data-ttu-id="72c0f-150">在動作方法的中設定中斷點 `WeatherForecastController` `Get` 。</span><span class="sxs-lookup"><span data-stu-id="72c0f-150">Set a breakpoint in the `WeatherForecastController` in the `Get` action method.</span></span>
1. <span data-ttu-id="72c0f-151">流覽至 [**提取資料**] 索引標籤，以叫用元件中的第一個中斷點， `FetchData` 然後再對伺服器發出 HTTP 要求：</span><span class="sxs-lookup"><span data-stu-id="72c0f-151">Browse to the **Fetch Data** tab to hit the first breakpoint in the `FetchData` component just before it issues an HTTP request to the server:</span></span>

   ![Debug Fetch 資料](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-fetch-data.png)

1. <span data-ttu-id="72c0f-153">按<kbd>F5</kbd>繼續執行，然後在的伺服器上叫用中斷點 `WeatherForecastController` ：</span><span class="sxs-lookup"><span data-stu-id="72c0f-153">Press <kbd>F5</kbd> to continue execution and then hit the breakpoint on the server in the `WeatherForecastController`:</span></span>

   ![Debug server](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-server.png)

1. <span data-ttu-id="72c0f-155">再按一次<kbd>F5</kbd> ，讓執行繼續，並查看所轉譯的氣象預測資料表。</span><span class="sxs-lookup"><span data-stu-id="72c0f-155">Press <kbd>F5</kbd> again to let execution continue and see the weather forecast table rendered.</span></span>

<a id="vscode"></a>

## <a name="visual-studio-code"></a><span data-ttu-id="72c0f-156">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="72c0f-156">Visual Studio Code</span></span>

<span data-ttu-id="72c0f-157">若要 Blazor 在 Visual Studio Code 中進行 WebAssembly 應用程式的 debug：</span><span class="sxs-lookup"><span data-stu-id="72c0f-157">To debug a Blazor WebAssembly app in Visual Studio Code:</span></span>
 
1. <span data-ttu-id="72c0f-158">安裝[c # 擴充](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)功能和[JavaScript 偵錯工具（夜間）](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly)延伸，並 `debug.javascript.usePreview` 將設定為 `true` 。</span><span class="sxs-lookup"><span data-stu-id="72c0f-158">Install the [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the [JavaScript Debugger (Nightly)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) extension with `debug.javascript.usePreview` set to `true`.</span></span>

   ![延伸模組](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-extensions.png)

   ![JS 預覽偵錯工具](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-js-use-preview.png)

1. <span data-ttu-id="72c0f-161">開啟 Blazor 已啟用偵測的現有 WebAssembly 應用程式。</span><span class="sxs-lookup"><span data-stu-id="72c0f-161">Open an existing Blazor WebAssembly app with debugging enabled.</span></span>

   * <span data-ttu-id="72c0f-162">如果您收到下列通知，表示需要額外的設定才能啟用偵錯工具，請確認您已安裝正確的延伸模組，並已啟用 JavaScript 預覽偵測，然後重載視窗：</span><span class="sxs-lookup"><span data-stu-id="72c0f-162">If you get the following notification that additional setup is required to enable debugging, confirm that you have the correct extensions installed and JavaScript preview debugging enabled and then reload the window:</span></span>

     ![需要額外的設定](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-additional-setup.png)

   * <span data-ttu-id="72c0f-164">通知會提供將所需的資產新增至應用程式，以進行建立和調試。</span><span class="sxs-lookup"><span data-stu-id="72c0f-164">A notification offers to add the required assets to the app for building and debugging.</span></span> <span data-ttu-id="72c0f-165">選取 **[是]**：</span><span class="sxs-lookup"><span data-stu-id="72c0f-165">Select **Yes**:</span></span>

     ![新增必要的資產](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-required-assets.png)

1. <span data-ttu-id="72c0f-167">在偵錯工具中啟動應用程式是兩個步驟的進程：</span><span class="sxs-lookup"><span data-stu-id="72c0f-167">Starting the app in the debugger is a two-step process:</span></span>

   <span data-ttu-id="72c0f-168">1 \。</span><span class="sxs-lookup"><span data-stu-id="72c0f-168">1\.</span></span> <span data-ttu-id="72c0f-169">**首先**，使用 **.net Core 啟動（ Blazor 獨立式）** 啟動設定來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="72c0f-169">**First**, start the app using the **.NET Core Launch (Blazor Standalone)** launch configuration.</span></span>

   <span data-ttu-id="72c0f-170">2 \。</span><span class="sxs-lookup"><span data-stu-id="72c0f-170">2\.</span></span> <span data-ttu-id="72c0f-171">**應用程式啟動之後**，請使用 chrome 啟動設定（需要 chrome）**中的 .Net Core Debug Blazor Web 元件**來啟動瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="72c0f-171">**After the app has started**, start the browser using the **.NET Core Debug Blazor Web Assembly in Chrome** launch configuration (requires Chrome).</span></span> <span data-ttu-id="72c0f-172">若要使用 Edge 而不是 Chrome，請將 `type` *vscode/啟動*中啟動設定的從變更 `pwa-chrome` 為 `pwa-msedge` 。</span><span class="sxs-lookup"><span data-stu-id="72c0f-172">To use Edge instead of Chrome, change the `type` of the launch configuration in *.vscode/launch.json* from `pwa-chrome` to `pwa-msedge`.</span></span>

1. <span data-ttu-id="72c0f-173">在元件的方法中設定中斷點 `IncrementCount` `Counter` ，然後選取按鈕以叫用中斷點：</span><span class="sxs-lookup"><span data-stu-id="72c0f-173">Set a breakpoint in the `IncrementCount` method in the `Counter` component and then select the button to hit the breakpoint:</span></span>

   ![VS Code 中的 Debug 計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-debug-counter.png)

## <a name="debug-in-the-browser"></a><span data-ttu-id="72c0f-175">瀏覽器中的 Debug</span><span class="sxs-lookup"><span data-stu-id="72c0f-175">Debug in the browser</span></span>

1. <span data-ttu-id="72c0f-176">在開發環境中執行應用程式的 Debug 組建。</span><span class="sxs-lookup"><span data-stu-id="72c0f-176">Run a Debug build of the app in the Development environment.</span></span>

1. <span data-ttu-id="72c0f-177">按<kbd>Shift</kbd> + <kbd>Alt</kbd> + <kbd>D</kbd>。</span><span class="sxs-lookup"><span data-stu-id="72c0f-177">Press <kbd>Shift</kbd>+<kbd>Alt</kbd>+<kbd>D</kbd>.</span></span>

1. <span data-ttu-id="72c0f-178">瀏覽器必須在啟用遠端偵錯功能的情況下執行。</span><span class="sxs-lookup"><span data-stu-id="72c0f-178">The browser must be run with remote debugging enabled.</span></span> <span data-ttu-id="72c0f-179">如果已停用遠端偵錯程式，就會產生 [找**不到可調試的瀏覽器]** 索引標籤錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="72c0f-179">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated.</span></span> <span data-ttu-id="72c0f-180">錯誤頁面包含在開啟偵錯工具的情況之下執行瀏覽器的指示，讓 Blazor 偵錯工具 proxy 可以連接到應用程式。</span><span class="sxs-lookup"><span data-stu-id="72c0f-180">The error page contains instructions for running the browser with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="72c0f-181">*關閉所有瀏覽器實例*，然後依照指示重新開機瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="72c0f-181">*Close all browser instances* and restart the browser as instructed.</span></span>

<span data-ttu-id="72c0f-182">當瀏覽器在啟用遠端偵錯程式的情況下執行時，[偵錯工具] 鍵盤快速鍵會開啟新的 [偵錯工具]經過一段時間之後，[**來源**] 索引標籤會顯示應用程式中的 .net 元件清單。</span><span class="sxs-lookup"><span data-stu-id="72c0f-182">Once the browser is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="72c0f-183">展開每個元件，並 *.cs*尋找 / *.razor*可用來進行偵錯工具的 .cs 原始檔案。</span><span class="sxs-lookup"><span data-stu-id="72c0f-183">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="72c0f-184">設定中斷點、切換回應用程式的索引標籤，並在程式碼執行時叫用中斷點。</span><span class="sxs-lookup"><span data-stu-id="72c0f-184">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="72c0f-185">叫用中斷點之後，以單一步驟（<kbd>F10</kbd>），透過程式碼或繼續（<kbd>F8</kbd>）程式碼執行正常。</span><span class="sxs-lookup"><span data-stu-id="72c0f-185">After a breakpoint is hit, single-step (<kbd>F10</kbd>) through the code or resume (<kbd>F8</kbd>) code execution normally.</span></span>

Blazor<span data-ttu-id="72c0f-186">提供的偵錯工具 proxy 會執行[Chrome DevTools 通訊協定](https://chromedevtools.github.io/devtools-protocol/)，並使用來擴充通訊協定。NET 特定資訊。</span><span class="sxs-lookup"><span data-stu-id="72c0f-186"> provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="72c0f-187">當您按下 [調試鍵盤快速鍵] 時，會將 Blazor Chrome DevTools 指向 proxy。</span><span class="sxs-lookup"><span data-stu-id="72c0f-187">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="72c0f-188">Proxy 會連線到您想要進行調試的瀏覽器視窗（因此需要啟用遠端偵錯）。</span><span class="sxs-lookup"><span data-stu-id="72c0f-188">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="72c0f-189">瀏覽器來源對應</span><span class="sxs-lookup"><span data-stu-id="72c0f-189">Browser source maps</span></span>

<span data-ttu-id="72c0f-190">瀏覽器來源對應可讓瀏覽器將已編譯的檔案對應回原始來源檔案，而且通常會用於用戶端的偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="72c0f-190">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="72c0f-191">不過， Blazor 目前不會將 c # 直接對應至 JavaScript/WASM。</span><span class="sxs-lookup"><span data-stu-id="72c0f-191">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="72c0f-192">相反地， Blazor 會在瀏覽器中進行 IL 轉譯，因此來源對應不相關。</span><span class="sxs-lookup"><span data-stu-id="72c0f-192">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="72c0f-193">疑難排解</span><span class="sxs-lookup"><span data-stu-id="72c0f-193">Troubleshoot</span></span>

<span data-ttu-id="72c0f-194">如果您遇到錯誤，下列秘訣可能會有説明：</span><span class="sxs-lookup"><span data-stu-id="72c0f-194">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="72c0f-195">在 [**偵錯工具**] 索引標籤中，開啟瀏覽器中的開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="72c0f-195">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="72c0f-196">在主控台中，執行 `localStorage.clear()` 以移除任何中斷點。</span><span class="sxs-lookup"><span data-stu-id="72c0f-196">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
