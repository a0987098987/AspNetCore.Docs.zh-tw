---
title: 開始使用 ASP.NET Core Blazor
author: guardrex
description: 藉由使用您選擇的工具來建立 Blazor 應用程式，開始使用 Blazor。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/09/2019
no-loc:
- Blazor
uid: blazor/get-started
ms.openlocfilehash: e368ecaf931d392de7e52ec2d5a2dfd171c2c86f
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/09/2019
ms.locfileid: "74943757"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="a040f-103">開始使用 ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="a040f-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="a040f-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a040f-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="a040f-105">開始使用 Blazor：</span><span class="sxs-lookup"><span data-stu-id="a040f-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="a040f-106">安裝[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="a040f-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="a040f-107">選擇性地安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)範本：</span><span class="sxs-lookup"><span data-stu-id="a040f-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="a040f-108">安裝[.Net Core 3.1 或更新版本（預覽） SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="a040f-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="a040f-109">在命令 shell 中執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="a040f-109">Run the following command in a command shell.</span></span> <span data-ttu-id="a040f-110">[AspNetCore.Blazor。範本](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)套件有預覽版本，而 Blazor WebAssembly 處於預覽階段。</span><span class="sxs-lookup"><span data-stu-id="a040f-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="a040f-111">遵循您選擇的工具的指導方針：</span><span class="sxs-lookup"><span data-stu-id="a040f-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a040f-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a040f-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="a040f-113">1 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-113">1\.</span></span> <span data-ttu-id="a040f-114">使用**ASP.NET 和 網頁程式開發**工作負載安裝[Visual Studio 16.4 或更新版本](https://visualstudio.microsoft.com/vs/preview/)。</span><span class="sxs-lookup"><span data-stu-id="a040f-114">Install [Visual Studio 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="a040f-115">2 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-115">2\.</span></span> <span data-ttu-id="a040f-116">建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="a040f-116">Create a new project.</span></span>

   <span data-ttu-id="a040f-117">3 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-117">3\.</span></span> <span data-ttu-id="a040f-118">選取 [ **Blazor 應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="a040f-118">Select **Blazor App**.</span></span> <span data-ttu-id="a040f-119">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="a040f-119">Select **Next**.</span></span>

   <span data-ttu-id="a040f-120">4 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-120">4\.</span></span> <span data-ttu-id="a040f-121">在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="a040f-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="a040f-122">確認 [**位置**] 專案正確，或提供專案的 [位置]。</span><span class="sxs-lookup"><span data-stu-id="a040f-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="a040f-123">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="a040f-123">Select **Create**.</span></span>

   <span data-ttu-id="a040f-124">5 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-124">5\.</span></span> <span data-ttu-id="a040f-125">如需 Blazor WebAssembly 體驗，請選擇 [ **Blazor WebAssembly 應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="a040f-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="a040f-126">如需 Blazor 伺服器體驗，請選擇 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="a040f-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="a040f-127">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="a040f-127">Select **Create**.</span></span> <span data-ttu-id="a040f-128">如需這兩個 Blazor 裝載模型的詳細資訊， *Blazor 伺服器*和 *Blazor WebAssembly*，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="a040f-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="a040f-129">6。</span><span class="sxs-lookup"><span data-stu-id="a040f-129">6\.</span></span> <span data-ttu-id="a040f-130">按下 **Ctrl**+**F5** 即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a040f-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a040f-131">如果您已安裝 ASP.NET Core Blazor （Preview 6 或更早版本）先前預覽版本的 Blazor Visual Studio 延伸模組，則可以卸載擴充功能。</span><span class="sxs-lookup"><span data-stu-id="a040f-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="a040f-132">在命令介面中安裝 Blazor 範本，現在已足以呈現 Visual Studio 中的範本。</span><span class="sxs-lookup"><span data-stu-id="a040f-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a040f-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a040f-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="a040f-134">1 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-134">1\.</span></span> <span data-ttu-id="a040f-135">安裝 [Visual Studio Code (英文)](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="a040f-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="a040f-136">2 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-136">2\.</span></span> <span data-ttu-id="a040f-137">安裝[ C# Visual Studio Code 擴充](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)功能的最新版本。</span><span class="sxs-lookup"><span data-stu-id="a040f-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="a040f-138">3 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-138">3\.</span></span> <span data-ttu-id="a040f-139">如需 Blazor WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="a040f-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="a040f-140">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="a040f-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="a040f-141">如需這兩個 Blazor 裝載模型的詳細資訊， *Blazor 伺服器*和 *Blazor WebAssembly*，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="a040f-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="a040f-142">4 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-142">4\.</span></span> <span data-ttu-id="a040f-143">在 Visual Studio Code 中開啟 [ *WebApplication1* ] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a040f-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="a040f-144">5 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-144">5\.</span></span> <span data-ttu-id="a040f-145">若為 Blazor 伺服器專案，IDE 會要求您新增資產以建立和對專案進行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="a040f-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="a040f-146">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="a040f-146">Select **Yes**.</span></span>

   <span data-ttu-id="a040f-147">6。</span><span class="sxs-lookup"><span data-stu-id="a040f-147">6\.</span></span> <span data-ttu-id="a040f-148">如果使用 Blazor 伺服器應用程式，請使用 Visual Studio Code 偵錯工具來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a040f-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="a040f-149">如果使用 Blazor WebAssembly 應用程式，請從應用程式的專案資料夾執行 `dotnet run`。</span><span class="sxs-lookup"><span data-stu-id="a040f-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="a040f-150">7 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-150">7\.</span></span> <span data-ttu-id="a040f-151">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="a040f-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a040f-152">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a040f-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="a040f-153">1 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-153">1\.</span></span> <span data-ttu-id="a040f-154">安裝[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)。</span><span class="sxs-lookup"><span data-stu-id="a040f-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span> <span data-ttu-id="a040f-155">將[更新通道切換為 [預覽](/visualstudio/mac/install-preview)]。</span><span class="sxs-lookup"><span data-stu-id="a040f-155">Switch the [Update channel to Preview](/visualstudio/mac/install-preview).</span></span>

   <span data-ttu-id="a040f-156">2 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-156">2\.</span></span> <span data-ttu-id="a040f-157">選取 **[** 檔案] > [**新增方案**] 或 [建立**新專案**]。</span><span class="sxs-lookup"><span data-stu-id="a040f-157">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="a040f-158">3 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-158">3\.</span></span> <span data-ttu-id="a040f-159">在側邊欄中，選取 [ **.Net Core** > **應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="a040f-159">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="a040f-160">4 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-160">4\.</span></span> <span data-ttu-id="a040f-161">選取 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="a040f-161">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="a040f-162">目前只有 Blazor 伺服器範本可在 Visual Studio for Mac 中使用。</span><span class="sxs-lookup"><span data-stu-id="a040f-162">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="a040f-163">如需 Blazor WebAssembly 體驗，請遵循 [ **.NET Core CLI** ] 索引標籤上的指示。選取 Blazor 伺服器範本之後，請選取 **[下一步]** 。</span><span class="sxs-lookup"><span data-stu-id="a040f-163">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="a040f-164">如需這兩個 Blazor 裝載模型的詳細資訊， *Blazor 伺服器*和 *Blazor WebAssembly*，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="a040f-164">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="a040f-165">5 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-165">5\.</span></span> <span data-ttu-id="a040f-166">將**目標 Framework**設定為 **.net Core 3.1** ，然後選取 **[下一步]** 。</span><span class="sxs-lookup"><span data-stu-id="a040f-166">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="a040f-167">6。</span><span class="sxs-lookup"><span data-stu-id="a040f-167">6\.</span></span> <span data-ttu-id="a040f-168">在 [**專案名稱**] 欄位中，將應用程式命名為 `WebApplication1`。</span><span class="sxs-lookup"><span data-stu-id="a040f-168">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="a040f-169">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="a040f-169">Select **Create**.</span></span>

   <span data-ttu-id="a040f-170">7 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-170">7\.</span></span> <span data-ttu-id="a040f-171">選取 [**執行**] > **執行而不進行調試**程式，以在不進行偵錯工具的*情況下*執行</span><span class="sxs-lookup"><span data-stu-id="a040f-171">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="a040f-172">使用 [**開始調試**程式] 執行應用程式，以*使用調試*程式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a040f-172">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a040f-173">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a040f-173">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="a040f-174">如需 Blazor WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="a040f-174">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="a040f-175">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="a040f-175">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="a040f-176">如需這兩個 Blazor 裝載模型的詳細資訊， *Blazor 伺服器*和 *Blazor WebAssembly*，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="a040f-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="a040f-177">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="a040f-177">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="a040f-178">安裝最新的[.Net Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)。</span><span class="sxs-lookup"><span data-stu-id="a040f-178">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span></span>

1. <span data-ttu-id="a040f-179">選擇性地安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)範本：</span><span class="sxs-lookup"><span data-stu-id="a040f-179">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="a040f-180">安裝[.Net Core 3.1 或更新版本（預覽） SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="a040f-180">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="a040f-181">在命令 shell 中執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="a040f-181">Run the following command in a command shell.</span></span> <span data-ttu-id="a040f-182">[AspNetCore.Blazor。範本](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)套件有預覽版本，而 Blazor WebAssembly 處於預覽階段。</span><span class="sxs-lookup"><span data-stu-id="a040f-182">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="a040f-183">遵循您選擇的工具的指導方針：</span><span class="sxs-lookup"><span data-stu-id="a040f-183">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a040f-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a040f-184">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="a040f-185">1 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-185">1\.</span></span> <span data-ttu-id="a040f-186">使用**ASP.NET 和 網頁程式開發**工作負載安裝最新的[Visual Studio](https://visualstudio.com/vs/) 。</span><span class="sxs-lookup"><span data-stu-id="a040f-186">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="a040f-187">2 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-187">2\.</span></span> <span data-ttu-id="a040f-188">選擇性地使用適用于 Blazor WebAssembly 應用程式開發的**ASP.NET 和 網頁程式開發**工作負載，安裝[Visual Studio 16.4 Preview 2 或更新版本](https://visualstudio.microsoft.com/vs/preview/)。</span><span class="sxs-lookup"><span data-stu-id="a040f-188">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="a040f-189">3 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-189">3\.</span></span> <span data-ttu-id="a040f-190">建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="a040f-190">Create a new project.</span></span>

   <span data-ttu-id="a040f-191">4 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-191">4\.</span></span> <span data-ttu-id="a040f-192">選取 [ **Blazor 應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="a040f-192">Select **Blazor App**.</span></span> <span data-ttu-id="a040f-193">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="a040f-193">Select **Next**.</span></span>

   <span data-ttu-id="a040f-194">5 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-194">5\.</span></span> <span data-ttu-id="a040f-195">在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="a040f-195">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="a040f-196">確認 [**位置**] 專案正確，或提供專案的 [位置]。</span><span class="sxs-lookup"><span data-stu-id="a040f-196">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="a040f-197">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="a040f-197">Select **Create**.</span></span>

   <span data-ttu-id="a040f-198">6。</span><span class="sxs-lookup"><span data-stu-id="a040f-198">6\.</span></span> <span data-ttu-id="a040f-199">如需 Blazor WebAssembly 體驗，請選擇 [ **Blazor WebAssembly 應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="a040f-199">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="a040f-200">如需 Blazor 伺服器體驗，請選擇 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="a040f-200">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="a040f-201">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="a040f-201">Select **Create**.</span></span> <span data-ttu-id="a040f-202">如需這兩個 Blazor 裝載模型的詳細資訊， *Blazor 伺服器*和 *Blazor WebAssembly*，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="a040f-202">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="a040f-203">7 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-203">7\.</span></span> <span data-ttu-id="a040f-204">按下 **F5** 即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a040f-204">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a040f-205">如果您已安裝 ASP.NET Core Blazor （Preview 6 或更早版本）先前預覽版本的 Blazor Visual Studio 延伸模組，則可以卸載擴充功能。</span><span class="sxs-lookup"><span data-stu-id="a040f-205">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="a040f-206">在命令介面中安裝 Blazor 範本，現在已足以呈現 Visual Studio 中的範本。</span><span class="sxs-lookup"><span data-stu-id="a040f-206">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a040f-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a040f-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="a040f-208">1 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-208">1\.</span></span> <span data-ttu-id="a040f-209">安裝 [Visual Studio Code (英文)](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="a040f-209">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="a040f-210">2 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-210">2\.</span></span> <span data-ttu-id="a040f-211">安裝[ C# Visual Studio Code 擴充](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)功能的最新版本。</span><span class="sxs-lookup"><span data-stu-id="a040f-211">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="a040f-212">3 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-212">3\.</span></span> <span data-ttu-id="a040f-213">如需 Blazor WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="a040f-213">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="a040f-214">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="a040f-214">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="a040f-215">如需這兩個 Blazor 裝載模型的詳細資訊， *Blazor 伺服器*和 *Blazor WebAssembly*，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="a040f-215">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="a040f-216">4 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-216">4\.</span></span> <span data-ttu-id="a040f-217">在 Visual Studio Code 中開啟 [ *WebApplication1* ] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a040f-217">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="a040f-218">5 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-218">5\.</span></span> <span data-ttu-id="a040f-219">若為 Blazor 伺服器專案，IDE 會要求您新增資產以建立和對專案進行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="a040f-219">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="a040f-220">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="a040f-220">Select **Yes**.</span></span>

   <span data-ttu-id="a040f-221">6。</span><span class="sxs-lookup"><span data-stu-id="a040f-221">6\.</span></span> <span data-ttu-id="a040f-222">如果使用 Blazor 伺服器應用程式，請使用 Visual Studio Code 偵錯工具來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a040f-222">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="a040f-223">如果使用 Blazor WebAssembly 應用程式，請從應用程式的專案資料夾執行 `dotnet run`。</span><span class="sxs-lookup"><span data-stu-id="a040f-223">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="a040f-224">7 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-224">7\.</span></span> <span data-ttu-id="a040f-225">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="a040f-225">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a040f-226">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a040f-226">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="a040f-227">1 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-227">1\.</span></span> <span data-ttu-id="a040f-228">安裝[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)。</span><span class="sxs-lookup"><span data-stu-id="a040f-228">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span> <span data-ttu-id="a040f-229">將[更新通道切換為 [預覽](/visualstudio/mac/install-preview)]。</span><span class="sxs-lookup"><span data-stu-id="a040f-229">Switch the [Update channel to Preview](/visualstudio/mac/install-preview).</span></span>

   <span data-ttu-id="a040f-230">2 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-230">2\.</span></span> <span data-ttu-id="a040f-231">選取 **[** 檔案] > [**新增方案**] 或 [建立**新專案**]。</span><span class="sxs-lookup"><span data-stu-id="a040f-231">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="a040f-232">3 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-232">3\.</span></span> <span data-ttu-id="a040f-233">在側邊欄中，選取 [ **.Net Core** > **應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="a040f-233">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="a040f-234">4 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-234">4\.</span></span> <span data-ttu-id="a040f-235">選取 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="a040f-235">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="a040f-236">目前只有 Blazor 伺服器範本可在 Visual Studio for Mac 中使用。</span><span class="sxs-lookup"><span data-stu-id="a040f-236">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="a040f-237">如需 Blazor WebAssembly 體驗，請遵循 [ **.NET Core CLI** ] 索引標籤上的指示。選取 Blazor 伺服器範本之後，請選取 **[下一步]** 。</span><span class="sxs-lookup"><span data-stu-id="a040f-237">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="a040f-238">如需這兩個 Blazor 裝載模型的詳細資訊， *Blazor 伺服器*和 *Blazor WebAssembly*，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="a040f-238">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="a040f-239">5 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-239">5\.</span></span> <span data-ttu-id="a040f-240">將**目標 Framework**設定為 **.net Core 3.0** ，然後選取 **[下一步]** 。</span><span class="sxs-lookup"><span data-stu-id="a040f-240">Set the **Target Framework** to **.NET Core 3.0** and select **Next**.</span></span>

   <span data-ttu-id="a040f-241">6。</span><span class="sxs-lookup"><span data-stu-id="a040f-241">6\.</span></span> <span data-ttu-id="a040f-242">在 [**專案名稱**] 欄位中，將應用程式命名為 `WebApplication1`。</span><span class="sxs-lookup"><span data-stu-id="a040f-242">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="a040f-243">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="a040f-243">Select **Create**.</span></span>

   <span data-ttu-id="a040f-244">7 \。</span><span class="sxs-lookup"><span data-stu-id="a040f-244">7\.</span></span> <span data-ttu-id="a040f-245">選取 [**執行**] > **執行而不進行調試**程式，以在不進行偵錯工具的*情況下*執行</span><span class="sxs-lookup"><span data-stu-id="a040f-245">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="a040f-246">使用 [**開始調試**程式] 執行應用程式，以*使用調試*程式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a040f-246">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a040f-247">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a040f-247">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="a040f-248">如需 Blazor WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="a040f-248">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="a040f-249">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="a040f-249">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="a040f-250">如需這兩個 Blazor 裝載模型的詳細資訊， *Blazor 伺服器*和 *Blazor WebAssembly*，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="a040f-250">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="a040f-251">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="a040f-251">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="a040f-252">提要欄位中的索引標籤可使用多個頁面：</span><span class="sxs-lookup"><span data-stu-id="a040f-252">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="a040f-253">首頁</span><span class="sxs-lookup"><span data-stu-id="a040f-253">Home</span></span>
* <span data-ttu-id="a040f-254">計數器</span><span class="sxs-lookup"><span data-stu-id="a040f-254">Counter</span></span>
* <span data-ttu-id="a040f-255">提取資料</span><span class="sxs-lookup"><span data-stu-id="a040f-255">Fetch data</span></span>

<span data-ttu-id="a040f-256">在 [計數器] 頁面上，選取 [按我] 按鈕以在不重新整理頁面的情況下讓計數器遞增。</span><span class="sxs-lookup"><span data-stu-id="a040f-256">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="a040f-257">將網頁中的計數器遞增通常需要撰寫 JavaScript，但您可以使用C#Blazor。</span><span class="sxs-lookup"><span data-stu-id="a040f-257">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="a040f-258">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="a040f-258">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="a040f-259">在瀏覽器中 `/counter` 的要求，如同頂端的 `@page` 指示詞所指定，會導致 `Counter` 元件轉譯其內容。</span><span class="sxs-lookup"><span data-stu-id="a040f-259">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="a040f-260">元件會轉譯成轉譯樹狀結構的記憶體中標記法，然後用來以彈性且有效率的方式更新 UI。</span><span class="sxs-lookup"><span data-stu-id="a040f-260">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="a040f-261">每次選取 [**按我**] 按鈕時：</span><span class="sxs-lookup"><span data-stu-id="a040f-261">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="a040f-262">`onclick` 事件會引發。</span><span class="sxs-lookup"><span data-stu-id="a040f-262">The `onclick` event is fired.</span></span>
* <span data-ttu-id="a040f-263">呼叫 `IncrementCount` 方法。</span><span class="sxs-lookup"><span data-stu-id="a040f-263">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="a040f-264">`currentCount` 會遞增。</span><span class="sxs-lookup"><span data-stu-id="a040f-264">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="a040f-265">元件會再次轉譯。</span><span class="sxs-lookup"><span data-stu-id="a040f-265">The component is rendered again.</span></span>

<span data-ttu-id="a040f-266">執行時間會比較新的內容與先前的內容，而且只會將已變更的內容套用至檔物件模型（DOM）。</span><span class="sxs-lookup"><span data-stu-id="a040f-266">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="a040f-267">使用 HTML 語法將元件新增至另一個元件。</span><span class="sxs-lookup"><span data-stu-id="a040f-267">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="a040f-268">例如，藉由將 `<Counter />` 元素新增至 `Index` 元件，將 `Counter` 元件新增至應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="a040f-268">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="a040f-269">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="a040f-269">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="a040f-270">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a040f-270">Run the app.</span></span> <span data-ttu-id="a040f-271">首頁有自己的計數器，由 `Counter` 元件提供。</span><span class="sxs-lookup"><span data-stu-id="a040f-271">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="a040f-272">元件參數是使用屬性或[子內容](xref:blazor/components#child-content)所指定，可讓您設定子元件上的屬性。</span><span class="sxs-lookup"><span data-stu-id="a040f-272">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="a040f-273">若要將參數新增至 `Counter` 元件，請更新元件的 `@code` 區塊：</span><span class="sxs-lookup"><span data-stu-id="a040f-273">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="a040f-274">加入具有 `[Parameter]` 屬性之 `IncrementAmount` 的公用屬性。</span><span class="sxs-lookup"><span data-stu-id="a040f-274">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="a040f-275">將 `IncrementCount` 方法變更為在增加 `currentCount`的值時使用 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="a040f-275">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="a040f-276">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="a040f-276">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="a040f-277">使用屬性，在 `Index` 元件的 `<Counter>` 元素中指定 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="a040f-277">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="a040f-278">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="a040f-278">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="a040f-279">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a040f-279">Run the app.</span></span> <span data-ttu-id="a040f-280">`Index` 元件有自己的計數器，每次選取 [按**我**] 按鈕時，就會遞增10。</span><span class="sxs-lookup"><span data-stu-id="a040f-280">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="a040f-281">`/counter` 的 `Counter` 元件（*razor*）會繼續遞增一。</span><span class="sxs-lookup"><span data-stu-id="a040f-281">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a040f-282">後續步驟</span><span class="sxs-lookup"><span data-stu-id="a040f-282">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="a040f-283">其他資源</span><span class="sxs-lookup"><span data-stu-id="a040f-283">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
