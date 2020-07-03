---
title: 適用于 ASP.NET Core 的工具Blazor
author: guardrex
description: 瞭解可用於建立 Blazor 應用程式的工具。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/tooling
ms.openlocfilehash: 3ff610225c798624b7ead7d365924ae3d193829d
ms.sourcegitcommit: 66fca14611eba141d455fe0bd2c37803062e439c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2020
ms.locfileid: "85944941"
---
<!-- zone_pivot_groups: operating-systems -->

# <a name="tooling-for-aspnet-core-blazor"></a><span data-ttu-id="355be-103">適用于 ASP.NET Core 的工具Blazor</span><span class="sxs-lookup"><span data-stu-id="355be-103">Tooling for ASP.NET Core Blazor</span></span>

<span data-ttu-id="355be-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="355be-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="355be-105">選取適用于您平臺的工具：</span><span class="sxs-lookup"><span data-stu-id="355be-105">Select the tooling for your platform:</span></span>

* <span data-ttu-id="355be-106">Windows：選取 [ **Visual Studio** ] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="355be-106">Windows: Select the **Visual Studio** tab.</span></span>
* <span data-ttu-id="355be-107">Linux：選取 [ **Visual Studio Code** ] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="355be-107">Linux: Select the **Visual Studio Code** tab.</span></span>
* <span data-ttu-id="355be-108">macOS：選取 [ **Visual Studio for Mac** ] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="355be-108">macOS: Select the **Visual Studio for Mac** tab.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="355be-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="355be-109">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="355be-110">使用**ASP.NET 和 網頁程式開發**工作負載安裝最新版的[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) 。</span><span class="sxs-lookup"><span data-stu-id="355be-110">Install the latest version of [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

1. <span data-ttu-id="355be-111">建立新專案。</span><span class="sxs-lookup"><span data-stu-id="355be-111">Create a new project.</span></span>

1. <span data-ttu-id="355be-112">選取 [ \*\* Blazor 應用程式\*\*]。</span><span class="sxs-lookup"><span data-stu-id="355be-112">Select **Blazor App**.</span></span> <span data-ttu-id="355be-113">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="355be-113">Select **Next**.</span></span>

1. <span data-ttu-id="355be-114">在 [專案名稱]\*\*\*\* 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="355be-114">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="355be-115">確認 [**位置**] 專案正確，或提供專案的 [位置]。</span><span class="sxs-lookup"><span data-stu-id="355be-115">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="355be-116">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="355be-116">Select **Create**.</span></span>

