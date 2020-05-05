---
title: 開始使用 ASP.NET CoreBlazor
author: guardrex
description: 開始使用Blazor ，方法是使用Blazor您選擇的工具來建立應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 052a787fbe6411dbaa953f10fcd982dfbd41f1af
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82769451"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="071c6-103">開始使用 ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="071c6-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="071c6-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="071c6-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="071c6-105">若要開始使用 Blazor，請遵循您所選工具的指導方針：</span><span class="sxs-lookup"><span data-stu-id="071c6-105">To get started with Blazor, follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="071c6-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="071c6-106">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="071c6-107">若要建立 Blazor 伺服器應用程式，請使用**ASP.NET 和 網頁程式開發**工作負載安裝最新版的[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) 。</span><span class="sxs-lookup"><span data-stu-id="071c6-107">To create Blazor Server apps, install the latest version of [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="071c6-108">若要建立 Blazor 伺服器和 Blazor WebAssembly 應用程式，請使用**ASP.NET 和 網頁程式開發**工作負載安裝[Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/)的最新預覽。</span><span class="sxs-lookup"><span data-stu-id="071c6-108">To create Blazor Server and Blazor WebAssembly apps, install the latest preview of [Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="071c6-109">如需這兩個 Blazor 裝載模型的詳細資訊，請參閱<xref:blazor/hosting-models> *Blazor WebAssembly*和*Blazor Server*。</span><span class="sxs-lookup"><span data-stu-id="071c6-109">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="071c6-110">執行下列命令來安裝 Blazor WebAssembly Preview 範本：</span><span class="sxs-lookup"><span data-stu-id="071c6-110">Install the Blazor WebAssembly Preview template by running the following command:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-rc1.20223.4
   ```

1. <span data-ttu-id="071c6-111">建立新專案。</span><span class="sxs-lookup"><span data-stu-id="071c6-111">Create a new project.</span></span>

1. <span data-ttu-id="071c6-112">選取 [ **Blazor 應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="071c6-112">Select **Blazor App**.</span></span> <span data-ttu-id="071c6-113">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="071c6-113">Select **Next**.</span></span>

1. <span data-ttu-id="071c6-114">在 [專案名稱]\*\*\*\* 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="071c6-114">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="071c6-115">確認 [**位置**] 專案正確，或提供專案的 [位置]。</span><span class="sxs-lookup"><span data-stu-id="071c6-115">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="071c6-116">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="071c6-116">Select **Create**.</span></span>

1. <span data-ttu-id="071c6-117">如需 Blazor WebAssembly 體驗（Visual Studio 16.6 Preview 2 或更新版本），請選擇 [ **Blazor WebAssembly 應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="071c6-117">For a Blazor WebAssembly experience (Visual Studio 16.6 Preview 2 or later), choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="071c6-118">如需 Blazor 伺服器體驗（Visual Studio 16.4 或更新版本），請選擇 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="071c6-118">For a Blazor Server experience (Visual Studio 16.4 or later), choose the **Blazor Server App** template.</span></span> <span data-ttu-id="071c6-119">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="071c6-119">Select **Create**.</span></span>

1. <span data-ttu-id="071c6-120">按<kbd>Ctrl</kbd>+<kbd>F5</kbd>執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="071c6-120">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="071c6-121">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="071c6-121">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="071c6-122">安裝[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="071c6-122">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="071c6-123">藉由執行下列命令，選擇性地安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview 範本：</span><span class="sxs-lookup"><span data-stu-id="071c6-123">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview template by running the following command:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-rc1.20223.4
   ```
   
   <span data-ttu-id="071c6-124">如需這兩個 Blazor 裝載模型的詳細資訊，請參閱<xref:blazor/hosting-models> *Blazor WebAssembly*和*Blazor Server*。</span><span class="sxs-lookup"><span data-stu-id="071c6-124">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

   > [!NOTE]
   > <span data-ttu-id="071c6-125">使用 3.2 Preview Blazor WebAssembly 範本時，**需要** [.NET Core SDK 版本3.1.201 或更新版本](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="071c6-125">The [.NET Core SDK version 3.1.201 or later](https://dotnet.microsoft.com/download/dotnet-core/3.1) is **required** to use the 3.2 Preview Blazor WebAssembly template.</span></span> <span data-ttu-id="071c6-126">藉由`dotnet --version`在命令 shell 中執行，確認已安裝的 .NET Core SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="071c6-126">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="071c6-127">安裝 [Visual Studio Code](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="071c6-127">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

1. <span data-ttu-id="071c6-128">安裝適用于[Visual Studio Code 擴充](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)功能的最新 c # 和[JavaScript 偵錯工具（夜間）](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly)延伸模組，並`debug.javascript.usePreview`將設定為。 `true`</span><span class="sxs-lookup"><span data-stu-id="071c6-128">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the [JavaScript Debugger (Nightly)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) extension with `debug.javascript.usePreview` set to `true`.</span></span>

1. <span data-ttu-id="071c6-129">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="071c6-129">For a Blazor Server experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   <span data-ttu-id="071c6-130">如需 Blazor 的 WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="071c6-130">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

1. <span data-ttu-id="071c6-131">在 Visual Studio Code 中開啟 [ *WebApplication1* ] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="071c6-131">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

1. <span data-ttu-id="071c6-132">IDE 會要求您新增資產，以建立和對專案進行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="071c6-132">The IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="071c6-133">選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="071c6-133">Select **Yes**.</span></span>

1. <span data-ttu-id="071c6-134">使用 Blazor 伺服器，使用 Visual Studio Code 偵錯工具執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="071c6-134">With Blazor Server, run the app using the Visual Studio Code debugger.</span></span>

   <span data-ttu-id="071c6-135">透過 Blazor WebAssembly，使用 **.Net Core 啟動（Blazor 獨立式）** 啟動設定來啟動應用程式，然後在 Chrome 啟動設定（需要 chrome）**中使用 .Net Core Debug Blazor Web 元件**來啟動瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="071c6-135">With Blazor WebAssembly, start the app using the **.NET Core Launch (Blazor Standalone)** launch configuration and then start the browser using the **.NET Core Debug Blazor Web Assembly in Chrome** launch configuration (requires Chrome).</span></span> <span data-ttu-id="071c6-136">如需詳細資訊，請參閱<xref:blazor/debug#visual-studio-code>。</span><span class="sxs-lookup"><span data-stu-id="071c6-136">For more information, see <xref:blazor/debug#visual-studio-code>.</span></span>

1. <span data-ttu-id="071c6-137">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="071c6-137">In a browser, navigate to `https://localhost:5001`.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="071c6-138">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="071c6-138">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="071c6-139">Visual Studio for Mac 支援 Blazor 伺服器。</span><span class="sxs-lookup"><span data-stu-id="071c6-139">Blazor Server is supported in Visual Studio for Mac.</span></span> <span data-ttu-id="071c6-140">目前不支援 Blazor WebAssembly。</span><span class="sxs-lookup"><span data-stu-id="071c6-140">Blazor WebAssembly isn't supported at this time.</span></span> <span data-ttu-id="071c6-141">若要在 macOS 上建立 Blazor WebAssembly apps，請遵循 [ **.NET Core CLI** ] 索引標籤上的指導方針。如需這兩個 Blazor 裝載模型的詳細資訊，請參閱<xref:blazor/hosting-models> *Blazor WebAssembly*和*Blazor Server*。</span><span class="sxs-lookup"><span data-stu-id="071c6-141">To create Blazor WebAssembly apps on macOS, follow the guidance on the **.NET Core CLI** tab. For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="071c6-142">安裝[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)。</span><span class="sxs-lookup"><span data-stu-id="071c6-142">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

1. <span data-ttu-id="071c6-143"> > 在 [**開始] 視\窗\*\*\**中選取 [** 檔案] [**新增方案**] 或 [建立**新**專案]。</span><span class="sxs-lookup"><span data-stu-id="071c6-143">Select **File** > **New Solution** or create a **New** project from the **Start Window**.</span></span>

1. <span data-ttu-id="071c6-144">在提要欄位中，選取 [ **.net Core** > **應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="071c6-144">In the sidebar, select **.NET Core** > **App**.</span></span>

1. <span data-ttu-id="071c6-145">選取 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="071c6-145">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="071c6-146">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="071c6-146">Select **Next**.</span></span>

1. <span data-ttu-id="071c6-147">確認下列設定：</span><span class="sxs-lookup"><span data-stu-id="071c6-147">Confirm the following configurations:</span></span>

   * <span data-ttu-id="071c6-148">設定為 **.Net Core 3.1**的**目標 Framework** 。</span><span class="sxs-lookup"><span data-stu-id="071c6-148">**Target Framework** set to **.NET Core 3.1**.</span></span>
   * <span data-ttu-id="071c6-149">**驗證**設為 [**無驗證**]。</span><span class="sxs-lookup"><span data-stu-id="071c6-149">**Authentication** set to **No Authentication**.</span></span>
   
   <span data-ttu-id="071c6-150">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="071c6-150">Select **Next**.</span></span>

1. <span data-ttu-id="071c6-151">在 [**專案名稱**] 欄位中，將`WebApplication1`應用程式命名為。</span><span class="sxs-lookup"><span data-stu-id="071c6-151">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="071c6-152">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="071c6-152">Select **Create**.</span></span>

1. <span data-ttu-id="071c6-153">選取 [**執行** > **而不進行調試**程式]，以*在沒有偵錯工具的情況下*執行 app。</span><span class="sxs-lookup"><span data-stu-id="071c6-153">Select **Run** > **Start Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="071c6-154">目前不支援這次的調試。</span><span class="sxs-lookup"><span data-stu-id="071c6-154">Debugging isn't supported at this time.</span></span>

<!-- HOLD FOR 8.6 GA

1. Select **File** > **New Solution** or create a **New** project from the **Start Window**.

1. In the sidebar, select **Web and Console** > **App**.

1. For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template. For a Blazor Server experience, choose the **Blazor Server App** template. Select **Next**.

   For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.

1. Confirm the following configurations:

   * **Target Framework** set to **.NET Core 3.1**.
   * **Authentication** set to **No Authentication**.
   
   Select **Next**.

1. In the **Project Name** field, name the app `WebApplication1`. Select **Create**.

1. Select **Run** > **Start Without Debugging** to run the app *without the debugger*. Run the app with **Run** > **Start Debugging** or the Run (&#9654;) button to run the app *with the debugger*.

-->

<span data-ttu-id="071c6-155">如果出現會信任開發憑證的提示，請信任憑證並繼續。</span><span class="sxs-lookup"><span data-stu-id="071c6-155">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span> <span data-ttu-id="071c6-156">需要使用者和 keychain 密碼，才能信任憑證。</span><span class="sxs-lookup"><span data-stu-id="071c6-156">The user and keychain passwords are required to trust the certificate.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="071c6-157">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="071c6-157">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="071c6-158">安裝[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="071c6-158">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="071c6-159">藉由執行下列命令，選擇性地安裝 Blazor WebAssembly preview 範本：</span><span class="sxs-lookup"><span data-stu-id="071c6-159">Optionally install the Blazor WebAssembly preview template by running the following command:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-rc1.20223.4
   ```
   
   <span data-ttu-id="071c6-160">如需這兩個 Blazor 裝載模型的詳細資訊，請參閱<xref:blazor/hosting-models> *Blazor WebAssembly*和*Blazor Server*。</span><span class="sxs-lookup"><span data-stu-id="071c6-160">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

   > [!NOTE]
   > <span data-ttu-id="071c6-161">使用 3.2 Preview Blazor WebAssembly 範本時，**需要** [.NET Core SDK 版本3.1.201 或更新版本](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="071c6-161">The [.NET Core SDK version 3.1.201 or later](https://dotnet.microsoft.com/download/dotnet-core/3.1) is **required** to use the 3.2 Preview Blazor WebAssembly template.</span></span> <span data-ttu-id="071c6-162">藉由`dotnet --version`在命令 shell 中執行，確認已安裝的 .NET Core SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="071c6-162">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="071c6-163">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="071c6-163">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="071c6-164">如需 Blazor 的 WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="071c6-164">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="071c6-165">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="071c6-165">In a browser, navigate to `https://localhost:5001`.</span></span>

---

<span data-ttu-id="071c6-166">提要欄位中的索引標籤可使用多個頁面：</span><span class="sxs-lookup"><span data-stu-id="071c6-166">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="071c6-167">家庭</span><span class="sxs-lookup"><span data-stu-id="071c6-167">Home</span></span>
* <span data-ttu-id="071c6-168">計數器</span><span class="sxs-lookup"><span data-stu-id="071c6-168">Counter</span></span>
* <span data-ttu-id="071c6-169">提取資料</span><span class="sxs-lookup"><span data-stu-id="071c6-169">Fetch data</span></span>

<span data-ttu-id="071c6-170">在 [計數器] 頁面上，選取 [按我]\*\*\*\* 按鈕以在不重新整理頁面的情況下讓計數器遞增。</span><span class="sxs-lookup"><span data-stu-id="071c6-170">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="071c6-171">將網頁中的計數器遞增通常需要撰寫 JavaScript，但Blazor您可以使用 c #。</span><span class="sxs-lookup"><span data-stu-id="071c6-171">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="071c6-172">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="071c6-172">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="071c6-173">`/counter`在瀏覽器中的要求（如頂端的`@page`指示詞所指定）會導致`Counter`元件轉譯其內容。</span><span class="sxs-lookup"><span data-stu-id="071c6-173">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="071c6-174">元件會轉譯成轉譯樹狀結構的記憶體中標記法，然後用來以彈性且有效率的方式更新 UI。</span><span class="sxs-lookup"><span data-stu-id="071c6-174">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="071c6-175">每次選取 [**按我**] 按鈕時：</span><span class="sxs-lookup"><span data-stu-id="071c6-175">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="071c6-176">引發`onclick`事件。</span><span class="sxs-lookup"><span data-stu-id="071c6-176">The `onclick` event is fired.</span></span>
* <span data-ttu-id="071c6-177">已呼叫 `IncrementCount` 方法。</span><span class="sxs-lookup"><span data-stu-id="071c6-177">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="071c6-178">`currentCount`會遞增。</span><span class="sxs-lookup"><span data-stu-id="071c6-178">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="071c6-179">元件會再次轉譯。</span><span class="sxs-lookup"><span data-stu-id="071c6-179">The component is rendered again.</span></span>

<span data-ttu-id="071c6-180">執行時間會比較新的內容與先前的內容，而且只會將已變更的內容套用至檔物件模型（DOM）。</span><span class="sxs-lookup"><span data-stu-id="071c6-180">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="071c6-181">使用 HTML 語法將元件新增至另一個元件。</span><span class="sxs-lookup"><span data-stu-id="071c6-181">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="071c6-182">例如，藉由將`Counter` `<Counter />`元素新增至`Index`元件，將元件新增至應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="071c6-182">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="071c6-183">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="071c6-183">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="071c6-184">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="071c6-184">Run the app.</span></span> <span data-ttu-id="071c6-185">首頁有自己的計數器，由`Counter`元件提供。</span><span class="sxs-lookup"><span data-stu-id="071c6-185">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="071c6-186">元件參數是使用屬性或[子內容](xref:blazor/components#child-content)所指定，可讓您設定子元件上的屬性。</span><span class="sxs-lookup"><span data-stu-id="071c6-186">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="071c6-187">若要將參數新增至`Counter`元件，請更新元件的`@code`區塊：</span><span class="sxs-lookup"><span data-stu-id="071c6-187">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="071c6-188">`IncrementAmount`使用`[Parameter]`屬性加入的公用屬性。</span><span class="sxs-lookup"><span data-stu-id="071c6-188">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="071c6-189">將 `IncrementCount` 方法變更為在增加 `currentCount`的值時使用 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="071c6-189">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="071c6-190">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="071c6-190">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="071c6-191">使用屬性`IncrementAmount` ，在`Index`元件的`<Counter>`元素中指定。</span><span class="sxs-lookup"><span data-stu-id="071c6-191">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="071c6-192">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="071c6-192">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="071c6-193">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="071c6-193">Run the app.</span></span> <span data-ttu-id="071c6-194">`Index`元件有自己的計數器，每次選取 [按**我**] 按鈕時，就會遞增10。</span><span class="sxs-lookup"><span data-stu-id="071c6-194">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="071c6-195">中`Counter` `/counter`的元件（*razor*）會繼續遞增一。</span><span class="sxs-lookup"><span data-stu-id="071c6-195">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="071c6-196">後續步驟</span><span class="sxs-lookup"><span data-stu-id="071c6-196">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="071c6-197">其他資源</span><span class="sxs-lookup"><span data-stu-id="071c6-197">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
