---
title: 開始使用 Blazor
author: guardrex
description: 了解如何開始使用 Blazor 建立和修改 Blazor 專案。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2019
uid: spa/blazor/get-started
ms.openlocfilehash: f46bd9af0f0762e794349d4e98de5c086a690d72
ms.sourcegitcommit: a1c43150ed46aa01572399e8aede50d4668745ca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2019
ms.locfileid: "58327225"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="abf96-103">開始使用 Blazor</span><span class="sxs-lookup"><span data-stu-id="abf96-103">Get started with Blazor</span></span>

<span data-ttu-id="abf96-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="abf96-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="abf96-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="abf96-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="abf96-106">必要條件：</span><span class="sxs-lookup"><span data-stu-id="abf96-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="abf96-107">若要在 Visual Studio 中建立第一個 Blazor 專案：</span><span class="sxs-lookup"><span data-stu-id="abf96-107">To create your first Blazor project in Visual Studio:</span></span>

1. <span data-ttu-id="abf96-108">安裝最新[Blazor 擴充](https://go.microsoft.com/fwlink/?linkid=870389)從 Visual Studio Marketplace。</span><span class="sxs-lookup"><span data-stu-id="abf96-108">Install the latest [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="abf96-109">此步驟會 Blazor 範本提供給 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="abf96-109">This step makes Blazor templates available to Visual Studio.</span></span>
1. <span data-ttu-id="abf96-110">在命令殼層中執行下列命令，請 Blazor 範本適用於.NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="abf96-110">Make the Blazor templates available for use with the .NET Core CLI by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="abf96-111">選取 **檔案** > **新專案** > **Web** > **ASP.NET Core Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="abf96-111">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="abf96-112">請確定 **.NET Core**並**ASP.NET Core 3.0**選取頂端。</span><span class="sxs-lookup"><span data-stu-id="abf96-112">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="abf96-113">選擇 **Blazor** 範本，然後選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="abf96-113">Choose the **Blazor** template and select **OK**.</span></span>
1. <span data-ttu-id="abf96-114">按下 **F5** 即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="abf96-114">Press **F5** to run the app.</span></span>

<span data-ttu-id="abf96-115">恭喜您！</span><span class="sxs-lookup"><span data-stu-id="abf96-115">Congratulations!</span></span> <span data-ttu-id="abf96-116">您剛剛執行第一個 Blazor 應用程式 ！</span><span class="sxs-lookup"><span data-stu-id="abf96-116">You just ran your first Blazor app!</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Blazor project in Visual Studio Code:

1. Execute the following command in a command shell:

   ```console
   dotnet new blazor -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Visual Studio code offers to create assets to build and debug the app, which includes the *tasks.json* and *launch.json* files. Select **Yes** to add the assets.

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Blazor app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Blazor project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **Blazor** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Blazor app!
-->

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="abf96-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="abf96-117">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="abf96-118">必要條件：</span><span class="sxs-lookup"><span data-stu-id="abf96-118">Prerequisites:</span></span>

* [<span data-ttu-id="abf96-119">.NET core SDK 3.0 預覽</span><span class="sxs-lookup"><span data-stu-id="abf96-119">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="abf96-120">在命令殼層中執行下列命令，將新增 Blazor 範本：</span><span class="sxs-lookup"><span data-stu-id="abf96-120">Add the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="abf96-121">在命令殼層中建立第一個 Blazor 專案：</span><span class="sxs-lookup"><span data-stu-id="abf96-121">Create your first Blazor project in a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="abf96-122">在瀏覽器中，巡覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="abf96-122">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="abf96-123">恭喜您！</span><span class="sxs-lookup"><span data-stu-id="abf96-123">Congratulations!</span></span> <span data-ttu-id="abf96-124">您剛剛執行第一個 Blazor 應用程式 ！</span><span class="sxs-lookup"><span data-stu-id="abf96-124">You just ran your first Blazor app!</span></span>

---

## <a name="blazor-project"></a><span data-ttu-id="abf96-125">Blazor 專案</span><span class="sxs-lookup"><span data-stu-id="abf96-125">Blazor project</span></span>

<span data-ttu-id="abf96-126">執行應用程式時，多個頁面都是從資訊看板中的索引標籤：</span><span class="sxs-lookup"><span data-stu-id="abf96-126">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="abf96-127">首頁</span><span class="sxs-lookup"><span data-stu-id="abf96-127">Home</span></span>
* <span data-ttu-id="abf96-128">計數器</span><span class="sxs-lookup"><span data-stu-id="abf96-128">Counter</span></span>
* <span data-ttu-id="abf96-129">擷取資料</span><span class="sxs-lookup"><span data-stu-id="abf96-129">Fetch data</span></span>

<span data-ttu-id="abf96-130">在 [計數器] 頁面上，選取 [按我] 按鈕以在不重新整理頁面的情況下讓計數器遞增。</span><span class="sxs-lookup"><span data-stu-id="abf96-130">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="abf96-131">遞增計數器，以在網頁上通常需要撰寫 JavaScript，但 Blazor 提供更好的方法使用C#。</span><span class="sxs-lookup"><span data-stu-id="abf96-131">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

<span data-ttu-id="abf96-132">*Pages/Counter.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="abf96-132">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="abf96-133">申請`/counter`在瀏覽器中所指定`@page`指示詞，在頂端，導致計數器元件來呈現其內容。</span><span class="sxs-lookup"><span data-stu-id="abf96-133">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="abf96-134">元件會轉譯成接著可用來更新 UI 富彈性又有效率的方式呈現樹狀結構的記憶體中表示法。</span><span class="sxs-lookup"><span data-stu-id="abf96-134">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="abf96-135">每次**Click me**按鈕已選取：</span><span class="sxs-lookup"><span data-stu-id="abf96-135">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="abf96-136">`onclick`引發事件。</span><span class="sxs-lookup"><span data-stu-id="abf96-136">The `onclick` event is fired.</span></span>
* <span data-ttu-id="abf96-137">已呼叫 `IncrementCount` 方法。</span><span class="sxs-lookup"><span data-stu-id="abf96-137">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="abf96-138">`currentCount`會遞增。</span><span class="sxs-lookup"><span data-stu-id="abf96-138">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="abf96-139">元件會轉譯一次。</span><span class="sxs-lookup"><span data-stu-id="abf96-139">The component is rendered again.</span></span>

<span data-ttu-id="abf96-140">執行階段會比較新的內容來將舊內容，並只會套用已變更的內容與文件物件模型 (DOM)。</span><span class="sxs-lookup"><span data-stu-id="abf96-140">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="abf96-141">您可以將元件加入另一個元件使用 HTML 的類似語法。</span><span class="sxs-lookup"><span data-stu-id="abf96-141">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="abf96-142">元件參數會指定使用屬性或子內容。</span><span class="sxs-lookup"><span data-stu-id="abf96-142">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="abf96-143">比方說，將計數器元件可以加入至應用程式的首頁加`<Counter />`索引元件的項目。</span><span class="sxs-lookup"><span data-stu-id="abf96-143">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="abf96-144">在  *pages/Index.cshtml*，問卷提示元件取代計數器元件：</span><span class="sxs-lookup"><span data-stu-id="abf96-144">In *Pages/Index.cshtml*, replace the Survey Prompt component with a Counter component:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="abf96-145">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="abf96-145">Run the app.</span></span> <span data-ttu-id="abf96-146">在首頁上都有它自己的計數器。</span><span class="sxs-lookup"><span data-stu-id="abf96-146">The homepage has its own counter.</span></span>

<span data-ttu-id="abf96-147">若要將參數加入至計數器的元件，更新 元件的`@functions`區塊：</span><span class="sxs-lookup"><span data-stu-id="abf96-147">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="abf96-148">新增的屬性`IncrementAmount`附有`[Parameter]`屬性。</span><span class="sxs-lookup"><span data-stu-id="abf96-148">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="abf96-149">將 `IncrementCount` 方法變更為在增加 `currentCount`的值時使用 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="abf96-149">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="abf96-150">*Pages/Counter.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="abf96-150">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="abf96-151">使用屬性在「首頁」元件的 `<Counter>` 元素中指定 `IncrementAmount` 參數。</span><span class="sxs-lookup"><span data-stu-id="abf96-151">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="abf96-152">*Pages/Index.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="abf96-152">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="abf96-153">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="abf96-153">Run the app.</span></span> <span data-ttu-id="abf96-154">在首頁上有它自己的十個在每次增加的計數器**Click me**按鈕已選取。</span><span class="sxs-lookup"><span data-stu-id="abf96-154">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="abf96-155">後續步驟</span><span class="sxs-lookup"><span data-stu-id="abf96-155">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
