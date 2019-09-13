---
title: Debug ASP.NET Core Blazor
author: guardrex
description: 瞭解如何調試 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/22/2019
uid: blazor/debug
ms.openlocfilehash: e9477e504d32fd1dd5d6c87392386d1131f46e9f
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2019
ms.locfileid: "70964001"
---
# <a name="debug-aspnet-core-blazor"></a><span data-ttu-id="5a757-103">Debug ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="5a757-103">Debug ASP.NET Core Blazor</span></span>

[<span data-ttu-id="5a757-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="5a757-104">Daniel Roth</span></span>](https://github.com/danroth27)

<span data-ttu-id="5a757-105">在 Chrome 的 WebAssembly 上執行的 Blazor WebAssembly 應用程式，有舊版*的支援。*</span><span class="sxs-lookup"><span data-stu-id="5a757-105">*Early* support exists for debugging Blazor WebAssembly apps running on WebAssembly in Chrome.</span></span>

<span data-ttu-id="5a757-106">偵錯工具功能有限。</span><span class="sxs-lookup"><span data-stu-id="5a757-106">Debugger capabilities are limited.</span></span> <span data-ttu-id="5a757-107">可用的案例包括：</span><span class="sxs-lookup"><span data-stu-id="5a757-107">Available scenarios include:</span></span>

* <span data-ttu-id="5a757-108">設定和移除中斷點。</span><span class="sxs-lookup"><span data-stu-id="5a757-108">Set and remove breakpoints.</span></span>
* <span data-ttu-id="5a757-109">透過程式碼或`F10`繼續（`F8`）程式碼執行的單一步驟（）。</span><span class="sxs-lookup"><span data-stu-id="5a757-109">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="5a757-110">在 [*區域變數*] 顯示中，觀察、 `int` `string`和`bool`類型之任何本機變數的值。</span><span class="sxs-lookup"><span data-stu-id="5a757-110">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="5a757-111">查看呼叫堆疊，包括從 JavaScript 轉換成 .NET，以及從 .NET 到 JavaScript 的呼叫鏈。</span><span class="sxs-lookup"><span data-stu-id="5a757-111">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="5a757-112">您*不能*：</span><span class="sxs-lookup"><span data-stu-id="5a757-112">You *can't*:</span></span>

* <span data-ttu-id="5a757-113">觀察不是`int`、 `string`或`bool`之任何區域變數的值。</span><span class="sxs-lookup"><span data-stu-id="5a757-113">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="5a757-114">觀察任何類別屬性或欄位的值。</span><span class="sxs-lookup"><span data-stu-id="5a757-114">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="5a757-115">將滑鼠停留在變數上以查看其值。</span><span class="sxs-lookup"><span data-stu-id="5a757-115">Hover over variables to see their values.</span></span>
* <span data-ttu-id="5a757-116">評估主控台中的運算式。</span><span class="sxs-lookup"><span data-stu-id="5a757-116">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="5a757-117">逐步執行跨非同步呼叫。</span><span class="sxs-lookup"><span data-stu-id="5a757-117">Step across async calls.</span></span>
* <span data-ttu-id="5a757-118">執行大部分其他的一般調試情況。</span><span class="sxs-lookup"><span data-stu-id="5a757-118">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="5a757-119">開發進一步的偵錯工具，是工程小組的持續焦點。</span><span class="sxs-lookup"><span data-stu-id="5a757-119">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5a757-120">必要條件</span><span class="sxs-lookup"><span data-stu-id="5a757-120">Prerequisites</span></span>

<span data-ttu-id="5a757-121">調試需要下列其中一個瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="5a757-121">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="5a757-122">Google Chrome （版本70或更新版本）</span><span class="sxs-lookup"><span data-stu-id="5a757-122">Google Chrome (version 70 or later)</span></span>
* <span data-ttu-id="5a757-123">Microsoft Edge 預覽（[Edge 開發人員通道](https://www.microsoftedgeinsider.com)）</span><span class="sxs-lookup"><span data-stu-id="5a757-123">Microsoft Edge preview ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span></span>

## <a name="procedure"></a><span data-ttu-id="5a757-124">程序</span><span class="sxs-lookup"><span data-stu-id="5a757-124">Procedure</span></span>

1. <span data-ttu-id="5a757-125">在 configuration 中`Debug`執行 Blazor WebAssembly 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5a757-125">Run a Blazor WebAssembly app in `Debug` configuration.</span></span> <span data-ttu-id="5a757-126">將選項傳遞至[dotnet 執行](/dotnet/core/tools/dotnet-run)命令： `dotnet run --configuration Debug`。 `--configuration Debug`</span><span class="sxs-lookup"><span data-stu-id="5a757-126">Pass the `--configuration Debug` option to the [dotnet run](/dotnet/core/tools/dotnet-run) command: `dotnet run --configuration Debug`.</span></span>
1. <span data-ttu-id="5a757-127">在瀏覽器中存取應用程式。</span><span class="sxs-lookup"><span data-stu-id="5a757-127">Access the app in the browser.</span></span>
1. <span data-ttu-id="5a757-128">將鍵盤焦點放在應用程式，而不是 [開發人員工具] 面板。</span><span class="sxs-lookup"><span data-stu-id="5a757-128">Place the keyboard focus on the app, not the developer tools panel.</span></span> <span data-ttu-id="5a757-129">起始調試時，可以關閉開發人員工具面板。</span><span class="sxs-lookup"><span data-stu-id="5a757-129">The developer tools panel can be closed when debugging is initiated.</span></span>
1. <span data-ttu-id="5a757-130">選取下列 Blazor 特定的鍵盤快速鍵：</span><span class="sxs-lookup"><span data-stu-id="5a757-130">Select the following Blazor-specific keyboard shortcut:</span></span>
   * <span data-ttu-id="5a757-131">`Shift+Alt+D`在 Windows/Linux 上</span><span class="sxs-lookup"><span data-stu-id="5a757-131">`Shift+Alt+D` on Windows/Linux</span></span>
   * <span data-ttu-id="5a757-132">`Shift+Cmd+D`在 macOS 上</span><span class="sxs-lookup"><span data-stu-id="5a757-132">`Shift+Cmd+D` on macOS</span></span>
1. <span data-ttu-id="5a757-133">依照畫面上所列的步驟，在啟用遠端偵測的情況下重新開機瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="5a757-133">Follow the steps listed on the screen to restart the browser with remote debugging enabled.</span></span>
1. <span data-ttu-id="5a757-134">再次選取下列 Blazor 特定的鍵盤快速鍵，以啟動 [debug] 會話：</span><span class="sxs-lookup"><span data-stu-id="5a757-134">Select the following Blazor-specific keyboard shortcut once again to start the debug session:</span></span>
   * <span data-ttu-id="5a757-135">`Shift+Alt+D`在 Windows/Linux 上</span><span class="sxs-lookup"><span data-stu-id="5a757-135">`Shift+Alt+D` on Windows/Linux</span></span>
   * <span data-ttu-id="5a757-136">`Shift+Cmd+D`在 macOS 上</span><span class="sxs-lookup"><span data-stu-id="5a757-136">`Shift+Cmd+D` on macOS</span></span>

## <a name="enable-remote-debugging"></a><span data-ttu-id="5a757-137">啟用遠端偵錯</span><span class="sxs-lookup"><span data-stu-id="5a757-137">Enable remote debugging</span></span>

<span data-ttu-id="5a757-138">如果已停用遠端偵錯程式，則 Chrome 會產生 [**找不到可調試的瀏覽器]** 索引標籤錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="5a757-138">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="5a757-139">錯誤頁面包含執行 Chrome 並開啟偵錯工具埠的指示，讓 Blazor 的偵錯工具 proxy 可以連接到應用程式。</span><span class="sxs-lookup"><span data-stu-id="5a757-139">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="5a757-140">*關閉所有 Chrome 實例*，並依指示重新開機 Chrome。</span><span class="sxs-lookup"><span data-stu-id="5a757-140">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="5a757-141">偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="5a757-141">Debug the app</span></span>

<span data-ttu-id="5a757-142">當 Chrome 在啟用遠端偵錯程式的情況下執行時，[偵錯工具] 鍵盤快速鍵會開啟新的 [偵錯工具]經過一段時間之後，[**來源**] 索引標籤會顯示應用程式中的 .net 元件清單。</span><span class="sxs-lookup"><span data-stu-id="5a757-142">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="5a757-143">展開每個元件，並尋找可用來進行偵錯工具的 *.cs* /*原始檔案。*</span><span class="sxs-lookup"><span data-stu-id="5a757-143">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="5a757-144">設定中斷點、切換回應用程式的索引標籤，並在程式碼執行時叫用中斷點。</span><span class="sxs-lookup"><span data-stu-id="5a757-144">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="5a757-145">到達中斷點之後，透過程式碼或繼續（`F10` `F8`）程式碼執行的單一步驟（）。</span><span class="sxs-lookup"><span data-stu-id="5a757-145">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

<span data-ttu-id="5a757-146">Blazor 提供的偵錯工具 proxy 會執行[Chrome DevTools 通訊協定](https://chromedevtools.github.io/devtools-protocol/)，並使用來擴充通訊協定。NET 特定資訊。</span><span class="sxs-lookup"><span data-stu-id="5a757-146">Blazor provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="5a757-147">當您按下 [調試鍵盤快速鍵] 時，Blazor 會將 Chrome DevTools 指向 proxy。</span><span class="sxs-lookup"><span data-stu-id="5a757-147">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="5a757-148">Proxy 會連線到您想要進行調試的瀏覽器視窗（因此需要啟用遠端偵錯）。</span><span class="sxs-lookup"><span data-stu-id="5a757-148">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="5a757-149">瀏覽器來源對應</span><span class="sxs-lookup"><span data-stu-id="5a757-149">Browser source maps</span></span>

<span data-ttu-id="5a757-150">瀏覽器來源對應可讓瀏覽器將已編譯的檔案對應回原始來源檔案，而且通常會用於用戶端的偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="5a757-150">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="5a757-151">不過，Blazor 目前不會C#直接對應至 JAVASCRIPT/WASM。</span><span class="sxs-lookup"><span data-stu-id="5a757-151">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="5a757-152">相反地，Blazor 會在瀏覽器中執行 IL 轉譯，因此來源對應並不相關。</span><span class="sxs-lookup"><span data-stu-id="5a757-152">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshooting-tip"></a><span data-ttu-id="5a757-153">疑難排解秘訣</span><span class="sxs-lookup"><span data-stu-id="5a757-153">Troubleshooting tip</span></span>

<span data-ttu-id="5a757-154">如果您遇到錯誤，下列秘訣可能會有説明：</span><span class="sxs-lookup"><span data-stu-id="5a757-154">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="5a757-155">在 [**偵錯工具**] 索引標籤中，開啟瀏覽器中的開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="5a757-155">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="5a757-156">在主控台中，執行`localStorage.clear()`以移除任何中斷點。</span><span class="sxs-lookup"><span data-stu-id="5a757-156">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
