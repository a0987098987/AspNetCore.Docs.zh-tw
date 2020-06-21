---
title: 建立您的第一個 Blazor 應用程式
author: guardrex
description: 逐步建立 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/first-blazor-app
ms.openlocfilehash: 2de33deccbb86bd810c4337f5aa105118de4d9b4
ms.sourcegitcommit: 77729ba225d5143c0e3954db005906f4a5c7da95
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2020
ms.locfileid: "85122148"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="cb3ef-103">建立您的第一個 Blazor 應用程式</span><span class="sxs-lookup"><span data-stu-id="cb3ef-103">Build your first Blazor app</span></span>

<span data-ttu-id="cb3ef-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="cb3ef-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="cb3ef-105">本教學課程說明如何建立和修改 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-105">This tutorial shows you how to build and modify a Blazor app.</span></span> <span data-ttu-id="cb3ef-106">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="cb3ef-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cb3ef-107">建立待辦事項清單 Blazor 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="cb3ef-107">Create a todo list Blazor app project</span></span>
> * <span data-ttu-id="cb3ef-108">修改 Razor 元件</span><span class="sxs-lookup"><span data-stu-id="cb3ef-108">Modify Razor components</span></span>
> * <span data-ttu-id="cb3ef-109">在元件中使用事件處理和資料系結</span><span class="sxs-lookup"><span data-stu-id="cb3ef-109">Use event handling and data binding in components</span></span>
> * <span data-ttu-id="cb3ef-110">在應用程式中使用相依性插入（DI）和路由 Blazor</span><span class="sxs-lookup"><span data-stu-id="cb3ef-110">Use dependency injection (DI) and routing in a Blazor app</span></span>

<span data-ttu-id="cb3ef-111">在本教學課程結尾，您將會有一個可運作的待辦事項清單應用程式。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-111">At the end of this tutorial, you'll have a working todo list app.</span></span>

## <a name="build-components"></a><span data-ttu-id="cb3ef-112">組建元件</span><span class="sxs-lookup"><span data-stu-id="cb3ef-112">Build components</span></span>

1. <span data-ttu-id="cb3ef-113">請遵循本文中的指導方針 <xref:blazor/get-started> ，為 Blazor 本教學課程建立專案。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-113">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span> <span data-ttu-id="cb3ef-114">將專案命名為 `ToDoList`。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-114">Name the project `ToDoList`.</span></span>

1. <span data-ttu-id="cb3ef-115">流覽至每個應用程式在資料夾中的三個頁面 `Pages` ： `Home` 、 `Counter` 和 `Fetch data` 。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-115">Browse to each of the app's three pages in the `Pages` folder: `Home`, `Counter`, and `Fetch data`.</span></span> <span data-ttu-id="cb3ef-116">這些頁面是由元件檔案 Razor `Index.razor` 、和所執行 `Counter.razor` `FetchData.razor` 。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-116">These pages are implemented by the Razor component files `Index.razor`, `Counter.razor`, and `FetchData.razor`.</span></span>

1. <span data-ttu-id="cb3ef-117">在 `Counter` 頁面上，選取要在不重新整理頁面的情況下遞增計數器的按鈕。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-117">On the `Counter` page, select the button to increment the counter without a page refresh.</span></span> <span data-ttu-id="cb3ef-118">將網頁中的計數器遞增通常需要撰寫 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-118">Incrementing a counter in a webpage normally requires writing JavaScript.</span></span> <span data-ttu-id="cb3ef-119">使用 Blazor ，您可以改為撰寫 c #。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-119">With Blazor, you can write C# instead.</span></span>

