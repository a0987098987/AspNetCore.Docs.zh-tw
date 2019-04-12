---
title: 開始使用 Razor 元件
author: guardrex
description: 了解如何開始使用 Razor 元件建立和修改 Razor 元件專案。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/get-started
ms.openlocfilehash: 151e58497b0f22fa7c5a9bde1f665eeb73fd5dc3
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515415"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="22f02-103">開始使用 Razor 元件</span><span class="sxs-lookup"><span data-stu-id="22f02-103">Get started with Razor Components</span></span>

<span data-ttu-id="22f02-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="22f02-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

# [<a name="visual-studio"></a><span data-ttu-id="22f02-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22f02-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="22f02-106">必要條件：</span><span class="sxs-lookup"><span data-stu-id="22f02-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="22f02-107">若要在 Visual Studio 中建立第一個 Razor 元件專案：</span><span class="sxs-lookup"><span data-stu-id="22f02-107">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="22f02-108">安裝最新[.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)版本。</span><span class="sxs-lookup"><span data-stu-id="22f02-108">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>
1. <span data-ttu-id="22f02-109">啟用 Visual Studio 來使用預覽版 Sdk:</span><span class="sxs-lookup"><span data-stu-id="22f02-109">Enable Visual Studio to use preview SDKs:</span></span>
   1. <span data-ttu-id="22f02-110">開啟**工具** > **選項**功能表列中。</span><span class="sxs-lookup"><span data-stu-id="22f02-110">Open **Tools** > **Options** in the menu bar.</span></span>
   1. <span data-ttu-id="22f02-111">開啟**專案和方案**節點。</span><span class="sxs-lookup"><span data-stu-id="22f02-111">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="22f02-112">開啟 **.NET Core**  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="22f02-112">Open the **.NET Core** tab.</span></span>
   1. <span data-ttu-id="22f02-113">核取方塊**使用的.NET Core SDK 預覽**。</span><span class="sxs-lookup"><span data-stu-id="22f02-113">Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="22f02-114">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="22f02-114">Select **OK**.</span></span>
1. <span data-ttu-id="22f02-115">建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="22f02-115">Create a new project.</span></span>
1. <span data-ttu-id="22f02-116">選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="22f02-116">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="22f02-117">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="22f02-117">Select **Next**.</span></span>
1. <span data-ttu-id="22f02-118">提供的名稱**專案名稱**欄位。</span><span class="sxs-lookup"><span data-stu-id="22f02-118">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="22f02-119">確認**位置**項目是否正確，或提供專案的位置。</span><span class="sxs-lookup"><span data-stu-id="22f02-119">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="22f02-120">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="22f02-120">Select **Create**.</span></span>
1. <span data-ttu-id="22f02-121">請確定 **.NET Core**並**ASP.NET Core 3.0**選取頂端。</span><span class="sxs-lookup"><span data-stu-id="22f02-121">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="22f02-122">選擇**Razor 元件**範本，然後選取**建立**。</span><span class="sxs-lookup"><span data-stu-id="22f02-122">Choose the **Razor Components** template and select **Create**.</span></span>
1. <span data-ttu-id="22f02-123">按下 **F5** 即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="22f02-123">Press **F5** to run the app.</span></span>

<span data-ttu-id="22f02-124">恭喜您！</span><span class="sxs-lookup"><span data-stu-id="22f02-124">Congratulations!</span></span> <span data-ttu-id="22f02-125">您剛剛執行第一個 Razor 元件應用程式 ！</span><span class="sxs-lookup"><span data-stu-id="22f02-125">You just ran your first Razor Components app!</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Razor Components project in Visual Studio Code:

