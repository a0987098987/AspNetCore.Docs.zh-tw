---
title: 建置您的第一個 Razor 元件應用程式
author: guardrex
description: 逐步建置 Razor 元件應用程式，並了解基本 Razor 元件概念。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: c0f7b27fdfc770f8001625ecb3bf8d50af517b99
ms.sourcegitcommit: 10e14b85490f064395e9b2f423d21e3c2d39ed8b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "57978419"
---
# <a name="build-your-first-razor-components-app"></a><span data-ttu-id="91631-103">建置您的第一個 Razor 元件應用程式</span><span class="sxs-lookup"><span data-stu-id="91631-103">Build your first Razor Components app</span></span>

<span data-ttu-id="91631-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="91631-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="91631-105">本教學課程說明如何使用 Razor 元件來建置應用程式，並示範基本 Razor 元件概念。</span><span class="sxs-lookup"><span data-stu-id="91631-105">This tutorial shows you how to build an app with Razor Components and demonstrates basic Razor Components concepts.</span></span> <span data-ttu-id="91631-106">您可以使用 Razor 元件型專案 (.NET Core 3.0 或更新版本中可支援) 或使用 Blazor 型專案 (.NET Core 未來的版本中可支援) 來進行此教學課程。</span><span class="sxs-lookup"><span data-stu-id="91631-106">You can enjoy this tutorial using either a Razor Components-based project (supported in .NET Core 3.0 or later) or using a Blazor-based project (supported in a future release of .NET Core).</span></span>

<span data-ttu-id="91631-107">針對使用 ASP.NET Core Razor 元件 (建議使用) 的體驗：</span><span class="sxs-lookup"><span data-stu-id="91631-107">For an experience using ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="91631-108">依照 <xref:razor-components/get-started> 中的指引來建立 Razor 元件型專案。</span><span class="sxs-lookup"><span data-stu-id="91631-108">Follow the guidance in <xref:razor-components/get-started> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="91631-109">將專案命名為 `RazorComponents`。</span><span class="sxs-lookup"><span data-stu-id="91631-109">Name the project `RazorComponents`.</span></span>

<span data-ttu-id="91631-110">針對使用 Blazor 的體驗：</span><span class="sxs-lookup"><span data-stu-id="91631-110">For an experience using Blazor:</span></span>

* <span data-ttu-id="91631-111">依照 <xref:spa/blazor/get-started> 中的指引來建立 Blazor 型專案。</span><span class="sxs-lookup"><span data-stu-id="91631-111">Follow the guidance in <xref:spa/blazor/get-started> to create a Blazor-based project.</span></span>
* <span data-ttu-id="91631-112">將專案命名為 `Blazor`。</span><span class="sxs-lookup"><span data-stu-id="91631-112">Name the project `Blazor`.</span></span>

<span data-ttu-id="91631-113">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="91631-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="91631-114">如需了解先決條件，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="91631-114">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="91631-115">組建元件</span><span class="sxs-lookup"><span data-stu-id="91631-115">Build components</span></span>

1. <span data-ttu-id="91631-116">瀏覽至每個應用程式在 [Components/Pages] 資料夾 (Blazor 中的 [Pages]) 的三個頁面：首頁、計數器和擷取資料。</span><span class="sxs-lookup"><span data-stu-id="91631-116">Browse to each of the app's three pages in the *Components/Pages* folder (*Pages* in Blazor): Home, Counter, and Fetch data.</span></span> <span data-ttu-id="91631-117">這些頁面會由下列 Razor 元件檔案實作：*Index.razor\*\*Counter.razor* 及 *FetchData.razor*。</span><span class="sxs-lookup"><span data-stu-id="91631-117">These pages are implemented by Razor Component files: *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span> <span data-ttu-id="91631-118">(Blazor 會繼續使用 *.cshtml* 副檔名：*Index.cshtml*、*Counter.cshtml*及 *FetchData.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="91631-118">(Blazor continues to use the *.cshtml* file extension: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.)</span></span>

1. <span data-ttu-id="91631-119">在 [計數器] 頁面上，選取 [按我] 按鈕以在不重新整理頁面的情況下讓計數器遞增。</span><span class="sxs-lookup"><span data-stu-id="91631-119">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="91631-120">讓網頁中的計數器遞增通常需要撰寫 JavaScript，但 Razor 元件提供一個使用 C# 的更好方法。</span><span class="sxs-lookup"><span data-stu-id="91631-120">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

