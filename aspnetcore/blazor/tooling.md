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
zone_pivot_groups: operating-systems-set-one
ms.openlocfilehash: 538257597ec5c6c705d8280a817c409debe22224
ms.sourcegitcommit: c6467f86c09b1f608b09d37d33c8c43de7ae8bc7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2020
ms.locfileid: "85946817"
---
# <a name="tooling-for-aspnet-core-blazor"></a><span data-ttu-id="041cf-103">適用于 ASP.NET Core 的工具Blazor</span><span class="sxs-lookup"><span data-stu-id="041cf-103">Tooling for ASP.NET Core Blazor</span></span>

<span data-ttu-id="041cf-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="041cf-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

::: zone pivot="os-windows"

1. <span data-ttu-id="041cf-105">使用**ASP.NET 和 網頁程式開發**工作負載安裝最新版的[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) 。</span><span class="sxs-lookup"><span data-stu-id="041cf-105">Install the latest version of [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

1. <span data-ttu-id="041cf-106">建立新專案。</span><span class="sxs-lookup"><span data-stu-id="041cf-106">Create a new project.</span></span>

1. <span data-ttu-id="041cf-107">選取 [ \*\* Blazor 應用程式\*\*]。</span><span class="sxs-lookup"><span data-stu-id="041cf-107">Select **Blazor App**.</span></span> <span data-ttu-id="041cf-108">選取 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="041cf-108">Select **Next**.</span></span>

1. <span data-ttu-id="041cf-109">在 [專案名稱]\*\*\*\* 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="041cf-109">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="041cf-110">確認 [**位置**] 專案正確，或提供專案的 [位置]。</span><span class="sxs-lookup"><span data-stu-id="041cf-110">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="041cf-111">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="041cf-111">Select **Create**.</span></span>

1. <span data-ttu-id="041cf-112">如需 Blazor WebAssembly 體驗，請選擇\*\* Blazor WebAssembly 應用程式\*\*範本。</span><span class="sxs-lookup"><span data-stu-id="041cf-112">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="041cf-113">如需 Blazor Server 體驗，請選擇\*\* Blazor Server 應用程式\*\*範本。</span><span class="sxs-lookup"><span data-stu-id="041cf-113">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="041cf-114">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="041cf-114">Select **Create**.</span></span>

   <span data-ttu-id="041cf-115">如需這兩個 Blazor 裝載模型的詳細資訊，請 *Blazor WebAssembly* *Blazor Server* 參閱 <xref:blazor/hosting-models> 。</span><span class="sxs-lookup"><span data-stu-id="041cf-115">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="041cf-116">按<kbd>Ctrl</kbd> + <kbd>F5</kbd>執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="041cf-116">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.</span></span>

::: zone-end

::: zone pivot="os-linux"

1. <span data-ttu-id="041cf-117">安裝最新版的[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="041cf-117">Install the latest version of the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span> <span data-ttu-id="041cf-118">如果您先前已安裝 SDK，您可以在命令 shell 中執行下列命令，以判斷已安裝的版本：</span><span class="sxs-lookup"><span data-stu-id="041cf-118">If you previously installed the SDK, you can determine your installed version by executing the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet --version
   ```

1. <span data-ttu-id="041cf-119">安裝最新版的[Visual Studio Code](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="041cf-119">Install the latest version of [Visual Studio Code](https://code.visualstudio.com/).</span></span>

1. <span data-ttu-id="041cf-120">安裝適用于[Visual Studio Code 擴充](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)功能的最新 c # 和[JavaScript 偵錯工具（夜間）](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly)延伸模組，並 `debug.javascript.usePreview` 將設定為 `true` 。</span><span class="sxs-lookup"><span data-stu-id="041cf-120">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the [JavaScript Debugger (Nightly)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) extension with `debug.javascript.usePreview` set to `true`.</span></span>

   <span data-ttu-id="041cf-121">若要將設定 `debug.javascript.usePreview` 為 `true` 使用 VS Code UI， **File**請開啟 [檔案  >  **喜好**  >  **設定**]，然後搜尋 `debug javascript use preview` 。</span><span class="sxs-lookup"><span data-stu-id="041cf-121">To set `debug.javascript.usePreview` to `true` using the VS Code UI, open **File** > **Preferences** > **Settings** and search for `debug javascript use preview`.</span></span> <span data-ttu-id="041cf-122">選取 [**使用適用于 Node.js 和 Chrome 的新的預覽 JavaScript 偵錯工具**] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="041cf-122">Select the check box for **Use the new in-preview JavaScript debugger for Node.js and Chrome**.</span></span>

1. <span data-ttu-id="041cf-123">如需 Blazor WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="041cf-123">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   <span data-ttu-id="041cf-124">如需 Blazor Server 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="041cf-124">For a Blazor Server experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   <span data-ttu-id="041cf-125">如需這兩個 Blazor 裝載模型的詳細資訊，請 *Blazor WebAssembly* *Blazor Server* 參閱 <xref:blazor/hosting-models> 。</span><span class="sxs-lookup"><span data-stu-id="041cf-125">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="041cf-126">在 Visual Studio Code 中開啟 `WebApplication1` 資料夾。</span><span class="sxs-lookup"><span data-stu-id="041cf-126">Open the `WebApplication1` folder in Visual Studio Code.</span></span>

1. <span data-ttu-id="041cf-127">IDE 會要求您新增資產，以建立和對專案進行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="041cf-127">The IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="041cf-128">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="041cf-128">Select **Yes**.</span></span>

1. <span data-ttu-id="041cf-129">按<kbd>Ctrl</kbd> + <kbd>F5</kbd>執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="041cf-129">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.</span></span>

::: zone-end

::: zone pivot="os-macos"

1. <span data-ttu-id="041cf-130">安裝[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)。</span><span class="sxs-lookup"><span data-stu-id="041cf-130">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

1. <span data-ttu-id="041cf-131">**File**  >  在 [**開始] 視窗**中選取 [檔案] [**新增方案**] 或 [建立**新**專案]。</span><span class="sxs-lookup"><span data-stu-id="041cf-131">Select **File** > **New Solution** or create a **New** project from the **Start Window**.</span></span>

1. <span data-ttu-id="041cf-132">在提要欄位中，選取 [ **Web 和主控台**  >  **應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="041cf-132">In the sidebar, select **Web and Console** > **App**.</span></span>

   <span data-ttu-id="041cf-133">如需 Blazor WebAssembly 體驗，請選擇\*\* Blazor WebAssembly 應用程式\*\*範本。</span><span class="sxs-lookup"><span data-stu-id="041cf-133">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="041cf-134">如需 Blazor Server 體驗，請選擇\*\* Blazor Server 應用程式\*\*範本。</span><span class="sxs-lookup"><span data-stu-id="041cf-134">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="041cf-135">選取 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="041cf-135">Select **Next**.</span></span>

   <span data-ttu-id="041cf-136">如需這兩個 Blazor 裝載模型的詳細資訊，請 *Blazor WebAssembly* *Blazor Server* 參閱 <xref:blazor/hosting-models> 。</span><span class="sxs-lookup"><span data-stu-id="041cf-136">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="041cf-137">確認下列設定：</span><span class="sxs-lookup"><span data-stu-id="041cf-137">Confirm the following configurations:</span></span>

   * <span data-ttu-id="041cf-138">設定為 **.Net Core 3.1**的**目標 Framework** 。</span><span class="sxs-lookup"><span data-stu-id="041cf-138">**Target Framework** set to **.NET Core 3.1**.</span></span>
   * <span data-ttu-id="041cf-139">**驗證**設為 [**無驗證**]。</span><span class="sxs-lookup"><span data-stu-id="041cf-139">**Authentication** set to **No Authentication**.</span></span>
   
   <span data-ttu-id="041cf-140">選取 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="041cf-140">Select **Next**.</span></span>

1. <span data-ttu-id="041cf-141">在 [**專案名稱**] 欄位中，將應用程式命名為 `WebApplication1` 。</span><span class="sxs-lookup"><span data-stu-id="041cf-141">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="041cf-142">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="041cf-142">Select **Create**.</span></span>

1. <span data-ttu-id="041cf-143">選取 [**執行**  >  **而不進行調試**程式]，以*在沒有偵錯工具的情況下*執行 app。</span><span class="sxs-lookup"><span data-stu-id="041cf-143">Select **Run** > **Start Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="041cf-144">使用 [**執行**  >  **開始調試**程式] 或 [執行] （&#9654;）按鈕執行應用程式，以*使用調試*程式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="041cf-144">Run the app with **Run** > **Start Debugging** or the Run (&#9654;) button to run the app *with the debugger*.</span></span>

<span data-ttu-id="041cf-145">如果出現會信任開發憑證的提示，請信任憑證並繼續。</span><span class="sxs-lookup"><span data-stu-id="041cf-145">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span> <span data-ttu-id="041cf-146">需要使用者和 keychain 密碼，才能信任憑證。</span><span class="sxs-lookup"><span data-stu-id="041cf-146">The user and keychain passwords are required to trust the certificate.</span></span>

::: zone-end
