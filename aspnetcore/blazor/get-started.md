---
title: 開始使用 ASP.NET Core Blazor
author: guardrex
description: 藉由使用您選擇的工具來建立 Blazor 應用程式，開始使用 Blazor。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/28/2019
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: bd33d874b3d6122f2ab820e9b147b0e62ba03a58
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2020
ms.locfileid: "76869576"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="99506-103">開始使用 ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="99506-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="99506-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="99506-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="99506-105">開始使用 Blazor：</span><span class="sxs-lookup"><span data-stu-id="99506-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="99506-106">安裝[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="99506-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="99506-107">選擇性安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)範本：</span><span class="sxs-lookup"><span data-stu-id="99506-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="99506-108">安裝[.Net Core 3.1 或更新版本（預覽） SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="99506-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="99506-109">在命令 shell 中執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="99506-109">Run the following command in a command shell.</span></span> <span data-ttu-id="99506-110">Blazor WebAssembly 處於預覽階段時， [AspNetCore. Blazor](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)套件具有預覽版本。</span><span class="sxs-lookup"><span data-stu-id="99506-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.2.0-preview1.20073.1
   ```

1. <span data-ttu-id="99506-111">遵循您選擇的工具的指導方針：</span><span class="sxs-lookup"><span data-stu-id="99506-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="99506-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="99506-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="99506-113">1\.</span><span class="sxs-lookup"><span data-stu-id="99506-113">1\.</span></span> <span data-ttu-id="99506-114">使用**ASP.NET 和 網頁程式開發**工作負載安裝[Visual Studio 2019 16.4 版或更新](https://visualstudio.microsoft.com/vs/preview/)版本。</span><span class="sxs-lookup"><span data-stu-id="99506-114">Install [Visual Studio 2019 version 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="99506-115">2\.</span><span class="sxs-lookup"><span data-stu-id="99506-115">2\.</span></span> <span data-ttu-id="99506-116">建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="99506-116">Create a new project.</span></span>

   <span data-ttu-id="99506-117">3\.</span><span class="sxs-lookup"><span data-stu-id="99506-117">3\.</span></span> <span data-ttu-id="99506-118">選取 [ **Blazor 應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="99506-118">Select **Blazor App**.</span></span> <span data-ttu-id="99506-119">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="99506-119">Select **Next**.</span></span>

   <span data-ttu-id="99506-120">4\.</span><span class="sxs-lookup"><span data-stu-id="99506-120">4\.</span></span> <span data-ttu-id="99506-121">在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="99506-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="99506-122">確認 [**位置**] 專案正確，或提供專案的 [位置]。</span><span class="sxs-lookup"><span data-stu-id="99506-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="99506-123">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="99506-123">Select **Create**.</span></span>

   <span data-ttu-id="99506-124">5\.</span><span class="sxs-lookup"><span data-stu-id="99506-124">5\.</span></span> <span data-ttu-id="99506-125">如需 Blazor 的 WebAssembly 體驗，請選擇 [ **Blazor WebAssembly 應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="99506-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="99506-126">如需 Blazor 伺服器體驗，請選擇 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="99506-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="99506-127">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="99506-127">Select **Create**.</span></span> <span data-ttu-id="99506-128">如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="99506-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="99506-129">6\.</span><span class="sxs-lookup"><span data-stu-id="99506-129">6\.</span></span> <span data-ttu-id="99506-130">按下 **Ctrl**+**F5** 即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="99506-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="99506-131">如果您已安裝 ASP.NET Core Blazor （Preview 6 或更早版本）先前預覽版本的 Blazor Visual Studio 延伸模組，則可以卸載擴充功能。</span><span class="sxs-lookup"><span data-stu-id="99506-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="99506-132">在命令介面中安裝 Blazor 範本，現在已足以在 Visual Studio 中呈現範本。</span><span class="sxs-lookup"><span data-stu-id="99506-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="99506-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="99506-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="99506-134">1\.</span><span class="sxs-lookup"><span data-stu-id="99506-134">1\.</span></span> <span data-ttu-id="99506-135">安裝 [Visual Studio Code (英文)](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="99506-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="99506-136">2\.</span><span class="sxs-lookup"><span data-stu-id="99506-136">2\.</span></span> <span data-ttu-id="99506-137">安裝[ C# Visual Studio Code 擴充](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)功能的最新版本。</span><span class="sxs-lookup"><span data-stu-id="99506-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="99506-138">3\.</span><span class="sxs-lookup"><span data-stu-id="99506-138">3\.</span></span> <span data-ttu-id="99506-139">如需 Blazor 的 WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="99506-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="99506-140">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="99506-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="99506-141">如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="99506-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="99506-142">4\.</span><span class="sxs-lookup"><span data-stu-id="99506-142">4\.</span></span> <span data-ttu-id="99506-143">在 Visual Studio Code 中開啟 [ *WebApplication1* ] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="99506-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="99506-144">5\.</span><span class="sxs-lookup"><span data-stu-id="99506-144">5\.</span></span> <span data-ttu-id="99506-145">若為 Blazor 伺服器專案，IDE 會要求您新增資產以建立和對專案進行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="99506-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="99506-146">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="99506-146">Select **Yes**.</span></span>

   <span data-ttu-id="99506-147">6\.</span><span class="sxs-lookup"><span data-stu-id="99506-147">6\.</span></span> <span data-ttu-id="99506-148">如果使用 Blazor 伺服器應用程式，請使用 Visual Studio Code 偵錯工具來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="99506-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="99506-149">如果使用 Blazor WebAssembly 應用程式，請從應用程式的專案資料夾執行 `dotnet run`。</span><span class="sxs-lookup"><span data-stu-id="99506-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="99506-150">7\.</span><span class="sxs-lookup"><span data-stu-id="99506-150">7\.</span></span> <span data-ttu-id="99506-151">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="99506-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="99506-152">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="99506-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="99506-153">1\.</span><span class="sxs-lookup"><span data-stu-id="99506-153">1\.</span></span> <span data-ttu-id="99506-154">安裝[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)。</span><span class="sxs-lookup"><span data-stu-id="99506-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="99506-155">2\.</span><span class="sxs-lookup"><span data-stu-id="99506-155">2\.</span></span> <span data-ttu-id="99506-156">選取 **[** 檔案] > [**新增方案**] 或 [建立**新專案**]。</span><span class="sxs-lookup"><span data-stu-id="99506-156">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="99506-157">3\.</span><span class="sxs-lookup"><span data-stu-id="99506-157">3\.</span></span> <span data-ttu-id="99506-158">在側邊欄中，選取 [ **.Net Core** > **應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="99506-158">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="99506-159">4\.</span><span class="sxs-lookup"><span data-stu-id="99506-159">4\.</span></span> <span data-ttu-id="99506-160">選取 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="99506-160">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="99506-161">目前只有 Blazor 伺服器範本可在 Visual Studio for Mac 中使用。</span><span class="sxs-lookup"><span data-stu-id="99506-161">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="99506-162">如需 Blazor 的 WebAssembly 體驗，請遵循 [ **.NET Core CLI** ] 索引標籤上的指示。選取 Blazor 伺服器範本之後，請選取 **[下一步]** 。</span><span class="sxs-lookup"><span data-stu-id="99506-162">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="99506-163">如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="99506-163">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="99506-164">5\.</span><span class="sxs-lookup"><span data-stu-id="99506-164">5\.</span></span> <span data-ttu-id="99506-165">將**目標 Framework**設定為 **.net Core 3.1** ，然後選取 **[下一步]** 。</span><span class="sxs-lookup"><span data-stu-id="99506-165">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="99506-166">6\.</span><span class="sxs-lookup"><span data-stu-id="99506-166">6\.</span></span> <span data-ttu-id="99506-167">在 [**專案名稱**] 欄位中，將應用程式命名為 `WebApplication1`。</span><span class="sxs-lookup"><span data-stu-id="99506-167">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="99506-168">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="99506-168">Select **Create**.</span></span>

   <span data-ttu-id="99506-169">7\.</span><span class="sxs-lookup"><span data-stu-id="99506-169">7\.</span></span> <span data-ttu-id="99506-170">選取 [**執行**] > **執行而不進行調試**程式，以在不進行偵錯工具的*情況下*執行</span><span class="sxs-lookup"><span data-stu-id="99506-170">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="99506-171">使用 [**開始調試**程式] 執行應用程式，以*使用調試*程式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="99506-171">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

   <span data-ttu-id="99506-172">如果出現會信任開發憑證的提示，請信任憑證並繼續。</span><span class="sxs-lookup"><span data-stu-id="99506-172">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="99506-173">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="99506-173">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="99506-174">如需 Blazor 的 WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="99506-174">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="99506-175">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="99506-175">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="99506-176">如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="99506-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="99506-177">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="99506-177">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="99506-178">提要欄位中的索引標籤可使用多個頁面：</span><span class="sxs-lookup"><span data-stu-id="99506-178">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="99506-179">首頁</span><span class="sxs-lookup"><span data-stu-id="99506-179">Home</span></span>
* <span data-ttu-id="99506-180">計數器</span><span class="sxs-lookup"><span data-stu-id="99506-180">Counter</span></span>
* <span data-ttu-id="99506-181">提取資料</span><span class="sxs-lookup"><span data-stu-id="99506-181">Fetch data</span></span>

<span data-ttu-id="99506-182">在 [計數器] 頁面上，選取 [按我] 按鈕以在不重新整理頁面的情況下讓計數器遞增。</span><span class="sxs-lookup"><span data-stu-id="99506-182">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="99506-183">將網頁中的計數器遞增通常需要撰寫 JavaScript，但您可以使用C#Blazor。</span><span class="sxs-lookup"><span data-stu-id="99506-183">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="99506-184">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="99506-184">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="99506-185">在瀏覽器中 `/counter` 的要求，如同頂端的 `@page` 指示詞所指定，會導致 `Counter` 元件轉譯其內容。</span><span class="sxs-lookup"><span data-stu-id="99506-185">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="99506-186">元件會轉譯成轉譯樹狀結構的記憶體中標記法，然後用來以彈性且有效率的方式更新 UI。</span><span class="sxs-lookup"><span data-stu-id="99506-186">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="99506-187">每次選取 [**按我**] 按鈕時：</span><span class="sxs-lookup"><span data-stu-id="99506-187">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="99506-188">`onclick` 事件會引發。</span><span class="sxs-lookup"><span data-stu-id="99506-188">The `onclick` event is fired.</span></span>
* <span data-ttu-id="99506-189">呼叫 `IncrementCount` 方法。</span><span class="sxs-lookup"><span data-stu-id="99506-189">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="99506-190">`currentCount` 會遞增。</span><span class="sxs-lookup"><span data-stu-id="99506-190">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="99506-191">元件會再次轉譯。</span><span class="sxs-lookup"><span data-stu-id="99506-191">The component is rendered again.</span></span>

<span data-ttu-id="99506-192">執行時間會比較新的內容與先前的內容，而且只會將已變更的內容套用至檔物件模型（DOM）。</span><span class="sxs-lookup"><span data-stu-id="99506-192">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="99506-193">使用 HTML 語法將元件新增至另一個元件。</span><span class="sxs-lookup"><span data-stu-id="99506-193">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="99506-194">例如，藉由將 `<Counter />` 元素新增至 `Index` 元件，將 `Counter` 元件新增至應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="99506-194">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="99506-195">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="99506-195">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="99506-196">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="99506-196">Run the app.</span></span> <span data-ttu-id="99506-197">首頁有自己的計數器，由 `Counter` 元件提供。</span><span class="sxs-lookup"><span data-stu-id="99506-197">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="99506-198">元件參數是使用屬性或[子內容](xref:blazor/components#child-content)所指定，可讓您設定子元件上的屬性。</span><span class="sxs-lookup"><span data-stu-id="99506-198">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="99506-199">若要將參數新增至 `Counter` 元件，請更新元件的 `@code` 區塊：</span><span class="sxs-lookup"><span data-stu-id="99506-199">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="99506-200">加入具有 `[Parameter]` 屬性之 `IncrementAmount` 的公用屬性。</span><span class="sxs-lookup"><span data-stu-id="99506-200">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="99506-201">將 `IncrementCount` 方法變更為在增加 `currentCount`的值時使用 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="99506-201">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="99506-202">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="99506-202">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="99506-203">使用屬性，在 `Index` 元件的 `<Counter>` 元素中指定 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="99506-203">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="99506-204">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="99506-204">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="99506-205">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="99506-205">Run the app.</span></span> <span data-ttu-id="99506-206">`Index` 元件有自己的計數器，每次選取 [按**我**] 按鈕時，就會遞增10。</span><span class="sxs-lookup"><span data-stu-id="99506-206">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="99506-207">`/counter` 的 `Counter` 元件（*razor*）會繼續遞增一。</span><span class="sxs-lookup"><span data-stu-id="99506-207">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="99506-208">後續步驟</span><span class="sxs-lookup"><span data-stu-id="99506-208">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="99506-209">其他資源</span><span class="sxs-lookup"><span data-stu-id="99506-209">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
