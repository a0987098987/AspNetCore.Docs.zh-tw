---
title: 開始使用 ASP.NET Core Blazor
author: guardrex
description: 使用您選擇的工具來建立 Blazor 應用程式，以開始使用 Blazor。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/25/2019
uid: blazor/get-started
ms.openlocfilehash: ef9113dbfdbbd5920c4358cdac0c77c60f40b7c8
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/12/2019
ms.locfileid: "72288793"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="007fb-103">開始使用 ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="007fb-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="007fb-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="007fb-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="007fb-105">開始使用 Blazor：</span><span class="sxs-lookup"><span data-stu-id="007fb-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="007fb-106">安裝最新的[.Net Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)版本。</span><span class="sxs-lookup"><span data-stu-id="007fb-106">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="007fb-107">在命令 shell 中執行下列命令，以安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)範本：</span><span class="sxs-lookup"><span data-stu-id="007fb-107">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by running the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview9.19465.2
   ```

1. <span data-ttu-id="007fb-108">遵循您選擇的工具的指導方針：</span><span class="sxs-lookup"><span data-stu-id="007fb-108">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="007fb-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="007fb-109">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="007fb-110">1 \。</span><span class="sxs-lookup"><span data-stu-id="007fb-110">1\.</span></span> <span data-ttu-id="007fb-111">使用**ASP.NET 和 網頁程式開發**工作負載安裝最新的[Visual Studio](https://visualstudio.com/vs/) 。</span><span class="sxs-lookup"><span data-stu-id="007fb-111">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="007fb-112">2 \。</span><span class="sxs-lookup"><span data-stu-id="007fb-112">2\.</span></span> <span data-ttu-id="007fb-113">建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="007fb-113">Create a new project.</span></span>

   <span data-ttu-id="007fb-114">3 \。</span><span class="sxs-lookup"><span data-stu-id="007fb-114">3\.</span></span> <span data-ttu-id="007fb-115">選取 [ **Blazor 應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="007fb-115">Select **Blazor App**.</span></span> <span data-ttu-id="007fb-116">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="007fb-116">Select **Next**.</span></span>

   <span data-ttu-id="007fb-117">4 \。</span><span class="sxs-lookup"><span data-stu-id="007fb-117">4\.</span></span> <span data-ttu-id="007fb-118">在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="007fb-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="007fb-119">確認 [**位置**] 專案正確，或提供專案的 [位置]。</span><span class="sxs-lookup"><span data-stu-id="007fb-119">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="007fb-120">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="007fb-120">Select **Create**.</span></span>

   <span data-ttu-id="007fb-121">5 \。</span><span class="sxs-lookup"><span data-stu-id="007fb-121">5\.</span></span> <span data-ttu-id="007fb-122">如需 Blazor 的 WebAssembly 體驗，請選擇 [ **Blazor WebAssembly 應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="007fb-122">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="007fb-123">如需 Blazor 伺服器體驗，請選擇 [ **Blazor 伺服器應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="007fb-123">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="007fb-124">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="007fb-124">Select **Create**.</span></span> <span data-ttu-id="007fb-125">如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="007fb-125">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="007fb-126">6。</span><span class="sxs-lookup"><span data-stu-id="007fb-126">6\.</span></span> <span data-ttu-id="007fb-127">按下 **F5** 即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="007fb-127">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="007fb-128">如果您已安裝 ASP.NET Core Blazor （Preview 6 或更早版本）先前預覽版本的 Blazor Visual Studio 延伸模組，則可以卸載擴充功能。</span><span class="sxs-lookup"><span data-stu-id="007fb-128">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="007fb-129">在命令介面中安裝 Blazor 範本，現在已足以在 Visual Studio 中呈現範本。</span><span class="sxs-lookup"><span data-stu-id="007fb-129">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="007fb-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="007fb-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="007fb-131">1 \。</span><span class="sxs-lookup"><span data-stu-id="007fb-131">1\.</span></span> <span data-ttu-id="007fb-132">安裝 [Visual Studio Code (英文)](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="007fb-132">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="007fb-133">2 \。</span><span class="sxs-lookup"><span data-stu-id="007fb-133">2\.</span></span> <span data-ttu-id="007fb-134">安裝[ C# Visual Studio Code 擴充](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)功能的最新版本。</span><span class="sxs-lookup"><span data-stu-id="007fb-134">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="007fb-135">3 \。</span><span class="sxs-lookup"><span data-stu-id="007fb-135">3\.</span></span> <span data-ttu-id="007fb-136">如需 Blazor 的 WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="007fb-136">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="007fb-137">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="007fb-137">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="007fb-138">如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="007fb-138">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="007fb-139">4 \。</span><span class="sxs-lookup"><span data-stu-id="007fb-139">4\.</span></span> <span data-ttu-id="007fb-140">在 Visual Studio Code 中開啟 [ *WebApplication1* ] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="007fb-140">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="007fb-141">5 \。</span><span class="sxs-lookup"><span data-stu-id="007fb-141">5\.</span></span> <span data-ttu-id="007fb-142">若為 Blazor 伺服器專案，IDE 會要求您新增資產以建立和對專案進行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="007fb-142">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="007fb-143">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="007fb-143">Select **Yes**.</span></span>

   <span data-ttu-id="007fb-144">6。</span><span class="sxs-lookup"><span data-stu-id="007fb-144">6\.</span></span> <span data-ttu-id="007fb-145">如果使用 Blazor 伺服器應用程式，請使用 Visual Studio Code 偵錯工具來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="007fb-145">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="007fb-146">如果使用 Blazor WebAssembly 應用程式，請從應用程式的專案資料夾執行 `dotnet run`。</span><span class="sxs-lookup"><span data-stu-id="007fb-146">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="007fb-147">7 \。</span><span class="sxs-lookup"><span data-stu-id="007fb-147">7\.</span></span> <span data-ttu-id="007fb-148">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="007fb-148">In a browser, navigate to `https://localhost:5001`.</span></span>

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

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="007fb-149">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="007fb-149">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="007fb-150">如需 Blazor 的 WebAssembly 體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="007fb-150">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="007fb-151">如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="007fb-151">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="007fb-152">如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="007fb-152">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="007fb-153">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="007fb-153">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="007fb-154">提要欄位中的索引標籤可使用多個頁面：</span><span class="sxs-lookup"><span data-stu-id="007fb-154">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="007fb-155">首頁</span><span class="sxs-lookup"><span data-stu-id="007fb-155">Home</span></span>
* <span data-ttu-id="007fb-156">計數器</span><span class="sxs-lookup"><span data-stu-id="007fb-156">Counter</span></span>
* <span data-ttu-id="007fb-157">提取資料</span><span class="sxs-lookup"><span data-stu-id="007fb-157">Fetch data</span></span>

