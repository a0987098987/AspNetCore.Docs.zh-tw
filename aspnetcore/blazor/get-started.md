---
title: 開始使用 ASP.NET CoreBlazor
author: guardrex
description: 開始使用Blazor ，方法是使用Blazor您選擇的工具來建立應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/30/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 8ef55b92c45aa07113fd4601a3c7464b42125623
ms.sourcegitcommit: 6318d2bdd63116e178c34492a904be85ec9ac108
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/30/2020
ms.locfileid: "82604762"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="706a7-103">開始使用 ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="706a7-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="706a7-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="706a7-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="706a7-105">若要開始使用 Blazor，請遵循您所選工具的指導方針：</span><span class="sxs-lookup"><span data-stu-id="706a7-105">To get started with Blazor, follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="706a7-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="706a7-106">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="706a7-107">若要建立 Blazor 伺服器應用程式，請使用**ASP.NET 和 網頁程式開發**工作負載安裝最新版的[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) 。</span><span class="sxs-lookup"><span data-stu-id="706a7-107">To create Blazor Server apps, install the latest version of [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="706a7-108">若要建立 Blazor 伺服器和 Blazor WebAssembly 應用程式，請使用**ASP.NET 和 網頁程式開發**工作負載安裝[Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/)的最新預覽。</span><span class="sxs-lookup"><span data-stu-id="706a7-108">To create Blazor Server and Blazor WebAssembly apps, install the latest preview of [Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="706a7-109">如需這兩個 Blazor 裝載模型的詳細資訊，請參閱<xref:blazor/hosting-models> *Blazor WebAssembly*和*Blazor Server*。</span><span class="sxs-lookup"><span data-stu-id="706a7-109">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="706a7-110">執行下列命令來安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview 範本：</span><span class="sxs-lookup"><span data-stu-id="706a7-110">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview template by running the following command:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-rc1.20223.4
   ```

1. <span data-ttu-id="706a7-111">建立新專案。</span><span class="sxs-lookup"><span data-stu-id="706a7-111">Create a new project.</span></span>

1. <span data-ttu-id="706a7-112">選取 [ **Blazor 應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="706a7-112">Select **Blazor App**.</span></span> <span data-ttu-id="706a7-113">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="706a7-113">Select **Next**.</span></span>

1. <span data-ttu-id="706a7-114">在 [專案名稱]\*\*\*\* 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="706a7-114">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="706a7-115">確認 [**位置**] 專案正確，或提供專案的 [位置]。</span><span class="sxs-lookup"><span data-stu-id="706a7-115">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="706a7-116">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="706a7-116">Select **Create**.</span></span>

1. <span data-ttu-id="706a7-117">如需 Blazor WebAssembly 體驗（Visual Studio 16.6 Preview 2 或更新版本），請選擇 [ **Blazor WebAssembly 應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="706a7-117">For a Blazor WebAssembly experience (Visual Studio 16.6 Preview 2 or later), choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="706a7-118">如需 Blazor 伺服器體驗（Visual Studio 16.4 或更新版本），請選擇 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="706a7-118">For a Blazor Server experience (Visual Studio 16.4 or later), choose the **Blazor Server App** template.</span></span> <span data-ttu-id="706a7-119">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="706a7-119">Select **Create**.</span></span>

1. <span data-ttu-id="706a7-120">按<kbd>Ctrl</kbd>+<kbd>F5</kbd>執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="706a7-120">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="706a7-121">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="706a7-121">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="706a7-122">安裝[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="706a7-122">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="706a7-123">藉由執行下列命令，選擇性地安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview 範本：</span><span class="sxs-lookup"><span data-stu-id="706a7-123">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview template by running the following command:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-rc1.20223.4
   ```

   > [!NOTE]
   > <span data-ttu-id="706a7-124">使用 3.2 Preview 4 Blazor WebAssembly 範本時，**需要** [.NET Core SDK 版本3.1.201 或更新版本](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="706a7-124">The [.NET Core SDK version 3.1.201 or later](https://dotnet.microsoft.com/download/dotnet-core/3.1) is **required** to use the 3.2 Preview 4 Blazor WebAssembly template.</span></span> <span data-ttu-id="706a7-125">藉由`dotnet --version`在命令 shell 中執行，確認已安裝的 .NET Core SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="706a7-125">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="706a7-126">安裝 [Visual Studio Code](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="706a7-126">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

1. <span data-ttu-id="706a7-127">安裝適用于[Visual Studio Code 擴充](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)功能的最新 c # 和[JavaScript 偵錯工具（夜間）](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly)延伸模組，並`debug.javascript.usePreview`將設定為。 `true`</span><span class="sxs-lookup"><span data-stu-id="706a7-127">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the [JavaScript Debugger (Nightly)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) extension with `debug.javascript.usePreview` set to `true`.</span></span>

1. <span data-ttu-id="706a7-128">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="706a7-128">For a Blazor Server experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   <span data-ttu-id="706a7-129">如需 Blazor 的 WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="706a7-129">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   <span data-ttu-id="706a7-130">如需這兩個 Blazor 裝載模型的詳細資訊，請參閱<xref:blazor/hosting-models> *Blazor Server*和*Blazor WebAssembly*。</span><span class="sxs-lookup"><span data-stu-id="706a7-130">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="706a7-131">在 Visual Studio Code 中開啟 [ *WebApplication1* ] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="706a7-131">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

1. <span data-ttu-id="706a7-132">IDE 會要求您新增資產，以建立和對專案進行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="706a7-132">The IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="706a7-133">選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="706a7-133">Select **Yes**.</span></span>

1. <span data-ttu-id="706a7-134">使用 Blazor 伺服器，使用 Visual Studio Code 偵錯工具執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="706a7-134">With Blazor Server, run the app using the Visual Studio Code debugger.</span></span>

   <span data-ttu-id="706a7-135">透過 Blazor WebAssembly，使用 **.Net Core 啟動（Blazor 獨立式）** 啟動設定來啟動應用程式，然後在 Chrome 啟動設定（需要 chrome）**中使用 .Net Core Debug Blazor Web 元件**來啟動瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="706a7-135">With Blazor WebAssembly, start the app using the **.NET Core Launch (Blazor Standalone)** launch configuration and then start the browser using the **.NET Core Debug Blazor Web Assembly in Chrome** launch configuration (requires Chrome).</span></span> <span data-ttu-id="706a7-136">如需詳細資訊，請參閱 <xref:blazor/debug#visual-studio-code>。</span><span class="sxs-lookup"><span data-stu-id="706a7-136">For more information, see <xref:blazor/debug#visual-studio-code>.</span></span>

1. <span data-ttu-id="706a7-137">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="706a7-137">In a browser, navigate to `https://localhost:5001`.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="706a7-138">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="706a7-138">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="706a7-139">Visual Studio for Mac 支援 Blazor 伺服器。</span><span class="sxs-lookup"><span data-stu-id="706a7-139">Blazor Server is supported in Visual Studio for Mac.</span></span> <span data-ttu-id="706a7-140">目前不支援 Blazor WebAssembly。</span><span class="sxs-lookup"><span data-stu-id="706a7-140">Blazor WebAssembly isn't supported at this time.</span></span> <span data-ttu-id="706a7-141">若要在 macOS 上建立 Blazor WebAssembly 應用程式，請遵循 [ **.NET Core CLI** ] 索引標籤上的指導方針。</span><span class="sxs-lookup"><span data-stu-id="706a7-141">To build Blazor WebAssembly apps on macOS, follow the guidance on the **.NET Core CLI** tab.</span></span>

1. <span data-ttu-id="706a7-142">安裝[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)。</span><span class="sxs-lookup"><span data-stu-id="706a7-142">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

1. <span data-ttu-id="706a7-143"> > 選取 **[** 檔案] [**新增方案**] 或 [建立**新專案**]。</span><span class="sxs-lookup"><span data-stu-id="706a7-143">Select **File** > **New Solution** or create a **New Project**.</span></span>

1. <span data-ttu-id="706a7-144">在提要欄位中，選取 [ **Web 和主控台** > **應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="706a7-144">In the sidebar, select **Web and Console** > **App**.</span></span>

1. <span data-ttu-id="706a7-145">選取 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="706a7-145">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="706a7-146">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="706a7-146">Select **Next**.</span></span>

   <span data-ttu-id="706a7-147">如需 Blazor 伺服器裝載模型的詳細資訊， <xref:blazor/hosting-models>請參閱。</span><span class="sxs-lookup"><span data-stu-id="706a7-147">For information on the Blazor Server hosting model, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="706a7-148">確認 [**目標 Framework** ] 已設定為 [ **.Net Core 3.1** ]，然後選取 **[下一步]**。</span><span class="sxs-lookup"><span data-stu-id="706a7-148">Confirm that the **Target Framework** is set to **.NET Core 3.1** and select **Next**.</span></span>

1. <span data-ttu-id="706a7-149">在 [**專案名稱**] 欄位中，將`WebApplication1`應用程式命名為。</span><span class="sxs-lookup"><span data-stu-id="706a7-149">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="706a7-150">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="706a7-150">Select **Create**.</span></span>

1. <span data-ttu-id="706a7-151">選取 [**執行** > **但不執行調試**程式]，在沒有偵錯工具的*情況下*執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="706a7-151">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="706a7-152">使用 [**開始調試**程式] 或 [執行] （&#9654;）按鈕執行應用程式，以*使用調試*程式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="706a7-152">Run the app with **Start Debugging** or the Run (&#9654;) button to run the app *with the debugger*.</span></span>

<span data-ttu-id="706a7-153">如果出現會信任開發憑證的提示，請信任憑證並繼續。</span><span class="sxs-lookup"><span data-stu-id="706a7-153">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span> <span data-ttu-id="706a7-154">需要使用者和 keychain 密碼，才能信任憑證。</span><span class="sxs-lookup"><span data-stu-id="706a7-154">The user and keychain passwords are required to trust the certificate.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="706a7-155">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="706a7-155">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="706a7-156">安裝[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="706a7-156">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="706a7-157">藉由執行下列命令，選擇性地安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview 範本：</span><span class="sxs-lookup"><span data-stu-id="706a7-157">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview template by running the following command:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-rc1.20223.4
   ```

   > [!NOTE]
   > <span data-ttu-id="706a7-158">使用 3.2 Preview 4 Blazor WebAssembly 範本時，**需要** [.NET Core SDK 版本3.1.201 或更新版本](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="706a7-158">The [.NET Core SDK version 3.1.201 or later](https://dotnet.microsoft.com/download/dotnet-core/3.1) is **required** to use the 3.2 Preview 4 Blazor WebAssembly template.</span></span> <span data-ttu-id="706a7-159">藉由`dotnet --version`在命令 shell 中執行，確認已安裝的 .NET Core SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="706a7-159">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="706a7-160">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="706a7-160">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="706a7-161">如需 Blazor 的 WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="706a7-161">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="706a7-162">如需這兩個 Blazor 裝載模型的詳細資訊，請參閱<xref:blazor/hosting-models> *Blazor Server*和*Blazor WebAssembly*。</span><span class="sxs-lookup"><span data-stu-id="706a7-162">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="706a7-163">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="706a7-163">In a browser, navigate to `https://localhost:5001`.</span></span>

---

<span data-ttu-id="706a7-164">提要欄位中的索引標籤可使用多個頁面：</span><span class="sxs-lookup"><span data-stu-id="706a7-164">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="706a7-165">家庭</span><span class="sxs-lookup"><span data-stu-id="706a7-165">Home</span></span>
* <span data-ttu-id="706a7-166">計數器</span><span class="sxs-lookup"><span data-stu-id="706a7-166">Counter</span></span>
* <span data-ttu-id="706a7-167">提取資料</span><span class="sxs-lookup"><span data-stu-id="706a7-167">Fetch data</span></span>

<span data-ttu-id="706a7-168">在 [計數器] 頁面上，選取 [按我]\*\*\*\* 按鈕以在不重新整理頁面的情況下讓計數器遞增。</span><span class="sxs-lookup"><span data-stu-id="706a7-168">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="706a7-169">將網頁中的計數器遞增通常需要撰寫 JavaScript，但Blazor您可以使用 c #。</span><span class="sxs-lookup"><span data-stu-id="706a7-169">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="706a7-170">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="706a7-170">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="706a7-171">`/counter`在瀏覽器中的要求（如頂端的`@page`指示詞所指定）會導致`Counter`元件轉譯其內容。</span><span class="sxs-lookup"><span data-stu-id="706a7-171">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="706a7-172">元件會轉譯成轉譯樹狀結構的記憶體中標記法，然後用來以彈性且有效率的方式更新 UI。</span><span class="sxs-lookup"><span data-stu-id="706a7-172">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="706a7-173">每次選取 [**按我**] 按鈕時：</span><span class="sxs-lookup"><span data-stu-id="706a7-173">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="706a7-174">引發`onclick`事件。</span><span class="sxs-lookup"><span data-stu-id="706a7-174">The `onclick` event is fired.</span></span>
* <span data-ttu-id="706a7-175">已呼叫 `IncrementCount` 方法。</span><span class="sxs-lookup"><span data-stu-id="706a7-175">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="706a7-176">`currentCount`會遞增。</span><span class="sxs-lookup"><span data-stu-id="706a7-176">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="706a7-177">元件會再次轉譯。</span><span class="sxs-lookup"><span data-stu-id="706a7-177">The component is rendered again.</span></span>

<span data-ttu-id="706a7-178">執行時間會比較新的內容與先前的內容，而且只會將已變更的內容套用至檔物件模型（DOM）。</span><span class="sxs-lookup"><span data-stu-id="706a7-178">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="706a7-179">使用 HTML 語法將元件新增至另一個元件。</span><span class="sxs-lookup"><span data-stu-id="706a7-179">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="706a7-180">例如，藉由將`Counter` `<Counter />`元素新增至`Index`元件，將元件新增至應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="706a7-180">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="706a7-181">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="706a7-181">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="706a7-182">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="706a7-182">Run the app.</span></span> <span data-ttu-id="706a7-183">首頁有自己的計數器，由`Counter`元件提供。</span><span class="sxs-lookup"><span data-stu-id="706a7-183">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="706a7-184">元件參數是使用屬性或[子內容](xref:blazor/components#child-content)所指定，可讓您設定子元件上的屬性。</span><span class="sxs-lookup"><span data-stu-id="706a7-184">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="706a7-185">若要將參數新增至`Counter`元件，請更新元件的`@code`區塊：</span><span class="sxs-lookup"><span data-stu-id="706a7-185">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="706a7-186">`IncrementAmount`使用`[Parameter]`屬性加入的公用屬性。</span><span class="sxs-lookup"><span data-stu-id="706a7-186">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="706a7-187">將 `IncrementCount` 方法變更為在增加 `currentCount`的值時使用 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="706a7-187">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="706a7-188">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="706a7-188">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="706a7-189">使用屬性`IncrementAmount` ，在`Index`元件的`<Counter>`元素中指定。</span><span class="sxs-lookup"><span data-stu-id="706a7-189">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="706a7-190">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="706a7-190">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="706a7-191">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="706a7-191">Run the app.</span></span> <span data-ttu-id="706a7-192">`Index`元件有自己的計數器，每次選取 [按**我**] 按鈕時，就會遞增10。</span><span class="sxs-lookup"><span data-stu-id="706a7-192">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="706a7-193">中`Counter` `/counter`的元件（*razor*）會繼續遞增一。</span><span class="sxs-lookup"><span data-stu-id="706a7-193">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="706a7-194">後續步驟</span><span class="sxs-lookup"><span data-stu-id="706a7-194">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="706a7-195">其他資源</span><span class="sxs-lookup"><span data-stu-id="706a7-195">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
