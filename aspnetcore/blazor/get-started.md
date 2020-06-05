---
title: 開始使用 ASP.NET CoreBlazor
author: guardrex
description: 開始使用， Blazor 方法是 Blazor 使用您選擇的工具來建立應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 08229283882928c4cc733de19840d25872846c97
ms.sourcegitcommit: cd73744bd75fdefb31d25ab906df237f07ee7a0a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2020
ms.locfileid: "84452027"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="872da-103">開始使用 ASP.NET CoreBlazor</span><span class="sxs-lookup"><span data-stu-id="872da-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="872da-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="872da-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="872da-105">若要開始使用 Blazor ，請遵循您所選工具的指導方針：</span><span class="sxs-lookup"><span data-stu-id="872da-105">To get started with Blazor, follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="872da-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="872da-106">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="872da-107">使用**ASP.NET 和 網頁程式開發**工作負載安裝最新版的[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) 。</span><span class="sxs-lookup"><span data-stu-id="872da-107">Install the latest version of [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

1. <span data-ttu-id="872da-108">建立新專案。</span><span class="sxs-lookup"><span data-stu-id="872da-108">Create a new project.</span></span>

1. <span data-ttu-id="872da-109">選取 [ \*\* Blazor 應用程式\*\*]。</span><span class="sxs-lookup"><span data-stu-id="872da-109">Select **Blazor App**.</span></span> <span data-ttu-id="872da-110">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="872da-110">Select **Next**.</span></span>

1. <span data-ttu-id="872da-111">在 [專案名稱]\*\*\*\* 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="872da-111">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="872da-112">確認 [**位置**] 專案正確，或提供專案的 [位置]。</span><span class="sxs-lookup"><span data-stu-id="872da-112">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="872da-113">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="872da-113">Select **Create**.</span></span>

1. <span data-ttu-id="872da-114">如需 Blazor WebAssembly 體驗，請選擇 [ \*\* Blazor WebAssembly 應用程式\*\*] 範本。</span><span class="sxs-lookup"><span data-stu-id="872da-114">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="872da-115">如需 Blazor 伺服器體驗，請選擇 [ \*\* Blazor 伺服器應用程式\*\*] 範本。</span><span class="sxs-lookup"><span data-stu-id="872da-115">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="872da-116">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="872da-116">Select **Create**.</span></span>

   <span data-ttu-id="872da-117">如需這兩個 Blazor 裝載模型（ \* Blazor WebAssembly*和* Blazor 伺服器\*）的詳細資訊，請參閱 <xref:blazor/hosting-models> 。</span><span class="sxs-lookup"><span data-stu-id="872da-117">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="872da-118">按<kbd>Ctrl</kbd> + <kbd>F5</kbd>執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="872da-118">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="872da-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="872da-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="872da-120">安裝最新版的[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="872da-120">Install the latest version of the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span> <span data-ttu-id="872da-121">如果您先前已安裝 SDK，您可以在命令 shell 中執行下列命令，以判斷已安裝的版本：</span><span class="sxs-lookup"><span data-stu-id="872da-121">If you previously installed the SDK, you can determine your installed version by executing the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet --version
   ```

1. <span data-ttu-id="872da-122">安裝最新版的[Visual Studio Code](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="872da-122">Install the latest version of [Visual Studio Code](https://code.visualstudio.com/).</span></span>

1. <span data-ttu-id="872da-123">安裝適用于[Visual Studio Code 擴充](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)功能的最新 c # 和[JavaScript 偵錯工具（夜間）](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly)延伸模組，並 `debug.javascript.usePreview` 將設定為 `true` 。</span><span class="sxs-lookup"><span data-stu-id="872da-123">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the [JavaScript Debugger (Nightly)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) extension with `debug.javascript.usePreview` set to `true`.</span></span>

  <span data-ttu-id="872da-124">若要將設定 `debug.javascript.usePreview` 為 `true` 使用 VS Code UI， **File**請開啟 [檔案  >  **喜好**  >  **設定**]，然後搜尋 `debug javascript use preview` 。</span><span class="sxs-lookup"><span data-stu-id="872da-124">To set `debug.javascript.usePreview` to `true` using the VS Code UI, open **File** > **Preferences** > **Settings** and search for `debug javascript use preview`.</span></span> <span data-ttu-id="872da-125">選取 [**使用適用于 node.js 的新預覽版 JavaScript 偵錯工具] 和 [Chrome**] 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="872da-125">Select the check box for **Use the new in-preview JavaScript debugger for Node.js and Chrome**.</span></span>

1. <span data-ttu-id="872da-126">如需 Blazor WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="872da-126">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   <span data-ttu-id="872da-127">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="872da-127">For a Blazor Server experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   <span data-ttu-id="872da-128">如需這兩個 Blazor 裝載模型（ \* Blazor WebAssembly*和* Blazor 伺服器\*）的詳細資訊，請參閱 <xref:blazor/hosting-models> 。</span><span class="sxs-lookup"><span data-stu-id="872da-128">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="872da-129">在 Visual Studio Code 中開啟 [ *WebApplication1* ] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="872da-129">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

1. <span data-ttu-id="872da-130">IDE 會要求您新增資產，以建立和對專案進行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="872da-130">The IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="872da-131">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="872da-131">Select **Yes**.</span></span>

1. <span data-ttu-id="872da-132">按<kbd>Ctrl</kbd> + <kbd>F5</kbd>執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="872da-132">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="872da-133">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="872da-133">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="872da-134">安裝[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)。</span><span class="sxs-lookup"><span data-stu-id="872da-134">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

1. <span data-ttu-id="872da-135">**File**  >  在 [**開始] 視窗**中選取 [檔案] [**新增方案**] 或 [建立**新**專案]。</span><span class="sxs-lookup"><span data-stu-id="872da-135">Select **File** > **New Solution** or create a **New** project from the **Start Window**.</span></span>

1. <span data-ttu-id="872da-136">在提要欄位中，選取 [ **Web 和主控台**  >  **應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="872da-136">In the sidebar, select **Web and Console** > **App**.</span></span>

   <span data-ttu-id="872da-137">如需 Blazor WebAssembly 體驗，請選擇 [ \*\* Blazor WebAssembly 應用程式\*\*] 範本。</span><span class="sxs-lookup"><span data-stu-id="872da-137">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="872da-138">如需 Blazor 伺服器體驗，請選擇 [ \*\* Blazor 伺服器應用程式\*\*] 範本。</span><span class="sxs-lookup"><span data-stu-id="872da-138">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="872da-139">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="872da-139">Select **Next**.</span></span>

   <span data-ttu-id="872da-140">如需這兩個 Blazor 裝載模型（ \* Blazor WebAssembly*和* Blazor 伺服器\*）的詳細資訊，請參閱 <xref:blazor/hosting-models> 。</span><span class="sxs-lookup"><span data-stu-id="872da-140">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="872da-141">確認下列設定：</span><span class="sxs-lookup"><span data-stu-id="872da-141">Confirm the following configurations:</span></span>

   * <span data-ttu-id="872da-142">設定為 **.Net Core 3.1**的**目標 Framework** 。</span><span class="sxs-lookup"><span data-stu-id="872da-142">**Target Framework** set to **.NET Core 3.1**.</span></span>
   * <span data-ttu-id="872da-143">**驗證**設為 [**無驗證**]。</span><span class="sxs-lookup"><span data-stu-id="872da-143">**Authentication** set to **No Authentication**.</span></span>
   
   <span data-ttu-id="872da-144">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="872da-144">Select **Next**.</span></span>

1. <span data-ttu-id="872da-145">在 [**專案名稱**] 欄位中，將應用程式命名為 `WebApplication1` 。</span><span class="sxs-lookup"><span data-stu-id="872da-145">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="872da-146">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="872da-146">Select **Create**.</span></span>

1. <span data-ttu-id="872da-147">選取 [**執行**  >  **而不進行調試**程式]，以*在沒有偵錯工具的情況下*執行 app。</span><span class="sxs-lookup"><span data-stu-id="872da-147">Select **Run** > **Start Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="872da-148">使用 [**執行**  >  **開始調試**程式] 或 [執行] （&#9654;）按鈕執行應用程式，以*使用調試*程式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="872da-148">Run the app with **Run** > **Start Debugging** or the Run (&#9654;) button to run the app *with the debugger*.</span></span>

<span data-ttu-id="872da-149">如果出現會信任開發憑證的提示，請信任憑證並繼續。</span><span class="sxs-lookup"><span data-stu-id="872da-149">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span> <span data-ttu-id="872da-150">需要使用者和 keychain 密碼，才能信任憑證。</span><span class="sxs-lookup"><span data-stu-id="872da-150">The user and keychain passwords are required to trust the certificate.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="872da-151">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="872da-151">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="872da-152">安裝最新版的[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。</span><span class="sxs-lookup"><span data-stu-id="872da-152">Install the latest version of the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span> <span data-ttu-id="872da-153">如果您先前已安裝 SDK，您可以在命令 shell 中執行下列命令，以判斷已安裝的版本：</span><span class="sxs-lookup"><span data-stu-id="872da-153">If you previously installed the SDK, you can determine your installed version by executing the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet --version
   ```

1. <span data-ttu-id="872da-154">如需 Blazor WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="872da-154">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="872da-155">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="872da-155">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="872da-156">如需這兩個 Blazor 裝載模型（ \* Blazor WebAssembly*和* Blazor 伺服器\*）的詳細資訊，請參閱 <xref:blazor/hosting-models> 。</span><span class="sxs-lookup"><span data-stu-id="872da-156">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="872da-157">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="872da-157">In a browser, navigate to `https://localhost:5001`.</span></span>

---

<span data-ttu-id="872da-158">提要欄位中的索引標籤可使用多個頁面：</span><span class="sxs-lookup"><span data-stu-id="872da-158">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="872da-159">家庭</span><span class="sxs-lookup"><span data-stu-id="872da-159">Home</span></span>
* <span data-ttu-id="872da-160">計數器</span><span class="sxs-lookup"><span data-stu-id="872da-160">Counter</span></span>
* <span data-ttu-id="872da-161">提取資料</span><span class="sxs-lookup"><span data-stu-id="872da-161">Fetch data</span></span>

<span data-ttu-id="872da-162">在 [計數器] 頁面上，選取 [按我]\*\*\*\* 按鈕以在不重新整理頁面的情況下讓計數器遞增。</span><span class="sxs-lookup"><span data-stu-id="872da-162">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="872da-163">將網頁中的計數器遞增通常需要撰寫 JavaScript，但 Blazor 您可以使用 c #。</span><span class="sxs-lookup"><span data-stu-id="872da-163">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="872da-164">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="872da-164">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="872da-165">`/counter`在瀏覽器中的要求（如頂端的指示詞所指定） `@page` 會導致 `Counter` 元件轉譯其內容。</span><span class="sxs-lookup"><span data-stu-id="872da-165">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="872da-166">元件會轉譯成轉譯樹狀結構的記憶體中標記法，然後用來以彈性且有效率的方式更新 UI。</span><span class="sxs-lookup"><span data-stu-id="872da-166">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="872da-167">每次選取 [**按我**] 按鈕時：</span><span class="sxs-lookup"><span data-stu-id="872da-167">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="872da-168">`onclick`引發事件。</span><span class="sxs-lookup"><span data-stu-id="872da-168">The `onclick` event is fired.</span></span>
* <span data-ttu-id="872da-169">已呼叫 `IncrementCount` 方法。</span><span class="sxs-lookup"><span data-stu-id="872da-169">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="872da-170">`currentCount`會遞增。</span><span class="sxs-lookup"><span data-stu-id="872da-170">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="872da-171">元件會再次轉譯。</span><span class="sxs-lookup"><span data-stu-id="872da-171">The component is rendered again.</span></span>

<span data-ttu-id="872da-172">執行時間會比較新的內容與先前的內容，而且只會將已變更的內容套用至檔物件模型（DOM）。</span><span class="sxs-lookup"><span data-stu-id="872da-172">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="872da-173">使用 HTML 語法將元件新增至另一個元件。</span><span class="sxs-lookup"><span data-stu-id="872da-173">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="872da-174">例如，藉 `Counter` 由將元素新增至元件，將元件新增至應用程式的首頁 `<Counter />` `Index` 。</span><span class="sxs-lookup"><span data-stu-id="872da-174">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="872da-175">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="872da-175">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="872da-176">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="872da-176">Run the app.</span></span> <span data-ttu-id="872da-177">首頁有自己的計數器，由元件提供 `Counter` 。</span><span class="sxs-lookup"><span data-stu-id="872da-177">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="872da-178">元件參數是使用屬性或[子內容](xref:blazor/components#child-content)所指定，可讓您設定子元件上的屬性。</span><span class="sxs-lookup"><span data-stu-id="872da-178">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="872da-179">若要將參數新增至 `Counter` 元件，請更新元件的 `@code` 區塊：</span><span class="sxs-lookup"><span data-stu-id="872da-179">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="872da-180">使用屬性加入的公用屬性 `IncrementAmount` [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) 。</span><span class="sxs-lookup"><span data-stu-id="872da-180">Add a public property for `IncrementAmount` with a [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) attribute.</span></span>
* <span data-ttu-id="872da-181">將 `IncrementCount` 方法變更為在增加 `currentCount`的值時使用 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="872da-181">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="872da-182">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="872da-182">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="872da-183">`IncrementAmount`使用屬性，在 `Index` 元件的 `<Counter>` 元素中指定。</span><span class="sxs-lookup"><span data-stu-id="872da-183">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="872da-184">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="872da-184">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="872da-185">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="872da-185">Run the app.</span></span> <span data-ttu-id="872da-186">`Index`元件有自己的計數器，每次選取 [按**我**] 按鈕時，就會遞增10。</span><span class="sxs-lookup"><span data-stu-id="872da-186">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="872da-187">`Counter`中的元件（*razor*）會 `/counter` 繼續遞增一。</span><span class="sxs-lookup"><span data-stu-id="872da-187">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="872da-188">後續步驟</span><span class="sxs-lookup"><span data-stu-id="872da-188">Next steps</span></span>

<span data-ttu-id="872da-189">逐步建立 Blazor 應用程式：</span><span class="sxs-lookup"><span data-stu-id="872da-189">Build a Blazor app step-by-step:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="872da-190">其他資源</span><span class="sxs-lookup"><span data-stu-id="872da-190">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
