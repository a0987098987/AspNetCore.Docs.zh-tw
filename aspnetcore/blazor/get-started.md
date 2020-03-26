---
title: 開始使用 ASP.NET Core Blazor
author: guardrex
description: 藉由使用您選擇的工具來建立 Blazor 應用程式，開始使用 Blazor。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: abecb640930c1e5770c0fad45a1e9a6df31a20f4
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306448"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="36483-103">開始使用 ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="36483-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="36483-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="36483-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="36483-105">若要開始使用 Blazor，請遵循您所選工具的指導方針：</span><span class="sxs-lookup"><span data-stu-id="36483-105">To get started with Blazor, follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="36483-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="36483-106">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="36483-107">若要建立 Blazor 伺服器應用程式，請使用**ASP.NET 和 網頁程式開發**工作負載安裝[Visual Studio 2019 16.4 版或更新](https://visualstudio.microsoft.com/vs/preview/)版本。</span><span class="sxs-lookup"><span data-stu-id="36483-107">To create Blazor Server apps, install [Visual Studio 2019 version 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="36483-108">若要建立 Blazor 伺服器和 Blazor WebAssembly 應用程式，請使用**ASP.NET 和 網頁程式開發**工作負載安裝 Visual Studio 2019 16.6 Preview 2 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="36483-108">To create Blazor Server and Blazor WebAssembly apps, install Visual Studio 2019 16.6 Preview 2 or later with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="36483-109">如需*Blazor WebAssembly*和*Blazor Server*這兩個 Blazor 裝載模型的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="36483-109">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="36483-110">建立新專案。</span><span class="sxs-lookup"><span data-stu-id="36483-110">Create a new project.</span></span>

