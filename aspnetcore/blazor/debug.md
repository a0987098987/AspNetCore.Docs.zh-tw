---
title: 'Debug ASP.NET Core:::no-loc(Blazor WebAssembly):::'
author: guardrex
description: '瞭解如何調試 :::no-loc(Blazor)::: 程式。'
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/15/2020
no-loc:
- ':::no-loc(Blazor):::'
- ':::no-loc(Blazor Server):::'
- ':::no-loc(Blazor WebAssembly):::'
- ':::no-loc(Identity):::'
- ":::no-loc(Let's Encrypt):::"
- ':::no-loc(Razor):::'
- ':::no-loc(SignalR):::'
uid: blazor/debug
ms.openlocfilehash: 14943b9f7847ac9144addfdf16a003f6fc8c340c
ms.sourcegitcommit: cc845634a490c49ff869c89b6e422b6d65d0e886
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2020
ms.locfileid: "87159710"
---
# <a name="debug-aspnet-core-no-locblazor-webassembly"></a><span data-ttu-id="2b114-103">Debug ASP.NET Core:::no-loc(Blazor WebAssembly):::</span><span class="sxs-lookup"><span data-stu-id="2b114-103">Debug ASP.NET Core :::no-loc(Blazor WebAssembly):::</span></span>

[<span data-ttu-id="2b114-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="2b114-104">Daniel Roth</span></span>](https://github.com/danroth27)

<span data-ttu-id="2b114-105">:::no-loc(Blazor WebAssembly):::應用程式可以使用瀏覽器開發工具，以 Chromium 為基礎的瀏覽器（邊緣/Chrome）進行調試。</span><span class="sxs-lookup"><span data-stu-id="2b114-105">:::no-loc(Blazor WebAssembly)::: apps can be debugged using the browser dev tools in Chromium-based browsers (Edge/Chrome).</span></span> <span data-ttu-id="2b114-106">或者，您可以使用 Visual Studio 或 Visual Studio Code 來對應用程式進行 debug 錯。</span><span class="sxs-lookup"><span data-stu-id="2b114-106">Alternatively, you can debug your app using Visual Studio or Visual Studio Code.</span></span>

<span data-ttu-id="2b114-107">可用的案例包括：</span><span class="sxs-lookup"><span data-stu-id="2b114-107">Available scenarios include:</span></span>

* <span data-ttu-id="2b114-108">設定和移除中斷點。</span><span class="sxs-lookup"><span data-stu-id="2b114-108">Set and remove breakpoints.</span></span>
* <span data-ttu-id="2b114-109">在 Visual Studio 和 Visual Studio Code （<kbd>F5</kbd>支援）中，以支援偵錯工具的方式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="2b114-109">Run the app with debugging support in Visual Studio and Visual Studio Code (<kbd>F5</kbd> support).</span></span>
* <span data-ttu-id="2b114-110">透過程式碼的單一步驟（<kbd>F10</kbd>）。</span><span class="sxs-lookup"><span data-stu-id="2b114-110">Single-step (<kbd>F10</kbd>) through the code.</span></span>
* <span data-ttu-id="2b114-111">在瀏覽器中使用<kbd>F8</kbd>或 Visual Studio 或 Visual Studio Code 中的<kbd>F5</kbd>繼續執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="2b114-111">Resume code execution with <kbd>F8</kbd> in a browser or <kbd>F5</kbd> in Visual Studio or Visual Studio Code.</span></span>
* <span data-ttu-id="2b114-112">在 [*區域變數*] 顯示中，觀察本機變數的值。</span><span class="sxs-lookup"><span data-stu-id="2b114-112">In the *Locals* display, observe the values of local variables.</span></span>
* <span data-ttu-id="2b114-113">查看呼叫堆疊，包括從 JavaScript 轉換成 .NET，以及從 .NET 到 JavaScript 的呼叫鏈。</span><span class="sxs-lookup"><span data-stu-id="2b114-113">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="2b114-114">目前，您*無法*：</span><span class="sxs-lookup"><span data-stu-id="2b114-114">For now, you *can't*:</span></span>

* <span data-ttu-id="2b114-115">中斷未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2b114-115">Break on unhandled exceptions.</span></span>
* <span data-ttu-id="2b114-116">在應用程式啟動期間叫用中斷點。</span><span class="sxs-lookup"><span data-stu-id="2b114-116">Hit breakpoints during app startup.</span></span>

<span data-ttu-id="2b114-117">我們將繼續改善即將發行的版本中的調試過程。</span><span class="sxs-lookup"><span data-stu-id="2b114-117">We will continue to improve the debugging experience in upcoming releases.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2b114-118">先決條件</span><span class="sxs-lookup"><span data-stu-id="2b114-118">Prerequisites</span></span>

<span data-ttu-id="2b114-119">調試需要下列其中一個瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="2b114-119">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="2b114-120">Google Chrome （70版或更新版本）（預設值）</span><span class="sxs-lookup"><span data-stu-id="2b114-120">Google Chrome (version 70 or later) (default)</span></span>
* <span data-ttu-id="2b114-121">Microsoft Edge （版本80或更新版本）</span><span class="sxs-lookup"><span data-stu-id="2b114-121">Microsoft Edge (version 80 or later)</span></span>

## <a name="enable-debugging-for-visual-studio-and-visual-studio-code"></a><span data-ttu-id="2b114-122">啟用 Visual Studio 和 Visual Studio Code 的偵錯工具</span><span class="sxs-lookup"><span data-stu-id="2b114-122">Enable debugging for Visual Studio and Visual Studio Code</span></span>

<span data-ttu-id="2b114-123">若要啟用現有應用程式的偵測功能 :::no-loc(Blazor WebAssembly)::: ，請更新 `launchSettings.json` 啟始專案中的檔案，以 `inspectUri` 在每個啟動設定檔中包含下列屬性：</span><span class="sxs-lookup"><span data-stu-id="2b114-123">To enable debugging for an existing :::no-loc(Blazor WebAssembly)::: app, update the `launchSettings.json` file in the startup project to include the following `inspectUri` property in each launch profile:</span></span>

```json
"inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}"
```

<span data-ttu-id="2b114-124">更新之後，檔案 `launchSettings.json` 看起來應該類似下列範例：</span><span class="sxs-lookup"><span data-stu-id="2b114-124">Once updated, the `launchSettings.json` file should look similar to the following example:</span></span>

[!code-json[](debug/launchSettings.json?highlight=14,22)]

<span data-ttu-id="2b114-125">`inspectUri`屬性：</span><span class="sxs-lookup"><span data-stu-id="2b114-125">The `inspectUri` property:</span></span>

* <span data-ttu-id="2b114-126">可讓 IDE 偵測應用程式是否為應用程式 :::no-loc(Blazor WebAssembly)::: 。</span><span class="sxs-lookup"><span data-stu-id="2b114-126">Enables the IDE to detect that the app is a :::no-loc(Blazor WebAssembly)::: app.</span></span>
* <span data-ttu-id="2b114-127">指示腳本的偵錯工具，透過的偵錯工具 proxy 連接到瀏覽器 :::no-loc(Blazor)::: 。</span><span class="sxs-lookup"><span data-stu-id="2b114-127">Instructs the script debugging infrastructure to connect to the browser through :::no-loc(Blazor):::'s debugging proxy.</span></span>

<span data-ttu-id="2b114-128">在 `wsProtocol` 啟動的瀏覽器（）上，websocket 通訊協定（）、主機（ `url.hostname` ）、埠（ `url.port` ）和偵測器 URI 的預留位置值 `browserInspectUri` 是由架構所提供。</span><span class="sxs-lookup"><span data-stu-id="2b114-128">The placeholder values for the WebSockets protocol (`wsProtocol`), host (`url.hostname`), port (`url.port`), and inspector URI on the launched browser (`browserInspectUri`) are provided by the framework.</span></span>

## <a name="visual-studio"></a><span data-ttu-id="2b114-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2b114-129">Visual Studio</span></span>

<span data-ttu-id="2b114-130">若要 :::no-loc(Blazor WebAssembly)::: 在 Visual Studio 中進行應用程式的 debug：</span><span class="sxs-lookup"><span data-stu-id="2b114-130">To debug a :::no-loc(Blazor WebAssembly)::: app in Visual Studio:</span></span>

1. <span data-ttu-id="2b114-131">建立新的 ASP.NET Core 託管 :::no-loc(Blazor WebAssembly)::: 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2b114-131">Create a new ASP.NET Core hosted :::no-loc(Blazor WebAssembly)::: app.</span></span>
1. <span data-ttu-id="2b114-132">按<kbd>F5</kbd>以在偵錯工具中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="2b114-132">Press <kbd>F5</kbd> to run the app in the debugger.</span></span>
1. <span data-ttu-id="2b114-133">在 `Pages/Counter.razor` 方法的中設定中斷點 `IncrementCount` 。</span><span class="sxs-lookup"><span data-stu-id="2b114-133">Set a breakpoint in `Pages/Counter.razor` in the `IncrementCount` method.</span></span>
1. <span data-ttu-id="2b114-134">流覽至 [] 索引標籤 **`Counter`** ，然後選取按鈕以叫用中斷點：</span><span class="sxs-lookup"><span data-stu-id="2b114-134">Browse to the **`Counter`** tab and select the button to hit the breakpoint:</span></span>

   ![Debug 計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-counter.png)

1. <span data-ttu-id="2b114-136">查看 `currentCount` [區域變數] 視窗中的欄位值：</span><span class="sxs-lookup"><span data-stu-id="2b114-136">Check out the value of the `currentCount` field in the locals window:</span></span>

   ![View 區域變數](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-locals.png)

1. <span data-ttu-id="2b114-138">按<kbd>F5</kbd>繼續執行。</span><span class="sxs-lookup"><span data-stu-id="2b114-138">Press <kbd>F5</kbd> to continue execution.</span></span>

<span data-ttu-id="2b114-139">在對您的 :::no-loc(Blazor WebAssembly)::: 應用程式進行調試時，您也可以對伺服器程式碼進行 debug：</span><span class="sxs-lookup"><span data-stu-id="2b114-139">While debugging your :::no-loc(Blazor WebAssembly)::: app, you can also debug your server code:</span></span>

1. <span data-ttu-id="2b114-140">在的頁面中設定中斷點 `Pages/FetchData.razor` <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> 。</span><span class="sxs-lookup"><span data-stu-id="2b114-140">Set a breakpoint in the `Pages/FetchData.razor` page in <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A>.</span></span>
1. <span data-ttu-id="2b114-141">在動作方法的中設定中斷點 `WeatherForecastController` `Get` 。</span><span class="sxs-lookup"><span data-stu-id="2b114-141">Set a breakpoint in the `WeatherForecastController` in the `Get` action method.</span></span>
1. <span data-ttu-id="2b114-142">流覽至索引 **`Fetch Data`** 標籤，以叫用元件中的第一個中斷點， `FetchData` 然後再對伺服器發出 HTTP 要求：</span><span class="sxs-lookup"><span data-stu-id="2b114-142">Browse to the **`Fetch Data`** tab to hit the first breakpoint in the `FetchData` component just before it issues an HTTP request to the server:</span></span>

   ![Debug Fetch 資料](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-fetch-data.png)

1. <span data-ttu-id="2b114-144">按<kbd>F5</kbd>繼續執行，然後在的伺服器上叫用中斷點 `WeatherForecastController` ：</span><span class="sxs-lookup"><span data-stu-id="2b114-144">Press <kbd>F5</kbd> to continue execution and then hit the breakpoint on the server in the `WeatherForecastController`:</span></span>

   ![Debug server](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-server.png)

1. <span data-ttu-id="2b114-146">再按一次<kbd>F5</kbd> ，讓執行繼續，並查看所轉譯的氣象預測資料表。</span><span class="sxs-lookup"><span data-stu-id="2b114-146">Press <kbd>F5</kbd> again to let execution continue and see the weather forecast table rendered.</span></span>

<a id="vscode"></a>

## <a name="visual-studio-code"></a><span data-ttu-id="2b114-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2b114-147">Visual Studio Code</span></span>

### <a name="debug-standalone-no-locblazor-webassembly"></a><span data-ttu-id="2b114-148">獨立調試:::no-loc(Blazor WebAssembly):::</span><span class="sxs-lookup"><span data-stu-id="2b114-148">Debug standalone :::no-loc(Blazor WebAssembly):::</span></span>

1. <span data-ttu-id="2b114-149">:::no-loc(Blazor WebAssembly):::在 VS Code 中開啟獨立應用程式。</span><span class="sxs-lookup"><span data-stu-id="2b114-149">Open the standalone :::no-loc(Blazor WebAssembly)::: app in VS Code.</span></span>

   <span data-ttu-id="2b114-150">如果您收到下列通知，表示需要額外的設定才能啟用偵錯工具：</span><span class="sxs-lookup"><span data-stu-id="2b114-150">If you receive the following notification that additional setup is required to enable debugging:</span></span>
   
   * <span data-ttu-id="2b114-151">確認您已安裝正確的延伸模組。</span><span class="sxs-lookup"><span data-stu-id="2b114-151">Confirm that you have the correct extensions installed.</span></span>
   * <span data-ttu-id="2b114-152">確認 JavaScript preview 的偵錯工具已啟用。</span><span class="sxs-lookup"><span data-stu-id="2b114-152">Confirm that JavaScript preview debugging is enabled.</span></span>
   * <span data-ttu-id="2b114-153">重載視窗。</span><span class="sxs-lookup"><span data-stu-id="2b114-153">Reload the window.</span></span>

   ![需要額外的設定](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-additional-setup.png)

1. <span data-ttu-id="2b114-155">使用<kbd>F5</kbd>鍵盤快速鍵或功能表項目開始進行調試。</span><span class="sxs-lookup"><span data-stu-id="2b114-155">Start debugging using the <kbd>F5</kbd> keyboard shortcut or the menu item.</span></span>

1. <span data-ttu-id="2b114-156">出現提示時，請選取 [ \*\* :::no-loc(Blazor WebAssembly)::: Debug\*\* ] 選項來開始進行調試。</span><span class="sxs-lookup"><span data-stu-id="2b114-156">When prompted, select the **:::no-loc(Blazor WebAssembly)::: Debug** option to start debugging.</span></span>

   ![可用的調試選項清單](index/_static/blazor-vscode-debugtypes.png)

1. <span data-ttu-id="2b114-158">隨即啟動獨立應用程式，並開啟偵錯工具瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="2b114-158">The standalone app is launched, and a debugging browser is opened.</span></span>

1. <span data-ttu-id="2b114-159">在元件的方法中設定中斷點 `IncrementCount` `Counter` ，然後選取按鈕以叫用中斷點：</span><span class="sxs-lookup"><span data-stu-id="2b114-159">Set a breakpoint in the `IncrementCount` method in the `Counter` component and then select the button to hit the breakpoint:</span></span>

   ![VS Code 中的 Debug 計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-debug-counter.png)

### <a name="debug-hosted-no-locblazor-webassembly"></a><span data-ttu-id="2b114-161">已裝載的調試:::no-loc(Blazor WebAssembly):::</span><span class="sxs-lookup"><span data-stu-id="2b114-161">Debug hosted :::no-loc(Blazor WebAssembly):::</span></span>

1. <span data-ttu-id="2b114-162">:::no-loc(Blazor WebAssembly):::在 VS Code 中開啟裝載應用程式的 [解決方案] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="2b114-162">Open the hosted :::no-loc(Blazor WebAssembly)::: app's solution folder in VS Code.</span></span>

1. <span data-ttu-id="2b114-163">如果沒有為專案設定啟動設定，則會出現下列通知。</span><span class="sxs-lookup"><span data-stu-id="2b114-163">If there's no launch configuration set for the project, the following notification appears.</span></span> <span data-ttu-id="2b114-164">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="2b114-164">Select **Yes**.</span></span>

   ![新增必要的資產](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-required-assets.png)

1. <span data-ttu-id="2b114-166">在視窗頂端的 [命令選擇區] 中，選取裝載解決方案內的*伺服器*專案。</span><span class="sxs-lookup"><span data-stu-id="2b114-166">In the command palette at the top of the window, select the *Server* project within the hosted solution.</span></span>

<span data-ttu-id="2b114-167">`launch.json`會使用啟動偵錯工具來產生檔案。</span><span class="sxs-lookup"><span data-stu-id="2b114-167">A `launch.json` file is generated with the launch configuration for launching the debugger.</span></span>

### <a name="attach-to-an-existing-debugging-session"></a><span data-ttu-id="2b114-168">附加至現有的偵錯工具會話</span><span class="sxs-lookup"><span data-stu-id="2b114-168">Attach to an existing debugging session</span></span>

<span data-ttu-id="2b114-169">若要附加至執行 :::no-loc(Blazor)::: 中的應用程式，請使用下列設定來建立檔案 `launch.json` ：</span><span class="sxs-lookup"><span data-stu-id="2b114-169">To attach to a running :::no-loc(Blazor)::: app, create a `launch.json` file with the following configuration:</span></span>

```json
{
  "type": "blazorwasm",
  "request": "attach",
  "name": "Attach to Existing :::no-loc(Blazor WebAssembly)::: Application"
}
```

> [!NOTE]
> <span data-ttu-id="2b114-170">只有針對獨立應用程式才支援附加至「偵測會話」。</span><span class="sxs-lookup"><span data-stu-id="2b114-170">Attaching to a debugging session is only supported for standalone apps.</span></span> <span data-ttu-id="2b114-171">若要使用完整堆疊的偵錯工具，您必須從 VS Code 啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="2b114-171">To use full-stack debugging, you must launch the app from VS Code.</span></span>

### <a name="launch-configuration-options"></a><span data-ttu-id="2b114-172">啟動設定選項</span><span class="sxs-lookup"><span data-stu-id="2b114-172">Launch configuration options</span></span>

<span data-ttu-id="2b114-173">以下是針對 `blazorwasm` debug 類型（）支援的啟動設定選項 `.vscode/launch.json` 。</span><span class="sxs-lookup"><span data-stu-id="2b114-173">The following launch configuration options are supported for the `blazorwasm` debug type (`.vscode/launch.json`).</span></span>

| <span data-ttu-id="2b114-174">選項</span><span class="sxs-lookup"><span data-stu-id="2b114-174">Option</span></span>    | <span data-ttu-id="2b114-175">說明</span><span class="sxs-lookup"><span data-stu-id="2b114-175">Description</span></span> |
| --------- | ----------- |
| `request` | <span data-ttu-id="2b114-176">使用 `launch` 來啟動並將偵錯工具連結至 :::no-loc(Blazor WebAssembly)::: 應用程式，或將 `attach` 偵錯工具附加至已在執行中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2b114-176">Use `launch` to launch and attach a debugging session to a :::no-loc(Blazor WebAssembly)::: app or `attach` to attach a debugging session to an already-running app.</span></span> |
| `url`     | <span data-ttu-id="2b114-177">要在瀏覽器中開啟的 URL。</span><span class="sxs-lookup"><span data-stu-id="2b114-177">The URL to open in the browser when debugging.</span></span> <span data-ttu-id="2b114-178">預設為 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="2b114-178">Defaults to `https://localhost:5001`.</span></span> |
| `browser` | <span data-ttu-id="2b114-179">要為偵錯工具啟動的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="2b114-179">The browser to launch for the debugging session.</span></span> <span data-ttu-id="2b114-180">設為 `edge` 或 `chrome`。</span><span class="sxs-lookup"><span data-stu-id="2b114-180">Set to `edge` or `chrome`.</span></span> <span data-ttu-id="2b114-181">預設為 `chrome`。</span><span class="sxs-lookup"><span data-stu-id="2b114-181">Defaults to `chrome`.</span></span> |
| `trace`   | <span data-ttu-id="2b114-182">用來從 JS 偵錯工具產生記錄。</span><span class="sxs-lookup"><span data-stu-id="2b114-182">Used to generate logs from the JS debugger.</span></span> <span data-ttu-id="2b114-183">將設定為 `true` 以產生記錄。</span><span class="sxs-lookup"><span data-stu-id="2b114-183">Set to `true` to generate logs.</span></span> |
| `hosted`  | <span data-ttu-id="2b114-184">`true`如果啟動和偵測託管應用程式，則必須設定為 :::no-loc(Blazor WebAssembly)::: 。</span><span class="sxs-lookup"><span data-stu-id="2b114-184">Must be set to `true` if launching and debugging a hosted :::no-loc(Blazor WebAssembly)::: app.</span></span> |
| `webRoot` | <span data-ttu-id="2b114-185">指定 web 伺服器的絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="2b114-185">Specifies the absolute path of the web server.</span></span> <span data-ttu-id="2b114-186">如果從子路由提供應用程式，則應設定。</span><span class="sxs-lookup"><span data-stu-id="2b114-186">Should be set if an app is served from a sub-route.</span></span> |
| `timeout` | <span data-ttu-id="2b114-187">等候偵錯工具附加的毫秒數。</span><span class="sxs-lookup"><span data-stu-id="2b114-187">The number of milliseconds to wait for the debugging session to attach.</span></span> <span data-ttu-id="2b114-188">預設為30000毫秒（30秒）。</span><span class="sxs-lookup"><span data-stu-id="2b114-188">Defaults to 30,000 milliseconds (30 seconds).</span></span> |
| `program` | <span data-ttu-id="2b114-189">可執行檔的參考，可執行裝載應用程式的伺服器。</span><span class="sxs-lookup"><span data-stu-id="2b114-189">A reference to the executable to run the server of the hosted app.</span></span> <span data-ttu-id="2b114-190">如果為，則必須設定 `hosted` `true` 。</span><span class="sxs-lookup"><span data-stu-id="2b114-190">Must be set if `hosted` is `true`.</span></span> |
| `cwd`     | <span data-ttu-id="2b114-191">要在其下啟動應用程式的工作目錄。</span><span class="sxs-lookup"><span data-stu-id="2b114-191">The working directory to launch the app under.</span></span> <span data-ttu-id="2b114-192">如果為，則必須設定 `hosted` `true` 。</span><span class="sxs-lookup"><span data-stu-id="2b114-192">Must be set if `hosted` is `true`.</span></span> |
| `env`     | <span data-ttu-id="2b114-193">要提供給已啟動進程的環境變數。</span><span class="sxs-lookup"><span data-stu-id="2b114-193">The environment variables to provide to the launched process.</span></span> <span data-ttu-id="2b114-194">只有在設為時才適用 `hosted` `true` 。</span><span class="sxs-lookup"><span data-stu-id="2b114-194">Only applicable if `hosted` is set to `true`.</span></span> |

### <a name="example-launch-configurations"></a><span data-ttu-id="2b114-195">啟動設定範例</span><span class="sxs-lookup"><span data-stu-id="2b114-195">Example launch configurations</span></span>

#### <a name="launch-and-debug-a-standalone-no-locblazor-webassembly-app"></a><span data-ttu-id="2b114-196">啟動和調試獨立 :::no-loc(Blazor WebAssembly)::: 應用程式</span><span class="sxs-lookup"><span data-stu-id="2b114-196">Launch and debug a standalone :::no-loc(Blazor WebAssembly)::: app</span></span>

```json
{
  "type": "blazorwasm",
  "request": "launch",
  "name": "Launch and Debug"
}
```

#### <a name="attach-to-a-running-app-at-a-specified-url"></a><span data-ttu-id="2b114-197">在指定的 URL 附加至執行中的應用程式</span><span class="sxs-lookup"><span data-stu-id="2b114-197">Attach to a running app at a specified URL</span></span>

```json
{
  "type": "blazorwasm",
  "request": "attach",
  "name": "Attach and Debug",
  "url": "http://localhost:5000"
}
```

#### <a name="launch-and-debug-a-hosted-no-locblazor-webassembly-app-with-microsoft-edge"></a><span data-ttu-id="2b114-198">使用 Microsoft Edge 啟動和調試託管 :::no-loc(Blazor WebAssembly)::: 應用程式</span><span class="sxs-lookup"><span data-stu-id="2b114-198">Launch and debug a hosted :::no-loc(Blazor WebAssembly)::: app with Microsoft Edge</span></span>

<span data-ttu-id="2b114-199">瀏覽器設定預設為 Google Chrome。</span><span class="sxs-lookup"><span data-stu-id="2b114-199">Browser configuration defaults to Google Chrome.</span></span> <span data-ttu-id="2b114-200">當您使用 Microsoft Edge 進行偵錯工具時，請將設定 `browser` 為 `edge` 。</span><span class="sxs-lookup"><span data-stu-id="2b114-200">When using Microsoft Edge for debugging, set `browser` to `edge`.</span></span> <span data-ttu-id="2b114-201">若要使用 Google Chrome，請不要設定 `browser` 選項，或將選項的值設定為 `chrome` 。</span><span class="sxs-lookup"><span data-stu-id="2b114-201">To use Google Chrome, either don't set the `browser` option or set the option's value to `chrome`.</span></span>

```json
{
  "name": "Launch and Debug Hosted :::no-loc(Blazor WebAssembly)::: App",
  "type": "blazorwasm",
  "request": "launch",
  "hosted": true,
  "program": "${workspaceFolder}/Server/bin/Debug/netcoreapp3.1/MyHostedApp.Server.dll",
  "cwd": "${workspaceFolder}/Server",
  "browser": "edge"
}
```

<span data-ttu-id="2b114-202">在上述範例中， `MyHostedApp.Server.dll` 是*伺服器*應用程式的元件。</span><span class="sxs-lookup"><span data-stu-id="2b114-202">In the preceding example, `MyHostedApp.Server.dll` is the *Server* app's assembly.</span></span> <span data-ttu-id="2b114-203">`.vscode`資料夾位於 `Client` 、 `Server` 和資料夾旁的方案資料夾中 `Shared` 。</span><span class="sxs-lookup"><span data-stu-id="2b114-203">The `.vscode` folder is located in the solution's folder next to the `Client`, `Server`, and `Shared` folders.</span></span>

## <a name="debug-in-the-browser"></a><span data-ttu-id="2b114-204">瀏覽器中的 Debug</span><span class="sxs-lookup"><span data-stu-id="2b114-204">Debug in the browser</span></span>

1. <span data-ttu-id="2b114-205">在開發環境中執行應用程式的 Debug 組建。</span><span class="sxs-lookup"><span data-stu-id="2b114-205">Run a Debug build of the app in the Development environment.</span></span>

1. <span data-ttu-id="2b114-206">啟動瀏覽器並流覽至應用程式的 URL （例如， `https://localhost:5001` ）。</span><span class="sxs-lookup"><span data-stu-id="2b114-206">Launch a browser and navigate to the app's URL (for example, `https://localhost:5001`).</span></span>

1. <span data-ttu-id="2b114-207">在瀏覽器中，按下<kbd>Shift</kbd> + <kbd>Alt</kbd> + <kbd>D</kbd>，嘗試開始進行遠端偵錯程式。</span><span class="sxs-lookup"><span data-stu-id="2b114-207">In the browser, attempt to commence remote debugging by pressing <kbd>Shift</kbd>+<kbd>Alt</kbd>+<kbd>D</kbd>.</span></span>

   <span data-ttu-id="2b114-208">瀏覽器必須在啟用遠端偵測功能的情況下執行，這不是預設值。</span><span class="sxs-lookup"><span data-stu-id="2b114-208">The browser must be running with remote debugging enabled, which isn't the default.</span></span> <span data-ttu-id="2b114-209">如果已停用遠端偵錯程式，將會顯示 [**找不到可調試的瀏覽器]** 索引標籤錯誤頁面，並提供啟動瀏覽器並開啟偵錯工具的指示。</span><span class="sxs-lookup"><span data-stu-id="2b114-209">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is rendered with instructions for launching the browser with the debugging port open.</span></span> <span data-ttu-id="2b114-210">遵循瀏覽器的指示，這會開啟新的瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="2b114-210">Follow the instructions for your browser, which opens a new browser window.</span></span> <span data-ttu-id="2b114-211">關閉先前的瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="2b114-211">Close the previous browser window.</span></span>

1. <span data-ttu-id="2b114-212">一旦瀏覽器在啟用遠端偵錯程式的情況下執行，偵錯工具鍵盤快速鍵（<kbd>Shift</kbd> + <kbd>Alt</kbd> + <kbd>D</kbd>）就會開啟新的偵錯工具索引標籤。</span><span class="sxs-lookup"><span data-stu-id="2b114-212">Once the browser is running with remote debugging enabled, the debugging keyboard shortcut (<kbd>Shift</kbd>+<kbd>Alt</kbd>+<kbd>D</kbd>) opens a new debugger tab.</span></span>

1. <span data-ttu-id="2b114-213">經過一段時間之後，[**來源**] 索引標籤會顯示該節點內的應用程式 .net 元件清單 `file://` 。</span><span class="sxs-lookup"><span data-stu-id="2b114-213">After a moment, the **Sources** tab shows a list of the app's .NET assemblies within the `file://` node.</span></span>

1. <span data-ttu-id="2b114-214">在元件程式碼（檔案 `.razor` ）和 c # 程式碼檔案（ `.cs` ）中，您設定的中斷點會在執行程式碼時叫用。</span><span class="sxs-lookup"><span data-stu-id="2b114-214">In component code (`.razor` files) and C# code files (`.cs`), breakpoints that you set are hit when code executes.</span></span> <span data-ttu-id="2b114-215">叫用中斷點之後，以單一步驟（<kbd>F10</kbd>），透過程式碼或繼續（<kbd>F8</kbd>）程式碼執行正常。</span><span class="sxs-lookup"><span data-stu-id="2b114-215">After a breakpoint is hit, single-step (<kbd>F10</kbd>) through the code or resume (<kbd>F8</kbd>) code execution normally.</span></span>

<span data-ttu-id="2b114-216">:::no-loc(Blazor):::提供的偵錯工具 proxy 會執行[Chrome DevTools 通訊協定](https://chromedevtools.github.io/devtools-protocol/)，並使用來擴充通訊協定。NET 特定資訊。</span><span class="sxs-lookup"><span data-stu-id="2b114-216">:::no-loc(Blazor)::: provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="2b114-217">當您按下 [調試鍵盤快速鍵] 時，會將 :::no-loc(Blazor)::: Chrome DevTools 指向 proxy。</span><span class="sxs-lookup"><span data-stu-id="2b114-217">When debugging keyboard shortcut is pressed, :::no-loc(Blazor)::: points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="2b114-218">Proxy 會連線到您想要進行調試的瀏覽器視窗（因此需要啟用遠端偵錯）。</span><span class="sxs-lookup"><span data-stu-id="2b114-218">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="2b114-219">瀏覽器來源對應</span><span class="sxs-lookup"><span data-stu-id="2b114-219">Browser source maps</span></span>

<span data-ttu-id="2b114-220">瀏覽器來源對應可讓瀏覽器將已編譯的檔案對應回原始來源檔案，而且通常會用於用戶端的偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="2b114-220">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="2b114-221">不過， :::no-loc(Blazor)::: 目前不會將 c # 直接對應至 JavaScript/WASM。</span><span class="sxs-lookup"><span data-stu-id="2b114-221">However, :::no-loc(Blazor)::: doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="2b114-222">相反地， :::no-loc(Blazor)::: 會在瀏覽器中進行 IL 轉譯，因此來源對應不相關。</span><span class="sxs-lookup"><span data-stu-id="2b114-222">Instead, :::no-loc(Blazor)::: does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="2b114-223">疑難排解</span><span class="sxs-lookup"><span data-stu-id="2b114-223">Troubleshoot</span></span>

<span data-ttu-id="2b114-224">如果您遇到錯誤，下列秘訣可能會有説明：</span><span class="sxs-lookup"><span data-stu-id="2b114-224">If you're running into errors, the following tips may help:</span></span>

* <span data-ttu-id="2b114-225">在 [**偵錯工具**] 索引標籤中，開啟瀏覽器中的開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="2b114-225">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="2b114-226">在主控台中，執行 `localStorage.clear()` 以移除任何中斷點。</span><span class="sxs-lookup"><span data-stu-id="2b114-226">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
* <span data-ttu-id="2b114-227">確認您已安裝並信任 ASP.NET Core 的 HTTPS 開發憑證。</span><span class="sxs-lookup"><span data-stu-id="2b114-227">Confirm that you've installed and trusted the ASP.NET Core HTTPS development certificate.</span></span> <span data-ttu-id="2b114-228">如需詳細資訊，請參閱 <xref:security/enforcing-ssl#troubleshoot-certificate-problems> 。</span><span class="sxs-lookup"><span data-stu-id="2b114-228">For more information, see <xref:security/enforcing-ssl#troubleshoot-certificate-problems>.</span></span>
* <span data-ttu-id="2b114-229">Visual Studio 需要 [**工具**] [選項] [一般] 中的 [**啟用 ASP.NET （Chrome、Edge 和 IE）的 JavaScript 偵錯工具**] 選項  >  **Options**  >  **Debugging**  >  \*\* \*\*。</span><span class="sxs-lookup"><span data-stu-id="2b114-229">Visual Studio requires the **Enable JavaScript debugging for ASP.NET (Chrome, Edge and IE)** option in **Tools** > **Options** > **Debugging** > **General**.</span></span> <span data-ttu-id="2b114-230">這是 Visual Studio 的預設設定。</span><span class="sxs-lookup"><span data-stu-id="2b114-230">This is the default setting for Visual Studio.</span></span> <span data-ttu-id="2b114-231">如果偵錯工具無法運作，請確認已選取此選項。</span><span class="sxs-lookup"><span data-stu-id="2b114-231">If debugging isn't working, confirm that the option is selected.</span></span>
