---
title: 開始使用ASP.NET核心Blazor
author: guardrex
description: Blazor使用您選擇的工具建Blazor構應用,開始入門。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: c49209afde21046a6bc0b197cc4b8d93b164b23e
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80471822"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="92d8b-103">開始ASP.NET核心布拉佐爾</span><span class="sxs-lookup"><span data-stu-id="92d8b-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="92d8b-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="92d8b-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="92d8b-105">要開始使用 Blazor,請按照您選擇的工具指南操作:</span><span class="sxs-lookup"><span data-stu-id="92d8b-105">To get started with Blazor, follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="92d8b-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="92d8b-106">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="92d8b-107">要創建 Blazor Server 應用,請使用**ASP.NET 和 Web 開發**工作負載安裝最新版本的[Visual Studio 2019。](https://visualstudio.microsoft.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="92d8b-107">To create Blazor Server apps, install the latest version of [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="92d8b-108">要創建 Blazor Server 和 Blazor WebAssembly 應用,請使用**ASP.NET 和網路開發**工作負載安裝[Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/)的最新預覽版。</span><span class="sxs-lookup"><span data-stu-id="92d8b-108">To create Blazor Server and Blazor WebAssembly apps, install the latest preview of [Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="92d8b-109">有關兩個布拉佐託管模型,*布拉佐網路大會*和*布拉佐伺服器*的資訊<xref:blazor/hosting-models>,請參閱 。</span><span class="sxs-lookup"><span data-stu-id="92d8b-109">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="92d8b-110">建立新專案。</span><span class="sxs-lookup"><span data-stu-id="92d8b-110">Create a new project.</span></span>

1. <span data-ttu-id="92d8b-111">選擇**布拉佐爾應用程式**。</span><span class="sxs-lookup"><span data-stu-id="92d8b-111">Select **Blazor App**.</span></span> <span data-ttu-id="92d8b-112">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="92d8b-112">Select **Next**.</span></span>

1. <span data-ttu-id="92d8b-113">在 [專案名稱]\*\*\*\* 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="92d8b-113">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="92d8b-114">確認 **「位置**」條目正確或為專案提供位置。</span><span class="sxs-lookup"><span data-stu-id="92d8b-114">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="92d8b-115">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="92d8b-115">Select **Create**.</span></span>

1. <span data-ttu-id="92d8b-116">有關 Blazor Web 組裝體驗(可視化工作室 16.6 預覽版 2 或更高版本),請選擇**Blazor Web 組裝應用**範本。</span><span class="sxs-lookup"><span data-stu-id="92d8b-116">For a Blazor WebAssembly experience (Visual Studio 16.6 Preview 2 or later), choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="92d8b-117">有關 Blazor 伺服器體驗(可視化工作室 16.4 或更高版本),請選擇**Blazor 伺服器應用**範本。</span><span class="sxs-lookup"><span data-stu-id="92d8b-117">For a Blazor Server experience (Visual Studio 16.4 or later), choose the **Blazor Server App** template.</span></span> <span data-ttu-id="92d8b-118">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="92d8b-118">Select **Create**.</span></span>

1. <span data-ttu-id="92d8b-119">按<kbd>Ctrl</kbd>+<kbd>F5</kbd>運行應用程式。</span><span class="sxs-lookup"><span data-stu-id="92d8b-119">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="92d8b-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="92d8b-120">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="92d8b-121">安裝[.NET 核心 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="92d8b-121">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="92d8b-122">通過運行以下命令,可選地安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)預覽樣本:</span><span class="sxs-lookup"><span data-stu-id="92d8b-122">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview template by running the following command:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > <span data-ttu-id="92d8b-123">[.NET 核心 SDK 版本 3.1.201 或更高版本](https://dotnet.microsoft.com/download/dotnet-core/3.1)**需要**使用 3.2 預覽 3 Blazor WebAssembly 範本。</span><span class="sxs-lookup"><span data-stu-id="92d8b-123">The [.NET Core SDK version 3.1.201 or later](https://dotnet.microsoft.com/download/dotnet-core/3.1) is **required** to use the 3.2 Preview 3 Blazor WebAssembly template.</span></span> <span data-ttu-id="92d8b-124">通過在命令外殼中運行`dotnet --version`來確認已安裝的 .NET Core SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="92d8b-124">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="92d8b-125">安裝[視覺化工作室代碼](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="92d8b-125">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

