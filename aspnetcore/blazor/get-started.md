---
title: 開始使用 ASP.NET Core Blazor
author: guardrex
description: 藉由使用您選擇的工具來建立 Blazor 應用程式，開始使用 Blazor。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/25/2019
no-loc:
- Blazor
uid: blazor/get-started
ms.openlocfilehash: 7d495bddde3c01c743db9757204a5cf59d8b160b
ms.sourcegitcommit: 918d7000b48a2892750264b852bad9e96a1165a7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2019
ms.locfileid: "74550319"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="60ff4-103">開始使用 ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="60ff4-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="60ff4-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="60ff4-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="60ff4-105">開始使用 Blazor：</span><span class="sxs-lookup"><span data-stu-id="60ff4-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="60ff4-106">安裝[.Net Core 3.1 PREVIEW SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="60ff4-106">Install the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="60ff4-107">在命令 shell 中執行下列命令，以安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)範本。</span><span class="sxs-lookup"><span data-stu-id="60ff4-107">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by running the following command in a command shell.</span></span> <span data-ttu-id="60ff4-108">[AspNetCore.Blazor。範本](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)套件有預覽版本，而 Blazor WebAssembly 處於預覽階段。</span><span class="sxs-lookup"><span data-stu-id="60ff4-108">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview3.19555.2
   ```

1. <span data-ttu-id="60ff4-109">遵循您選擇的工具的指導方針：</span><span class="sxs-lookup"><span data-stu-id="60ff4-109">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60ff4-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60ff4-110">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="60ff4-111">1 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-111">1\.</span></span> <span data-ttu-id="60ff4-112">使用**ASP.NET 和 網頁程式開發**工作負載安裝[Visual Studio 16.4 Preview 2 或更新版本](https://visualstudio.microsoft.com/vs/preview/)。</span><span class="sxs-lookup"><span data-stu-id="60ff4-112">Install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="60ff4-113">2 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-113">2\.</span></span> <span data-ttu-id="60ff4-114">建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="60ff4-114">Create a new project.</span></span>

   <span data-ttu-id="60ff4-115">3 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-115">3\.</span></span> <span data-ttu-id="60ff4-116">選取 [ **Blazor 應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-116">Select **Blazor App**.</span></span> <span data-ttu-id="60ff4-117">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-117">Select **Next**.</span></span>

   <span data-ttu-id="60ff4-118">4 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-118">4\.</span></span> <span data-ttu-id="60ff4-119">在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="60ff4-119">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="60ff4-120">確認 [**位置**] 專案正確，或提供專案的 [位置]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-120">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="60ff4-121">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-121">Select **Create**.</span></span>

   <span data-ttu-id="60ff4-122">5 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-122">5\.</span></span> <span data-ttu-id="60ff4-123">如需 Blazor WebAssembly 體驗，請選擇 [ **Blazor WebAssembly 應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="60ff4-123">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="60ff4-124">如需 Blazor 伺服器體驗，請選擇 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="60ff4-124">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="60ff4-125">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-125">Select **Create**.</span></span> <span data-ttu-id="60ff4-126">如需這兩個 Blazor 裝載模型的詳細資訊， *Blazor 伺服器*和 *Blazor WebAssembly*，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="60ff4-126">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="60ff4-127">6。</span><span class="sxs-lookup"><span data-stu-id="60ff4-127">6\.</span></span> <span data-ttu-id="60ff4-128">按下 **Ctrl**+**F5** 即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="60ff4-128">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="60ff4-129">如果您已安裝 ASP.NET Core Blazor （Preview 6 或更早版本）先前預覽版本的 Blazor Visual Studio 延伸模組，則可以卸載擴充功能。</span><span class="sxs-lookup"><span data-stu-id="60ff4-129">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="60ff4-130">在命令介面中安裝 Blazor 範本，現在已足以呈現 Visual Studio 中的範本。</span><span class="sxs-lookup"><span data-stu-id="60ff4-130">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="60ff4-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="60ff4-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="60ff4-132">1 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-132">1\.</span></span> <span data-ttu-id="60ff4-133">安裝 [Visual Studio Code (英文)](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="60ff4-133">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="60ff4-134">2 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-134">2\.</span></span> <span data-ttu-id="60ff4-135">安裝[ C# Visual Studio Code 擴充](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)功能的最新版本。</span><span class="sxs-lookup"><span data-stu-id="60ff4-135">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="60ff4-136">3 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-136">3\.</span></span> <span data-ttu-id="60ff4-137">如需 Blazor WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="60ff4-137">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="60ff4-138">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="60ff4-138">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="60ff4-139">如需這兩個 Blazor 裝載模型的詳細資訊， *Blazor 伺服器*和 *Blazor WebAssembly*，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="60ff4-139">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="60ff4-140">4 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-140">4\.</span></span> <span data-ttu-id="60ff4-141">在 Visual Studio Code 中開啟 [ *WebApplication1* ] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="60ff4-141">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="60ff4-142">5 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-142">5\.</span></span> <span data-ttu-id="60ff4-143">若為 Blazor 伺服器專案，IDE 會要求您新增資產以建立和對專案進行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="60ff4-143">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="60ff4-144">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-144">Select **Yes**.</span></span>

   <span data-ttu-id="60ff4-145">6。</span><span class="sxs-lookup"><span data-stu-id="60ff4-145">6\.</span></span> <span data-ttu-id="60ff4-146">如果使用 Blazor 伺服器應用程式，請使用 Visual Studio Code 偵錯工具來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="60ff4-146">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="60ff4-147">如果使用 Blazor WebAssembly 應用程式，請從應用程式的專案資料夾執行 `dotnet run`。</span><span class="sxs-lookup"><span data-stu-id="60ff4-147">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="60ff4-148">7 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-148">7\.</span></span> <span data-ttu-id="60ff4-149">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="60ff4-149">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="60ff4-150">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="60ff4-150">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="60ff4-151">1 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-151">1\.</span></span> <span data-ttu-id="60ff4-152">安裝[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)。</span><span class="sxs-lookup"><span data-stu-id="60ff4-152">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span> <span data-ttu-id="60ff4-153">將[更新通道切換為 [預覽](/visualstudio/mac/install-preview)]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-153">Switch the [Update channel to Preview](/visualstudio/mac/install-preview).</span></span>

   <span data-ttu-id="60ff4-154">2 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-154">2\.</span></span> <span data-ttu-id="60ff4-155">選取 **[** 檔案] > [**新增方案**] 或 [建立**新專案**]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-155">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="60ff4-156">3 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-156">3\.</span></span> <span data-ttu-id="60ff4-157">在側邊欄中，選取 [ **.Net Core** > **應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-157">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="60ff4-158">4 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-158">4\.</span></span> <span data-ttu-id="60ff4-159">選取 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="60ff4-159">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="60ff4-160">目前只有 Blazor 伺服器範本可在 Visual Studio for Mac 中使用。</span><span class="sxs-lookup"><span data-stu-id="60ff4-160">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="60ff4-161">如需 Blazor WebAssembly 體驗，請遵循 [ **.NET Core CLI** ] 索引標籤上的指示。選取 Blazor 伺服器範本之後，請選取 **[下一步]** 。</span><span class="sxs-lookup"><span data-stu-id="60ff4-161">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="60ff4-162">如需這兩個 Blazor 裝載模型的詳細資訊， *Blazor 伺服器*和 *Blazor WebAssembly*，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="60ff4-162">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="60ff4-163">5 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-163">5\.</span></span> <span data-ttu-id="60ff4-164">**目標 Framework**預設為 **.net core 3.0** （如果已安裝 3.1 Preview SDK，則為 **.net core 3.1** ）。</span><span class="sxs-lookup"><span data-stu-id="60ff4-164">The **Target Framework** defaults to **.NET Core 3.0** (or **.NET Core 3.1** if the 3.1 Preview SDK is installed).</span></span> <span data-ttu-id="60ff4-165">選取架構，然後選取 **[下一步]** 。</span><span class="sxs-lookup"><span data-stu-id="60ff4-165">Select the framework and select **Next**.</span></span>

   <span data-ttu-id="60ff4-166">6。</span><span class="sxs-lookup"><span data-stu-id="60ff4-166">6\.</span></span> <span data-ttu-id="60ff4-167">在 [**專案名稱**] 欄位中，將應用程式命名為 `WebApplication1`。</span><span class="sxs-lookup"><span data-stu-id="60ff4-167">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="60ff4-168">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-168">Select **Create**.</span></span>

   <span data-ttu-id="60ff4-169">7 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-169">7\.</span></span> <span data-ttu-id="60ff4-170">選取 [**執行**] > **執行而不進行調試**程式，以在不進行偵錯工具的*情況下*執行</span><span class="sxs-lookup"><span data-stu-id="60ff4-170">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="60ff4-171">使用 [**開始調試**程式] 執行應用程式，以*使用調試*程式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="60ff4-171">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="60ff4-172">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="60ff4-172">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="60ff4-173">如需 Blazor WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="60ff4-173">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="60ff4-174">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="60ff4-174">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="60ff4-175">如需這兩個 Blazor 裝載模型的詳細資訊， *Blazor 伺服器*和 *Blazor WebAssembly*，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="60ff4-175">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="60ff4-176">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="60ff4-176">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="60ff4-177">安裝最新的[.Net Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)版本。</span><span class="sxs-lookup"><span data-stu-id="60ff4-177">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="60ff4-178">安裝[.Net Core 3.1 PREVIEW SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) ，然後在命令 shell 中執行下列命令，選擇性地安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)範本：</span><span class="sxs-lookup"><span data-stu-id="60ff4-178">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by installing the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) and then running the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview3.19555.2
   ```