<span data-ttu-id="007fb-158">在 [計數器] 頁面上，選取 [按我] 按鈕以在不重新整理頁面的情況下讓計數器遞增。</span><span class="sxs-lookup"><span data-stu-id="007fb-158">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="007fb-159">將網頁中的計數器遞增通常需要撰寫 JavaScript，但透過 Blazor，您可以C#使用。</span><span class="sxs-lookup"><span data-stu-id="007fb-159">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="007fb-160">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="007fb-160">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="007fb-161">在瀏覽器中 `/counter` 的要求，如同頂端的 `@page` 指示詞所指定，會導致 @no__t 2 元件轉譯其內容。</span><span class="sxs-lookup"><span data-stu-id="007fb-161">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="007fb-162">元件會轉譯成轉譯樹狀結構的記憶體中標記法，然後用來以彈性且有效率的方式更新 UI。</span><span class="sxs-lookup"><span data-stu-id="007fb-162">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="007fb-163">每次選取 [**按我**] 按鈕時：</span><span class="sxs-lookup"><span data-stu-id="007fb-163">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="007fb-164">@No__t-0 事件會引發。</span><span class="sxs-lookup"><span data-stu-id="007fb-164">The `onclick` event is fired.</span></span>
* <span data-ttu-id="007fb-165">已呼叫 `IncrementCount` 方法。</span><span class="sxs-lookup"><span data-stu-id="007fb-165">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="007fb-166">@No__t-0 會遞增。</span><span class="sxs-lookup"><span data-stu-id="007fb-166">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="007fb-167">元件會再次轉譯。</span><span class="sxs-lookup"><span data-stu-id="007fb-167">The component is rendered again.</span></span>

<span data-ttu-id="007fb-168">執行時間會比較新的內容與先前的內容，而且只會將已變更的內容套用至檔物件模型（DOM）。</span><span class="sxs-lookup"><span data-stu-id="007fb-168">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="007fb-169">使用 HTML 語法將元件新增至另一個元件。</span><span class="sxs-lookup"><span data-stu-id="007fb-169">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="007fb-170">例如，將 `Counter` 元件新增至應用程式的首頁，方法是將 `<Counter />` 元素新增至 @no__t 2 元件。</span><span class="sxs-lookup"><span data-stu-id="007fb-170">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="007fb-171">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="007fb-171">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="007fb-172">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="007fb-172">Run the app.</span></span> <span data-ttu-id="007fb-173">首頁有自己的計數器，由 @no__t 0 元件提供。</span><span class="sxs-lookup"><span data-stu-id="007fb-173">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="007fb-174">元件參數是使用屬性或[子內容](xref:blazor/components#child-content)所指定，可讓您設定子元件上的屬性。</span><span class="sxs-lookup"><span data-stu-id="007fb-174">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="007fb-175">若要將參數新增至 `Counter` 元件，請更新元件的 `@code` 區塊：</span><span class="sxs-lookup"><span data-stu-id="007fb-175">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="007fb-176">使用 `[Parameter]` 屬性來新增 `IncrementAmount` 的公用屬性。</span><span class="sxs-lookup"><span data-stu-id="007fb-176">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="007fb-177">將 `IncrementCount` 方法變更為在增加 `currentCount`的值時使用 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="007fb-177">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="007fb-178">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="007fb-178">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="007fb-179">使用屬性，在 `Index` 元件的 `<Counter>` 元素中指定 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="007fb-179">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="007fb-180">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="007fb-180">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="007fb-181">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="007fb-181">Run the app.</span></span> <span data-ttu-id="007fb-182">@No__t 0 元件有自己的計數器，每次選取 [按**我**] 按鈕時，就會遞增10。</span><span class="sxs-lookup"><span data-stu-id="007fb-182">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="007fb-183">@No__t-2 的 `Counter` 元件（*razor*）會繼續遞增一。</span><span class="sxs-lookup"><span data-stu-id="007fb-183">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="007fb-184">後續步驟</span><span class="sxs-lookup"><span data-stu-id="007fb-184">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="007fb-185">其他資源</span><span class="sxs-lookup"><span data-stu-id="007fb-185">Additional resources</span></span>

* <xref:signalr/introduction>