1. <span data-ttu-id="92d8b-126">安裝最新的[C# 視覺化工作室碼延伸](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)與[JavaScript 除錯器(夜間)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly)延伸,`debug.javascript.usePreview`設定為`true`。</span><span class="sxs-lookup"><span data-stu-id="92d8b-126">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the [JavaScript Debugger (Nightly)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) extension with `debug.javascript.usePreview` set to `true`.</span></span>

1. <span data-ttu-id="92d8b-127">要獲得 Blazor Server 體驗,請在命令外殼中執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="92d8b-127">For a Blazor Server experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   <span data-ttu-id="92d8b-128">要獲得 Blazor WebAssembly 體驗,請在命令外殼中執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="92d8b-128">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   <span data-ttu-id="92d8b-129">有關兩個布拉佐託管模型,*布拉佐伺服器*和*布拉佐網路大會*的資訊<xref:blazor/hosting-models>,請參閱 。</span><span class="sxs-lookup"><span data-stu-id="92d8b-129">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="92d8b-130">打開視覺化工作室代碼中的*Web 應用程式 1*資料夾。</span><span class="sxs-lookup"><span data-stu-id="92d8b-130">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

1. <span data-ttu-id="92d8b-131">IDE 請求添加資源以生成和調試專案。</span><span class="sxs-lookup"><span data-stu-id="92d8b-131">The IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="92d8b-132">選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="92d8b-132">Select **Yes**.</span></span>

1. <span data-ttu-id="92d8b-133">使用可視化工作室代碼調試器運行應用。</span><span class="sxs-lookup"><span data-stu-id="92d8b-133">Run the app using the Visual Studio Code debugger.</span></span>

1. <span data-ttu-id="92d8b-134">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="92d8b-134">In a browser, navigate to `https://localhost:5001`.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="92d8b-135">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="92d8b-135">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="92d8b-136">Mac 視覺工作室支援 Blazor 伺服器。</span><span class="sxs-lookup"><span data-stu-id="92d8b-136">Blazor Server is supported in Visual Studio for Mac.</span></span> <span data-ttu-id="92d8b-137">布拉佐網路組裝目前不受支援。</span><span class="sxs-lookup"><span data-stu-id="92d8b-137">Blazor WebAssembly isn't supported at this time.</span></span> <span data-ttu-id="92d8b-138">要在 macOS 上建構 Blazor WebAssembly 應用,請按照 **.NET 核心 CLI**選項卡上的指南操作。</span><span class="sxs-lookup"><span data-stu-id="92d8b-138">To build Blazor WebAssembly apps on macOS, follow the guidance on the **.NET Core CLI** tab.</span></span>

1. <span data-ttu-id="92d8b-139">[安裝 Mac 的視覺工作室](https://visualstudio.microsoft.com/vs/mac/)。</span><span class="sxs-lookup"><span data-stu-id="92d8b-139">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

1. <span data-ttu-id="92d8b-140">選擇**檔案** > **新解決方案**或建立新**專案**。</span><span class="sxs-lookup"><span data-stu-id="92d8b-140">Select **File** > **New Solution** or create a **New Project**.</span></span>

1. <span data-ttu-id="92d8b-141">在側邊列中,選擇 **.NET 核心** > **應用**。</span><span class="sxs-lookup"><span data-stu-id="92d8b-141">In the sidebar, select **.NET Core** > **App**.</span></span>

1. <span data-ttu-id="92d8b-142">選擇**Blazor 伺服器應用**範本。</span><span class="sxs-lookup"><span data-stu-id="92d8b-142">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="92d8b-143">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="92d8b-143">Select **Create**.</span></span>

   <span data-ttu-id="92d8b-144">有關 Blazor 伺服器託管模型的資訊,請<xref:blazor/hosting-models>參閱 。</span><span class="sxs-lookup"><span data-stu-id="92d8b-144">For information on the Blazor Server hosting model, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="92d8b-145">將**目標框架**設置為 **.NET 核心 3.1,** 然後選擇 **「下一步**」 。</span><span class="sxs-lookup"><span data-stu-id="92d8b-145">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

1. <span data-ttu-id="92d8b-146">在「**項目名稱」** 欄位中,`WebApplication1`為套用命名 。</span><span class="sxs-lookup"><span data-stu-id="92d8b-146">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="92d8b-147">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="92d8b-147">Select **Create**.</span></span>

1. <span data-ttu-id="92d8b-148">選擇 **「** > **執行而不除錯執行**」 以在沒有*除錯器的情況下*執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="92d8b-148">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="92d8b-149">使用 **「開始除錯」** 執行應用,*以便使用除錯器*執行應用。</span><span class="sxs-lookup"><span data-stu-id="92d8b-149">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

<span data-ttu-id="92d8b-150">如果出現信任開發證書的提示,請信任證書並繼續。</span><span class="sxs-lookup"><span data-stu-id="92d8b-150">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="92d8b-151">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="92d8b-151">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="92d8b-152">安裝[.NET 核心 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="92d8b-152">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="92d8b-153">通過運行以下命令,可選地安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)預覽樣本:</span><span class="sxs-lookup"><span data-stu-id="92d8b-153">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview template by running the following command:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > <span data-ttu-id="92d8b-154">[.NET 核心 SDK 版本 3.1.201 或更高版本](https://dotnet.microsoft.com/download/dotnet-core/3.1)**需要**使用 3.2 預覽 3 Blazor WebAssembly 範本。</span><span class="sxs-lookup"><span data-stu-id="92d8b-154">The [.NET Core SDK version 3.1.201 or later](https://dotnet.microsoft.com/download/dotnet-core/3.1) is **required** to use the 3.2 Preview 3 Blazor WebAssembly template.</span></span> <span data-ttu-id="92d8b-155">通過在命令外殼中運行`dotnet --version`來確認已安裝的 .NET Core SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="92d8b-155">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="92d8b-156">要獲得 Blazor Server 體驗,請在命令外殼中執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="92d8b-156">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="92d8b-157">要獲得 Blazor WebAssembly 體驗,請在命令外殼中執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="92d8b-157">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="92d8b-158">有關兩個布拉佐託管模型,*布拉佐伺服器*和*布拉佐網路大會*的資訊<xref:blazor/hosting-models>,請參閱 。</span><span class="sxs-lookup"><span data-stu-id="92d8b-158">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="92d8b-159">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="92d8b-159">In a browser, navigate to `https://localhost:5001`.</span></span>

---

<span data-ttu-id="92d8b-160">側邊列中的選項卡提供多個頁面:</span><span class="sxs-lookup"><span data-stu-id="92d8b-160">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="92d8b-161">Home</span><span class="sxs-lookup"><span data-stu-id="92d8b-161">Home</span></span>
* <span data-ttu-id="92d8b-162">計數器</span><span class="sxs-lookup"><span data-stu-id="92d8b-162">Counter</span></span>
* <span data-ttu-id="92d8b-163">取得資料</span><span class="sxs-lookup"><span data-stu-id="92d8b-163">Fetch data</span></span>

<span data-ttu-id="92d8b-164">在 [計數器] 頁面上，選取 [按我]\*\*\*\* 按鈕以在不重新整理頁面的情況下讓計數器遞增。</span><span class="sxs-lookup"><span data-stu-id="92d8b-164">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="92d8b-165">在網頁中增加計數器通常需要編寫 JavaScript,但Blazor使用 C# 時可以使用 C#。</span><span class="sxs-lookup"><span data-stu-id="92d8b-165">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="92d8b-166">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="92d8b-166">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="92d8b-167">瀏覽器中的請求`/counter`(如頂部`@page`的 指令所指定的`Counter`)會導致 元件呈現其內容。</span><span class="sxs-lookup"><span data-stu-id="92d8b-167">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="92d8b-168">元件呈現為呈現樹的記憶體中表示形式,然後可用於以靈活、高效的方式更新 UI。</span><span class="sxs-lookup"><span data-stu-id="92d8b-168">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="92d8b-169">每次選擇 **「按下我」** 按鈕時:</span><span class="sxs-lookup"><span data-stu-id="92d8b-169">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="92d8b-170">事件`onclick`已觸發。</span><span class="sxs-lookup"><span data-stu-id="92d8b-170">The `onclick` event is fired.</span></span>
* <span data-ttu-id="92d8b-171">已呼叫 `IncrementCount` 方法。</span><span class="sxs-lookup"><span data-stu-id="92d8b-171">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="92d8b-172">遞`currentCount`增。</span><span class="sxs-lookup"><span data-stu-id="92d8b-172">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="92d8b-173">元件將再次呈現。</span><span class="sxs-lookup"><span data-stu-id="92d8b-173">The component is rendered again.</span></span>

<span data-ttu-id="92d8b-174">運行時將新內容與以前的內容進行比較,並且僅將更改的內容應用於文檔物件模型 (DOM)。</span><span class="sxs-lookup"><span data-stu-id="92d8b-174">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="92d8b-175">使用 HTML 語法將元件新增到另一個元件。</span><span class="sxs-lookup"><span data-stu-id="92d8b-175">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="92d8b-176">例如,通過將`Counter``<Counter />`元素添加到`Index`元件,將元件添加到應用的主頁。</span><span class="sxs-lookup"><span data-stu-id="92d8b-176">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="92d8b-177">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="92d8b-177">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="92d8b-178">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="92d8b-178">Run the app.</span></span> <span data-ttu-id="92d8b-179">主頁有其自己的計數器由`Counter`元件提供。</span><span class="sxs-lookup"><span data-stu-id="92d8b-179">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="92d8b-180">元件參數使用屬性或[子內容](xref:blazor/components#child-content)指定,允許您在子元件上設置屬性。</span><span class="sxs-lookup"><span data-stu-id="92d8b-180">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="92d8b-181">要加入`Counter`元件加入參數,請更新元件的`@code`區塊:</span><span class="sxs-lookup"><span data-stu-id="92d8b-181">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="92d8b-182">使用`[Parameter]`屬性添加`IncrementAmount`公共屬性。</span><span class="sxs-lookup"><span data-stu-id="92d8b-182">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="92d8b-183">將 `IncrementCount` 方法變更為在增加 `currentCount`的值時使用 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="92d8b-183">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="92d8b-184">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="92d8b-184">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="92d8b-185">使用屬性`IncrementAmount``Index`在元件`<Counter>`的元素中指定 。</span><span class="sxs-lookup"><span data-stu-id="92d8b-185">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="92d8b-186">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="92d8b-186">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="92d8b-187">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="92d8b-187">Run the app.</span></span> <span data-ttu-id="92d8b-188">元件`Index`有自己的計數器,每次選擇 **「按下我」** 按鈕時,該計數器都會增加 10。</span><span class="sxs-lookup"><span data-stu-id="92d8b-188">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="92d8b-189">在`Counter``/counter`元件 (*反.razor*) 繼續增加一個.</span><span class="sxs-lookup"><span data-stu-id="92d8b-189">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="92d8b-190">後續步驟</span><span class="sxs-lookup"><span data-stu-id="92d8b-190">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="92d8b-191">其他資源</span><span class="sxs-lookup"><span data-stu-id="92d8b-191">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
