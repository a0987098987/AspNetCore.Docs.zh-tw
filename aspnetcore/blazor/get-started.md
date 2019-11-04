---
title: 開始使用 ASP.NET Core Blazor
author: guardrex
description: 使用您選擇的工具來建立 Blazor 應用程式，以開始使用 Blazor。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/21/2019
uid: blazor/get-started
ms.openlocfilehash: 80ff7b42a44e722dd27bc4fde53a066863448e10
ms.sourcegitcommit: 810d5831169770ee240d03207d6671dabea2486e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2019
ms.locfileid: "72779118"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="01aaa-103">開始使用 ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="01aaa-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="01aaa-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="01aaa-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="01aaa-105">開始使用 Blazor：</span><span class="sxs-lookup"><span data-stu-id="01aaa-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="01aaa-106">安裝[.Net Core 3.1 PREVIEW SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="01aaa-106">Install the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="01aaa-107">在命令 shell 中執行下列命令，以安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)範本。</span><span class="sxs-lookup"><span data-stu-id="01aaa-107">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by running the following command in a command shell.</span></span> <span data-ttu-id="01aaa-108">Blazor WebAssembly 處於預覽階段時， [AspNetCore. Blazor](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)套件具有預覽版本。</span><span class="sxs-lookup"><span data-stu-id="01aaa-108">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview1.19508.20
   ```

1. <span data-ttu-id="01aaa-109">遵循您選擇的工具的指導方針：</span><span class="sxs-lookup"><span data-stu-id="01aaa-109">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="01aaa-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01aaa-110">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="01aaa-111">1 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-111">1\.</span></span> <span data-ttu-id="01aaa-112">使用**ASP.NET 和 網頁程式開發**工作負載安裝[Visual Studio 16.4 Preview 2 或更新版本](https://visualstudio.microsoft.com/vs/preview/)。</span><span class="sxs-lookup"><span data-stu-id="01aaa-112">Install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="01aaa-113">2 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-113">2\.</span></span> <span data-ttu-id="01aaa-114">建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="01aaa-114">Create a new project.</span></span>

   <span data-ttu-id="01aaa-115">3 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-115">3\.</span></span> <span data-ttu-id="01aaa-116">選取 [ **Blazor 應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="01aaa-116">Select **Blazor App**.</span></span> <span data-ttu-id="01aaa-117">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="01aaa-117">Select **Next**.</span></span>

   <span data-ttu-id="01aaa-118">4 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-118">4\.</span></span> <span data-ttu-id="01aaa-119">在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="01aaa-119">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="01aaa-120">確認 [**位置**] 專案正確，或提供專案的 [位置]。</span><span class="sxs-lookup"><span data-stu-id="01aaa-120">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="01aaa-121">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="01aaa-121">Select **Create**.</span></span>

   <span data-ttu-id="01aaa-122">5 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-122">5\.</span></span> <span data-ttu-id="01aaa-123">如需 Blazor 的 WebAssembly 體驗，請選擇 [ **Blazor WebAssembly 應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="01aaa-123">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="01aaa-124">如需 Blazor 伺服器體驗，請選擇 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="01aaa-124">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="01aaa-125">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="01aaa-125">Select **Create**.</span></span> <span data-ttu-id="01aaa-126">如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="01aaa-126">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="01aaa-127">6。</span><span class="sxs-lookup"><span data-stu-id="01aaa-127">6\.</span></span> <span data-ttu-id="01aaa-128">按下 **Ctrl**+**F5** 即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="01aaa-128">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="01aaa-129">如果您已安裝 ASP.NET Core Blazor （Preview 6 或更早版本）先前預覽版本的 Blazor Visual Studio 延伸模組，則可以卸載擴充功能。</span><span class="sxs-lookup"><span data-stu-id="01aaa-129">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="01aaa-130">在命令介面中安裝 Blazor 範本，現在已足以在 Visual Studio 中呈現範本。</span><span class="sxs-lookup"><span data-stu-id="01aaa-130">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="01aaa-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="01aaa-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="01aaa-132">1 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-132">1\.</span></span> <span data-ttu-id="01aaa-133">安裝 [Visual Studio Code (英文)](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="01aaa-133">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="01aaa-134">2 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-134">2\.</span></span> <span data-ttu-id="01aaa-135">安裝[ C# Visual Studio Code 擴充](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)功能的最新版本。</span><span class="sxs-lookup"><span data-stu-id="01aaa-135">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="01aaa-136">3 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-136">3\.</span></span> <span data-ttu-id="01aaa-137">如需 Blazor 的 WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="01aaa-137">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="01aaa-138">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="01aaa-138">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="01aaa-139">如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="01aaa-139">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="01aaa-140">4 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-140">4\.</span></span> <span data-ttu-id="01aaa-141">在 Visual Studio Code 中開啟 [ *WebApplication1* ] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="01aaa-141">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="01aaa-142">5 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-142">5\.</span></span> <span data-ttu-id="01aaa-143">若為 Blazor 伺服器專案，IDE 會要求您新增資產以建立和對專案進行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="01aaa-143">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="01aaa-144">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="01aaa-144">Select **Yes**.</span></span>

   <span data-ttu-id="01aaa-145">6。</span><span class="sxs-lookup"><span data-stu-id="01aaa-145">6\.</span></span> <span data-ttu-id="01aaa-146">如果使用 Blazor 伺服器應用程式，請使用 Visual Studio Code 偵錯工具來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="01aaa-146">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="01aaa-147">如果使用 Blazor WebAssembly 應用程式，請從應用程式的專案資料夾執行 `dotnet run`。</span><span class="sxs-lookup"><span data-stu-id="01aaa-147">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="01aaa-148">7 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-148">7\.</span></span> <span data-ttu-id="01aaa-149">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="01aaa-149">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="01aaa-150">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="01aaa-150">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="01aaa-151">如需 Blazor 的 WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="01aaa-151">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="01aaa-152">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="01aaa-152">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="01aaa-153">如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="01aaa-153">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="01aaa-154">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="01aaa-154">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="01aaa-155">安裝最新的[.Net Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)版本。</span><span class="sxs-lookup"><span data-stu-id="01aaa-155">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="01aaa-156">安裝[.Net Core 3.1 PREVIEW SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) ，然後在命令 shell 中執行下列命令，選擇性地安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)範本：</span><span class="sxs-lookup"><span data-stu-id="01aaa-156">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by installing the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) and then running the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview1.19508.20
   ```