1. <span data-ttu-id="cb3ef-120">檢查檔案 `Counter` 中元件的執行 `Counter.razor` 。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-120">Examine the implementation of the `Counter` component in the `Counter.razor` file.</span></span>

   <span data-ttu-id="cb3ef-121">`Pages/Counter.razor`:</span><span class="sxs-lookup"><span data-stu-id="cb3ef-121">`Pages/Counter.razor`:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="cb3ef-122">`Counter` 元件的 UI 是使用 HTML 來定義的。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-122">The UI of the `Counter` component is defined using HTML.</span></span> <span data-ttu-id="cb3ef-123">動態轉譯邏輯（例如，迴圈、條件、運算式）是使用名為的內嵌 c # 語法加入 [Razor](xref:mvc/views/razor) 。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-123">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="cb3ef-124">HTML 標記和 C# 轉譯邏輯會在組建時轉換為元件類別。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-124">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="cb3ef-125">所產生 .NET 類別的名稱會與檔案名稱相符。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-125">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="cb3ef-126">元件類別的成員均定義於 `@code` 區塊中。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-126">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="cb3ef-127">在 `@code` 區塊中，會指定元件狀態 (屬性、欄位) 來處理事件或定義其他元件邏輯。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-127">In the `@code` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="cb3ef-128">然後會使用這些成員作為元件轉譯邏輯的一部分，並用於處理事件。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-128">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="cb3ef-129">選取 [計數器遞增] 按鈕時：</span><span class="sxs-lookup"><span data-stu-id="cb3ef-129">When the counter increment button is selected:</span></span>

   * <span data-ttu-id="cb3ef-130">會呼叫 `Counter` 元件的已註冊 `onclick` 處理常式 (`IncrementCount` 方法)。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-130">The `Counter` component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="cb3ef-131">`Counter` 元件會重新產生其轉譯樹狀結構。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-131">The `Counter` component regenerates its render tree.</span></span>
   * <span data-ttu-id="cb3ef-132">新的轉譯樹狀結構會與先前的樹狀結構做比較。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-132">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="cb3ef-133">只會套用對「文件物件模型」(DOM) 所做的修改。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-133">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="cb3ef-134">顯示的計數即會更新。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-134">The displayed count is updated.</span></span>

1. <span data-ttu-id="cb3ef-135">修改 `Counter` 元件的 C# 邏輯，讓計數遞增 2 而不是 1。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-135">Modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="cb3ef-136">重建並執行應用程式以查看變更。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-136">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="cb3ef-137">選取按鈕。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-137">Select the button.</span></span> <span data-ttu-id="cb3ef-138">計數器會遞增二。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-138">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="cb3ef-139">使用元件</span><span class="sxs-lookup"><span data-stu-id="cb3ef-139">Use components</span></span>

<span data-ttu-id="cb3ef-140">使用 HTML 語法來將一個元件包含在另一個元件中。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-140">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="cb3ef-141">藉 `Counter` `Index` 由將 `<Counter />` 元素新增至 `Index` 元件（），將元件新增至應用程式的元件 `Index.razor` 。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-141">Add the `Counter` component to the app's `Index` component by adding a `<Counter />` element to the `Index` component (`Index.razor`).</span></span>

   <span data-ttu-id="cb3ef-142">如果您在 Blazor 此體驗中使用 WebAssembly， `SurveyPrompt` 元件會使用元件 `Index` 。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-142">If you're using Blazor WebAssembly for this experience, a `SurveyPrompt` component is used by the `Index` component.</span></span> <span data-ttu-id="cb3ef-143">使用 `<Counter />` 元素取代 `<SurveyPrompt>` 元素。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-143">Replace the `<SurveyPrompt>` element with a `<Counter />` element.</span></span> <span data-ttu-id="cb3ef-144">如果您使用 Blazor 伺服器應用程式來進行此體驗，請將 `<Counter />` 元素新增至 `Index` 元件：</span><span class="sxs-lookup"><span data-stu-id="cb3ef-144">If you're using a Blazor Server app for this experience, add the `<Counter />` element to the `Index` component:</span></span>

   <span data-ttu-id="cb3ef-145">`Pages/Index.razor`:</span><span class="sxs-lookup"><span data-stu-id="cb3ef-145">`Pages/Index.razor`:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="cb3ef-146">重新建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-146">Rebuild and run the app.</span></span> <span data-ttu-id="cb3ef-147">`Index` 元件有自己的計數器。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-147">The `Index` component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="cb3ef-148">元件參數</span><span class="sxs-lookup"><span data-stu-id="cb3ef-148">Component parameters</span></span>

<span data-ttu-id="cb3ef-149">元件也可以有參數。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-149">Components can also have parameters.</span></span> <span data-ttu-id="cb3ef-150">元件參數是在元件類別上使用具有屬性的公用屬性來定義 [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) 。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-150">Component parameters are defined using public properties on the component class with the [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) attribute.</span></span> <span data-ttu-id="cb3ef-151">使用這些屬性來指定標記中元件的引數。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-151">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="cb3ef-152">更新元件的 `@code` c # 程式碼，如下所示：</span><span class="sxs-lookup"><span data-stu-id="cb3ef-152">Update the component's `@code` C# code as follows:</span></span>

   * <span data-ttu-id="cb3ef-153">新增 `IncrementAmount` 具有屬性的公用屬性 [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) 。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-153">Add a public `IncrementAmount` property with the [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) attribute.</span></span>
   * <span data-ttu-id="cb3ef-154">`IncrementCount` `IncrementAmount` 當增加的值時，請將方法變更為使用屬性 `currentCount` 。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-154">Change the `IncrementCount` method to use the `IncrementAmount` property when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="cb3ef-155">`Pages/Counter.razor`:</span><span class="sxs-lookup"><span data-stu-id="cb3ef-155">`Pages/Counter.razor`:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

   <!-- Add back when supported.
       > [!NOTE]
       > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
   -->

