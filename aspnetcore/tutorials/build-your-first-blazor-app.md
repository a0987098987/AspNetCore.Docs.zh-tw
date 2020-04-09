---
title: 建置第Blazor一個應用程式
author: guardrex
description: 逐步Blazor構建應用。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/20/2020
no-loc:
- Blazor
uid: tutorials/first-blazor-app
ms.openlocfilehash: 138057c2ceb9ed01bdf958c01f5cf2275387df23
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "79989438"
---
# <a name="build-your-first-opno-locblazor-app"></a><span data-ttu-id="22880-103">建置第Blazor一個應用程式</span><span class="sxs-lookup"><span data-stu-id="22880-103">Build your first Blazor app</span></span>

<span data-ttu-id="22880-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="22880-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="22880-105">本教程演示如何構建和修改Blazor應用。</span><span class="sxs-lookup"><span data-stu-id="22880-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

## <a name="build-components"></a><span data-ttu-id="22880-106">組建元件</span><span class="sxs-lookup"><span data-stu-id="22880-106">Build components</span></span>

1. <span data-ttu-id="22880-107">按照本文中的<xref:blazor/get-started>指南為本教程創建Blazor專案。</span><span class="sxs-lookup"><span data-stu-id="22880-107">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span> <span data-ttu-id="22880-108">將專案命名為 *ToDoList*。</span><span class="sxs-lookup"><span data-stu-id="22880-108">Name the project *ToDoList*.</span></span>

1. <span data-ttu-id="22880-109">瀏覽到 *「主頁」* 資料夾中應用的三個頁面:主頁、計數器和提取數據。</span><span class="sxs-lookup"><span data-stu-id="22880-109">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="22880-110">這些頁面會透過 Razor 元件檔案 *Index.razor*、*Counter.razor* 及 *FetchData.razor* 來實作。</span><span class="sxs-lookup"><span data-stu-id="22880-110">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="22880-111">在 [計數器] 頁面上，選取 [按我]\*\*\*\* 按鈕以在不重新整理頁面的情況下讓計數器遞增。</span><span class="sxs-lookup"><span data-stu-id="22880-111">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="22880-112">在網頁中增加計數器通常需要編寫 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="22880-112">Incrementing a counter in a webpage normally requires writing JavaScript.</span></span> <span data-ttu-id="22880-113">使用Blazor可以改為編寫 C#。</span><span class="sxs-lookup"><span data-stu-id="22880-113">With Blazor, you can write C# instead.</span></span>

1. <span data-ttu-id="22880-114">檢查 \*Counter.razor`Counter` 檔案中 \* 元件的實作。</span><span class="sxs-lookup"><span data-stu-id="22880-114">Examine the implementation of the `Counter` component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="22880-115">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="22880-115">*Pages/Counter.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="22880-116">`Counter` 元件的 UI 是使用 HTML 來定義的。</span><span class="sxs-lookup"><span data-stu-id="22880-116">The UI of the `Counter` component is defined using HTML.</span></span> <span data-ttu-id="22880-117">動態轉譯邏輯 (例如迴圈、條件、運算式) 是使用內嵌的 C# 語法 (稱為 [Razor](xref:mvc/views/razor)) 來新增的。</span><span class="sxs-lookup"><span data-stu-id="22880-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="22880-118">HTML 標記和 C# 轉譯邏輯會在組建時轉換為元件類別。</span><span class="sxs-lookup"><span data-stu-id="22880-118">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="22880-119">所產生 .NET 類別的名稱會與檔案名稱相符。</span><span class="sxs-lookup"><span data-stu-id="22880-119">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="22880-120">元件類別的成員均定義於 `@code` 區塊中。</span><span class="sxs-lookup"><span data-stu-id="22880-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="22880-121">在 `@code` 區塊中，會指定元件狀態 (屬性、欄位) 來處理事件或定義其他元件邏輯。</span><span class="sxs-lookup"><span data-stu-id="22880-121">In the `@code` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="22880-122">然後會使用這些成員作為元件轉譯邏輯的一部分，並用於處理事件。</span><span class="sxs-lookup"><span data-stu-id="22880-122">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="22880-123">選取 [Click me] \(按我\)\*\*\*\* 按鈕時：</span><span class="sxs-lookup"><span data-stu-id="22880-123">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="22880-124">會呼叫 `Counter` 元件的已註冊 `onclick` 處理常式 (`IncrementCount` 方法)。</span><span class="sxs-lookup"><span data-stu-id="22880-124">The `Counter` component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="22880-125">`Counter` 元件會重新產生其轉譯樹狀結構。</span><span class="sxs-lookup"><span data-stu-id="22880-125">The `Counter` component regenerates its render tree.</span></span>
   * <span data-ttu-id="22880-126">新的轉譯樹狀結構會與先前的樹狀結構做比較。</span><span class="sxs-lookup"><span data-stu-id="22880-126">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="22880-127">只會套用對「文件物件模型」(DOM) 所做的修改。</span><span class="sxs-lookup"><span data-stu-id="22880-127">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="22880-128">顯示的計數即會更新。</span><span class="sxs-lookup"><span data-stu-id="22880-128">The displayed count is updated.</span></span>