1. <span data-ttu-id="91631-121">檢查 *Counter.razor* 檔案中「計數器」元件的實作。</span><span class="sxs-lookup"><span data-stu-id="91631-121">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="91631-122">*Components/Pages/Counter.razor* (Blazor 中的 *Pages/Counter.cshtml*)：</span><span class="sxs-lookup"><span data-stu-id="91631-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="91631-123">「計數器」元件的 UI 是使用 HTML 來定義的。</span><span class="sxs-lookup"><span data-stu-id="91631-123">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="91631-124">動態轉譯邏輯 (例如迴圈、條件、運算式) 是使用內嵌的 C# 語法 (稱為 [Razor](xref:mvc/views/razor)) 來新增的。</span><span class="sxs-lookup"><span data-stu-id="91631-124">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="91631-125">HTML 標記和 C# 轉譯邏輯會在組建時轉換為元件類別。</span><span class="sxs-lookup"><span data-stu-id="91631-125">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="91631-126">所產生 .NET 類別的名稱會與檔案名稱相符。</span><span class="sxs-lookup"><span data-stu-id="91631-126">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="91631-127">元件類別的成員定義在 `@functions` 區塊中。</span><span class="sxs-lookup"><span data-stu-id="91631-127">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="91631-128">在 `@functions` 區塊中，會指定元件狀態 (屬性、欄位) 來處理事件或定義其他元件邏輯。</span><span class="sxs-lookup"><span data-stu-id="91631-128">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="91631-129">然後會使用這些成員作為元件轉譯邏輯的一部分，並用於處理事件。</span><span class="sxs-lookup"><span data-stu-id="91631-129">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="91631-130">選取 [Click me] \(按我\) 按鈕時：</span><span class="sxs-lookup"><span data-stu-id="91631-130">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="91631-131">會呼叫「計數器」元件的已註冊 `onclick` 處理常式 (`IncrementCount` 方法)。</span><span class="sxs-lookup"><span data-stu-id="91631-131">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="91631-132">「計數器」元件會重新產生其轉譯樹狀結構。</span><span class="sxs-lookup"><span data-stu-id="91631-132">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="91631-133">新的轉譯樹狀結構會與先前的樹狀結構做比較。</span><span class="sxs-lookup"><span data-stu-id="91631-133">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="91631-134">只會套用對「文件物件模型」(DOM) 所做的修改。</span><span class="sxs-lookup"><span data-stu-id="91631-134">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="91631-135">顯示的計數即會更新。</span><span class="sxs-lookup"><span data-stu-id="91631-135">The displayed count is updated.</span></span>

1. <span data-ttu-id="91631-136">修改「計數器」元件的 C# 邏輯，讓計數以 2 遞增而不是 1。</span><span class="sxs-lookup"><span data-stu-id="91631-136">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="91631-137">重建並執行應用程式以查看變更。</span><span class="sxs-lookup"><span data-stu-id="91631-137">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="91631-138">選取 [Click me] \(按我\) 按鈕，計數器就會以 2 遞增。</span><span class="sxs-lookup"><span data-stu-id="91631-138">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="91631-139">使用元件</span><span class="sxs-lookup"><span data-stu-id="91631-139">Use components</span></span>

<span data-ttu-id="91631-140">使用類似 HTML 的語法將一個元件包含在另一個元件中。</span><span class="sxs-lookup"><span data-stu-id="91631-140">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="91631-141">藉由將 `<Counter />` 元素新增至「索引」元件，將「計數器」元件新增至應用程式的「索引」(首頁) 元件。</span><span class="sxs-lookup"><span data-stu-id="91631-141">Add the Counter component to the app's Index (home page) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="91631-142">如果您使用 Blazor 來進行此體驗，則「索引」元件中會有「問卷提示」元件 (`<SurveyPrompt>` 元素)。</span><span class="sxs-lookup"><span data-stu-id="91631-142">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="91631-143">請以 `<Counter>` 元素取代 `<SurveyPrompt>` 元素。</span><span class="sxs-lookup"><span data-stu-id="91631-143">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="91631-144">*Components/Pages/Index.razor* (Blazor 中的 *Pages/Index.cshtml*)：</span><span class="sxs-lookup"><span data-stu-id="91631-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. <span data-ttu-id="91631-145">重建並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="91631-145">Rebuild and run the app.</span></span> <span data-ttu-id="91631-146">首頁有自己的計數器。</span><span class="sxs-lookup"><span data-stu-id="91631-146">The home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="91631-147">元件參數</span><span class="sxs-lookup"><span data-stu-id="91631-147">Component parameters</span></span>