1. <span data-ttu-id="36483-111">選取 [ **Blazor 應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="36483-111">Select **Blazor App**.</span></span> <span data-ttu-id="36483-112">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="36483-112">Select **Next**.</span></span>

1. <span data-ttu-id="36483-113">在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="36483-113">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="36483-114">確認 [**位置**] 專案正確，或提供專案的 [位置]。</span><span class="sxs-lookup"><span data-stu-id="36483-114">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="36483-115">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="36483-115">Select **Create**.</span></span>

1. <span data-ttu-id="36483-116">如需 Blazor WebAssembly 體驗（Visual Studio 16.6 Preview 2 或更新版本），請選擇 [ **Blazor WebAssembly 應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="36483-116">For a Blazor WebAssembly experience (Visual Studio 16.6 Preview 2 or later), choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="36483-117">如需 Blazor 伺服器體驗（Visual Studio 16.4 或更新版本），請選擇 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="36483-117">For a Blazor Server experience (Visual Studio 16.4 or later), choose the **Blazor Server App** template.</span></span> <span data-ttu-id="36483-118">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="36483-118">Select **Create**.</span></span>

1. <span data-ttu-id="36483-119">按下 <kbd>Ctrl</kbd>+<kbd>F5</kbd> 即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="36483-119">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="36483-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="36483-120">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="36483-121">安裝[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="36483-121">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="36483-122">藉由執行下列命令，選擇性地安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview 範本：</span><span class="sxs-lookup"><span data-stu-id="36483-122">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview template by running the following command:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > <span data-ttu-id="36483-123">使用 3.2 Preview 3 Blazor WebAssembly 範本時，**需要** [.NET Core SDK 版本3.1.201 或更新版本](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="36483-123">The [.NET Core SDK version 3.1.201 or later](https://dotnet.microsoft.com/download/dotnet-core/3.1) is **required** to use the 3.2 Preview 3 Blazor WebAssembly template.</span></span> <span data-ttu-id="36483-124">藉由在命令 shell 中執行 `dotnet --version`，確認已安裝的 .NET Core SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="36483-124">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="36483-125">安裝 [Visual Studio Code](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="36483-125">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

1. <span data-ttu-id="36483-126">安裝最新[ C#的 for Visual Studio Code 擴充](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)功能和[JavaScript 偵錯工具（夜間）](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly)延伸模組，並將 `debug.javascript.usePreview` 設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="36483-126">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the [JavaScript Debugger (Nightly)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) extension with `debug.javascript.usePreview` set to `true`.</span></span>

1. <span data-ttu-id="36483-127">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="36483-127">For a Blazor Server experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   <span data-ttu-id="36483-128">如需 Blazor 的 WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="36483-128">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   <span data-ttu-id="36483-129">如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="36483-129">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="36483-130">在 Visual Studio Code 中開啟 [ *WebApplication1* ] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="36483-130">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

1. <span data-ttu-id="36483-131">IDE 會要求您新增資產，以建立和對專案進行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="36483-131">The IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="36483-132">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="36483-132">Select **Yes**.</span></span>

1. <span data-ttu-id="36483-133">使用 Visual Studio Code 偵錯工具執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="36483-133">Tun the app using the Visual Studio Code debugger.</span></span>

1. <span data-ttu-id="36483-134">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="36483-134">In a browser, navigate to `https://localhost:5001`.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="36483-135">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="36483-135">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="36483-136">Visual Studio for Mac 支援 Blazor 伺服器。</span><span class="sxs-lookup"><span data-stu-id="36483-136">Blazor Server is supported in Visual Studio for Mac.</span></span> <span data-ttu-id="36483-137">目前不支援 Blazor WebAssembly。</span><span class="sxs-lookup"><span data-stu-id="36483-137">Blazor WebAssembly isn't supported at this time.</span></span> <span data-ttu-id="36483-138">若要在 macOS 上建立 Blazor WebAssembly 應用程式，請遵循 [ **.NET Core CLI** ] 索引標籤上的指導方針。</span><span class="sxs-lookup"><span data-stu-id="36483-138">To build Blazor WebAssembly apps on macOS, follow the guidance on the **.NET Core CLI** tab.</span></span>

1. <span data-ttu-id="36483-139">安裝[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)。</span><span class="sxs-lookup"><span data-stu-id="36483-139">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

1. <span data-ttu-id="36483-140">選取 **[** 檔案] > [**新增方案**] 或 [建立**新專案**]。</span><span class="sxs-lookup"><span data-stu-id="36483-140">Select **File** > **New Solution** or create a **New Project**.</span></span>

1. <span data-ttu-id="36483-141">在側邊欄中，選取 [ **.Net Core** > **應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="36483-141">In the sidebar, select **.NET Core** > **App**.</span></span>

1. <span data-ttu-id="36483-142">選取 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="36483-142">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="36483-143">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="36483-143">Select **Create**.</span></span>

   <span data-ttu-id="36483-144">如需 Blazor 伺服器裝載模型的詳細資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="36483-144">For information on the Blazor Server hosting model, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="36483-145">將**目標 Framework**設定為 **.net Core 3.1** ，然後選取 **[下一步]** 。</span><span class="sxs-lookup"><span data-stu-id="36483-145">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

1. <span data-ttu-id="36483-146">在 [**專案名稱**] 欄位中，將應用程式命名為 `WebApplication1`。</span><span class="sxs-lookup"><span data-stu-id="36483-146">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="36483-147">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="36483-147">Select **Create**.</span></span>

1. <span data-ttu-id="36483-148">選取 [**執行**] > **執行而不進行調試**程式，以在不進行偵錯工具的*情況下*執行</span><span class="sxs-lookup"><span data-stu-id="36483-148">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="36483-149">使用 [**開始調試**程式] 執行應用程式，以*使用調試*程式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="36483-149">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

<span data-ttu-id="36483-150">如果出現會信任開發憑證的提示，請信任憑證並繼續。</span><span class="sxs-lookup"><span data-stu-id="36483-150">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="36483-151">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="36483-151">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="36483-152">安裝[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="36483-152">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="36483-153">藉由執行下列命令，選擇性地安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview 範本：</span><span class="sxs-lookup"><span data-stu-id="36483-153">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview template by running the following command:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > <span data-ttu-id="36483-154">使用 3.2 Preview 3 Blazor WebAssembly 範本時，**需要** [.NET Core SDK 版本3.1.201 或更新版本](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="36483-154">The [.NET Core SDK version 3.1.201 or later](https://dotnet.microsoft.com/download/dotnet-core/3.1) is **required** to use the 3.2 Preview 3 Blazor WebAssembly template.</span></span> <span data-ttu-id="36483-155">藉由在命令 shell 中執行 `dotnet --version`，確認已安裝的 .NET Core SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="36483-155">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="36483-156">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="36483-156">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="36483-157">如需 Blazor 的 WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="36483-157">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="36483-158">如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="36483-158">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="36483-159">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="36483-159">In a browser, navigate to `https://localhost:5001`.</span></span>

---

<span data-ttu-id="36483-160">提要欄位中的索引標籤可使用多個頁面：</span><span class="sxs-lookup"><span data-stu-id="36483-160">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="36483-161">Home</span><span class="sxs-lookup"><span data-stu-id="36483-161">Home</span></span>
* <span data-ttu-id="36483-162">計數器</span><span class="sxs-lookup"><span data-stu-id="36483-162">Counter</span></span>
* <span data-ttu-id="36483-163">提取資料</span><span class="sxs-lookup"><span data-stu-id="36483-163">Fetch data</span></span>

<span data-ttu-id="36483-164">在 [計數器] 頁面上，選取 [按我] 按鈕以在不重新整理頁面的情況下讓計數器遞增。</span><span class="sxs-lookup"><span data-stu-id="36483-164">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="36483-165">將網頁中的計數器遞增通常需要撰寫 JavaScript，但您可以使用C#Blazor。</span><span class="sxs-lookup"><span data-stu-id="36483-165">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="36483-166">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="36483-166">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="36483-167">在瀏覽器中 `/counter` 的要求，如同頂端的 `@page` 指示詞所指定，會導致 `Counter` 元件轉譯其內容。</span><span class="sxs-lookup"><span data-stu-id="36483-167">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="36483-168">元件會轉譯成轉譯樹狀結構的記憶體中標記法，然後用來以彈性且有效率的方式更新 UI。</span><span class="sxs-lookup"><span data-stu-id="36483-168">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="36483-169">每次選取 [**按我**] 按鈕時：</span><span class="sxs-lookup"><span data-stu-id="36483-169">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="36483-170">`onclick` 事件會引發。</span><span class="sxs-lookup"><span data-stu-id="36483-170">The `onclick` event is fired.</span></span>
* <span data-ttu-id="36483-171">呼叫 `IncrementCount` 方法。</span><span class="sxs-lookup"><span data-stu-id="36483-171">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="36483-172">`currentCount` 會遞增。</span><span class="sxs-lookup"><span data-stu-id="36483-172">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="36483-173">元件會再次轉譯。</span><span class="sxs-lookup"><span data-stu-id="36483-173">The component is rendered again.</span></span>

<span data-ttu-id="36483-174">執行時間會比較新的內容與先前的內容，而且只會將已變更的內容套用至檔物件模型（DOM）。</span><span class="sxs-lookup"><span data-stu-id="36483-174">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="36483-175">使用 HTML 語法將元件新增至另一個元件。</span><span class="sxs-lookup"><span data-stu-id="36483-175">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="36483-176">例如，藉由將 `<Counter />` 元素新增至 `Index` 元件，將 `Counter` 元件新增至應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="36483-176">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="36483-177">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="36483-177">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="36483-178">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="36483-178">Run the app.</span></span> <span data-ttu-id="36483-179">首頁有自己的計數器，由 `Counter` 元件提供。</span><span class="sxs-lookup"><span data-stu-id="36483-179">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="36483-180">元件參數是使用屬性或[子內容](xref:blazor/components#child-content)所指定，可讓您設定子元件上的屬性。</span><span class="sxs-lookup"><span data-stu-id="36483-180">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="36483-181">若要將參數新增至 `Counter` 元件，請更新元件的 `@code` 區塊：</span><span class="sxs-lookup"><span data-stu-id="36483-181">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="36483-182">加入具有 `[Parameter]` 屬性之 `IncrementAmount` 的公用屬性。</span><span class="sxs-lookup"><span data-stu-id="36483-182">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="36483-183">將 `IncrementCount` 方法變更為在增加 `IncrementAmount`的值時使用 `currentCount`。</span><span class="sxs-lookup"><span data-stu-id="36483-183">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="36483-184">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="36483-184">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="36483-185">使用屬性，在 `Index` 元件的 `<Counter>` 元素中指定 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="36483-185">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="36483-186">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="36483-186">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="36483-187">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="36483-187">Run the app.</span></span> <span data-ttu-id="36483-188">`Index` 元件有自己的計數器，每次選取 [按**我**] 按鈕時，就會遞增10。</span><span class="sxs-lookup"><span data-stu-id="36483-188">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="36483-189">`/counter` 的 `Counter` 元件（*razor*）會繼續遞增一。</span><span class="sxs-lookup"><span data-stu-id="36483-189">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="36483-190">後續步驟</span><span class="sxs-lookup"><span data-stu-id="36483-190">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="36483-191">其他資源</span><span class="sxs-lookup"><span data-stu-id="36483-191">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