1. <span data-ttu-id="cb3ef-156">使用屬性在 `Index` 元件的 `<Counter>` 元素中指定 `IncrementAmount` 參數。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-156">Specify an `IncrementAmount` parameter in the `Index` component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="cb3ef-157">設定值來讓計數器以 10 遞增。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-157">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="cb3ef-158">`Pages/Index.razor`:</span><span class="sxs-lookup"><span data-stu-id="cb3ef-158">`Pages/Index.razor`:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="cb3ef-159">重新載入 `Index` 元件。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-159">Reload the `Index` component.</span></span> <span data-ttu-id="cb3ef-160">每次選取按鈕時，計數器就會遞增10。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-160">The counter increments by ten each time the button is selected.</span></span> <span data-ttu-id="cb3ef-161">`Counter` 元件中的計數器會繼續遞增一。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-161">The counter in the `Counter` component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="cb3ef-162">路由到元件</span><span class="sxs-lookup"><span data-stu-id="cb3ef-162">Route to components</span></span>

<span data-ttu-id="cb3ef-163">檔案 `@page` 頂端的 `Counter.razor` 指示詞指定 `Counter` 元件是路由端點。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-163">The `@page` directive at the top of the `Counter.razor` file specifies that the `Counter` component is a routing endpoint.</span></span> <span data-ttu-id="cb3ef-164">`Counter` 元件會處理傳送給 `/counter` 的要求。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-164">The `Counter` component handles requests sent to `/counter`.</span></span> <span data-ttu-id="cb3ef-165">若沒有 `@page` 指示詞，元件就不會處理路由傳送的要求，但其他元件仍可使用此元件。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-165">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="cb3ef-166">相依性插入</span><span class="sxs-lookup"><span data-stu-id="cb3ef-166">Dependency injection</span></span>

### <a name="blazor-server-experience"></a>Blazor<span data-ttu-id="cb3ef-167">伺服器體驗</span><span class="sxs-lookup"><span data-stu-id="cb3ef-167"> Server experience</span></span>

<span data-ttu-id="cb3ef-168">如果使用 Blazor 伺服器應用程式， `WeatherForecastService` 服務會在中註冊為[單一實例](xref:fundamentals/dependency-injection#service-lifetimes) `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-168">If working with a Blazor Server app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="cb3ef-169">服務的實例可透過相依性[插入（DI）](xref:fundamentals/dependency-injection)在整個應用程式中使用：</span><span class="sxs-lookup"><span data-stu-id="cb3ef-169">An instance of the service is available throughout the app via [dependency injection (DI)](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="cb3ef-170">指示詞 [`@inject`](xref:mvc/views/razor#inject) 是用來將服務的實例插入 `WeatherForecastService` `FetchData` 元件中。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-170">The [`@inject`](xref:mvc/views/razor#inject) directive is used to inject the instance of the `WeatherForecastService` service into the `FetchData` component.</span></span>

<span data-ttu-id="cb3ef-171">`Pages/FetchData.razor`:</span><span class="sxs-lookup"><span data-stu-id="cb3ef-171">`Pages/FetchData.razor`:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="cb3ef-172">`FetchData` 元件會使用插入的服務作為 `ForecastService`，以擷取 `WeatherForecast` 物件的陣列：</span><span class="sxs-lookup"><span data-stu-id="cb3ef-172">The `FetchData` component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

### <a name="blazor-webassembly-experience"></a>Blazor<span data-ttu-id="cb3ef-173">WebAssembly 體驗</span><span class="sxs-lookup"><span data-stu-id="cb3ef-173"> WebAssembly experience</span></span>

<span data-ttu-id="cb3ef-174">如果使用 Blazor WebAssembly 應用程式， <xref:System.Net.Http.HttpClient> 則會插入以從資料夾中的檔案取得氣象預測資料 `weather.json` `wwwroot/sample-data` 。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-174">If working with a Blazor WebAssembly app, <xref:System.Net.Http.HttpClient> is injected to obtain weather forecast data from the `weather.json` file in the `wwwroot/sample-data` folder.</span></span>

<span data-ttu-id="cb3ef-175">`Pages/FetchData.razor`:</span><span class="sxs-lookup"><span data-stu-id="cb3ef-175">`Pages/FetchData.razor`:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-9)]

