---
<span data-ttu-id="1e1bf-101">標題： ' Debug ASP.NET Core Blazor WebAssembly ' author： guardrex description： ' 瞭解如何調試 Blazor 程式。 '</span><span class="sxs-lookup"><span data-stu-id="1e1bf-101">title: 'Debug ASP.NET Core Blazor WebAssembly' author: guardrex description: 'Learn how to debug Blazor apps.'</span></span>
<span data-ttu-id="1e1bf-102">monikerRange： ' >= aspnetcore-3.1 ' ms-chap： riande ms. custom： mvc ms. date： 05/31/2020 no-loc：</span><span class="sxs-lookup"><span data-stu-id="1e1bf-102">monikerRange: '>= aspnetcore-3.1' ms.author: riande ms.custom: mvc ms.date: 05/31/2020 no-loc:</span></span>
- <span data-ttu-id="1e1bf-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1e1bf-103">'Blazor'</span></span>
- <span data-ttu-id="1e1bf-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1e1bf-104">'Identity'</span></span>
- <span data-ttu-id="1e1bf-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1e1bf-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="1e1bf-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1e1bf-106">'Razor'</span></span>
- <span data-ttu-id="1e1bf-107">' SignalR ' uid： blazor/debug</span><span class="sxs-lookup"><span data-stu-id="1e1bf-107">'SignalR' uid: blazor/debug</span></span>

---
# <a name="debug-aspnet-core-blazor-webassembly"></a><span data-ttu-id="1e1bf-108">Debug ASP.NET Core Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="1e1bf-108">Debug ASP.NET Core Blazor WebAssembly</span></span>