1. <span data-ttu-id="22880-129">修改 `Counter` 元件的 C# 邏輯，讓計數遞增 2 而不是 1。</span><span class="sxs-lookup"><span data-stu-id="22880-129">Modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="22880-130">重建並執行應用程式以查看變更。</span><span class="sxs-lookup"><span data-stu-id="22880-130">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="22880-131">選取 [按我]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="22880-131">Select the **Click me** button.</span></span> <span data-ttu-id="22880-132">計數器會遞增二。</span><span class="sxs-lookup"><span data-stu-id="22880-132">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="22880-133">使用元件</span><span class="sxs-lookup"><span data-stu-id="22880-133">Use components</span></span>

<span data-ttu-id="22880-134">使用 HTML 語法來將一個元件包含在另一個元件中。</span><span class="sxs-lookup"><span data-stu-id="22880-134">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="22880-135">透過將 `<Counter />` 元素新增至 `Index` 元件 (*Index.razor*)，來將 `Counter` 元件新增至應用程式的 `Index` 元件。</span><span class="sxs-lookup"><span data-stu-id="22880-135">Add the `Counter` component to the app's `Index` component by adding a `<Counter />` element to the `Index` component (*Index.razor*).</span></span>

   <span data-ttu-id="22880-136">如果您正在此體驗中使用BlazorWebAssembly,`Index`則元件將`SurveyPrompt`使用 元件。</span><span class="sxs-lookup"><span data-stu-id="22880-136">If you're using Blazor WebAssembly for this experience, a `SurveyPrompt` component is used by the `Index` component.</span></span> <span data-ttu-id="22880-137">使用 `<Counter />` 元素取代 `<SurveyPrompt>` 元素。</span><span class="sxs-lookup"><span data-stu-id="22880-137">Replace the `<SurveyPrompt>` element with a `<Counter />` element.</span></span> <span data-ttu-id="22880-138">如果您使用此Blazor伺服器套用的此體驗,則將`<Counter />`元素加入元件`Index`:</span><span class="sxs-lookup"><span data-stu-id="22880-138">If you're using a Blazor Server app for this experience, add the `<Counter />` element to the `Index` component:</span></span>

   <span data-ttu-id="22880-139">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="22880-139">*Pages/Index.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="22880-140">重新建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="22880-140">Rebuild and run the app.</span></span> <span data-ttu-id="22880-141">`Index` 元件有自己的計數器。</span><span class="sxs-lookup"><span data-stu-id="22880-141">The `Index` component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="22880-142">元件參數</span><span class="sxs-lookup"><span data-stu-id="22880-142">Component parameters</span></span>

<span data-ttu-id="22880-143">元件也可以有參數。</span><span class="sxs-lookup"><span data-stu-id="22880-143">Components can also have parameters.</span></span> <span data-ttu-id="22880-144">元件參數使用具有`[Parameter]`屬性的元件類上的公共屬性進行定義。</span><span class="sxs-lookup"><span data-stu-id="22880-144">Component parameters are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="22880-145">使用這些屬性來指定標記中元件的引數。</span><span class="sxs-lookup"><span data-stu-id="22880-145">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="22880-146">更新元件的`@code`C# 代碼,如下所示:</span><span class="sxs-lookup"><span data-stu-id="22880-146">Update the component's `@code` C# code as follows:</span></span>

   * <span data-ttu-id="22880-147">使用`[Parameter]`屬性`IncrementAmount`添加公共屬性。</span><span class="sxs-lookup"><span data-stu-id="22880-147">Add a public `IncrementAmount` property with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="22880-148">增加`IncrementCount`的值 時`IncrementAmount`, 將方法更改`currentCount`為使用 屬性。</span><span class="sxs-lookup"><span data-stu-id="22880-148">Change the `IncrementCount` method to use the `IncrementAmount` property when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="22880-149">*Pages/Counter.razor*：</span><span class="sxs-lookup"><span data-stu-id="22880-149">*Pages/Counter.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

   <!-- Add back when supported.
       > [!NOTE]
       > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
   -->