<span data-ttu-id="cb3ef-176">[`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in)迴圈是用來將每個預測實例轉譯為天氣資料之資料表中的資料列：</span><span class="sxs-lookup"><span data-stu-id="cb3ef-176">An [`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="cb3ef-177">組建待辦事項清單</span><span class="sxs-lookup"><span data-stu-id="cb3ef-177">Build a todo list</span></span>

<span data-ttu-id="cb3ef-178">在實作簡單待辦事項清單的應用程式中新增元件。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-178">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="cb3ef-179">將新的 `Todo` Razor 元件新增至資料夾中的應用程式 `Pages` 。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-179">Add a new `Todo` Razor component to the app in the `Pages` folder.</span></span> <span data-ttu-id="cb3ef-180">如果您使用 Visual Studio，請在資料夾上按一下滑鼠右鍵 `Pages` ， **Add**然後選取 [  >  **新增專案**] [  >  \*\* Razor 元件\*\*]。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-180">If you're using Visual Studio, right-click the `Pages` folder and select **Add** > **New Item** > **Razor Component**.</span></span> <span data-ttu-id="cb3ef-181">為元件的檔案命名 `Todo.razor` 。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-181">Name the component's file `Todo.razor`.</span></span> <span data-ttu-id="cb3ef-182">在其他開發環境中，將空白檔案新增至 `Pages` 名為的資料夾 `Todo.razor` 。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-182">In other development environments, add a blank file to the `Pages` folder named `Todo.razor`.</span></span>

1. <span data-ttu-id="cb3ef-183">提供元件的初始標記：</span><span class="sxs-lookup"><span data-stu-id="cb3ef-183">Provide the initial markup for the component:</span></span>

   ```razor
   @page "/todo"

   <h3>Todo</h3>
   ```

1. <span data-ttu-id="cb3ef-184">將 `Todo` 元件新增至導覽列。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-184">Add the `Todo` component to the navigation bar.</span></span>

   <span data-ttu-id="cb3ef-185">`NavMenu`元件（ `Shared/NavMenu.razor` ）會用於應用程式的版面配置。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-185">The `NavMenu` component (`Shared/NavMenu.razor`) is used in the app's layout.</span></span> <span data-ttu-id="cb3ef-186">版面配置是可讓您避免應用程式中內容重複的元件。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-186">Layouts are components that allow you to avoid duplication of content in the app.</span></span>

   <span data-ttu-id="cb3ef-187">新增 `<NavLink>` 元件的專案， `Todo` 方法是在檔案中的現有清單專案下方新增下列清單專案標記 `Shared/NavMenu.razor` ：</span><span class="sxs-lookup"><span data-stu-id="cb3ef-187">Add a `<NavLink>` element for the `Todo` component by adding the following list item markup below the existing list items in the `Shared/NavMenu.razor` file:</span></span>

   ```razor
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="cb3ef-188">重新建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-188">Rebuild and run the app.</span></span> <span data-ttu-id="cb3ef-189">瀏覽新的 [待辦事項] 頁面，以確認 `Todo` 元件的連結可以運作。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-189">Visit the new Todo page to confirm that the link to the `Todo` component works.</span></span>

1. <span data-ttu-id="cb3ef-190">將檔案新增 `TodoItem.cs` 至專案的根目錄，以保存代表待辦事項專案的類別。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-190">Add a `TodoItem.cs` file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="cb3ef-191">請使用下列 `TodoItem` 類別的 C# 程式碼：</span><span class="sxs-lookup"><span data-stu-id="cb3ef-191">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="cb3ef-192">回到 `Todo` 元件（ `Pages/Todo.razor` ）：</span><span class="sxs-lookup"><span data-stu-id="cb3ef-192">Return to the `Todo` component (`Pages/Todo.razor`):</span></span>

   * <span data-ttu-id="cb3ef-193">在 `@code` 區塊中新增待辦事項的欄位。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-193">Add a field for the todo items in an `@code` block.</span></span> <span data-ttu-id="cb3ef-194">`Todo` 元件會使用此欄位來維護待辦事項清單的狀態。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-194">The `Todo` component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="cb3ef-195">新增未排序的清單標記和 `foreach` 迴圈，將每個待辦事項轉譯為清單項目 (`<li>`)。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-195">Add unordered list markup and a `foreach` loop to render each todo item as a list item (`<li>`).</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="cb3ef-196">應用程式需要 UI 元素，才能將待辦事項新增至清單。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-196">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="cb3ef-197">在未排序清單 (`<ul>...</ul>`) 下方新增文字輸出 (`<input>`) 與按鈕 (`<button>`)：</span><span class="sxs-lookup"><span data-stu-id="cb3ef-197">Add a text input (`<input>`) and a button (`<button>`) below the unordered list (`<ul>...</ul>`):</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="cb3ef-198">重新建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-198">Rebuild and run the app.</span></span> <span data-ttu-id="cb3ef-199">**`Add todo`** 選取按鈕時，不會發生任何事，因為事件處理常式未連接到按鈕。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-199">When the **`Add todo`** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="cb3ef-200">將 `AddTodo` 方法新增至 `Todo` 元件並註冊，以便使用 `@onclick` 屬性來進行按鈕選取。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-200">Add an `AddTodo` method to the `Todo` component and register it for button selections using the `@onclick` attribute.</span></span> <span data-ttu-id="cb3ef-201">當選取按鈕時，就會呼叫 `AddTodo` C# 方法：</span><span class="sxs-lookup"><span data-stu-id="cb3ef-201">The `AddTodo` C# method is called when the button is selected:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. <span data-ttu-id="cb3ef-202">若要取得新待辦事項的標題，請在 `@code` 區塊頂端新增 `newTodo` 字串欄位，然後使用 `<input>` 元素中的 `bind` 屬性將它繫結至文字輸入的值：</span><span class="sxs-lookup"><span data-stu-id="cb3ef-202">To get the title of the new todo item, add a `newTodo` string field at the top of the `@code` block and bind it to the value of the text input using the `bind` attribute in the `<input>` element:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```razor
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. <span data-ttu-id="cb3ef-203">更新 `AddTodo` 方法，將 `TodoItem` 與指定的標題新增至清單。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-203">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="cb3ef-204">請將 `newTodo` 設定為空字串，以清除文字輸入的值：</span><span class="sxs-lookup"><span data-stu-id="cb3ef-204">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="cb3ef-205">重新建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-205">Rebuild and run the app.</span></span> <span data-ttu-id="cb3ef-206">請將一些待辦事項新增至待辦事項清單，以測試新程式碼。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-206">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="cb3ef-207">每個待辦事項的標題文字都可設定為可編輯，而核取方塊則可協助使用者記錄已完成的項目。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-207">The title text for each todo item can be made editable, and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="cb3ef-208">請為每個待辦事項新增核取方塊輸入，然後將其值繫結至 `IsDone` 屬性。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-208">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="cb3ef-209">將 `@todo.Title` 變更為繫結至 `@todo.Title` 的 `<input>` 元素：</span><span class="sxs-lookup"><span data-stu-id="cb3ef-209">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="cb3ef-210">若要確認是否已繫結這些值，請更新 `<h3>` 標頭，以顯示未完成之待辦事項 (`IsDone` 為 `false`) 的數目計數。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-210">To verify that these values are bound, update the `<h3>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```razor
   <h3>Todo (@todos.Count(todo => !todo.IsDone))</h3>
   ```

1. <span data-ttu-id="cb3ef-211">完成的 `Todo` 元件（ `Pages/Todo.razor` ）：</span><span class="sxs-lookup"><span data-stu-id="cb3ef-211">The completed `Todo` component (`Pages/Todo.razor`):</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="cb3ef-212">重新建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-212">Rebuild and run the app.</span></span> <span data-ttu-id="cb3ef-213">請新增待辦事項，以測試新程式碼。</span><span class="sxs-lookup"><span data-stu-id="cb3ef-213">Add todo items to test the new code.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb3ef-214">後續步驟</span><span class="sxs-lookup"><span data-stu-id="cb3ef-214">Next steps</span></span>

<span data-ttu-id="cb3ef-215">在本教學課程中，您已了解如何：</span><span class="sxs-lookup"><span data-stu-id="cb3ef-215">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cb3ef-216">建立待辦事項清單 Blazor 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="cb3ef-216">Create a todo list Blazor app project</span></span>
> * <span data-ttu-id="cb3ef-217">修改 Razor 元件</span><span class="sxs-lookup"><span data-stu-id="cb3ef-217">Modify Razor components</span></span>
> * <span data-ttu-id="cb3ef-218">在元件中使用事件處理和資料系結</span><span class="sxs-lookup"><span data-stu-id="cb3ef-218">Use event handling and data binding in components</span></span>
> * <span data-ttu-id="cb3ef-219">在應用程式中使用相依性插入（DI）和路由 Blazor</span><span class="sxs-lookup"><span data-stu-id="cb3ef-219">Use dependency injection (DI) and routing in a Blazor app</span></span>

<span data-ttu-id="cb3ef-220">瞭解如何建立和使用元件：</span><span class="sxs-lookup"><span data-stu-id="cb3ef-220">Learn how to build and use components:</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/components/index>