[<span data-ttu-id="1e1bf-109">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="1e1bf-109">Daniel Roth</span></span>](https://github.com/danroth27)

Blazor<span data-ttu-id="1e1bf-110">WebAssembly 應用程式可以使用 Chromium 式瀏覽器中的瀏覽器開發工具（邊緣/Chrome）進行調試。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-110"> WebAssembly apps can be debugged using the browser dev tools in Chromium-based browsers (Edge/Chrome).</span></span> <span data-ttu-id="1e1bf-111">或者，您可以使用 Visual Studio 或 Visual Studio Code 來對應用程式進行 debug 錯。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-111">Alternatively, you can debug your app using Visual Studio or Visual Studio Code.</span></span>

<span data-ttu-id="1e1bf-112">可用的案例包括：</span><span class="sxs-lookup"><span data-stu-id="1e1bf-112">Available scenarios include:</span></span>

* <span data-ttu-id="1e1bf-113">設定和移除中斷點。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-113">Set and remove breakpoints.</span></span>
* <span data-ttu-id="1e1bf-114">在 Visual Studio 和 Visual Studio Code （<kbd>F5</kbd>支援）中，以支援偵錯工具的方式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-114">Run the app with debugging support in Visual Studio and Visual Studio Code (<kbd>F5</kbd> support).</span></span>
* <span data-ttu-id="1e1bf-115">透過程式碼的單一步驟（<kbd>F10</kbd>）。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-115">Single-step (<kbd>F10</kbd>) through the code.</span></span>
* <span data-ttu-id="1e1bf-116">在瀏覽器中使用<kbd>F8</kbd>或 Visual Studio 或 Visual Studio Code 中的<kbd>F5</kbd>繼續執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-116">Resume code execution with <kbd>F8</kbd> in a browser or <kbd>F5</kbd> in Visual Studio or Visual Studio Code.</span></span>
* <span data-ttu-id="1e1bf-117">在 [*區域變數*] 顯示中，觀察本機變數的值。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-117">In the *Locals* display, observe the values of local variables.</span></span>
* <span data-ttu-id="1e1bf-118">查看呼叫堆疊，包括從 JavaScript 轉換成 .NET，以及從 .NET 到 JavaScript 的呼叫鏈。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-118">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="1e1bf-119">目前，您*無法*：</span><span class="sxs-lookup"><span data-stu-id="1e1bf-119">For now, you *can't*:</span></span>

* <span data-ttu-id="1e1bf-120">中斷未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-120">Break on unhandled exceptions.</span></span>
* <span data-ttu-id="1e1bf-121">在應用程式啟動期間叫用中斷點。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-121">Hit breakpoints during app startup.</span></span>

<span data-ttu-id="1e1bf-122">我們將繼續改善即將發行的版本中的調試過程。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-122">We will continue to improve the debugging experience in upcoming releases.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1e1bf-123">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="1e1bf-123">Prerequisites</span></span>

<span data-ttu-id="1e1bf-124">調試需要下列其中一個瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="1e1bf-124">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="1e1bf-125">Microsoft Edge （版本80或更新版本）</span><span class="sxs-lookup"><span data-stu-id="1e1bf-125">Microsoft Edge (version 80 or later)</span></span>
* <span data-ttu-id="1e1bf-126">Google Chrome （版本70或更新版本）</span><span class="sxs-lookup"><span data-stu-id="1e1bf-126">Google Chrome (version 70 or later)</span></span>

## <a name="enable-debugging-for-visual-studio-and-visual-studio-code"></a><span data-ttu-id="1e1bf-127">啟用 Visual Studio 和 Visual Studio Code 的偵錯工具</span><span class="sxs-lookup"><span data-stu-id="1e1bf-127">Enable debugging for Visual Studio and Visual Studio Code</span></span>

<span data-ttu-id="1e1bf-128">若要啟用現有 Blazor WebAssembly 應用程式的偵測，請更新啟始專案中的*launchsettings.json* ，以 `inspectUri` 在每個啟動設定檔中包含下列屬性：</span><span class="sxs-lookup"><span data-stu-id="1e1bf-128">To enable debugging for an existing Blazor WebAssembly app, update the *launchSettings.json* file in the startup project to include the following `inspectUri` property in each launch profile:</span></span>

```json
"inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}"
```

<span data-ttu-id="1e1bf-129">更新之後， *launchsettings.json*看起來應該類似下列範例：</span><span class="sxs-lookup"><span data-stu-id="1e1bf-129">Once updated, the *launchSettings.json* file should look similar to the following example:</span></span>

[!code-json[](debug/launchSettings.json?highlight=14,22)]

<span data-ttu-id="1e1bf-130">`inspectUri`屬性：</span><span class="sxs-lookup"><span data-stu-id="1e1bf-130">The `inspectUri` property:</span></span>

* <span data-ttu-id="1e1bf-131">可讓 IDE 偵測應用程式是否為 Blazor WebAssembly 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-131">Enables the IDE to detect that the app is a Blazor WebAssembly app.</span></span>
* <span data-ttu-id="1e1bf-132">指示腳本的偵錯工具，透過的偵錯工具 proxy 連接到瀏覽器 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-132">Instructs the script debugging infrastructure to connect to the browser through Blazor's debugging proxy.</span></span>

<span data-ttu-id="1e1bf-133">在 `wsProtocol` 啟動的瀏覽器（）上，websocket 通訊協定（）、主機（ `url.hostname` ）、埠（ `url.port` ）和偵測器 URI 的預留位置值 `browserInspectUri` 是由架構所提供。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-133">The placeholder values for the WebSockets protocol (`wsProtocol`), host (`url.hostname`), port (`url.port`), and inspector URI on the launched browser (`browserInspectUri`) are provided by the framework.</span></span>

## <a name="visual-studio"></a><span data-ttu-id="1e1bf-134">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1e1bf-134">Visual Studio</span></span>

<span data-ttu-id="1e1bf-135">若要 Blazor 在 Visual Studio 中進行 WebAssembly 應用程式的 debug：</span><span class="sxs-lookup"><span data-stu-id="1e1bf-135">To debug a Blazor WebAssembly app in Visual Studio:</span></span>

1. <span data-ttu-id="1e1bf-136">建立新的 ASP.NET Core 託管 Blazor WebAssembly 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-136">Create a new ASP.NET Core hosted Blazor WebAssembly app.</span></span>
1. <span data-ttu-id="1e1bf-137">按<kbd>F5</kbd>以在偵錯工具中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-137">Press <kbd>F5</kbd> to run the app in the debugger.</span></span>
1. <span data-ttu-id="1e1bf-138">在方法的 [ *razor* ] 中設定中斷點 `IncrementCount` 。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-138">Set a breakpoint in *Counter.razor* in the `IncrementCount` method.</span></span>
1. <span data-ttu-id="1e1bf-139">流覽至 [**計數器**] 索引標籤，然後選取按鈕以點擊中斷點：</span><span class="sxs-lookup"><span data-stu-id="1e1bf-139">Browse to the **Counter** tab and select the button to hit the breakpoint:</span></span>

   ![Debug 計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-counter.png)

1. <span data-ttu-id="1e1bf-141">查看 `currentCount` [區域變數] 視窗中的欄位值：</span><span class="sxs-lookup"><span data-stu-id="1e1bf-141">Check out the value of the `currentCount` field in the locals window:</span></span>

   ![View 區域變數](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-locals.png)

1. <span data-ttu-id="1e1bf-143">按<kbd>F5</kbd>繼續執行。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-143">Press <kbd>F5</kbd> to continue execution.</span></span>

<span data-ttu-id="1e1bf-144">在偵測 Blazor WebAssembly 應用程式時，您也可以對伺服器程式碼進行偵錯工具：</span><span class="sxs-lookup"><span data-stu-id="1e1bf-144">While debugging your Blazor WebAssembly app, you can also debug your server code:</span></span>

1. <span data-ttu-id="1e1bf-145">在的 [ *FetchData* ] 頁面中設定中斷點 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> 。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-145">Set a breakpoint in the *FetchData.razor* page in <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A>.</span></span>
1. <span data-ttu-id="1e1bf-146">在動作方法的中設定中斷點 `WeatherForecastController` `Get` 。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-146">Set a breakpoint in the `WeatherForecastController` in the `Get` action method.</span></span>
1. <span data-ttu-id="1e1bf-147">流覽至 [**提取資料**] 索引標籤，以叫用元件中的第一個中斷點， `FetchData` 然後再對伺服器發出 HTTP 要求：</span><span class="sxs-lookup"><span data-stu-id="1e1bf-147">Browse to the **Fetch Data** tab to hit the first breakpoint in the `FetchData` component just before it issues an HTTP request to the server:</span></span>

   ![Debug Fetch 資料](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-fetch-data.png)

1. <span data-ttu-id="1e1bf-149">按<kbd>F5</kbd>繼續執行，然後在的伺服器上叫用中斷點 `WeatherForecastController` ：</span><span class="sxs-lookup"><span data-stu-id="1e1bf-149">Press <kbd>F5</kbd> to continue execution and then hit the breakpoint on the server in the `WeatherForecastController`:</span></span>

   ![Debug server](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-server.png)

1. <span data-ttu-id="1e1bf-151">再按一次<kbd>F5</kbd> ，讓執行繼續，並查看所轉譯的氣象預測資料表。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-151">Press <kbd>F5</kbd> again to let execution continue and see the weather forecast table rendered.</span></span>

<a id="vscode"></a>

## <a name="visual-studio-code"></a><span data-ttu-id="1e1bf-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1e1bf-152">Visual Studio Code</span></span>

<span data-ttu-id="1e1bf-153">若要 Blazor 在 Visual Studio Code 中進行 WebAssembly 應用程式的 debug：</span><span class="sxs-lookup"><span data-stu-id="1e1bf-153">To debug a Blazor WebAssembly app in Visual Studio Code:</span></span>
 
<span data-ttu-id="1e1bf-154">安裝[c # 擴充](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)功能和[JavaScript 偵錯工具（夜間）](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly)延伸，並 `debug.javascript.usePreview` 將設定為 `true` 。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-154">Install the [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the [JavaScript Debugger (Nightly)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) extension with `debug.javascript.usePreview` set to `true`.</span></span>

![延伸模組](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-extensions.png)

![JS 預覽偵錯工具](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-js-use-preview.png)

### <a name="debug-standalone-blazor-webassembly"></a><span data-ttu-id="1e1bf-157">Debug 獨立 Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="1e1bf-157">Debug standalone Blazor WebAssembly</span></span>

1. <span data-ttu-id="1e1bf-158">Blazor在 VS Code 中開啟獨立的 WebAssembly 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-158">Open the standalone Blazor WebAssembly app in VS Code.</span></span>

   <span data-ttu-id="1e1bf-159">如果您收到下列通知，表示需要額外的設定才能啟用偵錯工具：</span><span class="sxs-lookup"><span data-stu-id="1e1bf-159">If you receive the following notification that additional setup is required to enable debugging:</span></span>
   
   * <span data-ttu-id="1e1bf-160">確認您已安裝正確的延伸模組。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-160">Confirm that you have the correct extensions installed.</span></span>
   * <span data-ttu-id="1e1bf-161">確認 JavaScript preview 的偵錯工具已啟用。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-161">Confirm that JavaScript preview debugging is enabled.</span></span>
   * <span data-ttu-id="1e1bf-162">重載視窗。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-162">Reload the window.</span></span>

   ![需要額外的設定](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-additional-setup.png)

1. <span data-ttu-id="1e1bf-164">使用<kbd>F5</kbd>鍵盤快速鍵或功能表項目開始進行調試。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-164">Start debugging using the <kbd>F5</kbd> keyboard shortcut or the menu item.</span></span>

1. <span data-ttu-id="1e1bf-165">出現提示時，選取 [ \*\* Blazor WebAssembly] Debug\*\*選項以開始進行調試。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-165">When prompted, select the **Blazor WebAssembly Debug** option to start debugging.</span></span>

   ![可用的調試選項清單](index/_static/blazor-vscode-debugtypes.png)

1. <span data-ttu-id="1e1bf-167">隨即啟動獨立應用程式，並開啟偵錯工具瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-167">The standalone app is launched, and a debugging browser is opened.</span></span>

1. <span data-ttu-id="1e1bf-168">在元件的方法中設定中斷點 `IncrementCount` `Counter` ，然後選取按鈕以叫用中斷點：</span><span class="sxs-lookup"><span data-stu-id="1e1bf-168">Set a breakpoint in the `IncrementCount` method in the `Counter` component and then select the button to hit the breakpoint:</span></span>

   ![VS Code 中的 Debug 計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-debug-counter.png)

### <a name="debug-hosted-blazor-webassembly"></a><span data-ttu-id="1e1bf-170">Debug hosted Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="1e1bf-170">Debug hosted Blazor WebAssembly</span></span>

1. <span data-ttu-id="1e1bf-171">Blazor在 VS Code 中開啟 hosted WebAssembly 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-171">Open the hosted Blazor WebAssembly app in VS Code.</span></span>

1. <span data-ttu-id="1e1bf-172">如果沒有為專案設定啟動設定，則會出現下列通知。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-172">If there's no launch configuration set for the project, the following notification appears.</span></span> <span data-ttu-id="1e1bf-173">選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-173">Select **Yes**.</span></span>

   ![新增必要的資產](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-required-assets.png)

1. <span data-ttu-id="1e1bf-175">在 [選取範圍] 視窗中，選取裝載解決方案內的*伺服器*專案。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-175">In the selection window, select the *Server* project within the hosted solution.</span></span>

<span data-ttu-id="1e1bf-176">隨即會產生啟動的*json*檔案，並啟動偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-176">A *launch.json* file is generated with the launch configuration for launching the debugger.</span></span>

### <a name="attach-to-an-existing-debugging-session"></a><span data-ttu-id="1e1bf-177">附加至現有的偵錯工具會話</span><span class="sxs-lookup"><span data-stu-id="1e1bf-177">Attach to an existing debugging session</span></span>

<span data-ttu-id="1e1bf-178">若要附加至執行 Blazor 中的應用程式，請使用下列設定建立一個*啟動 json*檔案：</span><span class="sxs-lookup"><span data-stu-id="1e1bf-178">To attach to a running Blazor app, create a *launch.json* file with the following configuration:</span></span>

```json
{
  "type": "blazorwasm",
  "request": "attach",
  "name": "Attach to Existing Blazor WebAssembly Application"
}
```

> [!NOTE]
> <span data-ttu-id="1e1bf-179">只有針對獨立應用程式才支援附加至「偵測會話」。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-179">Attaching to a debugging session is only supported for standalone apps.</span></span> <span data-ttu-id="1e1bf-180">若要使用完整堆疊的偵錯工具，您必須從 VS Code 啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-180">To use full-stack debugging, you must launch the app from VS Code.</span></span>

### <a name="launch-configuration-options"></a><span data-ttu-id="1e1bf-181">啟動設定選項</span><span class="sxs-lookup"><span data-stu-id="1e1bf-181">Launch configuration options</span></span>

<span data-ttu-id="1e1bf-182">以下是支援的啟動設定選項： `blazorwasm` debug 類型。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-182">The following launch configuration options are supported for the `blazorwasm` debug type.</span></span>

| <span data-ttu-id="1e1bf-183">選項</span><span class="sxs-lookup"><span data-stu-id="1e1bf-183">Option</span></span>    | <span data-ttu-id="1e1bf-184">說明</span><span class="sxs-lookup"><span data-stu-id="1e1bf-184">Description</span></span> |
| --------- | ----------- |
| `request` | <span data-ttu-id="1e1bf-185">使用 `launch` 來啟動 WebAssembly 應用程式，並將其連結至已在執行中的 Blazor `attach` 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-185">Use `launch` to launch and attach a debugging session to a Blazor WebAssembly app or `attach` to attach a debugging session to an already-running app.</span></span> |
| `url`     | <span data-ttu-id="1e1bf-186">要在瀏覽器中開啟的 URL。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-186">The URL to open in the browser when debugging.</span></span> <span data-ttu-id="1e1bf-187">預設為 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-187">Defaults to `https://localhost:5001`.</span></span> |
| `browser` | <span data-ttu-id="1e1bf-188">要為偵錯工具啟動的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-188">The browser to launch for the debugging session.</span></span> <span data-ttu-id="1e1bf-189">設為 `edge` 或 `chrome`。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-189">Set to `edge` or `chrome`.</span></span> <span data-ttu-id="1e1bf-190">預設為 `chrome`。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-190">Defaults to `chrome`.</span></span> |
| `trace`   | <span data-ttu-id="1e1bf-191">用來從 JS 偵錯工具產生記錄。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-191">Used to generate logs from the JS debugger.</span></span> <span data-ttu-id="1e1bf-192">將設定為 `true` 以產生記錄。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-192">Set to `true` to generate logs.</span></span> |
| `hosted`  | <span data-ttu-id="1e1bf-193">`true`如果啟動和偵測託管的 WebAssembly 應用程式，則必須設定為 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-193">Must be set to `true` if launching and debugging a hosted Blazor WebAssembly app.</span></span> |
| `webRoot` | <span data-ttu-id="1e1bf-194">指定 web 伺服器的絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-194">Specifies the absolute path of the web server.</span></span> <span data-ttu-id="1e1bf-195">如果從子路由提供應用程式，則應設定。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-195">Should be set if an app is served from a sub-route.</span></span> |
| `timeout` | <span data-ttu-id="1e1bf-196">等候偵錯工具附加的毫秒數。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-196">The number of milliseconds to wait for the debugging session to attach.</span></span> <span data-ttu-id="1e1bf-197">預設為30000毫秒（30秒）。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-197">Defaults to 30,000 milliseconds (30 seconds).</span></span> |
| `program` | <span data-ttu-id="1e1bf-198">可執行檔的參考，可執行裝載應用程式的伺服器。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-198">A reference to the executable to run the server of the hosted app.</span></span> <span data-ttu-id="1e1bf-199">如果為，則必須設定 `hosted` `true` 。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-199">Must be set if `hosted` is `true`.</span></span> |
| `cwd`     | <span data-ttu-id="1e1bf-200">要在其下啟動應用程式的工作目錄。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-200">The working directory to launch the app under.</span></span> <span data-ttu-id="1e1bf-201">如果為，則必須設定 `hosted` `true` 。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-201">Must be set if `hosted` is `true`.</span></span> |
| `env`     | <span data-ttu-id="1e1bf-202">要提供給已啟動進程的環境變數。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-202">The environment variables to provide to the launched process.</span></span> <span data-ttu-id="1e1bf-203">只有在設為時才適用 `hosted` `true` 。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-203">Only applicable if `hosted` is set to `true`.</span></span> |

### <a name="example-launch-configurations"></a><span data-ttu-id="1e1bf-204">啟動設定範例</span><span class="sxs-lookup"><span data-stu-id="1e1bf-204">Example launch configurations</span></span>

#### <a name="launch-and-debug-a-standalone-blazor-webassembly-app"></a><span data-ttu-id="1e1bf-205">啟動和調試獨立 Blazor WebAssembly 應用程式</span><span class="sxs-lookup"><span data-stu-id="1e1bf-205">Launch and debug a standalone Blazor WebAssembly app</span></span>

```json
{
  "type": "blazorwasm",
  "request": "launch",
  "name": "Launch and Debug"
}
```

#### <a name="attach-to-a-running-app-at-a-specified-url"></a><span data-ttu-id="1e1bf-206">在指定的 URL 附加至執行中的應用程式</span><span class="sxs-lookup"><span data-stu-id="1e1bf-206">Attach to a running app at a specified URL</span></span>

```json
{
  "type": "blazorwasm",
  "request": "attach",
  "name": "Attach and Debug",
  "url": "http://localhost:5000"
}
```

#### <a name="launch-and-debug-a-hosted-blazor-webassembly-app"></a><span data-ttu-id="1e1bf-207">啟動和偵測託管的 Blazor WebAssembly 應用程式</span><span class="sxs-lookup"><span data-stu-id="1e1bf-207">Launch and debug a hosted Blazor WebAssembly app</span></span>

```json
{
  "type": "blazorwasm",
  "request": "launch",
  "name": "Launch and Debug Hosted App",
  "program": "${workspaceFolder}/Server/bin/Debug/netcoreapp3.1/MyHostedApp.Server.dll",
  "cwd": "${workspaceFolder}"
}
```

## <a name="debug-in-the-browser"></a><span data-ttu-id="1e1bf-208">瀏覽器中的 Debug</span><span class="sxs-lookup"><span data-stu-id="1e1bf-208">Debug in the browser</span></span>

1. <span data-ttu-id="1e1bf-209">在開發環境中執行應用程式的 Debug 組建。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-209">Run a Debug build of the app in the Development environment.</span></span>

1. <span data-ttu-id="1e1bf-210">按<kbd>Shift</kbd> + <kbd>Alt</kbd> + <kbd>D</kbd>。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-210">Press <kbd>Shift</kbd>+<kbd>Alt</kbd>+<kbd>D</kbd>.</span></span>

1. <span data-ttu-id="1e1bf-211">瀏覽器必須在啟用遠端偵錯功能的情況下執行。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-211">The browser must be run with remote debugging enabled.</span></span> <span data-ttu-id="1e1bf-212">如果已停用遠端偵錯程式，就會產生 [找**不到可調試的瀏覽器]** 索引標籤錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-212">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated.</span></span> <span data-ttu-id="1e1bf-213">錯誤頁面包含在開啟偵錯工具的情況之下執行瀏覽器的指示，讓 Blazor 偵錯工具 proxy 可以連接到應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-213">The error page contains instructions for running the browser with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="1e1bf-214">*關閉所有瀏覽器實例*，然後依照指示重新開機瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-214">*Close all browser instances* and restart the browser as instructed.</span></span>

<span data-ttu-id="1e1bf-215">當瀏覽器在啟用遠端偵錯程式的情況下執行時，[偵錯工具] 鍵盤快速鍵會開啟新的 [偵錯工具]經過一段時間之後，[**來源**] 索引標籤會顯示應用程式中的 .net 元件清單。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-215">Once the browser is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="1e1bf-216">展開每個元件，並 *.cs*尋找 / *.razor*可用來進行偵錯工具的 .cs 原始檔案。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-216">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="1e1bf-217">設定中斷點、切換回應用程式的索引標籤，並在程式碼執行時叫用中斷點。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-217">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="1e1bf-218">叫用中斷點之後，以單一步驟（<kbd>F10</kbd>），透過程式碼或繼續（<kbd>F8</kbd>）程式碼執行正常。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-218">After a breakpoint is hit, single-step (<kbd>F10</kbd>) through the code or resume (<kbd>F8</kbd>) code execution normally.</span></span>

Blazor<span data-ttu-id="1e1bf-219">提供的偵錯工具 proxy 會執行[Chrome DevTools 通訊協定](https://chromedevtools.github.io/devtools-protocol/)，並使用來擴充通訊協定。NET 特定資訊。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-219"> provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="1e1bf-220">當您按下 [調試鍵盤快速鍵] 時，會將 Blazor Chrome DevTools 指向 proxy。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-220">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="1e1bf-221">Proxy 會連線到您想要進行調試的瀏覽器視窗（因此需要啟用遠端偵錯）。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-221">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="1e1bf-222">瀏覽器來源對應</span><span class="sxs-lookup"><span data-stu-id="1e1bf-222">Browser source maps</span></span>

<span data-ttu-id="1e1bf-223">瀏覽器來源對應可讓瀏覽器將已編譯的檔案對應回原始來源檔案，而且通常會用於用戶端的偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-223">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="1e1bf-224">不過， Blazor 目前不會將 c # 直接對應至 JavaScript/WASM。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-224">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="1e1bf-225">相反地， Blazor 會在瀏覽器中進行 IL 轉譯，因此來源對應不相關。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-225">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="1e1bf-226">疑難排解</span><span class="sxs-lookup"><span data-stu-id="1e1bf-226">Troubleshoot</span></span>

<span data-ttu-id="1e1bf-227">如果您遇到錯誤，下列秘訣可能會有説明：</span><span class="sxs-lookup"><span data-stu-id="1e1bf-227">If you're running into errors, the following tips may help:</span></span>

* <span data-ttu-id="1e1bf-228">在 [**偵錯工具**] 索引標籤中，開啟瀏覽器中的開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-228">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="1e1bf-229">在主控台中，執行 `localStorage.clear()` 以移除任何中斷點。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-229">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
* <span data-ttu-id="1e1bf-230">確認您已安裝並信任 ASP.NET Core 的 HTTPS 開發憑證。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-230">Confirm that you've installed and trusted the ASP.NET Core HTTPS development certificate.</span></span> <span data-ttu-id="1e1bf-231">如需詳細資訊，請參閱<xref:security/enforcing-ssl#troubleshoot-certificate-problems>。</span><span class="sxs-lookup"><span data-stu-id="1e1bf-231">For more information, see <xref:security/enforcing-ssl#troubleshoot-certificate-problems>.</span></span>