1. <span data-ttu-id="22880-150">使用屬性在 `Index` 元件的 `<Counter>` 元素中指定 `IncrementAmount` 參數。</span><span class="sxs-lookup"><span data-stu-id="22880-150">Specify an `IncrementAmount` parameter in the `Index` component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="22880-151">設定值來讓計數器以 10 遞增。</span><span class="sxs-lookup"><span data-stu-id="22880-151">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="22880-152">*Pages/Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="22880-152">*Pages/Index.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="22880-153">重新載入 `Index` 元件。</span><span class="sxs-lookup"><span data-stu-id="22880-153">Reload the `Index` component.</span></span> <span data-ttu-id="22880-154">每次選取 [Click me] \(按我\)\*\*\*\* 按鈕時，計數器都會以 10 遞增。</span><span class="sxs-lookup"><span data-stu-id="22880-154">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="22880-155">`Counter` 元件中的計數器會繼續遞增一。</span><span class="sxs-lookup"><span data-stu-id="22880-155">The counter in the `Counter` component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="22880-156">路由到元件</span><span class="sxs-lookup"><span data-stu-id="22880-156">Route to components</span></span>

<span data-ttu-id="22880-157">*Counter.razor* 檔案頂端的 `@page` 指示詞會指定 `Counter` 元件是路由端點。</span><span class="sxs-lookup"><span data-stu-id="22880-157">The `@page` directive at the top of the *Counter.razor* file specifies that the `Counter` component is a routing endpoint.</span></span> <span data-ttu-id="22880-158">`Counter` 元件會處理傳送給 `/counter` 的要求。</span><span class="sxs-lookup"><span data-stu-id="22880-158">The `Counter` component handles requests sent to `/counter`.</span></span> <span data-ttu-id="22880-159">若沒有 `@page` 指示詞，元件就不會處理路由傳送的要求，但其他元件仍可使用此元件。</span><span class="sxs-lookup"><span data-stu-id="22880-159">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="22880-160">相依性插入</span><span class="sxs-lookup"><span data-stu-id="22880-160">Dependency injection</span></span>

### <a name="opno-locblazor-server-experience"></a>Blazor<span data-ttu-id="22880-161">伺服器體驗</span><span class="sxs-lookup"><span data-stu-id="22880-161"> Server experience</span></span>