<span data-ttu-id="91631-148">元件也可以有參數。</span><span class="sxs-lookup"><span data-stu-id="91631-148">Components can also have parameters.</span></span> <span data-ttu-id="91631-149">定義元件參數時，是在元件類別上使用以 `[Parameter]` 裝飾的非公開屬性來定義。</span><span class="sxs-lookup"><span data-stu-id="91631-149">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="91631-150">使用這些屬性來指定標記中元件的引數。</span><span class="sxs-lookup"><span data-stu-id="91631-150">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="91631-151">更新元件的 `@functions` C# 程式碼：</span><span class="sxs-lookup"><span data-stu-id="91631-151">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="91631-152">新增以 `[Parameter]` 屬性裝飾的 `IncrementAmount` 屬性。</span><span class="sxs-lookup"><span data-stu-id="91631-152">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="91631-153">將 `IncrementCount` 方法變更為在增加 `currentCount`的值時使用 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="91631-153">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="91631-154">*Components/Pages/Counter.razor* (Blazor 中的 *Pages/Counter.cshtml*)：</span><span class="sxs-lookup"><span data-stu-id="91631-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="91631-155">使用屬性在「首頁」元件的 `<Counter>` 元素中指定 `IncrementAmount` 參數。</span><span class="sxs-lookup"><span data-stu-id="91631-155">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="91631-156">設定值來讓計數器以 10 遞增。</span><span class="sxs-lookup"><span data-stu-id="91631-156">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="91631-157">*Components/Pages/Index.razor* (Blazor 中的 *Pages/Index.cshtml*)：</span><span class="sxs-lookup"><span data-stu-id="91631-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. <span data-ttu-id="91631-158">重新載入頁面。</span><span class="sxs-lookup"><span data-stu-id="91631-158">Reload the page.</span></span> <span data-ttu-id="91631-159">每次選取 [Click me] \(按我\) 按鈕時，首頁計數器都會以 10 遞增。</span><span class="sxs-lookup"><span data-stu-id="91631-159">The home page counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="91631-160">[計數器] 頁面上的計數器會以 1 遞增。</span><span class="sxs-lookup"><span data-stu-id="91631-160">The counter on the *Counter* page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="91631-161">路由到元件</span><span class="sxs-lookup"><span data-stu-id="91631-161">Route to components</span></span>

<span data-ttu-id="91631-162">*Counter.razor* 檔案頂端的 `@page` 指示詞會指定此元件是路由端點。</span><span class="sxs-lookup"><span data-stu-id="91631-162">The `@page` directive at the top of the *Counter.razor* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="91631-163">「計數器」元件會處理傳送給 `/Counter` 的要求。</span><span class="sxs-lookup"><span data-stu-id="91631-163">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="91631-164">若沒有 `@page` 指示詞，此元件就不會處理路由傳送的要求，但其他元件仍可使用此元件。</span><span class="sxs-lookup"><span data-stu-id="91631-164">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="91631-165">相依性插入</span><span class="sxs-lookup"><span data-stu-id="91631-165">Dependency injection</span></span>

<span data-ttu-id="91631-166">在應用程式服務容器中註冊的服務可透過[相依性插入 (DI)](xref:fundamentals/dependency-injection) 供元件使用。</span><span class="sxs-lookup"><span data-stu-id="91631-166">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="91631-167">請使用 `@inject` 指示詞將服務插入至元件。</span><span class="sxs-lookup"><span data-stu-id="91631-167">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="91631-168">檢查 FetchData 元件的指示詞。</span><span class="sxs-lookup"><span data-stu-id="91631-168">Examine the directives of the FetchData component.</span></span> <span data-ttu-id="91631-169">`@inject` 指示詞會用來將 `WeatherForecastService` 服務執行個體插入至元件：</span><span class="sxs-lookup"><span data-stu-id="91631-169">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component:</span></span>

