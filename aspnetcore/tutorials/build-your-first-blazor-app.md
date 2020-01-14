---
title: 建立您的第一個 Blazor 應用程式
author: guardrex
description: 逐步建立 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
no-loc:
- Blazor
uid: tutorials/first-blazor-app
ms.openlocfilehash: 8830dcf26b58b5f5fdd36b60298e7b365f99bdd9
ms.sourcegitcommit: 925cdbd94613243f33bc7613a62ea34006219931
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/13/2020
ms.locfileid: "75921296"
---
# <a name="build-your-first-opno-locblazor-app"></a><span data-ttu-id="a38f7-103">建立您的第一個 Blazor 應用程式</span><span class="sxs-lookup"><span data-stu-id="a38f7-103">Build your first Blazor app</span></span>

<span data-ttu-id="a38f7-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a38f7-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="a38f7-105">本教學課程說明如何建立和修改 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a38f7-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

<span data-ttu-id="a38f7-106">遵循 <xref:blazor/get-started> 文章中的指導方針，為本教學課程建立 Blazor 專案。</span><span class="sxs-lookup"><span data-stu-id="a38f7-106">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span> <span data-ttu-id="a38f7-107">將專案命名為 *ToDoList*。</span><span class="sxs-lookup"><span data-stu-id="a38f7-107">Name the project *ToDoList*.</span></span>

## <a name="build-components"></a><span data-ttu-id="a38f7-108">組建元件</span><span class="sxs-lookup"><span data-stu-id="a38f7-108">Build components</span></span>

1. <span data-ttu-id="a38f7-109">流覽至每個應用程式的 [ *pages* ] 資料夾中的三個頁面： [首頁]、[計數器] 和 [提取資料]。</span><span class="sxs-lookup"><span data-stu-id="a38f7-109">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="a38f7-110">這些頁面會透過 Razor 元件檔案 *Index.razor*、*Counter.razor* 及 *FetchData.razor* 來實作。</span><span class="sxs-lookup"><span data-stu-id="a38f7-110">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="a38f7-111">在 [計數器] 頁面上，選取 [按我] 按鈕以在不重新整理頁面的情況下讓計數器遞增。</span><span class="sxs-lookup"><span data-stu-id="a38f7-111">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="a38f7-112">將網頁中的計數器遞增通常需要撰寫 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="a38f7-112">Incrementing a counter in a webpage normally requires writing JavaScript.</span></span> <span data-ttu-id="a38f7-113">使用 Blazor，您可以改C#為撰寫。</span><span class="sxs-lookup"><span data-stu-id="a38f7-113">With Blazor, you can write C# instead.</span></span>

1. <span data-ttu-id="a38f7-114">檢查 *Counter.razor`Counter` 檔案中*  元件的實作。</span><span class="sxs-lookup"><span data-stu-id="a38f7-114">Examine the implementation of the `Counter` component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="a38f7-115">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="a38f7-115">*Pages/Counter.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="a38f7-116">`Counter` 元件的 UI 是使用 HTML 來定義的。</span><span class="sxs-lookup"><span data-stu-id="a38f7-116">The UI of the `Counter` component is defined using HTML.</span></span> <span data-ttu-id="a38f7-117">動態轉譯邏輯 (例如迴圈、條件、運算式) 是使用內嵌的 C# 語法 (稱為 [Razor](xref:mvc/views/razor)) 來新增的。</span><span class="sxs-lookup"><span data-stu-id="a38f7-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="a38f7-118">HTML 標記和 C# 轉譯邏輯會在組建時轉換為元件類別。</span><span class="sxs-lookup"><span data-stu-id="a38f7-118">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="a38f7-119">所產生 .NET 類別的名稱會與檔案名稱相符。</span><span class="sxs-lookup"><span data-stu-id="a38f7-119">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="a38f7-120">元件類別的成員均定義於 `@code` 區塊中。</span><span class="sxs-lookup"><span data-stu-id="a38f7-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="a38f7-121">在 `@code` 區塊中，會指定元件狀態 (屬性、欄位) 來處理事件或定義其他元件邏輯。</span><span class="sxs-lookup"><span data-stu-id="a38f7-121">In the `@code` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="a38f7-122">然後會使用這些成員作為元件轉譯邏輯的一部分，並用於處理事件。</span><span class="sxs-lookup"><span data-stu-id="a38f7-122">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="a38f7-123">選取 [Click me] \(按我\) 按鈕時：</span><span class="sxs-lookup"><span data-stu-id="a38f7-123">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="a38f7-124">會呼叫 `Counter` 元件的已註冊 `onclick` 處理常式 (`IncrementCount` 方法)。</span><span class="sxs-lookup"><span data-stu-id="a38f7-124">The `Counter` component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="a38f7-125">`Counter` 元件會重新產生其轉譯樹狀結構。</span><span class="sxs-lookup"><span data-stu-id="a38f7-125">The `Counter` component regenerates its render tree.</span></span>
   * <span data-ttu-id="a38f7-126">新的轉譯樹狀結構會與先前的樹狀結構做比較。</span><span class="sxs-lookup"><span data-stu-id="a38f7-126">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="a38f7-127">只會套用對「文件物件模型」(DOM) 所做的修改。</span><span class="sxs-lookup"><span data-stu-id="a38f7-127">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="a38f7-128">顯示的計數即會更新。</span><span class="sxs-lookup"><span data-stu-id="a38f7-128">The displayed count is updated.</span></span>