1. <span data-ttu-id="01aaa-157">遵循您選擇的工具的指導方針：</span><span class="sxs-lookup"><span data-stu-id="01aaa-157">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="01aaa-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01aaa-158">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="01aaa-159">1 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-159">1\.</span></span> <span data-ttu-id="01aaa-160">使用**ASP.NET 和 網頁程式開發**工作負載安裝最新的[Visual Studio](https://visualstudio.com/vs/) 。</span><span class="sxs-lookup"><span data-stu-id="01aaa-160">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="01aaa-161">2 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-161">2\.</span></span> <span data-ttu-id="01aaa-162">選擇性地使用適用于 Blazor WebAssembly 應用程式開發的**ASP.NET 和 網頁程式開發**工作負載，安裝[Visual Studio 16.4 Preview 2 或更新版本](https://visualstudio.microsoft.com/vs/preview/)。</span><span class="sxs-lookup"><span data-stu-id="01aaa-162">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="01aaa-163">3 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-163">3\.</span></span> <span data-ttu-id="01aaa-164">建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="01aaa-164">Create a new project.</span></span>

   <span data-ttu-id="01aaa-165">4 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-165">4\.</span></span> <span data-ttu-id="01aaa-166">選取 [ **Blazor 應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="01aaa-166">Select **Blazor App**.</span></span> <span data-ttu-id="01aaa-167">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="01aaa-167">Select **Next**.</span></span>

   <span data-ttu-id="01aaa-168">5 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-168">5\.</span></span> <span data-ttu-id="01aaa-169">在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="01aaa-169">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="01aaa-170">確認 [**位置**] 專案正確，或提供專案的 [位置]。</span><span class="sxs-lookup"><span data-stu-id="01aaa-170">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="01aaa-171">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="01aaa-171">Select **Create**.</span></span>

   <span data-ttu-id="01aaa-172">6。</span><span class="sxs-lookup"><span data-stu-id="01aaa-172">6\.</span></span> <span data-ttu-id="01aaa-173">如需 Blazor 的 WebAssembly 體驗，請選擇 [ **Blazor WebAssembly 應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="01aaa-173">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="01aaa-174">如需 Blazor 伺服器體驗，請選擇 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="01aaa-174">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="01aaa-175">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="01aaa-175">Select **Create**.</span></span> <span data-ttu-id="01aaa-176">如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="01aaa-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="01aaa-177">7 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-177">7\.</span></span> <span data-ttu-id="01aaa-178">按下 **F5** 即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="01aaa-178">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="01aaa-179">如果您已安裝 ASP.NET Core Blazor （Preview 6 或更早版本）先前預覽版本的 Blazor Visual Studio 延伸模組，則可以卸載擴充功能。</span><span class="sxs-lookup"><span data-stu-id="01aaa-179">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="01aaa-180">在命令介面中安裝 Blazor 範本，現在已足以在 Visual Studio 中呈現範本。</span><span class="sxs-lookup"><span data-stu-id="01aaa-180">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="01aaa-181">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="01aaa-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="01aaa-182">1 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-182">1\.</span></span> <span data-ttu-id="01aaa-183">安裝 [Visual Studio Code (英文)](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="01aaa-183">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="01aaa-184">2 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-184">2\.</span></span> <span data-ttu-id="01aaa-185">安裝[ C# Visual Studio Code 擴充](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)功能的最新版本。</span><span class="sxs-lookup"><span data-stu-id="01aaa-185">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="01aaa-186">3 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-186">3\.</span></span> <span data-ttu-id="01aaa-187">如需 Blazor 的 WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="01aaa-187">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="01aaa-188">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="01aaa-188">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="01aaa-189">如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="01aaa-189">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="01aaa-190">4 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-190">4\.</span></span> <span data-ttu-id="01aaa-191">在 Visual Studio Code 中開啟 [ *WebApplication1* ] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="01aaa-191">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="01aaa-192">5 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-192">5\.</span></span> <span data-ttu-id="01aaa-193">若為 Blazor 伺服器專案，IDE 會要求您新增資產以建立和對專案進行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="01aaa-193">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="01aaa-194">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="01aaa-194">Select **Yes**.</span></span>

   <span data-ttu-id="01aaa-195">6。</span><span class="sxs-lookup"><span data-stu-id="01aaa-195">6\.</span></span> <span data-ttu-id="01aaa-196">如果使用 Blazor 伺服器應用程式，請使用 Visual Studio Code 偵錯工具來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="01aaa-196">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="01aaa-197">如果使用 Blazor WebAssembly 應用程式，請從應用程式的專案資料夾執行 `dotnet run`。</span><span class="sxs-lookup"><span data-stu-id="01aaa-197">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="01aaa-198">7 \。</span><span class="sxs-lookup"><span data-stu-id="01aaa-198">7\.</span></span> <span data-ttu-id="01aaa-199">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="01aaa-199">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="01aaa-200">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="01aaa-200">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="01aaa-201">如需 Blazor 的 WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="01aaa-201">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="01aaa-202">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="01aaa-202">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="01aaa-203">如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="01aaa-203">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="01aaa-204">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="01aaa-204">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="01aaa-205">提要欄位中的索引標籤可使用多個頁面：</span><span class="sxs-lookup"><span data-stu-id="01aaa-205">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="01aaa-206">首頁</span><span class="sxs-lookup"><span data-stu-id="01aaa-206">Home</span></span>
* <span data-ttu-id="01aaa-207">計數器</span><span class="sxs-lookup"><span data-stu-id="01aaa-207">Counter</span></span>
* <span data-ttu-id="01aaa-208">提取資料</span><span class="sxs-lookup"><span data-stu-id="01aaa-208">Fetch data</span></span>

<span data-ttu-id="01aaa-209">在 [計數器] 頁面上，選取 [按我] 按鈕以在不重新整理頁面的情況下讓計數器遞增。</span><span class="sxs-lookup"><span data-stu-id="01aaa-209">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="01aaa-210">將網頁中的計數器遞增通常需要撰寫 JavaScript，但透過 Blazor，您可以C#使用。</span><span class="sxs-lookup"><span data-stu-id="01aaa-210">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="01aaa-211">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="01aaa-211">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="01aaa-212">在瀏覽器中 `/counter` 的要求，如同頂端的 `@page` 指示詞所指定，會導致 `Counter` 元件轉譯其內容。</span><span class="sxs-lookup"><span data-stu-id="01aaa-212">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="01aaa-213">元件會轉譯成轉譯樹狀結構的記憶體中標記法，然後用來以彈性且有效率的方式更新 UI。</span><span class="sxs-lookup"><span data-stu-id="01aaa-213">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="01aaa-214">每次選取 [**按我**] 按鈕時：</span><span class="sxs-lookup"><span data-stu-id="01aaa-214">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="01aaa-215">`onclick` 事件會引發。</span><span class="sxs-lookup"><span data-stu-id="01aaa-215">The `onclick` event is fired.</span></span>
* <span data-ttu-id="01aaa-216">已呼叫 `IncrementCount` 方法。</span><span class="sxs-lookup"><span data-stu-id="01aaa-216">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="01aaa-217">`currentCount` 會遞增。</span><span class="sxs-lookup"><span data-stu-id="01aaa-217">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="01aaa-218">元件會再次轉譯。</span><span class="sxs-lookup"><span data-stu-id="01aaa-218">The component is rendered again.</span></span>

<span data-ttu-id="01aaa-219">執行時間會比較新的內容與先前的內容，而且只會將已變更的內容套用至檔物件模型（DOM）。</span><span class="sxs-lookup"><span data-stu-id="01aaa-219">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="01aaa-220">使用 HTML 語法將元件新增至另一個元件。</span><span class="sxs-lookup"><span data-stu-id="01aaa-220">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="01aaa-221">例如，藉由將 `<Counter />` 元素新增至 `Index` 元件，將 `Counter` 元件新增至應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="01aaa-221">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="01aaa-222">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="01aaa-222">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="01aaa-223">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="01aaa-223">Run the app.</span></span> <span data-ttu-id="01aaa-224">首頁有自己的計數器，由 `Counter` 元件提供。</span><span class="sxs-lookup"><span data-stu-id="01aaa-224">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="01aaa-225">元件參數是使用屬性或[子內容](xref:blazor/components#child-content)所指定，可讓您設定子元件上的屬性。</span><span class="sxs-lookup"><span data-stu-id="01aaa-225">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="01aaa-226">若要將參數新增至 `Counter` 元件，請更新元件的 `@code` 區塊：</span><span class="sxs-lookup"><span data-stu-id="01aaa-226">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="01aaa-227">加入具有 `[Parameter]` 屬性之 `IncrementAmount` 的公用屬性。</span><span class="sxs-lookup"><span data-stu-id="01aaa-227">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="01aaa-228">將 `IncrementCount` 方法變更為在增加 `currentCount`的值時使用 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="01aaa-228">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="01aaa-229">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="01aaa-229">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="01aaa-230">使用屬性，在 `Index` 元件的 `<Counter>` 元素中指定 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="01aaa-230">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="01aaa-231">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="01aaa-231">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="01aaa-232">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="01aaa-232">Run the app.</span></span> <span data-ttu-id="01aaa-233">@No__t_0 元件有自己的計數器，每次選取 [按**我**] 按鈕時，就會遞增10。</span><span class="sxs-lookup"><span data-stu-id="01aaa-233">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="01aaa-234">@No__t_2 的 `Counter` 元件（*razor*）會繼續遞增一。</span><span class="sxs-lookup"><span data-stu-id="01aaa-234">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="01aaa-235">後續步驟</span><span class="sxs-lookup"><span data-stu-id="01aaa-235">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="01aaa-236">其他資源</span><span class="sxs-lookup"><span data-stu-id="01aaa-236">Additional resources</span></span>

* <xref:signalr/introduction>