1. Execute the following command from a command shell:

   ```console
   dotnet new razorcomponents -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Add a *.vscode* folder.

1. Add a *tasks.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/tasks.json)]

1. Add a *launch.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/launch.json)]

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Razor Components app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Razor Components app!
-->

# [<a name="net-core-cli"></a><span data-ttu-id="22f02-126">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="22f02-126">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="22f02-127">必要條件：</span><span class="sxs-lookup"><span data-stu-id="22f02-127">Prerequisites:</span></span>

* [<span data-ttu-id="22f02-128">.NET core SDK 3.0 預覽</span><span class="sxs-lookup"><span data-stu-id="22f02-128">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="22f02-129">若要從命令殼層中建立第一個 Razor 元件專案：</span><span class="sxs-lookup"><span data-stu-id="22f02-129">To create your first Razor Components project from a command shell:</span></span>

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="22f02-130">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="22f02-130">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="22f02-131">恭喜您！</span><span class="sxs-lookup"><span data-stu-id="22f02-131">Congratulations!</span></span> <span data-ttu-id="22f02-132">您剛剛執行第一個 Razor 元件應用程式 ！</span><span class="sxs-lookup"><span data-stu-id="22f02-132">You just ran your first Razor Components app!</span></span>

---

## <a name="razor-components-project"></a><span data-ttu-id="22f02-133">Razor 元件專案</span><span class="sxs-lookup"><span data-stu-id="22f02-133">Razor Components project</span></span>

<span data-ttu-id="22f02-134">Razor 元件使用 Razor 語法撰寫，但編譯的方式不同於 Razor Pages 和 MVC 的檢視。</span><span class="sxs-lookup"><span data-stu-id="22f02-134">Razor Components are authored using Razor syntax but are compiled differently than Razor Pages and MVC views.</span></span> <span data-ttu-id="22f02-135">*.Razor*檔案延伸模組用來指定 Razor 元件。</span><span class="sxs-lookup"><span data-stu-id="22f02-135">The *.razor* file extension is used to specify a Razor Component.</span></span> <span data-ttu-id="22f02-136">Razor Pages 和 MVC 檢視就會使用持續 *.cshtml*副檔名。</span><span class="sxs-lookup"><span data-stu-id="22f02-136">Razor Pages and MVC views continue to use the *.cshtml* file extension.</span></span>

> [!NOTE]
> <span data-ttu-id="22f02-137">Razor 元件可以使用撰寫 *.cshtml* ，只要這些檔案可視為使用 Razor 元件檔案副檔名`_RazorComponentInclude`MSBuild 屬性。</span><span class="sxs-lookup"><span data-stu-id="22f02-137">Razor Components can be authored using the *.cshtml* file extension as long as those files are identified as Razor Component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="22f02-138">比方說，建立使用 Razor 元件範本的應用程式指定所有 *.cshtml*下方的檔案*元件*資料夾應該視為 Razor 元件：</span><span class="sxs-lookup"><span data-stu-id="22f02-138">For example, an app created using the Razor Component template specifies that all *.cshtml* files under the *Components* folder should be treated as Razor Components:</span></span>
>
> ```xml
> <_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
> ```

<span data-ttu-id="22f02-139">執行應用程式時，多個頁面都是從資訊看板中的索引標籤：</span><span class="sxs-lookup"><span data-stu-id="22f02-139">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="22f02-140">首頁</span><span class="sxs-lookup"><span data-stu-id="22f02-140">Home</span></span>
* <span data-ttu-id="22f02-141">計數器</span><span class="sxs-lookup"><span data-stu-id="22f02-141">Counter</span></span>
* <span data-ttu-id="22f02-142">擷取資料</span><span class="sxs-lookup"><span data-stu-id="22f02-142">Fetch data</span></span>

<span data-ttu-id="22f02-143">在 [計數器] 頁面上，選取 [按我] 按鈕以在不重新整理頁面的情況下讓計數器遞增。</span><span class="sxs-lookup"><span data-stu-id="22f02-143">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="22f02-144">讓網頁中的計數器遞增通常需要撰寫 JavaScript，但 Razor 元件提供一個使用 C# 的更好方法。</span><span class="sxs-lookup"><span data-stu-id="22f02-144">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="22f02-145">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="22f02-145">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor)]

<span data-ttu-id="22f02-146">申請`/counter`在瀏覽器中所指定`@page`指示詞，在頂端，導致計數器元件來呈現其內容。</span><span class="sxs-lookup"><span data-stu-id="22f02-146">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="22f02-147">元件會轉譯成接著可用來更新 UI 富彈性又有效率的方式呈現樹狀結構的記憶體中表示法。</span><span class="sxs-lookup"><span data-stu-id="22f02-147">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="22f02-148">每次**Click me**按鈕已選取：</span><span class="sxs-lookup"><span data-stu-id="22f02-148">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="22f02-149">`onclick`引發事件。</span><span class="sxs-lookup"><span data-stu-id="22f02-149">The `onclick` event is fired.</span></span>
* <span data-ttu-id="22f02-150">已呼叫 `IncrementCount` 方法。</span><span class="sxs-lookup"><span data-stu-id="22f02-150">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="22f02-151">`currentCount`會遞增。</span><span class="sxs-lookup"><span data-stu-id="22f02-151">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="22f02-152">元件會轉譯一次。</span><span class="sxs-lookup"><span data-stu-id="22f02-152">The component is rendered again.</span></span>

<span data-ttu-id="22f02-153">執行階段會比較新的內容來將舊內容，並只會套用已變更的內容與文件物件模型 (DOM)。</span><span class="sxs-lookup"><span data-stu-id="22f02-153">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="22f02-154">您可以將元件加入另一個元件使用 HTML 的類似語法。</span><span class="sxs-lookup"><span data-stu-id="22f02-154">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="22f02-155">元件參數會指定使用屬性或子內容。</span><span class="sxs-lookup"><span data-stu-id="22f02-155">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="22f02-156">比方說，將計數器元件可以加入至應用程式的首頁加`<Counter />`索引元件的項目。</span><span class="sxs-lookup"><span data-stu-id="22f02-156">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="22f02-157">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="22f02-157">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="22f02-158">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="22f02-158">Run the app.</span></span> <span data-ttu-id="22f02-159">在首頁上都有它自己的計數器。</span><span class="sxs-lookup"><span data-stu-id="22f02-159">The homepage has its own counter.</span></span>

<span data-ttu-id="22f02-160">若要將參數加入至計數器的元件，更新 元件的`@functions`區塊：</span><span class="sxs-lookup"><span data-stu-id="22f02-160">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="22f02-161">新增的屬性`IncrementAmount`附有`[Parameter]`屬性。</span><span class="sxs-lookup"><span data-stu-id="22f02-161">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="22f02-162">將 `IncrementCount` 方法變更為在增加 `currentCount`的值時使用 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="22f02-162">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="22f02-163">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="22f02-163">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4,8)]

<span data-ttu-id="22f02-164">使用屬性在「首頁」元件的 `<Counter>` 元素中指定 `IncrementAmount` 參數。</span><span class="sxs-lookup"><span data-stu-id="22f02-164">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="22f02-165">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="22f02-165">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

<span data-ttu-id="22f02-166">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="22f02-166">Run the app.</span></span> <span data-ttu-id="22f02-167">在首頁上有它自己的十個在每次增加的計數器**Click me**按鈕已選取。</span><span class="sxs-lookup"><span data-stu-id="22f02-167">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="22f02-168">後續步驟</span><span class="sxs-lookup"><span data-stu-id="22f02-168">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