1. <span data-ttu-id="a38f7-129">修改 `Counter` 元件的 C# 邏輯，讓計數遞增 2 而不是 1。</span><span class="sxs-lookup"><span data-stu-id="a38f7-129">Modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="a38f7-130">重建並執行應用程式以查看變更。</span><span class="sxs-lookup"><span data-stu-id="a38f7-130">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="a38f7-131">選取 [按我] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a38f7-131">Select the **Click me** button.</span></span> <span data-ttu-id="a38f7-132">計數器會遞增二。</span><span class="sxs-lookup"><span data-stu-id="a38f7-132">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="a38f7-133">使用元件</span><span class="sxs-lookup"><span data-stu-id="a38f7-133">Use components</span></span>

<span data-ttu-id="a38f7-134">使用 HTML 語法來將一個元件包含在另一個元件中。</span><span class="sxs-lookup"><span data-stu-id="a38f7-134">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="a38f7-135">透過將 `<Counter />` 元素新增至 `Index` 元件 (*Index.razor*)，來將 `Counter` 元件新增至應用程式的 `Index` 元件。</span><span class="sxs-lookup"><span data-stu-id="a38f7-135">Add the `Counter` component to the app's `Index` component by adding a `<Counter />` element to the `Index` component (*Index.razor*).</span></span>

   <span data-ttu-id="a38f7-136">如果您使用 Blazor WebAssembly 來進行這項體驗，`Index` 元件會使用 `SurveyPrompt` 元件。</span><span class="sxs-lookup"><span data-stu-id="a38f7-136">If you're using Blazor WebAssembly for this experience, a `SurveyPrompt` component is used by the `Index` component.</span></span> <span data-ttu-id="a38f7-137">使用 `<Counter />` 元素取代 `<SurveyPrompt>` 元素。</span><span class="sxs-lookup"><span data-stu-id="a38f7-137">Replace the `<SurveyPrompt>` element with a `<Counter />` element.</span></span> <span data-ttu-id="a38f7-138">如果您使用 Blazor 伺服器應用程式進行這項體驗，請將 `<Counter />` 元素新增至 `Index` 元件：</span><span class="sxs-lookup"><span data-stu-id="a38f7-138">If you're using a Blazor Server app for this experience, add the `<Counter />` element to the `Index` component:</span></span>

   <span data-ttu-id="a38f7-139">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="a38f7-139">*Pages/Index.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="a38f7-140">重建並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a38f7-140">Rebuild and run the app.</span></span> <span data-ttu-id="a38f7-141">`Index` 元件有自己的計數器。</span><span class="sxs-lookup"><span data-stu-id="a38f7-141">The `Index` component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="a38f7-142">元件參數</span><span class="sxs-lookup"><span data-stu-id="a38f7-142">Component parameters</span></span>

