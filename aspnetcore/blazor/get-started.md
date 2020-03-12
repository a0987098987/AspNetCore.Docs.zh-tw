---
title: 開始使用 ASP.NET Core Blazor
author: guardrex
description: 藉由使用您選擇的工具來建立 Blazor 應用程式，開始使用 Blazor。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/10/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 89c7529d2b8ec97db731f7c7268e19937c398115
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083248"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="53bf3-103">開始使用 ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="53bf3-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="53bf3-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="53bf3-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="53bf3-105">開始使用 Blazor：</span><span class="sxs-lookup"><span data-stu-id="53bf3-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="53bf3-106">安裝[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="53bf3-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="53bf3-107">選擇性安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)範本：</span><span class="sxs-lookup"><span data-stu-id="53bf3-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="53bf3-108">安裝[.Net Core 3.1.102 或更新版本（預覽） SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="53bf3-108">Install the [.NET Core 3.1.102 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="53bf3-109">在命令 shell 中執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="53bf3-109">Run the following command in a command shell.</span></span> <span data-ttu-id="53bf3-110">Blazor WebAssembly 處於預覽階段時， [WebAssembly](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/)套件會有預覽版本。</span><span class="sxs-lookup"><span data-stu-id="53bf3-110">The [Microsoft.AspNetCore.Components.WebAssembly.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview2.20160.5
   ```

   > [!NOTE]
   > <span data-ttu-id="53bf3-111">**需要**.NET Core SDK 版本3.1.102 或更新版本，才能使用 3.2 Preview 2 Blazor WebAssembly 範本。</span><span class="sxs-lookup"><span data-stu-id="53bf3-111">.NET Core SDK version 3.1.102 or later is **required** to use the 3.2 Preview 2 Blazor WebAssembly template.</span></span> <span data-ttu-id="53bf3-112">藉由在命令 shell 中執行 `dotnet --version`，確認已安裝的 .NET Core SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="53bf3-112">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="53bf3-113">遵循您選擇的工具的指導方針：</span><span class="sxs-lookup"><span data-stu-id="53bf3-113">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studio"></a>[<span data-ttu-id="53bf3-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="53bf3-114">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="53bf3-115">1\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-115">1\.</span></span> <span data-ttu-id="53bf3-116">使用**ASP.NET 和 網頁程式開發**工作負載安裝[Visual Studio 2019 16.4 版或更新](https://visualstudio.microsoft.com/vs/preview/)版本。</span><span class="sxs-lookup"><span data-stu-id="53bf3-116">Install [Visual Studio 2019 version 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="53bf3-117">2\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-117">2\.</span></span> <span data-ttu-id="53bf3-118">建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="53bf3-118">Create a new project.</span></span>

   <span data-ttu-id="53bf3-119">3\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-119">3\.</span></span> <span data-ttu-id="53bf3-120">選取 [ **Blazor 應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="53bf3-120">Select **Blazor App**.</span></span> <span data-ttu-id="53bf3-121">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="53bf3-121">Select **Next**.</span></span>

   <span data-ttu-id="53bf3-122">4\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-122">4\.</span></span> <span data-ttu-id="53bf3-123">在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="53bf3-123">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="53bf3-124">確認 [**位置**] 專案正確，或提供專案的 [位置]。</span><span class="sxs-lookup"><span data-stu-id="53bf3-124">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="53bf3-125">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="53bf3-125">Select **Create**.</span></span>

   <span data-ttu-id="53bf3-126">5\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-126">5\.</span></span> <span data-ttu-id="53bf3-127">如需 Blazor 的 WebAssembly 體驗，請選擇 [ **Blazor WebAssembly 應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="53bf3-127">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="53bf3-128">如需 Blazor 伺服器體驗，請選擇 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="53bf3-128">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="53bf3-129">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="53bf3-129">Select **Create**.</span></span> <span data-ttu-id="53bf3-130">如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="53bf3-130">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span> <span data-ttu-id="53bf3-131">如果 Blazor WebAssembly 範本不存在，請回到上一個步驟，然後重新安裝範本。</span><span class="sxs-lookup"><span data-stu-id="53bf3-131">If the Blazor WebAssembly template isn't present, return to the previous step and reinstall the template.</span></span>

   <span data-ttu-id="53bf3-132">6\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-132">6\.</span></span> <span data-ttu-id="53bf3-133">按下 **Ctrl**+**F5** 即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="53bf3-133">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="53bf3-134">如果您已安裝 ASP.NET Core Blazor （Preview 6 或更早版本）先前預覽版本的 Blazor Visual Studio 延伸模組，則可以卸載擴充功能。</span><span class="sxs-lookup"><span data-stu-id="53bf3-134">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="53bf3-135">在命令介面中安裝 Blazor 範本，現在已足以在 Visual Studio 中呈現範本。</span><span class="sxs-lookup"><span data-stu-id="53bf3-135">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-code"></a>[<span data-ttu-id="53bf3-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="53bf3-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="53bf3-137">1\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-137">1\.</span></span> <span data-ttu-id="53bf3-138">安裝 [Visual Studio Code](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="53bf3-138">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="53bf3-139">2\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-139">2\.</span></span> <span data-ttu-id="53bf3-140">安裝[ C# Visual Studio Code 擴充](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)功能的最新版本。</span><span class="sxs-lookup"><span data-stu-id="53bf3-140">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).</span></span>

   <span data-ttu-id="53bf3-141">3\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-141">3\.</span></span> <span data-ttu-id="53bf3-142">如需 Blazor 的 WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="53bf3-142">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="53bf3-143">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="53bf3-143">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="53bf3-144">如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="53bf3-144">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="53bf3-145">4\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-145">4\.</span></span> <span data-ttu-id="53bf3-146">在 Visual Studio Code 中開啟 [ *WebApplication1* ] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="53bf3-146">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="53bf3-147">5\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-147">5\.</span></span> <span data-ttu-id="53bf3-148">若為 Blazor 伺服器專案，IDE 會要求您新增資產以建立和對專案進行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="53bf3-148">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="53bf3-149">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="53bf3-149">Select **Yes**.</span></span>

   <span data-ttu-id="53bf3-150">6\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-150">6\.</span></span> <span data-ttu-id="53bf3-151">如果使用 Blazor 伺服器應用程式，請使用 Visual Studio Code 偵錯工具來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="53bf3-151">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="53bf3-152">如果使用 Blazor WebAssembly 應用程式，請從應用程式的專案資料夾執行 `dotnet run`。</span><span class="sxs-lookup"><span data-stu-id="53bf3-152">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="53bf3-153">7\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-153">7\.</span></span> <span data-ttu-id="53bf3-154">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="53bf3-154">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mac"></a>[<span data-ttu-id="53bf3-155">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="53bf3-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="53bf3-156">1\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-156">1\.</span></span> <span data-ttu-id="53bf3-157">安裝[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)。</span><span class="sxs-lookup"><span data-stu-id="53bf3-157">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="53bf3-158">2\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-158">2\.</span></span> <span data-ttu-id="53bf3-159">選取 **[** 檔案] > [**新增方案**] 或 [建立**新專案**]。</span><span class="sxs-lookup"><span data-stu-id="53bf3-159">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="53bf3-160">3\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-160">3\.</span></span> <span data-ttu-id="53bf3-161">在側邊欄中，選取 [ **.Net Core** > **應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="53bf3-161">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="53bf3-162">4\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-162">4\.</span></span> <span data-ttu-id="53bf3-163">選取 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="53bf3-163">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="53bf3-164">目前只有 Blazor 伺服器範本可在 Visual Studio for Mac 中使用。</span><span class="sxs-lookup"><span data-stu-id="53bf3-164">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="53bf3-165">如需 Blazor 的 WebAssembly 體驗，請遵循 [ **.NET Core CLI** ] 索引標籤上的指示。選取 Blazor 伺服器範本之後，請選取 **[下一步]** 。</span><span class="sxs-lookup"><span data-stu-id="53bf3-165">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="53bf3-166">如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="53bf3-166">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="53bf3-167">5\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-167">5\.</span></span> <span data-ttu-id="53bf3-168">將**目標 Framework**設定為 **.net Core 3.1** ，然後選取 **[下一步]** 。</span><span class="sxs-lookup"><span data-stu-id="53bf3-168">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="53bf3-169">6\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-169">6\.</span></span> <span data-ttu-id="53bf3-170">在 [**專案名稱**] 欄位中，將應用程式命名為 `WebApplication1`。</span><span class="sxs-lookup"><span data-stu-id="53bf3-170">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="53bf3-171">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="53bf3-171">Select **Create**.</span></span>

   <span data-ttu-id="53bf3-172">7\.</span><span class="sxs-lookup"><span data-stu-id="53bf3-172">7\.</span></span> <span data-ttu-id="53bf3-173">選取 [**執行**] > **執行而不進行調試**程式，以在不進行偵錯工具的*情況下*執行</span><span class="sxs-lookup"><span data-stu-id="53bf3-173">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="53bf3-174">使用 [**開始調試**程式] 執行應用程式，以*使用調試*程式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="53bf3-174">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

   <span data-ttu-id="53bf3-175">如果出現會信任開發憑證的提示，請信任憑證並繼續。</span><span class="sxs-lookup"><span data-stu-id="53bf3-175">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

   # <a name="net-core-cli"></a>[<span data-ttu-id="53bf3-176">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="53bf3-176">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="53bf3-177">如需 Blazor 的 WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="53bf3-177">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="53bf3-178">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="53bf3-178">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="53bf3-179">如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="53bf3-179">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="53bf3-180">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="53bf3-180">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="53bf3-181">提要欄位中的索引標籤可使用多個頁面：</span><span class="sxs-lookup"><span data-stu-id="53bf3-181">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="53bf3-182">首頁</span><span class="sxs-lookup"><span data-stu-id="53bf3-182">Home</span></span>
* <span data-ttu-id="53bf3-183">計數器</span><span class="sxs-lookup"><span data-stu-id="53bf3-183">Counter</span></span>
* <span data-ttu-id="53bf3-184">提取資料</span><span class="sxs-lookup"><span data-stu-id="53bf3-184">Fetch data</span></span>

<span data-ttu-id="53bf3-185">在 [計數器] 頁面上，選取 [按我] 按鈕以在不重新整理頁面的情況下讓計數器遞增。</span><span class="sxs-lookup"><span data-stu-id="53bf3-185">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="53bf3-186">將網頁中的計數器遞增通常需要撰寫 JavaScript，但您可以使用C#Blazor。</span><span class="sxs-lookup"><span data-stu-id="53bf3-186">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="53bf3-187">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="53bf3-187">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="53bf3-188">在瀏覽器中 `/counter` 的要求，如同頂端的 `@page` 指示詞所指定，會導致 `Counter` 元件轉譯其內容。</span><span class="sxs-lookup"><span data-stu-id="53bf3-188">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="53bf3-189">元件會轉譯成轉譯樹狀結構的記憶體中標記法，然後用來以彈性且有效率的方式更新 UI。</span><span class="sxs-lookup"><span data-stu-id="53bf3-189">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="53bf3-190">每次選取 [**按我**] 按鈕時：</span><span class="sxs-lookup"><span data-stu-id="53bf3-190">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="53bf3-191">`onclick` 事件會引發。</span><span class="sxs-lookup"><span data-stu-id="53bf3-191">The `onclick` event is fired.</span></span>
* <span data-ttu-id="53bf3-192">呼叫 `IncrementCount` 方法。</span><span class="sxs-lookup"><span data-stu-id="53bf3-192">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="53bf3-193">`currentCount` 會遞增。</span><span class="sxs-lookup"><span data-stu-id="53bf3-193">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="53bf3-194">元件會再次轉譯。</span><span class="sxs-lookup"><span data-stu-id="53bf3-194">The component is rendered again.</span></span>

<span data-ttu-id="53bf3-195">執行時間會比較新的內容與先前的內容，而且只會將已變更的內容套用至檔物件模型（DOM）。</span><span class="sxs-lookup"><span data-stu-id="53bf3-195">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="53bf3-196">使用 HTML 語法將元件新增至另一個元件。</span><span class="sxs-lookup"><span data-stu-id="53bf3-196">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="53bf3-197">例如，藉由將 `<Counter />` 元素新增至 `Index` 元件，將 `Counter` 元件新增至應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="53bf3-197">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="53bf3-198">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="53bf3-198">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="53bf3-199">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="53bf3-199">Run the app.</span></span> <span data-ttu-id="53bf3-200">首頁有自己的計數器，由 `Counter` 元件提供。</span><span class="sxs-lookup"><span data-stu-id="53bf3-200">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="53bf3-201">元件參數是使用屬性或[子內容](xref:blazor/components#child-content)所指定，可讓您設定子元件上的屬性。</span><span class="sxs-lookup"><span data-stu-id="53bf3-201">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="53bf3-202">若要將參數新增至 `Counter` 元件，請更新元件的 `@code` 區塊：</span><span class="sxs-lookup"><span data-stu-id="53bf3-202">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="53bf3-203">加入具有 `[Parameter]` 屬性之 `IncrementAmount` 的公用屬性。</span><span class="sxs-lookup"><span data-stu-id="53bf3-203">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="53bf3-204">將 `IncrementCount` 方法變更為在增加 `IncrementAmount`的值時使用 `currentCount`。</span><span class="sxs-lookup"><span data-stu-id="53bf3-204">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="53bf3-205">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="53bf3-205">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="53bf3-206">使用屬性，在 `Index` 元件的 `<Counter>` 元素中指定 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="53bf3-206">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="53bf3-207">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="53bf3-207">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="53bf3-208">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="53bf3-208">Run the app.</span></span> <span data-ttu-id="53bf3-209">`Index` 元件有自己的計數器，每次選取 [按**我**] 按鈕時，就會遞增10。</span><span class="sxs-lookup"><span data-stu-id="53bf3-209">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="53bf3-210">`/counter` 的 `Counter` 元件（*razor*）會繼續遞增一。</span><span class="sxs-lookup"><span data-stu-id="53bf3-210">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="53bf3-211">後續步驟</span><span class="sxs-lookup"><span data-stu-id="53bf3-211">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="53bf3-212">其他資源</span><span class="sxs-lookup"><span data-stu-id="53bf3-212">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