<span data-ttu-id="22880-162">如果使用Blazor伺服器應用,則該`WeatherForecastService`服務`Startup.ConfigureServices`在中 註冊為[單例](xref:fundamentals/dependency-injection#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="22880-162">If working with a Blazor Server app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="22880-163">該服務的實體可透過[相依相依 (DI)](xref:fundamentals/dependency-injection)在整個應用中可用:</span><span class="sxs-lookup"><span data-stu-id="22880-163">An instance of the service is available throughout the app via [dependency injection (DI)](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="22880-164">這個`@inject`指令用於將`WeatherForecastService`服務的實體注入到元件中`FetchData`。</span><span class="sxs-lookup"><span data-stu-id="22880-164">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the `FetchData` component.</span></span>

<span data-ttu-id="22880-165">*Pages/FetchData.razor*：</span><span class="sxs-lookup"><span data-stu-id="22880-165">*Pages/FetchData.razor*:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="22880-166">`FetchData` 元件會使用插入的服務作為 `ForecastService`，以擷取 `WeatherForecast` 物件的陣列：</span><span class="sxs-lookup"><span data-stu-id="22880-166">The `FetchData` component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

### <a name="opno-locblazor-webassembly-experience"></a>Blazor<span data-ttu-id="22880-167">網路組裝體驗</span><span class="sxs-lookup"><span data-stu-id="22880-167"> WebAssembly experience</span></span>

<span data-ttu-id="22880-168">如果使用BlazorWebAssembly`HttpClient`應用, 則注入以從*wwwroot/範例資料*資料夾中的*weather.json*檔中獲取天氣預報數據。</span><span class="sxs-lookup"><span data-stu-id="22880-168">If working with a Blazor WebAssembly app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder.</span></span>

<span data-ttu-id="22880-169">*Pages/FetchData.razor*：</span><span class="sxs-lookup"><span data-stu-id="22880-169">*Pages/FetchData.razor*:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="22880-170">循環[`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in)將每個預測實體呈現為天氣資料表中的行:</span><span class="sxs-lookup"><span data-stu-id="22880-170">An [`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="22880-171">組建待辦事項清單</span><span class="sxs-lookup"><span data-stu-id="22880-171">Build a todo list</span></span>

<span data-ttu-id="22880-172">在實作簡單待辦事項清單的應用程式中新增元件。</span><span class="sxs-lookup"><span data-stu-id="22880-172">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="22880-173">向`Todo`*「主頁」* 資料夾中的應用添加新的 Razor 元件。</span><span class="sxs-lookup"><span data-stu-id="22880-173">Add a new `Todo` Razor component to the app in the *Pages* folder.</span></span> <span data-ttu-id="22880-174">在可視化工作室中,右鍵按**下 「頁面」** 資料夾**Add** > ,然後選擇「**添加新專案** > **剃刀元件**」。。</span><span class="sxs-lookup"><span data-stu-id="22880-174">In Visual Studio, right-click the **Pages** folder and select **Add** > **New Item** > **Razor Component**.</span></span> <span data-ttu-id="22880-175">命名元件的檔案*Todo.razor*。</span><span class="sxs-lookup"><span data-stu-id="22880-175">Name the component's file *Todo.razor*.</span></span> <span data-ttu-id="22880-176">在其他開發環境中,向名為*Todo.razor*的**Pages**資料夾添加空白檔。</span><span class="sxs-lookup"><span data-stu-id="22880-176">In other development environments, add a blank file to the **Pages** folder named *Todo.razor*.</span></span>

1. <span data-ttu-id="22880-177">提供元件的初始標記：</span><span class="sxs-lookup"><span data-stu-id="22880-177">Provide the initial markup for the component:</span></span>

   ```razor
   @page "/todo"

   <h3>Todo</h3>
   ```

1. <span data-ttu-id="22880-178">將 `Todo` 元件新增至導覽列。</span><span class="sxs-lookup"><span data-stu-id="22880-178">Add the `Todo` component to the navigation bar.</span></span>

   <span data-ttu-id="22880-179">`NavMenu` 元件 (*Shared/NavMenu.razor*) 會用於應用程式的版面配置。</span><span class="sxs-lookup"><span data-stu-id="22880-179">The `NavMenu` component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="22880-180">版面配置是可讓您避免應用程式中內容重複的元件。</span><span class="sxs-lookup"><span data-stu-id="22880-180">Layouts are components that allow you to avoid duplication of content in the app.</span></span>

   <span data-ttu-id="22880-181">透過在 *Shared/NavMenu.razor* 檔案中的現有清單項目下方新增下列清單項目標記，為 `Todo` 元件新增 `<NavLink>` 元素：</span><span class="sxs-lookup"><span data-stu-id="22880-181">Add a `<NavLink>` element for the `Todo` component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```razor
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="22880-182">重新建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="22880-182">Rebuild and run the app.</span></span> <span data-ttu-id="22880-183">瀏覽新的 [待辦事項] 頁面，以確認 `Todo` 元件的連結可以運作。</span><span class="sxs-lookup"><span data-stu-id="22880-183">Visit the new Todo page to confirm that the link to the `Todo` component works.</span></span>

1. <span data-ttu-id="22880-184">將 *TodoItem.cs* 檔案新增至專案的根目錄，以保存代表待辦事項的類別。</span><span class="sxs-lookup"><span data-stu-id="22880-184">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="22880-185">請使用下列 `TodoItem` 類別的 C# 程式碼：</span><span class="sxs-lookup"><span data-stu-id="22880-185">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="22880-186">返回 `Todo` 元件 (*Pages/Todo.razor*)：</span><span class="sxs-lookup"><span data-stu-id="22880-186">Return to the `Todo` component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="22880-187">在 `@code` 區塊中新增待辦事項的欄位。</span><span class="sxs-lookup"><span data-stu-id="22880-187">Add a field for the todo items in an `@code` block.</span></span> <span data-ttu-id="22880-188">`Todo` 元件會使用此欄位來維護待辦事項清單的狀態。</span><span class="sxs-lookup"><span data-stu-id="22880-188">The `Todo` component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="22880-189">新增未排序的清單標記和 `foreach` 迴圈，將每個待辦事項轉譯為清單項目 (`<li>`)。</span><span class="sxs-lookup"><span data-stu-id="22880-189">Add unordered list markup and a `foreach` loop to render each todo item as a list item (`<li>`).</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="22880-190">應用程式需要 UI 元素，才能將待辦事項新增至清單。</span><span class="sxs-lookup"><span data-stu-id="22880-190">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="22880-191">在未排序清單 (`<ul>...</ul>`) 下方新增文字輸出 (`<input>`) 與按鈕 (`<button>`)：</span><span class="sxs-lookup"><span data-stu-id="22880-191">Add a text input (`<input>`) and a button (`<button>`) below the unordered list (`<ul>...</ul>`):</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="22880-192">重新建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="22880-192">Rebuild and run the app.</span></span> <span data-ttu-id="22880-193">選取 [Add todo] \(新增待辦事項\)\*\*\*\* 按鈕時，不會發生任何情況，因為事件處理常式並未連接至這個按鈕。</span><span class="sxs-lookup"><span data-stu-id="22880-193">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="22880-194">將 `AddTodo` 方法新增至 `Todo` 元件並註冊，以便使用 `@onclick` 屬性來進行按鈕選取。</span><span class="sxs-lookup"><span data-stu-id="22880-194">Add an `AddTodo` method to the `Todo` component and register it for button selections using the `@onclick` attribute.</span></span> <span data-ttu-id="22880-195">當選取按鈕時，就會呼叫 `AddTodo` C# 方法：</span><span class="sxs-lookup"><span data-stu-id="22880-195">The `AddTodo` C# method is called when the button is selected:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. <span data-ttu-id="22880-196">若要取得新待辦事項的標題，請在 `@code` 區塊頂端新增 `newTodo` 字串欄位，然後使用 `<input>` 元素中的 `bind` 屬性將它繫結至文字輸入的值：</span><span class="sxs-lookup"><span data-stu-id="22880-196">To get the title of the new todo item, add a `newTodo` string field at the top of the `@code` block and bind it to the value of the text input using the `bind` attribute in the `<input>` element:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```razor
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. <span data-ttu-id="22880-197">更新 `AddTodo` 方法，將 `TodoItem` 與指定的標題新增至清單。</span><span class="sxs-lookup"><span data-stu-id="22880-197">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="22880-198">請將 `newTodo` 設定為空字串，以清除文字輸入的值：</span><span class="sxs-lookup"><span data-stu-id="22880-198">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="22880-199">重新建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="22880-199">Rebuild and run the app.</span></span> <span data-ttu-id="22880-200">請將一些待辦事項新增至待辦事項清單，以測試新程式碼。</span><span class="sxs-lookup"><span data-stu-id="22880-200">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="22880-201">每個待辦事項的標題文字都可設定為可編輯，而核取方塊則可協助使用者記錄已完成的項目。</span><span class="sxs-lookup"><span data-stu-id="22880-201">The title text for each todo item can be made editable, and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="22880-202">請為每個待辦事項新增核取方塊輸入，然後將其值繫結至 `IsDone` 屬性。</span><span class="sxs-lookup"><span data-stu-id="22880-202">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="22880-203">將 `@todo.Title` 變更為繫結至 `@todo.Title` 的 `<input>` 元素：</span><span class="sxs-lookup"><span data-stu-id="22880-203">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="22880-204">若要確認是否已繫結這些值，請更新 `<h3>` 標頭，以顯示未完成之待辦事項 (`IsDone` 為 `false`) 的數目計數。</span><span class="sxs-lookup"><span data-stu-id="22880-204">To verify that these values are bound, update the `<h3>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```razor
   <h3>Todo (@todos.Count(todo => !todo.IsDone))</h3>
   ```

1. <span data-ttu-id="22880-205">已完成的 `Todo` 元件 (*Pages/Todo.razor*)：</span><span class="sxs-lookup"><span data-stu-id="22880-205">The completed `Todo` component (*Pages/Todo.razor*):</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="22880-206">重新建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="22880-206">Rebuild and run the app.</span></span> <span data-ttu-id="22880-207">請新增待辦事項，以測試新程式碼。</span><span class="sxs-lookup"><span data-stu-id="22880-207">Add todo items to test the new code.</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/components>