<span data-ttu-id="a38f7-143">元件也可以有參數。</span><span class="sxs-lookup"><span data-stu-id="a38f7-143">Components can also have parameters.</span></span> <span data-ttu-id="a38f7-144">元件參數是在元件類別上使用具有 `[Parameter]` 屬性的公用屬性來定義。</span><span class="sxs-lookup"><span data-stu-id="a38f7-144">Component parameters are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="a38f7-145">使用這些屬性來指定標記中元件的引數。</span><span class="sxs-lookup"><span data-stu-id="a38f7-145">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="a38f7-146">更新元件的 `@code` C#程式碼，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a38f7-146">Update the component's `@code` C# code as follows:</span></span>

   * <span data-ttu-id="a38f7-147">加入具有 `[Parameter]` 屬性的公用 `IncrementAmount` 屬性。</span><span class="sxs-lookup"><span data-stu-id="a38f7-147">Add a public `IncrementAmount` property with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="a38f7-148">當增加 `currentCount`的值時，請將 `IncrementCount` 方法變更為使用 `IncrementAmount` 屬性。</span><span class="sxs-lookup"><span data-stu-id="a38f7-148">Change the `IncrementCount` method to use the `IncrementAmount` property when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="a38f7-149">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="a38f7-149">*Pages/Counter.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

   <!-- Add back when supported.
       > [!NOTE]
       > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
   -->

1. <span data-ttu-id="a38f7-150">使用屬性在 `Index` 元件的 `<Counter>` 元素中指定 `IncrementAmount` 參數。</span><span class="sxs-lookup"><span data-stu-id="a38f7-150">Specify an `IncrementAmount` parameter in the `Index` component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="a38f7-151">設定值來讓計數器以 10 遞增。</span><span class="sxs-lookup"><span data-stu-id="a38f7-151">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="a38f7-152">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="a38f7-152">*Pages/Index.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="a38f7-153">重新載入 `Index` 元件。</span><span class="sxs-lookup"><span data-stu-id="a38f7-153">Reload the `Index` component.</span></span> <span data-ttu-id="a38f7-154">每次選取 [Click me] \(按我\) 按鈕時，計數器都會以 10 遞增。</span><span class="sxs-lookup"><span data-stu-id="a38f7-154">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="a38f7-155">`Counter` 元件中的計數器會繼續遞增一。</span><span class="sxs-lookup"><span data-stu-id="a38f7-155">The counter in the `Counter` component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="a38f7-156">路由到元件</span><span class="sxs-lookup"><span data-stu-id="a38f7-156">Route to components</span></span>

<span data-ttu-id="a38f7-157">*Counter.razor* 檔案頂端的 `@page` 指示詞會指定 `Counter` 元件是路由端點。</span><span class="sxs-lookup"><span data-stu-id="a38f7-157">The `@page` directive at the top of the *Counter.razor* file specifies that the `Counter` component is a routing endpoint.</span></span> <span data-ttu-id="a38f7-158">`Counter` 元件會處理傳送給 `/counter` 的要求。</span><span class="sxs-lookup"><span data-stu-id="a38f7-158">The `Counter` component handles requests sent to `/counter`.</span></span> <span data-ttu-id="a38f7-159">若沒有 `@page` 指示詞，元件就不會處理路由傳送的要求，但其他元件仍可使用此元件。</span><span class="sxs-lookup"><span data-stu-id="a38f7-159">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="a38f7-160">相依性插入</span><span class="sxs-lookup"><span data-stu-id="a38f7-160">Dependency injection</span></span>

### <a name="opno-locblazor-server-experience"></a>Blazor<span data-ttu-id="a38f7-161"> 伺服器體驗</span><span class="sxs-lookup"><span data-stu-id="a38f7-161"> Server experience</span></span>