<span data-ttu-id="91631-170">*Components/Pages/FetchData.razor* (Blazor 中的 *Pages/FetchData.cshtml*)：</span><span class="sxs-lookup"><span data-stu-id="91631-170">*Components/Pages/FetchData.razor* (*Pages/FetchData.cshtml* in Blazor):</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="91631-171">`WeatherForecastService` 服務會註冊為[單一項目](xref:fundamentals/dependency-injection#service-lifetimes)，讓一個服務執行個體可供整個應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="91631-171">The `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span>

<span data-ttu-id="91631-172">FetchData 元件會使用插入的服務作為 `ForecastService`，以擷取 `WeatherForecast` 物件的陣列：</span><span class="sxs-lookup"><span data-stu-id="91631-172">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="91631-173">[@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) 迴圈會用來將每個預測執行個體轉譯為天氣資料表中的資料列：</span><span class="sxs-lookup"><span data-stu-id="91631-173">A [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="91631-174">組建待辦事項清單</span><span class="sxs-lookup"><span data-stu-id="91631-174">Build a todo list</span></span>

<span data-ttu-id="91631-175">在實作簡單待辦事項清單的應用程式中新增頁面。</span><span class="sxs-lookup"><span data-stu-id="91631-175">Add a new page to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="91631-176">將名為 *Todo.razor* 的空白檔案新增至 [Components/Pages] 資料夾 (Blazor 中的 [Pages] 資料夾)。</span><span class="sxs-lookup"><span data-stu-id="91631-176">Add an empty file to the *Components/Pages* folder (*Pages* folder in Blazor) named *Todo.razor*.</span></span>

1. <span data-ttu-id="91631-177">提供頁面的初始標記：</span><span class="sxs-lookup"><span data-stu-id="91631-177">Provide the initial markup for the page:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="91631-178">將 [待辦事項] 頁面新增至導覽列。</span><span class="sxs-lookup"><span data-stu-id="91631-178">Add the Todo page to the navigation bar.</span></span>

   <span data-ttu-id="91631-179">NavMenu 元件 (*Components/Shared/NavMenu.razor* 或 Blazor 中的 *Shared/NavMenu.cshtml*) 會用於應用程式的版面配置。</span><span class="sxs-lookup"><span data-stu-id="91631-179">The NavMenu component (*Components/Shared/NavMenu.razor* or *Shared/NavMenu.cshtml* in Blazor) is used in the app's layout.</span></span> <span data-ttu-id="91631-180">版面配置是可讓您避免應用程式中內容重複的元件。</span><span class="sxs-lookup"><span data-stu-id="91631-180">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="91631-181">如需詳細資訊，請參閱<xref:razor-components/layouts>。</span><span class="sxs-lookup"><span data-stu-id="91631-181">For more information, see <xref:razor-components/layouts>.</span></span>

   <span data-ttu-id="91631-182">透過在 *Components/Shared/NavMenu.razor* (Blazor 中的 *Shared/NavMenu.cshtml*) 檔案中的現有清單項目底下新增下列清單項目標記，為 [待辦事項] 頁面新增 `<NavLink>`：</span><span class="sxs-lookup"><span data-stu-id="91631-182">Add a `<NavLink>` for the Todo page by adding the following list item markup below the existing list items in the *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor) file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="91631-183">重建並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="91631-183">Rebuild and run the app.</span></span> <span data-ttu-id="91631-184">瀏覽新的 [待辦事項] 頁面，以確認 [待辦事項] 頁面的連結可以運作。</span><span class="sxs-lookup"><span data-stu-id="91631-184">Visit the new Todo page to confirm that the link to the Todo page works.</span></span>

1. <span data-ttu-id="91631-185">將 *TodoItem.cs* 檔案新增至專案的根目錄，以保存代表待辦事項的類別。</span><span class="sxs-lookup"><span data-stu-id="91631-185">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="91631-186">請使用下列 `TodoItem` 類別的 C# 程式碼：</span><span class="sxs-lookup"><span data-stu-id="91631-186">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. <span data-ttu-id="91631-187">返回「待辦事項」元件 (*Components/Pages/Todo.razor* 或 Blazor 中的 *Pages/Todo.cshtml*)：</span><span class="sxs-lookup"><span data-stu-id="91631-187">Return to the Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   * <span data-ttu-id="91631-188">在 `@functions` 區塊中新增待辦事項的欄位。</span><span class="sxs-lookup"><span data-stu-id="91631-188">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="91631-189">「待辦事項」元件會使用此欄位來維護待辦事項清單的狀態。</span><span class="sxs-lookup"><span data-stu-id="91631-189">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="91631-190">新增未排序的清單標記和 `foreach` 迴圈，將每個待辦事項轉譯為清單項目。</span><span class="sxs-lookup"><span data-stu-id="91631-190">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="91631-191">應用程式需要 UI 項目，才能將待辦事項新增至清單。</span><span class="sxs-lookup"><span data-stu-id="91631-191">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="91631-192">在清單下方新增文字輸入和按鈕：</span><span class="sxs-lookup"><span data-stu-id="91631-192">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="91631-193">重建並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="91631-193">Rebuild and run the app.</span></span> <span data-ttu-id="91631-194">選取 [Add todo] \(新增待辦事項\) 按鈕時，不會發生任何情況，因為事件處理常式並未連接至這個按鈕。</span><span class="sxs-lookup"><span data-stu-id="91631-194">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="91631-195">將 `AddTodo` 方法新增至「待辦事項」元件，然後使用 `onclick` 屬性將它註冊用於按鈕點選：</span><span class="sxs-lookup"><span data-stu-id="91631-195">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="91631-196">當選取按鈕時，就會呼叫 `AddTodo` C# 方法。</span><span class="sxs-lookup"><span data-stu-id="91631-196">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="91631-197">若要取得新待辦事項的標題，請新增 `newTodo` 字串欄位，然後使用 `bind` 屬性將它繫結至文字輸入的值：</span><span class="sxs-lookup"><span data-stu-id="91631-197">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. <span data-ttu-id="91631-198">更新 `AddTodo` 方法，將 `TodoItem` 與指定的標題新增至清單。</span><span class="sxs-lookup"><span data-stu-id="91631-198">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="91631-199">請將 `newTodo` 設定為空字串，以清除文字輸入的值：</span><span class="sxs-lookup"><span data-stu-id="91631-199">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="91631-200">重建並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="91631-200">Rebuild and run the app.</span></span> <span data-ttu-id="91631-201">請將一些待辦事項新增至待辦事項清單，以測試新程式碼。</span><span class="sxs-lookup"><span data-stu-id="91631-201">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="91631-202">每個待辦事項的標題文字都可設定為可編輯，而核取方塊則可協助使用者記錄已完成的項目。</span><span class="sxs-lookup"><span data-stu-id="91631-202">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="91631-203">請為每個待辦事項新增核取方塊輸入，然後將其值繫結至 `IsDone` 屬性。</span><span class="sxs-lookup"><span data-stu-id="91631-203">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="91631-204">將 `@todo.Title` 變更為繫結至 `@todo.Title` 的 `<input>` 元素：</span><span class="sxs-lookup"><span data-stu-id="91631-204">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="91631-205">若要確認是否已繫結這些值，請更新 `<h1>` 標頭，以顯示未完成之待辦事項 (`IsDone` 為 `false`) 的數目計數。</span><span class="sxs-lookup"><span data-stu-id="91631-205">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="91631-206">已完成的返回「待辦事項」元件 (*Components/Pages/Todo.razor* 或 Blazor 中的 *Pages/Todo.cshtml*)：</span><span class="sxs-lookup"><span data-stu-id="91631-206">The completed Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. <span data-ttu-id="91631-207">重建並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="91631-207">Rebuild and run the app.</span></span> <span data-ttu-id="91631-208">請新增待辦事項，以測試新程式碼。</span><span class="sxs-lookup"><span data-stu-id="91631-208">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="91631-209">發佈及部署應用程式</span><span class="sxs-lookup"><span data-stu-id="91631-209">Publish and deploy the app</span></span>

<span data-ttu-id="91631-210">若要發佈應用程式，請參閱 <xref:host-and-deploy/razor-components/index#publish-the-app>。</span><span class="sxs-lookup"><span data-stu-id="91631-210">To publish the app, see <xref:host-and-deploy/razor-components/index#publish-the-app>.</span></span>
