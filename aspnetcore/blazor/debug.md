---
title: Debug ASP.NET Core Blazor
author: guardrex
description: 瞭解如何 Blazor 應用程式進行 debug。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 1b0035af48b82807a6ae14835a41a1ecbef06bb6
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/17/2020
ms.locfileid: "76159985"
---
# <a name="debug-aspnet-core-opno-locblazor"></a><span data-ttu-id="207fe-103">Debug ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="207fe-103">Debug ASP.NET Core Blazor</span></span>

[<span data-ttu-id="207fe-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="207fe-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="207fe-105">在以 Chromium 為基礎的瀏覽器中使用瀏覽器開發工具（Chrome/Edge）進行偵錯工具 Blazor WebAssembly 時，會有*早期*支援。</span><span class="sxs-lookup"><span data-stu-id="207fe-105">*Early* support exists for debugging Blazor WebAssembly using the browser dev tools in Chromium-based browsers (Chrome/Edge).</span></span> <span data-ttu-id="207fe-106">工作進行中：</span><span class="sxs-lookup"><span data-stu-id="207fe-106">Work is in progress to:</span></span>

* <span data-ttu-id="207fe-107">在 Visual Studio 中完全啟用調試。</span><span class="sxs-lookup"><span data-stu-id="207fe-107">Fully enable debugging in Visual Studio.</span></span>
* <span data-ttu-id="207fe-108">在 Visual Studio Code 中啟用調試。</span><span class="sxs-lookup"><span data-stu-id="207fe-108">Enable debugging in Visual Studio Code.</span></span>

<span data-ttu-id="207fe-109">偵錯工具功能有限。</span><span class="sxs-lookup"><span data-stu-id="207fe-109">Debugger capabilities are limited.</span></span> <span data-ttu-id="207fe-110">可用的案例包括：</span><span class="sxs-lookup"><span data-stu-id="207fe-110">Available scenarios include:</span></span>

* <span data-ttu-id="207fe-111">設定和移除中斷點。</span><span class="sxs-lookup"><span data-stu-id="207fe-111">Set and remove breakpoints.</span></span>
* <span data-ttu-id="207fe-112">透過程式碼或繼續（`F8`）程式碼執行的單一步驟（`F10`）。</span><span class="sxs-lookup"><span data-stu-id="207fe-112">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="207fe-113">在 [*區域變數*] 顯示中，觀察 `int`、`string`和 `bool`類型的任何本機變數值。</span><span class="sxs-lookup"><span data-stu-id="207fe-113">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="207fe-114">查看呼叫堆疊，包括從 JavaScript 轉換成 .NET，以及從 .NET 到 JavaScript 的呼叫鏈。</span><span class="sxs-lookup"><span data-stu-id="207fe-114">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="207fe-115">您*不能*：</span><span class="sxs-lookup"><span data-stu-id="207fe-115">You *can't*:</span></span>

* <span data-ttu-id="207fe-116">觀察不是 `int`、`string`或 `bool`之任何區域變數的值。</span><span class="sxs-lookup"><span data-stu-id="207fe-116">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="207fe-117">觀察任何類別屬性或欄位的值。</span><span class="sxs-lookup"><span data-stu-id="207fe-117">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="207fe-118">將滑鼠停留在變數上以查看其值。</span><span class="sxs-lookup"><span data-stu-id="207fe-118">Hover over variables to see their values.</span></span>
* <span data-ttu-id="207fe-119">評估主控台中的運算式。</span><span class="sxs-lookup"><span data-stu-id="207fe-119">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="207fe-120">逐步執行跨非同步呼叫。</span><span class="sxs-lookup"><span data-stu-id="207fe-120">Step across async calls.</span></span>
* <span data-ttu-id="207fe-121">執行大部分其他的一般調試情況。</span><span class="sxs-lookup"><span data-stu-id="207fe-121">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="207fe-122">開發進一步的偵錯工具，是工程小組的持續焦點。</span><span class="sxs-lookup"><span data-stu-id="207fe-122">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="207fe-123">必要條件：</span><span class="sxs-lookup"><span data-stu-id="207fe-123">Prerequisites</span></span>

<span data-ttu-id="207fe-124">調試需要下列其中一個瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="207fe-124">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="207fe-125">Google Chrome （版本70或更新版本）</span><span class="sxs-lookup"><span data-stu-id="207fe-125">Google Chrome (version 70 or later)</span></span>
* <span data-ttu-id="207fe-126">Microsoft Edge 預覽（[Edge 開發人員通道](https://www.microsoftedgeinsider.com)）</span><span class="sxs-lookup"><span data-stu-id="207fe-126">Microsoft Edge preview ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span></span>

## <a name="procedure"></a><span data-ttu-id="207fe-127">程序</span><span class="sxs-lookup"><span data-stu-id="207fe-127">Procedure</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="207fe-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="207fe-128">Visual Studio</span></span>](#tab/visual-studio)

> [!WARNING]
> <span data-ttu-id="207fe-129">Visual Studio 中的偵錯工具支援是在開發的初期階段。</span><span class="sxs-lookup"><span data-stu-id="207fe-129">Debugging support in Visual Studio is at an early stage of development.</span></span> <span data-ttu-id="207fe-130">目前不支援**F5**調試。</span><span class="sxs-lookup"><span data-stu-id="207fe-130">**F5** debugging isn't currently supported.</span></span>

1. <span data-ttu-id="207fe-131">在 `Debug` 設定中執行 Blazor WebAssembly 應用程式而不進行任何偵測（**Ctrl**+**f5** ，而不是**f5**）。</span><span class="sxs-lookup"><span data-stu-id="207fe-131">Run a Blazor WebAssembly app in `Debug` configuration without debugging (**Ctrl**+**F5** instead of **F5**).</span></span>
1. <span data-ttu-id="207fe-132">開啟應用程式的 [Debug] 屬性（[ **debug** ] 功能表中的最後一個專案），然後複製 HTTP**應用程式 URL**。</span><span class="sxs-lookup"><span data-stu-id="207fe-132">Open the Debug properties of the app (last entry in the **Debug** menu) and copy the HTTP **App URL**.</span></span> <span data-ttu-id="207fe-133">使用以 Chromium 為基礎的瀏覽器（Edge Beta 或 Chrome），流覽至應用程式的 HTTP 位址（而非 HTTPS 位址）。</span><span class="sxs-lookup"><span data-stu-id="207fe-133">Browse to the HTTP address (not the HTTPS address) of the app using a Chromium-based browser (Edge Beta or Chrome).</span></span>
1. <span data-ttu-id="207fe-134">將鍵盤焦點放在應用程式的瀏覽器視窗中，而不是 [開發人員工具] 面板。</span><span class="sxs-lookup"><span data-stu-id="207fe-134">Place the keyboard focus on the app in the browser window, not the developer tools panel.</span></span> <span data-ttu-id="207fe-135">最好讓開發人員工具面板在此程式中關閉。</span><span class="sxs-lookup"><span data-stu-id="207fe-135">It's best to keep the developer tools panel closed for this procedure.</span></span> <span data-ttu-id="207fe-136">啟動偵錯工具之後，您可以重新開啟 [開發人員工具] 面板。</span><span class="sxs-lookup"><span data-stu-id="207fe-136">After debugging has started, you can re-open the developer tools panel.</span></span>
1. <span data-ttu-id="207fe-137">選取下列 Blazor特定的鍵盤快速鍵：</span><span class="sxs-lookup"><span data-stu-id="207fe-137">Select the following Blazor-specific keyboard shortcut:</span></span>

   * <span data-ttu-id="207fe-138">Windows 上的 `Shift+Alt+D`</span><span class="sxs-lookup"><span data-stu-id="207fe-138">`Shift+Alt+D` on Windows</span></span>
   * <span data-ttu-id="207fe-139">macOS 上的 `Shift+Cmd+D`</span><span class="sxs-lookup"><span data-stu-id="207fe-139">`Shift+Cmd+D` on macOS</span></span>

   <span data-ttu-id="207fe-140">如果您收到 [**找不到可調試的瀏覽器]** 索引標籤，請參閱[啟用遠端偵錯](#enable-remote-debugging)。</span><span class="sxs-lookup"><span data-stu-id="207fe-140">If you receive the **Unable to find debuggable browser tab**, see [Enable remote debugging](#enable-remote-debugging).</span></span>
   
   <span data-ttu-id="207fe-141">啟用遠端偵錯之後：</span><span class="sxs-lookup"><span data-stu-id="207fe-141">After enabling remote debugging:</span></span>
   
   <span data-ttu-id="207fe-142">1\.</span><span class="sxs-lookup"><span data-stu-id="207fe-142">1\.</span></span> <span data-ttu-id="207fe-143">新的瀏覽器視窗隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="207fe-143">A new browser window opens.</span></span> <span data-ttu-id="207fe-144">關閉先前的視窗。</span><span class="sxs-lookup"><span data-stu-id="207fe-144">Close the prior window.</span></span>

   <span data-ttu-id="207fe-145">2\.</span><span class="sxs-lookup"><span data-stu-id="207fe-145">2\.</span></span> <span data-ttu-id="207fe-146">將鍵盤焦點放在應用程式的瀏覽器視窗中。</span><span class="sxs-lookup"><span data-stu-id="207fe-146">Place the keyboard focus on the app in the browser window.</span></span>

   <span data-ttu-id="207fe-147">3\.</span><span class="sxs-lookup"><span data-stu-id="207fe-147">3\.</span></span> <span data-ttu-id="207fe-148">在新的瀏覽器視窗中選取 Blazor特定的鍵盤快速鍵：在 Windows 上 `Shift+Alt+D`，或在 macOS 上 `Shift+Cmd+D`。</span><span class="sxs-lookup"><span data-stu-id="207fe-148">Select the Blazor-specific keyboard shortcut in the new browser window: `Shift+Alt+D` on Windows or `Shift+Cmd+D` on macOS.</span></span>

   <span data-ttu-id="207fe-149">4\.</span><span class="sxs-lookup"><span data-stu-id="207fe-149">4\.</span></span> <span data-ttu-id="207fe-150">[ **DevTools** ] 索引標籤會在瀏覽器中開啟。</span><span class="sxs-lookup"><span data-stu-id="207fe-150">The **DevTools** tab opens in the browser.</span></span> <span data-ttu-id="207fe-151">**在瀏覽器視窗中重新選擇應用程式的索引標籤。**</span><span class="sxs-lookup"><span data-stu-id="207fe-151">**Reselect the app's tab in the browser window.**</span></span>

   <span data-ttu-id="207fe-152">若要將應用程式附加至 Visual Studio，請參閱[Visual Studio 中的附加至進程](#attach-to-process-in-visual-studio)一節。</span><span class="sxs-lookup"><span data-stu-id="207fe-152">To attach the app to Visual Studio, see the [Attach to process in Visual Studio](#attach-to-process-in-visual-studio) section.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="207fe-153">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="207fe-153">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="207fe-154">藉由將 `--configuration Debug` 選項傳遞至[dotnet 執行](/dotnet/core/tools/dotnet-run)命令，在 `Debug` 設定中執行 Blazor WebAssembly 應用程式： `dotnet run --configuration Debug`。</span><span class="sxs-lookup"><span data-stu-id="207fe-154">Run a Blazor WebAssembly app in `Debug` configuration by passing the `--configuration Debug` option to the [dotnet run](/dotnet/core/tools/dotnet-run) command: `dotnet run --configuration Debug`.</span></span>
1. <span data-ttu-id="207fe-155">在 shell 視窗中顯示的 HTTP URL 流覽至應用程式。</span><span class="sxs-lookup"><span data-stu-id="207fe-155">Navigate to the app at the HTTP URL shown in the shell's window.</span></span>
1. <span data-ttu-id="207fe-156">將鍵盤焦點放在應用程式，而不是 [開發人員工具] 面板。</span><span class="sxs-lookup"><span data-stu-id="207fe-156">Place the keyboard focus on the app, not the developer tools panel.</span></span> <span data-ttu-id="207fe-157">最好讓開發人員工具面板在此程式中關閉。</span><span class="sxs-lookup"><span data-stu-id="207fe-157">It's best to keep the developer tools panel closed for this procedure.</span></span> <span data-ttu-id="207fe-158">啟動偵錯工具之後，您可以重新開啟 [開發人員工具] 面板。</span><span class="sxs-lookup"><span data-stu-id="207fe-158">After debugging has started, you can re-open the developer tools panel.</span></span>
1. <span data-ttu-id="207fe-159">選取下列 Blazor特定的鍵盤快速鍵：</span><span class="sxs-lookup"><span data-stu-id="207fe-159">Select the following Blazor-specific keyboard shortcut:</span></span>

   * <span data-ttu-id="207fe-160">Windows 上的 `Shift+Alt+D`</span><span class="sxs-lookup"><span data-stu-id="207fe-160">`Shift+Alt+D` on Windows</span></span>
   * <span data-ttu-id="207fe-161">macOS 上的 `Shift+Cmd+D`</span><span class="sxs-lookup"><span data-stu-id="207fe-161">`Shift+Cmd+D` on macOS</span></span>

   <span data-ttu-id="207fe-162">如果您收到 [**找不到可調試的瀏覽器]** 索引標籤，請參閱[啟用遠端偵錯](#enable-remote-debugging)。</span><span class="sxs-lookup"><span data-stu-id="207fe-162">If you receive the **Unable to find debuggable browser tab**, see [Enable remote debugging](#enable-remote-debugging).</span></span>
   
   <span data-ttu-id="207fe-163">啟用遠端偵錯之後：</span><span class="sxs-lookup"><span data-stu-id="207fe-163">After enabling remote debugging:</span></span>
   
   <span data-ttu-id="207fe-164">1\.</span><span class="sxs-lookup"><span data-stu-id="207fe-164">1\.</span></span> <span data-ttu-id="207fe-165">新的瀏覽器視窗隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="207fe-165">A new browser window opens.</span></span> <span data-ttu-id="207fe-166">關閉先前的視窗。</span><span class="sxs-lookup"><span data-stu-id="207fe-166">Close the prior window.</span></span>

   <span data-ttu-id="207fe-167">2\.</span><span class="sxs-lookup"><span data-stu-id="207fe-167">2\.</span></span> <span data-ttu-id="207fe-168">將鍵盤焦點放在應用程式的瀏覽器視窗中，而不是 [開發人員工具] 面板。</span><span class="sxs-lookup"><span data-stu-id="207fe-168">Place the keyboard focus on the app in the browser window, not the developer tools panel.</span></span>

   <span data-ttu-id="207fe-169">3\.</span><span class="sxs-lookup"><span data-stu-id="207fe-169">3\.</span></span> <span data-ttu-id="207fe-170">在新的瀏覽器視窗中選取 Blazor特定的鍵盤快速鍵：在 Windows 上 `Shift+Alt+D`，或在 macOS 上 `Shift+Cmd+D`。</span><span class="sxs-lookup"><span data-stu-id="207fe-170">Select the Blazor-specific keyboard shortcut in the new browser window: `Shift+Alt+D` on Windows or `Shift+Cmd+D` on macOS.</span></span>

---

## <a name="enable-remote-debugging"></a><span data-ttu-id="207fe-171">啟用遠端偵錯</span><span class="sxs-lookup"><span data-stu-id="207fe-171">Enable remote debugging</span></span>

<span data-ttu-id="207fe-172">如果已停用遠端偵錯程式，則 Chrome 會產生 [**找不到可調試的瀏覽器]** 索引標籤錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="207fe-172">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="207fe-173">錯誤頁面包含執行 Chrome 並開啟偵錯工具埠的指示，讓 Blazor 的偵錯工具 proxy 可以連接到應用程式。</span><span class="sxs-lookup"><span data-stu-id="207fe-173">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="207fe-174">*關閉所有 Chrome 實例*，並依指示重新開機 Chrome。</span><span class="sxs-lookup"><span data-stu-id="207fe-174">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="207fe-175">偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="207fe-175">Debug the app</span></span>

<span data-ttu-id="207fe-176">當 Chrome 在啟用遠端偵錯程式的情況下執行時，[偵錯工具] 鍵盤快速鍵會開啟新的 [偵錯工具]經過一段時間之後，[**來源**] 索引標籤會顯示應用程式中的 .net 元件清單。</span><span class="sxs-lookup"><span data-stu-id="207fe-176">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="207fe-177">展開每個元件，並找出可用來進行偵錯工具的 *.cs*/ *. razor*原始程式檔。</span><span class="sxs-lookup"><span data-stu-id="207fe-177">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="207fe-178">設定中斷點、切換回應用程式的索引標籤，並在程式碼執行時叫用中斷點。</span><span class="sxs-lookup"><span data-stu-id="207fe-178">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="207fe-179">叫用中斷點之後，透過程式碼或繼續（`F8`）程式碼執行的單一步驟（`F10`）。</span><span class="sxs-lookup"><span data-stu-id="207fe-179">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

Blazor<span data-ttu-id="207fe-180"> 提供可執行[Chrome DevTools 通訊協定](https://chromedevtools.github.io/devtools-protocol/)，並以擴充通訊協定的偵錯工具 proxy。NET 特定資訊。</span><span class="sxs-lookup"><span data-stu-id="207fe-180"> provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="207fe-181">當您按下 [調試鍵盤快速鍵] 時，Blazor 會指向位於 proxy 的 Chrome DevTools。</span><span class="sxs-lookup"><span data-stu-id="207fe-181">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="207fe-182">Proxy 會連線到您想要進行調試的瀏覽器視窗（因此需要啟用遠端偵錯）。</span><span class="sxs-lookup"><span data-stu-id="207fe-182">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="attach-to-process-in-visual-studio"></a><span data-ttu-id="207fe-183">附加至 Visual Studio 中的進程</span><span class="sxs-lookup"><span data-stu-id="207fe-183">Attach to process in Visual Studio</span></span>

<span data-ttu-id="207fe-184">在 Visual Studio 中附加至應用程式的進程，是 Blazor WebAssembly 的*暫時性*偵錯工具，而**F5**調試正在開發中。</span><span class="sxs-lookup"><span data-stu-id="207fe-184">Attaching to the app's process in Visual Studio is a *temporary* debugging scenario for Blazor WebAssembly while **F5** debugging is in development.</span></span>

<span data-ttu-id="207fe-185">若要將執行中應用程式的進程附加至 Visual Studio：</span><span class="sxs-lookup"><span data-stu-id="207fe-185">To attach the running app's process to Visual Studio:</span></span>

1. <span data-ttu-id="207fe-186">在 Visual Studio 中，選取  **Debug** > **附加至進程**。</span><span class="sxs-lookup"><span data-stu-id="207fe-186">In Visual Studio, select **Debug** > **Attach to Process**.</span></span>
1. <span data-ttu-id="207fe-187">針對 [連線**類型**]，選取 [ **Chrome devtools protocol websocket （無驗證）** ]。</span><span class="sxs-lookup"><span data-stu-id="207fe-187">For the **Connection type**, select **Chrome devtools protocol websocket (no authentication)**.</span></span>
1. <span data-ttu-id="207fe-188">針對 [連線**目標**]，貼上應用程式的 HTTP 位址（而非 HTTPS 位址）。</span><span class="sxs-lookup"><span data-stu-id="207fe-188">For the **Connection target**, paste in the HTTP address (not the HTTPS address) of the app.</span></span>
1. <span data-ttu-id="207fe-189">選取 **[** 重新整理] 以重新整理 [**可用的進程**] 底下的專案。</span><span class="sxs-lookup"><span data-stu-id="207fe-189">Select **Refresh** to refresh the entries under **Available processes**.</span></span>
1. <span data-ttu-id="207fe-190">選取瀏覽器進程以進行偵錯工具，然後選取 [**附加**]。</span><span class="sxs-lookup"><span data-stu-id="207fe-190">Select the browser process to debug and select **Attach**.</span></span>
1. <span data-ttu-id="207fe-191">在 [**選取程式碼類型**] 對話方塊中，選取您要附加到（Edge 或 Chrome）之特定瀏覽器的程式碼類型，然後選取 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="207fe-191">In the **Select Code Type** dialog, select the code type for the specific browser you're attaching to (Edge or Chrome) and then select **OK**.</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="207fe-192">瀏覽器來源對應</span><span class="sxs-lookup"><span data-stu-id="207fe-192">Browser source maps</span></span>

<span data-ttu-id="207fe-193">瀏覽器來源對應可讓瀏覽器將已編譯的檔案對應回原始來源檔案，而且通常會用於用戶端的偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="207fe-193">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="207fe-194">不過，Blazor 目前不會C#直接對應至 JAVASCRIPT/WASM。</span><span class="sxs-lookup"><span data-stu-id="207fe-194">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="207fe-195">相反地，Blazor 會在瀏覽器中執行 IL 轉譯，因此來源對應不相關。</span><span class="sxs-lookup"><span data-stu-id="207fe-195">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="207fe-196">疑難排解</span><span class="sxs-lookup"><span data-stu-id="207fe-196">Troubleshoot</span></span>

<span data-ttu-id="207fe-197">如果您遇到錯誤，下列秘訣可能會有説明：</span><span class="sxs-lookup"><span data-stu-id="207fe-197">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="207fe-198">在 [**偵錯工具**] 索引標籤中，開啟瀏覽器中的開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="207fe-198">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="207fe-199">在主控台中，執行 `localStorage.clear()` 以移除任何中斷點。</span><span class="sxs-lookup"><span data-stu-id="207fe-199">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