1. <span data-ttu-id="355be-117">如需 Blazor WebAssembly 體驗，請選擇\*\* Blazor WebAssembly 應用程式\*\*範本。</span><span class="sxs-lookup"><span data-stu-id="355be-117">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="355be-118">如需 Blazor Server 體驗，請選擇\*\* Blazor Server 應用程式\*\*範本。</span><span class="sxs-lookup"><span data-stu-id="355be-118">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="355be-119">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="355be-119">Select **Create**.</span></span>

   <span data-ttu-id="355be-120">如需這兩個 Blazor 裝載模型的詳細資訊，請 *Blazor WebAssembly* *Blazor Server* 參閱 <xref:blazor/hosting-models> 。</span><span class="sxs-lookup"><span data-stu-id="355be-120">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="355be-121">按<kbd>Ctrl</kbd> + <kbd>F5</kbd>執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="355be-121">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="355be-122">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="355be-122">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="355be-123">安裝最新版的[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="355be-123">Install the latest version of the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span> <span data-ttu-id="355be-124">如果您先前已安裝 SDK，您可以在命令 shell 中執行下列命令，以判斷已安裝的版本：</span><span class="sxs-lookup"><span data-stu-id="355be-124">If you previously installed the SDK, you can determine your installed version by executing the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet --version
   ```

1. <span data-ttu-id="355be-125">安裝最新版的[Visual Studio Code](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="355be-125">Install the latest version of [Visual Studio Code](https://code.visualstudio.com/).</span></span>

1. <span data-ttu-id="355be-126">安裝適用于[Visual Studio Code 擴充](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)功能的最新 c # 和[JavaScript 偵錯工具（夜間）](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly)延伸模組，並 `debug.javascript.usePreview` 將設定為 `true` 。</span><span class="sxs-lookup"><span data-stu-id="355be-126">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the [JavaScript Debugger (Nightly)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) extension with `debug.javascript.usePreview` set to `true`.</span></span>

   <span data-ttu-id="355be-127">若要將設定 `debug.javascript.usePreview` 為 `true` 使用 VS Code UI， **File**請開啟 [檔案  >  **喜好**  >  **設定**]，然後搜尋 `debug javascript use preview` 。</span><span class="sxs-lookup"><span data-stu-id="355be-127">To set `debug.javascript.usePreview` to `true` using the VS Code UI, open **File** > **Preferences** > **Settings** and search for `debug javascript use preview`.</span></span> <span data-ttu-id="355be-128">選取 [**使用適用于 Node.js 和 Chrome 的新的預覽 JavaScript 偵錯工具**] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="355be-128">Select the check box for **Use the new in-preview JavaScript debugger for Node.js and Chrome**.</span></span>

1. <span data-ttu-id="355be-129">如需 Blazor WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="355be-129">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   <span data-ttu-id="355be-130">如需 Blazor Server 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="355be-130">For a Blazor Server experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   <span data-ttu-id="355be-131">如需這兩個 Blazor 裝載模型的詳細資訊，請 *Blazor WebAssembly* *Blazor Server* 參閱 <xref:blazor/hosting-models> 。</span><span class="sxs-lookup"><span data-stu-id="355be-131">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="355be-132">在 Visual Studio Code 中開啟 `WebApplication1` 資料夾。</span><span class="sxs-lookup"><span data-stu-id="355be-132">Open the `WebApplication1` folder in Visual Studio Code.</span></span>

1. <span data-ttu-id="355be-133">IDE 會要求您新增資產，以建立和對專案進行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="355be-133">The IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="355be-134">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="355be-134">Select **Yes**.</span></span>

1. <span data-ttu-id="355be-135">按<kbd>Ctrl</kbd> + <kbd>F5</kbd>執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="355be-135">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="355be-136">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="355be-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="355be-137">安裝[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)。</span><span class="sxs-lookup"><span data-stu-id="355be-137">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

1. <span data-ttu-id="355be-138">**File**  >  在 [**開始] 視窗**中選取 [檔案] [**新增方案**] 或 [建立**新**專案]。</span><span class="sxs-lookup"><span data-stu-id="355be-138">Select **File** > **New Solution** or create a **New** project from the **Start Window**.</span></span>

1. <span data-ttu-id="355be-139">在提要欄位中，選取 [ **Web 和主控台**  >  **應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="355be-139">In the sidebar, select **Web and Console** > **App**.</span></span>

   <span data-ttu-id="355be-140">如需 Blazor WebAssembly 體驗，請選擇\*\* Blazor WebAssembly 應用程式\*\*範本。</span><span class="sxs-lookup"><span data-stu-id="355be-140">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="355be-141">如需 Blazor Server 體驗，請選擇\*\* Blazor Server 應用程式\*\*範本。</span><span class="sxs-lookup"><span data-stu-id="355be-141">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="355be-142">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="355be-142">Select **Next**.</span></span>

   <span data-ttu-id="355be-143">如需這兩個 Blazor 裝載模型的詳細資訊，請 *Blazor WebAssembly* *Blazor Server* 參閱 <xref:blazor/hosting-models> 。</span><span class="sxs-lookup"><span data-stu-id="355be-143">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="355be-144">確認下列設定：</span><span class="sxs-lookup"><span data-stu-id="355be-144">Confirm the following configurations:</span></span>

   * <span data-ttu-id="355be-145">設定為 **.Net Core 3.1**的**目標 Framework** 。</span><span class="sxs-lookup"><span data-stu-id="355be-145">**Target Framework** set to **.NET Core 3.1**.</span></span>
   * <span data-ttu-id="355be-146">**驗證**設為 [**無驗證**]。</span><span class="sxs-lookup"><span data-stu-id="355be-146">**Authentication** set to **No Authentication**.</span></span>
   
   <span data-ttu-id="355be-147">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="355be-147">Select **Next**.</span></span>

1. <span data-ttu-id="355be-148">在 [**專案名稱**] 欄位中，將應用程式命名為 `WebApplication1` 。</span><span class="sxs-lookup"><span data-stu-id="355be-148">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="355be-149">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="355be-149">Select **Create**.</span></span>

1. <span data-ttu-id="355be-150">選取 [**執行**  >  **而不進行調試**程式]，以*在沒有偵錯工具的情況下*執行 app。</span><span class="sxs-lookup"><span data-stu-id="355be-150">Select **Run** > **Start Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="355be-151">使用 [**執行**  >  **開始調試**程式] 或 [執行] （&#9654;）按鈕執行應用程式，以*使用調試*程式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="355be-151">Run the app with **Run** > **Start Debugging** or the Run (&#9654;) button to run the app *with the debugger*.</span></span>

<span data-ttu-id="355be-152">如果出現會信任開發憑證的提示，請信任憑證並繼續。</span><span class="sxs-lookup"><span data-stu-id="355be-152">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span> <span data-ttu-id="355be-153">需要使用者和 keychain 密碼，才能信任憑證。</span><span class="sxs-lookup"><span data-stu-id="355be-153">The user and keychain passwords are required to trust the certificate.</span></span>

---

<!--

::: zone pivot="os-windows"

1. Install the latest version of [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.

1. Create a new project.

1. Select **Blazor App**. Select **Next**.

1. Provide a project name in the **Project name** field or accept the default project name. Confirm the **Location** entry is correct or provide a location for the project. Select **Create**.

1. For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template. For a Blazor Server experience, choose the **Blazor Server App** template. Select **Create**.

   For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.

1. Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.

::: zone-end

::: zone pivot="os-linux"

1. Install the latest version of the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1). If you previously installed the SDK, you can determine your installed version by executing the following command in a command shell:

   ```dotnetcli
   dotnet --version
   ```

1. Install the latest version of [Visual Studio Code](https://code.visualstudio.com/).

1. Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the [JavaScript Debugger (Nightly)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) extension with `debug.javascript.usePreview` set to `true`.

   To set `debug.javascript.usePreview` to `true` using the VS Code UI, open **File** > **Preferences** > **Settings** and search for `debug javascript use preview`. Select the check box for **Use the new in-preview JavaScript debugger for Node.js and Chrome**.

1. For a Blazor WebAssembly experience, execute the following command in a command shell:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   For a Blazor Server experience, execute the following command in a command shell:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.

1. Open the `WebApplication1` folder in Visual Studio Code.

1. The IDE requests that you add assets to build and debug the project. Select **Yes**.

1. Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.

::: zone-end

::: zone pivot="os-macos"

1. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).

1. Select **File** > **New Solution** or create a **New** project from the **Start Window**.

1. In the sidebar, select **Web and Console** > **App**.

   For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template. For a Blazor Server experience, choose the **Blazor Server App** template. Select **Next**.

   For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.

1. Confirm the following configurations:

   * **Target Framework** set to **.NET Core 3.1**.
   * **Authentication** set to **No Authentication**.
   
   Select **Next**.

1. In the **Project Name** field, name the app `WebApplication1`. Select **Create**.

1. Select **Run** > **Start Without Debugging** to run the app *without the debugger*. Run the app with **Run** > **Start Debugging** or the Run (&#9654;) button to run the app *with the debugger*.

If a prompt appears to trust the development certificate, trust the certificate and continue. The user and keychain passwords are required to trust the certificate.

::: zone-end

-->