<span data-ttu-id="a38f7-162">如果使用 Blazor 伺服器應用程式，`WeatherForecastService` 服務會在 `Startup.ConfigureServices`中註冊為[singleton](xref:fundamentals/dependency-injection#service-lifetimes) 。</span><span class="sxs-lookup"><span data-stu-id="a38f7-162">If working with a Blazor Server app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a38f7-163">服務的實例可透過相依性[插入（DI）](xref:fundamentals/dependency-injection)在整個應用程式中使用：</span><span class="sxs-lookup"><span data-stu-id="a38f7-163">An instance of the service is available throughout the app via [dependency injection (DI)](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="a38f7-164">`@inject` 指示詞是用來將 `WeatherForecastService` 服務的實例插入 `FetchData` 元件。</span><span class="sxs-lookup"><span data-stu-id="a38f7-164">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the `FetchData` component.</span></span>

<span data-ttu-id="a38f7-165">*Pages/FetchData.razor*：</span><span class="sxs-lookup"><span data-stu-id="a38f7-165">*Pages/FetchData.razor*:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="a38f7-166">`FetchData` 元件會使用插入的服務作為 `ForecastService`，以擷取 `WeatherForecast` 物件的陣列：</span><span class="sxs-lookup"><span data-stu-id="a38f7-166">The `FetchData` component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

### <a name="opno-locblazor-webassembly-experience"></a>Blazor<span data-ttu-id="a38f7-167"> WebAssembly 體驗</span><span class="sxs-lookup"><span data-stu-id="a38f7-167"> WebAssembly experience</span></span>

<span data-ttu-id="a38f7-168">如果使用 Blazor WebAssembly 應用程式，`HttpClient` 會插入，以從*wwwroot/sample-data*資料夾中的*氣象*檔案取得氣象預報資料。</span><span class="sxs-lookup"><span data-stu-id="a38f7-168">If working with a Blazor WebAssembly app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder.</span></span>

<span data-ttu-id="a38f7-169">*Pages/FetchData.razor*：</span><span class="sxs-lookup"><span data-stu-id="a38f7-169">*Pages/FetchData.razor*:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="a38f7-170">[`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in)迴圈是用來將每個預測實例轉譯為天氣資料之資料表中的資料列：</span><span class="sxs-lookup"><span data-stu-id="a38f7-170">An [`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="a38f7-171">組建待辦事項清單</span><span class="sxs-lookup"><span data-stu-id="a38f7-171">Build a todo list</span></span>

<span data-ttu-id="a38f7-172">在實作簡單待辦事項清單的應用程式中新增元件。</span><span class="sxs-lookup"><span data-stu-id="a38f7-172">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="a38f7-173">將名為 *Todo.razor* 的空白檔案新增至 *Pages* 資料夾中的應用程式：</span><span class="sxs-lookup"><span data-stu-id="a38f7-173">Add an empty file named *Todo.razor* to the app in the *Pages* folder:</span></span>

1. <span data-ttu-id="a38f7-174">提供元件的初始標記：</span><span class="sxs-lookup"><span data-stu-id="a38f7-174">Provide the initial markup for the component:</span></span>

   ```razor
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="a38f7-175">將 `Todo` 元件新增至導覽列。</span><span class="sxs-lookup"><span data-stu-id="a38f7-175">Add the `Todo` component to the navigation bar.</span></span>

   <span data-ttu-id="a38f7-176">`NavMenu` 元件 (*Shared/NavMenu.razor*) 會用於應用程式的版面配置。</span><span class="sxs-lookup"><span data-stu-id="a38f7-176">The `NavMenu` component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="a38f7-177">版面配置是可讓您避免應用程式中內容重複的元件。</span><span class="sxs-lookup"><span data-stu-id="a38f7-177">Layouts are components that allow you to avoid duplication of content in the app.</span></span>

   <span data-ttu-id="a38f7-178">透過在 *Shared/NavMenu.razor* 檔案中的現有清單項目下方新增下列清單項目標記，為 `Todo` 元件新增 `<NavLink>` 元素：</span><span class="sxs-lookup"><span data-stu-id="a38f7-178">Add a `<NavLink>` element for the `Todo` component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```razor
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="a38f7-179">重建並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a38f7-179">Rebuild and run the app.</span></span> <span data-ttu-id="a38f7-180">瀏覽新的 [待辦事項] 頁面，以確認 `Todo` 元件的連結可以運作。</span><span class="sxs-lookup"><span data-stu-id="a38f7-180">Visit the new Todo page to confirm that the link to the `Todo` component works.</span></span>

1. <span data-ttu-id="a38f7-181">將 *TodoItem.cs* 檔案新增至專案的根目錄，以保存代表待辦事項的類別。</span><span class="sxs-lookup"><span data-stu-id="a38f7-181">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="a38f7-182">請使用下列 `TodoItem` 類別的 C# 程式碼：</span><span class="sxs-lookup"><span data-stu-id="a38f7-182">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="a38f7-183">返回 `Todo` 元件 (*Pages/Todo.razor*)：</span><span class="sxs-lookup"><span data-stu-id="a38f7-183">Return to the `Todo` component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="a38f7-184">在 `@code` 區塊中新增待辦事項的欄位。</span><span class="sxs-lookup"><span data-stu-id="a38f7-184">Add a field for the todo items in an `@code` block.</span></span> <span data-ttu-id="a38f7-185">`Todo` 元件會使用此欄位來維護待辦事項清單的狀態。</span><span class="sxs-lookup"><span data-stu-id="a38f7-185">The `Todo` component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="a38f7-186">新增未排序的清單標記和 `foreach` 迴圈，將每個待辦事項轉譯為清單項目 (`<li>`)。</span><span class="sxs-lookup"><span data-stu-id="a38f7-186">Add unordered list markup and a `foreach` loop to render each todo item as a list item (`<li>`).</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="a38f7-187">應用程式需要 UI 元素，才能將待辦事項新增至清單。</span><span class="sxs-lookup"><span data-stu-id="a38f7-187">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="a38f7-188">在未排序清單 (`<ul>...</ul>`) 下方新增文字輸出 (`<input>`) 與按鈕 (`<button>`)：</span><span class="sxs-lookup"><span data-stu-id="a38f7-188">Add a text input (`<input>`) and a button (`<button>`) below the unordered list (`<ul>...</ul>`):</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="a38f7-189">重建並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a38f7-189">Rebuild and run the app.</span></span> <span data-ttu-id="a38f7-190">選取 [Add todo] \(新增待辦事項\) 按鈕時，不會發生任何情況，因為事件處理常式並未連接至這個按鈕。</span><span class="sxs-lookup"><span data-stu-id="a38f7-190">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="a38f7-191">將 `AddTodo` 方法新增至 `Todo` 元件並註冊，以便使用 `@onclick` 屬性來進行按鈕選取。</span><span class="sxs-lookup"><span data-stu-id="a38f7-191">Add an `AddTodo` method to the `Todo` component and register it for button selections using the `@onclick` attribute.</span></span> <span data-ttu-id="a38f7-192">當選取按鈕時，就會呼叫 `AddTodo` C# 方法：</span><span class="sxs-lookup"><span data-stu-id="a38f7-192">The `AddTodo` C# method is called when the button is selected:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. <span data-ttu-id="a38f7-193">若要取得新待辦事項的標題，請在 `@code` 區塊頂端新增 `newTodo` 字串欄位，然後使用 `<input>` 元素中的 `bind` 屬性將它繫結至文字輸入的值：</span><span class="sxs-lookup"><span data-stu-id="a38f7-193">To get the title of the new todo item, add a `newTodo` string field at the top of the `@code` block and bind it to the value of the text input using the `bind` attribute in the `<input>` element:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```razor
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. <span data-ttu-id="a38f7-194">更新 `AddTodo` 方法，將 `TodoItem` 與指定的標題新增至清單。</span><span class="sxs-lookup"><span data-stu-id="a38f7-194">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="a38f7-195">請將 `newTodo` 設定為空字串，以清除文字輸入的值：</span><span class="sxs-lookup"><span data-stu-id="a38f7-195">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="a38f7-196">重建並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a38f7-196">Rebuild and run the app.</span></span> <span data-ttu-id="a38f7-197">請將一些待辦事項新增至待辦事項清單，以測試新程式碼。</span><span class="sxs-lookup"><span data-stu-id="a38f7-197">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="a38f7-198">每個待辦事項的標題文字都可設定為可編輯，而核取方塊則可協助使用者記錄已完成的項目。</span><span class="sxs-lookup"><span data-stu-id="a38f7-198">The title text for each todo item can be made editable, and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="a38f7-199">請為每個待辦事項新增核取方塊輸入，然後將其值繫結至 `IsDone` 屬性。</span><span class="sxs-lookup"><span data-stu-id="a38f7-199">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="a38f7-200">將 `@todo.Title` 變更為繫結至 `@todo.Title` 的 `<input>` 元素：</span><span class="sxs-lookup"><span data-stu-id="a38f7-200">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="a38f7-201">若要確認是否已繫結這些值，請更新 `<h1>` 標頭，以顯示未完成之待辦事項 (`IsDone` 為 `false`) 的數目計數。</span><span class="sxs-lookup"><span data-stu-id="a38f7-201">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```razor
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="a38f7-202">已完成的 `Todo` 元件 (*Pages/Todo.razor*)：</span><span class="sxs-lookup"><span data-stu-id="a38f7-202">The completed `Todo` component (*Pages/Todo.razor*):</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="a38f7-203">重建並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a38f7-203">Rebuild and run the app.</span></span> <span data-ttu-id="a38f7-204">請新增待辦事項，以測試新程式碼。</span><span class="sxs-lookup"><span data-stu-id="a38f7-204">Add todo items to test the new code.</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/components>