1. <span data-ttu-id="60ff4-179">遵循您選擇的工具的指導方針：</span><span class="sxs-lookup"><span data-stu-id="60ff4-179">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60ff4-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60ff4-180">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="60ff4-181">1 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-181">1\.</span></span> <span data-ttu-id="60ff4-182">使用**ASP.NET 和 網頁程式開發**工作負載安裝最新的[Visual Studio](https://visualstudio.com/vs/) 。</span><span class="sxs-lookup"><span data-stu-id="60ff4-182">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="60ff4-183">2 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-183">2\.</span></span> <span data-ttu-id="60ff4-184">選擇性地使用適用于 Blazor WebAssembly 應用程式開發的**ASP.NET 和 網頁程式開發**工作負載，安裝[Visual Studio 16.4 Preview 2 或更新版本](https://visualstudio.microsoft.com/vs/preview/)。</span><span class="sxs-lookup"><span data-stu-id="60ff4-184">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="60ff4-185">3 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-185">3\.</span></span> <span data-ttu-id="60ff4-186">建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="60ff4-186">Create a new project.</span></span>

   <span data-ttu-id="60ff4-187">4 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-187">4\.</span></span> <span data-ttu-id="60ff4-188">選取 [ **Blazor 應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-188">Select **Blazor App**.</span></span> <span data-ttu-id="60ff4-189">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-189">Select **Next**.</span></span>

   <span data-ttu-id="60ff4-190">5 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-190">5\.</span></span> <span data-ttu-id="60ff4-191">在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="60ff4-191">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="60ff4-192">確認 [**位置**] 專案正確，或提供專案的 [位置]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-192">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="60ff4-193">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-193">Select **Create**.</span></span>

   <span data-ttu-id="60ff4-194">6。</span><span class="sxs-lookup"><span data-stu-id="60ff4-194">6\.</span></span> <span data-ttu-id="60ff4-195">如需 Blazor WebAssembly 體驗，請選擇 [ **Blazor WebAssembly 應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="60ff4-195">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="60ff4-196">如需 Blazor 伺服器體驗，請選擇 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="60ff4-196">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="60ff4-197">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-197">Select **Create**.</span></span> <span data-ttu-id="60ff4-198">如需這兩個 Blazor 裝載模型的詳細資訊， *Blazor 伺服器*和 *Blazor WebAssembly*，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="60ff4-198">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="60ff4-199">7 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-199">7\.</span></span> <span data-ttu-id="60ff4-200">按下 **F5** 即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="60ff4-200">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="60ff4-201">如果您已安裝 ASP.NET Core Blazor （Preview 6 或更早版本）先前預覽版本的 Blazor Visual Studio 延伸模組，則可以卸載擴充功能。</span><span class="sxs-lookup"><span data-stu-id="60ff4-201">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="60ff4-202">在命令介面中安裝 Blazor 範本，現在已足以呈現 Visual Studio 中的範本。</span><span class="sxs-lookup"><span data-stu-id="60ff4-202">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="60ff4-203">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="60ff4-203">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="60ff4-204">1 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-204">1\.</span></span> <span data-ttu-id="60ff4-205">安裝 [Visual Studio Code (英文)](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="60ff4-205">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="60ff4-206">2 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-206">2\.</span></span> <span data-ttu-id="60ff4-207">安裝[ C# Visual Studio Code 擴充](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)功能的最新版本。</span><span class="sxs-lookup"><span data-stu-id="60ff4-207">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="60ff4-208">3 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-208">3\.</span></span> <span data-ttu-id="60ff4-209">如需 Blazor WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="60ff4-209">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="60ff4-210">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="60ff4-210">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="60ff4-211">如需這兩個 Blazor 裝載模型的詳細資訊， *Blazor 伺服器*和 *Blazor WebAssembly*，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="60ff4-211">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="60ff4-212">4 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-212">4\.</span></span> <span data-ttu-id="60ff4-213">在 Visual Studio Code 中開啟 [ *WebApplication1* ] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="60ff4-213">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="60ff4-214">5 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-214">5\.</span></span> <span data-ttu-id="60ff4-215">若為 Blazor 伺服器專案，IDE 會要求您新增資產以建立和對專案進行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="60ff4-215">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="60ff4-216">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-216">Select **Yes**.</span></span>

   <span data-ttu-id="60ff4-217">6。</span><span class="sxs-lookup"><span data-stu-id="60ff4-217">6\.</span></span> <span data-ttu-id="60ff4-218">如果使用 Blazor 伺服器應用程式，請使用 Visual Studio Code 偵錯工具來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="60ff4-218">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="60ff4-219">如果使用 Blazor WebAssembly 應用程式，請從應用程式的專案資料夾執行 `dotnet run`。</span><span class="sxs-lookup"><span data-stu-id="60ff4-219">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="60ff4-220">7 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-220">7\.</span></span> <span data-ttu-id="60ff4-221">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="60ff4-221">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="60ff4-222">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="60ff4-222">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="60ff4-223">1 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-223">1\.</span></span> <span data-ttu-id="60ff4-224">安裝[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)。</span><span class="sxs-lookup"><span data-stu-id="60ff4-224">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span> <span data-ttu-id="60ff4-225">將[更新通道切換為 [預覽](/visualstudio/mac/install-preview)]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-225">Switch the [Update channel to Preview](/visualstudio/mac/install-preview).</span></span>

   <span data-ttu-id="60ff4-226">2 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-226">2\.</span></span> <span data-ttu-id="60ff4-227">選取 **[** 檔案] > [**新增方案**] 或 [建立**新專案**]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-227">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="60ff4-228">3 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-228">3\.</span></span> <span data-ttu-id="60ff4-229">在側邊欄中，選取 [ **.Net Core** > **應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-229">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="60ff4-230">4 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-230">4\.</span></span> <span data-ttu-id="60ff4-231">選取 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="60ff4-231">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="60ff4-232">目前只有 Blazor 伺服器範本可在 Visual Studio for Mac 中使用。</span><span class="sxs-lookup"><span data-stu-id="60ff4-232">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="60ff4-233">如需 Blazor WebAssembly 體驗，請遵循 [ **.NET Core CLI** ] 索引標籤上的指示。選取 Blazor 伺服器範本之後，請選取 **[下一步]** 。</span><span class="sxs-lookup"><span data-stu-id="60ff4-233">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="60ff4-234">如需這兩個 Blazor 裝載模型的詳細資訊， *Blazor 伺服器*和 *Blazor WebAssembly*，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="60ff4-234">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="60ff4-235">5 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-235">5\.</span></span> <span data-ttu-id="60ff4-236">**目標 Framework**預設為 **.net core 3.0** （如果已安裝 3.1 Preview SDK，則為 **.net core 3.1** ）。</span><span class="sxs-lookup"><span data-stu-id="60ff4-236">The **Target Framework** defaults to **.NET Core 3.0** (or **.NET Core 3.1** if the 3.1 Preview SDK is installed).</span></span> <span data-ttu-id="60ff4-237">選取架構，然後選取 **[下一步]** 。</span><span class="sxs-lookup"><span data-stu-id="60ff4-237">Select the framework and select **Next**.</span></span>

   <span data-ttu-id="60ff4-238">6。</span><span class="sxs-lookup"><span data-stu-id="60ff4-238">6\.</span></span> <span data-ttu-id="60ff4-239">在 [**專案名稱**] 欄位中，將應用程式命名為 `WebApplication1`。</span><span class="sxs-lookup"><span data-stu-id="60ff4-239">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="60ff4-240">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="60ff4-240">Select **Create**.</span></span>

   <span data-ttu-id="60ff4-241">7 \。</span><span class="sxs-lookup"><span data-stu-id="60ff4-241">7\.</span></span> <span data-ttu-id="60ff4-242">選取 [**執行**] > **執行而不進行調試**程式，以在不進行偵錯工具的*情況下*執行</span><span class="sxs-lookup"><span data-stu-id="60ff4-242">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="60ff4-243">使用 [**開始調試**程式] 執行應用程式，以*使用調試*程式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="60ff4-243">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="60ff4-244">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="60ff4-244">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="60ff4-245">如需 Blazor WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="60ff4-245">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="60ff4-246">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="60ff4-246">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="60ff4-247">如需這兩個 Blazor 裝載模型的詳細資訊， *Blazor 伺服器*和 *Blazor WebAssembly*，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="60ff4-247">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="60ff4-248">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="60ff4-248">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="60ff4-249">提要欄位中的索引標籤可使用多個頁面：</span><span class="sxs-lookup"><span data-stu-id="60ff4-249">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="60ff4-250">首頁</span><span class="sxs-lookup"><span data-stu-id="60ff4-250">Home</span></span>
* <span data-ttu-id="60ff4-251">計數器</span><span class="sxs-lookup"><span data-stu-id="60ff4-251">Counter</span></span>
* <span data-ttu-id="60ff4-252">提取資料</span><span class="sxs-lookup"><span data-stu-id="60ff4-252">Fetch data</span></span>

<span data-ttu-id="60ff4-253">在 [計數器] 頁面上，選取 [按我] 按鈕以在不重新整理頁面的情況下讓計數器遞增。</span><span class="sxs-lookup"><span data-stu-id="60ff4-253">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="60ff4-254">將網頁中的計數器遞增通常需要撰寫 JavaScript，但您可以使用C#Blazor。</span><span class="sxs-lookup"><span data-stu-id="60ff4-254">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="60ff4-255">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="60ff4-255">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="60ff4-256">在瀏覽器中 `/counter` 的要求，如同頂端的 `@page` 指示詞所指定，會導致 `Counter` 元件轉譯其內容。</span><span class="sxs-lookup"><span data-stu-id="60ff4-256">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="60ff4-257">元件會轉譯成轉譯樹狀結構的記憶體中標記法，然後用來以彈性且有效率的方式更新 UI。</span><span class="sxs-lookup"><span data-stu-id="60ff4-257">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="60ff4-258">每次選取 [**按我**] 按鈕時：</span><span class="sxs-lookup"><span data-stu-id="60ff4-258">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="60ff4-259">`onclick` 事件會引發。</span><span class="sxs-lookup"><span data-stu-id="60ff4-259">The `onclick` event is fired.</span></span>
* <span data-ttu-id="60ff4-260">呼叫 `IncrementCount` 方法。</span><span class="sxs-lookup"><span data-stu-id="60ff4-260">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="60ff4-261">`currentCount` 會遞增。</span><span class="sxs-lookup"><span data-stu-id="60ff4-261">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="60ff4-262">元件會再次轉譯。</span><span class="sxs-lookup"><span data-stu-id="60ff4-262">The component is rendered again.</span></span>

<span data-ttu-id="60ff4-263">執行時間會比較新的內容與先前的內容，而且只會將已變更的內容套用至檔物件模型（DOM）。</span><span class="sxs-lookup"><span data-stu-id="60ff4-263">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="60ff4-264">使用 HTML 語法將元件新增至另一個元件。</span><span class="sxs-lookup"><span data-stu-id="60ff4-264">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="60ff4-265">例如，藉由將 `<Counter />` 元素新增至 `Index` 元件，將 `Counter` 元件新增至應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="60ff4-265">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="60ff4-266">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="60ff4-266">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="60ff4-267">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="60ff4-267">Run the app.</span></span> <span data-ttu-id="60ff4-268">首頁有自己的計數器，由 `Counter` 元件提供。</span><span class="sxs-lookup"><span data-stu-id="60ff4-268">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="60ff4-269">元件參數是使用屬性或[子內容](xref:blazor/components#child-content)所指定，可讓您設定子元件上的屬性。</span><span class="sxs-lookup"><span data-stu-id="60ff4-269">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="60ff4-270">若要將參數新增至 `Counter` 元件，請更新元件的 `@code` 區塊：</span><span class="sxs-lookup"><span data-stu-id="60ff4-270">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="60ff4-271">加入具有 `[Parameter]` 屬性之 `IncrementAmount` 的公用屬性。</span><span class="sxs-lookup"><span data-stu-id="60ff4-271">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="60ff4-272">將 `IncrementCount` 方法變更為在增加 `currentCount`的值時使用 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="60ff4-272">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="60ff4-273">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="60ff4-273">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="60ff4-274">使用屬性，在 `Index` 元件的 `<Counter>` 元素中指定 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="60ff4-274">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="60ff4-275">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="60ff4-275">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="60ff4-276">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="60ff4-276">Run the app.</span></span> <span data-ttu-id="60ff4-277">`Index` 元件有自己的計數器，每次選取 [按**我**] 按鈕時，就會遞增10。</span><span class="sxs-lookup"><span data-stu-id="60ff4-277">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="60ff4-278">`/counter` 的 `Counter` 元件（*razor*）會繼續遞增一。</span><span class="sxs-lookup"><span data-stu-id="60ff4-278">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="60ff4-279">後續步驟</span><span class="sxs-lookup"><span data-stu-id="60ff4-279">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="60ff4-280">其他資源</span><span class="sxs-lookup"><span data-stu-id="60ff4-280">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
