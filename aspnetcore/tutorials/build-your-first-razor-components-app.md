---
title: 建置您的第一個 Razor 元件應用程式
author: guardrex
description: 逐步建置 Razor 元件應用程式，並了解基本 Razor 元件概念。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 0c3dd2366581d73bad44e2911602e13c6c0daf9a
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159339"
---
# <a name="build-your-first-razor-components-app"></a><span data-ttu-id="53cb9-103">建置您的第一個 Razor 元件應用程式</span><span class="sxs-lookup"><span data-stu-id="53cb9-103">Build your first Razor Components app</span></span>

<span data-ttu-id="53cb9-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="53cb9-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="53cb9-105">本教學課程說明如何使用 Razor 元件來建置應用程式，並示範基本 Razor 元件概念。</span><span class="sxs-lookup"><span data-stu-id="53cb9-105">This tutorial shows you how to build an app with Razor Components and demonstrates basic Razor Components concepts.</span></span> <span data-ttu-id="53cb9-106">您可以使用 Razor 元件型專案 (.NET Core 3.0 或更新版本中可支援) 或使用 Blazor 型專案 (.NET Core 未來的版本中可支援) 來進行此教學課程。</span><span class="sxs-lookup"><span data-stu-id="53cb9-106">You can enjoy this tutorial using either a Razor Components-based project (supported in .NET Core 3.0 or later) or using a Blazor-based project (supported in a future release of .NET Core).</span></span>

<span data-ttu-id="53cb9-107">針對使用 ASP.NET Core Razor 元件 (建議使用) 的體驗：</span><span class="sxs-lookup"><span data-stu-id="53cb9-107">For an experience using ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="53cb9-108">依照 <xref:razor-components/get-started> 中的指引來建立 Razor 元件型專案。</span><span class="sxs-lookup"><span data-stu-id="53cb9-108">Follow the guidance in <xref:razor-components/get-started> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="53cb9-109">將專案命名為 `RazorComponents`。</span><span class="sxs-lookup"><span data-stu-id="53cb9-109">Name the project `RazorComponents`.</span></span>
* <span data-ttu-id="53cb9-110">這會從 Razor 元件範本建立一個多專案解決方案。</span><span class="sxs-lookup"><span data-stu-id="53cb9-110">A multi-project solution is created from the Razor Components template.</span></span> <span data-ttu-id="53cb9-111">Razor 元件會以 *RazorComponents.App* 的形式產生。</span><span class="sxs-lookup"><span data-stu-id="53cb9-111">The Razor Components project is generated as *RazorComponents.App*.</span></span>

<span data-ttu-id="53cb9-112">針對使用 Blazor 的體驗：</span><span class="sxs-lookup"><span data-stu-id="53cb9-112">For an experience using Blazor:</span></span>

* <span data-ttu-id="53cb9-113">依照 <xref:spa/blazor/get-started> 中的指引來建立 Blazor 型專案。</span><span class="sxs-lookup"><span data-stu-id="53cb9-113">Follow the guidance in <xref:spa/blazor/get-started> to create a Blazor-based project.</span></span>
* <span data-ttu-id="53cb9-114">將專案命名為 `Blazor`。</span><span class="sxs-lookup"><span data-stu-id="53cb9-114">Name the project `Blazor`.</span></span>
* <span data-ttu-id="53cb9-115">這會從 Blazor 範本建立一個單一專案解決方案。</span><span class="sxs-lookup"><span data-stu-id="53cb9-115">A single-project solution is created from the Blazor template.</span></span>

<span data-ttu-id="53cb9-116">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="53cb9-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="53cb9-117">如需了解先決條件，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="53cb9-117">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="53cb9-118">組建元件</span><span class="sxs-lookup"><span data-stu-id="53cb9-118">Build components</span></span>

1. <span data-ttu-id="53cb9-119">瀏覽至每個應用程式的三個頁面：首頁、計數器和擷取資料。</span><span class="sxs-lookup"><span data-stu-id="53cb9-119">Browse to each of the app's three pages: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="53cb9-120">這些頁面會由 *Pages* 資料夾中的 Razor 檔案實作：*Index.cshtml*、*Counter.cshtml* 和 *FetchData.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="53cb9-120">These pages are implemented by Razor files in the *Pages* folder: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.</span></span>

1. <span data-ttu-id="53cb9-121">在 [計數器] 頁面上，選取 [按我] 按鈕以在不重新整理頁面的情況下讓計數器遞增。</span><span class="sxs-lookup"><span data-stu-id="53cb9-121">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="53cb9-122">讓網頁中的計數器遞增通常需要撰寫 JavaScript，但 Razor 元件提供一個使用 C# 的更好方法。</span><span class="sxs-lookup"><span data-stu-id="53cb9-122">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

1. <span data-ttu-id="53cb9-123">檢查 *Counter.cshtml* 檔案中「計數器」元件的實作。</span><span class="sxs-lookup"><span data-stu-id="53cb9-123">Examine the implementation of the Counter component in the *Counter.cshtml* file.</span></span>

   <span data-ttu-id="53cb9-124">*Pages/Counter.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="53cb9-124">*Pages/Counter.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.cshtml)]

   <span data-ttu-id="53cb9-125">「計數器」元件的 UI 是使用 HTML 來定義的。</span><span class="sxs-lookup"><span data-stu-id="53cb9-125">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="53cb9-126">動態轉譯邏輯 (例如迴圈、條件、運算式) 是使用內嵌的 C# 語法 (稱為 [Razor](xref:mvc/views/razor)) 來新增的。</span><span class="sxs-lookup"><span data-stu-id="53cb9-126">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="53cb9-127">HTML 標記和 C# 轉譯邏輯會在組建時轉換為元件類別。</span><span class="sxs-lookup"><span data-stu-id="53cb9-127">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="53cb9-128">所產生 .NET 類別的名稱會與檔案名稱相符。</span><span class="sxs-lookup"><span data-stu-id="53cb9-128">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="53cb9-129">元件類別的成員定義在 `@functions` 區塊中。</span><span class="sxs-lookup"><span data-stu-id="53cb9-129">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="53cb9-130">在 `@functions` 區塊中，會指定元件狀態 (屬性、欄位) 來處理事件或定義其他元件邏輯。</span><span class="sxs-lookup"><span data-stu-id="53cb9-130">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="53cb9-131">然後會使用這些成員作為元件轉譯邏輯的一部分，並用於處理事件。</span><span class="sxs-lookup"><span data-stu-id="53cb9-131">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="53cb9-132">選取 [Click me] \(按我\) 按鈕時：</span><span class="sxs-lookup"><span data-stu-id="53cb9-132">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="53cb9-133">會呼叫「計數器」元件的已註冊 `onclick` 處理常式 (`IncrementCount` 方法)。</span><span class="sxs-lookup"><span data-stu-id="53cb9-133">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="53cb9-134">「計數器」元件會重新產生其轉譯樹狀結構。</span><span class="sxs-lookup"><span data-stu-id="53cb9-134">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="53cb9-135">新的轉譯樹狀結構會與先前的樹狀結構做比較。</span><span class="sxs-lookup"><span data-stu-id="53cb9-135">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="53cb9-136">只會套用對「文件物件模型」(DOM) 所做的修改。</span><span class="sxs-lookup"><span data-stu-id="53cb9-136">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="53cb9-137">顯示的計數即會更新。</span><span class="sxs-lookup"><span data-stu-id="53cb9-137">The displayed count is updated.</span></span>

1. <span data-ttu-id="53cb9-138">修改「計數器」元件的 C# 邏輯，讓計數以 2 遞增而不是 1。</span><span class="sxs-lookup"><span data-stu-id="53cb9-138">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.cshtml?highlight=14)]

1. <span data-ttu-id="53cb9-139">重建並執行應用程式以查看變更。</span><span class="sxs-lookup"><span data-stu-id="53cb9-139">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="53cb9-140">選取 [Click me] \(按我\) 按鈕，計數器就會以 2 遞增。</span><span class="sxs-lookup"><span data-stu-id="53cb9-140">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="53cb9-141">使用元件</span><span class="sxs-lookup"><span data-stu-id="53cb9-141">Use components</span></span>

<span data-ttu-id="53cb9-142">使用類似 HTML 的語法將一個元件包含在另一個元件中。</span><span class="sxs-lookup"><span data-stu-id="53cb9-142">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="53cb9-143">藉由將 `<Counter />` 元素新增至「索引」元件，將「計數器」元件新增至應用程式的「索引」(首頁) 元件。</span><span class="sxs-lookup"><span data-stu-id="53cb9-143">Add the Counter component to the app's Index (home page) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="53cb9-144">如果您使用 Blazor 來進行此體驗，則「索引」元件中會有「問卷提示」元件 (`<SurveyPrompt>` 元素)。</span><span class="sxs-lookup"><span data-stu-id="53cb9-144">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="53cb9-145">請以 `<Counter>` 元素取代 `<SurveyPrompt>` 元素。</span><span class="sxs-lookup"><span data-stu-id="53cb9-145">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="53cb9-146">*Pages/Index.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="53cb9-146">*Pages/Index.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.cshtml?highlight=7)]

1. <span data-ttu-id="53cb9-147">重建並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="53cb9-147">Rebuild and run the app.</span></span> <span data-ttu-id="53cb9-148">首頁有自己的計數器。</span><span class="sxs-lookup"><span data-stu-id="53cb9-148">The home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="53cb9-149">元件參數</span><span class="sxs-lookup"><span data-stu-id="53cb9-149">Component parameters</span></span>

<span data-ttu-id="53cb9-150">元件也可以有參數。</span><span class="sxs-lookup"><span data-stu-id="53cb9-150">Components can also have parameters.</span></span> <span data-ttu-id="53cb9-151">定義元件參數時，是在元件類別上使用以 `[Parameter]` 裝飾的非公開屬性來定義。</span><span class="sxs-lookup"><span data-stu-id="53cb9-151">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="53cb9-152">使用這些屬性來指定標記中元件的引數。</span><span class="sxs-lookup"><span data-stu-id="53cb9-152">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="53cb9-153">更新元件的 `@functions` C# 程式碼：</span><span class="sxs-lookup"><span data-stu-id="53cb9-153">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="53cb9-154">新增以 `[Parameter]` 屬性裝飾的 `IncrementAmount` 屬性。</span><span class="sxs-lookup"><span data-stu-id="53cb9-154">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="53cb9-155">將 `IncrementCount` 方法變更為在增加 `currentCount`的值時使用 `IncrementAmount`。</span><span class="sxs-lookup"><span data-stu-id="53cb9-155">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="53cb9-156">*Pages/Counter.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="53cb9-156">*Pages/Counter.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Counter.cshtml?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="53cb9-157">使用屬性在「首頁」元件的 `<Counter>` 元素中指定 `IncrementAmount` 參數。</span><span class="sxs-lookup"><span data-stu-id="53cb9-157">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="53cb9-158">設定值來讓計數器以 10 遞增。</span><span class="sxs-lookup"><span data-stu-id="53cb9-158">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="53cb9-159">*Pages/Index.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="53cb9-159">*Pages/Index.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Index.cshtml?highlight=7)]

1. <span data-ttu-id="53cb9-160">重新載入頁面。</span><span class="sxs-lookup"><span data-stu-id="53cb9-160">Reload the page.</span></span> <span data-ttu-id="53cb9-161">每次選取 [Click me] \(按我\) 按鈕時，首頁計數器都會以 10 遞增。</span><span class="sxs-lookup"><span data-stu-id="53cb9-161">The home page counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="53cb9-162">[計數器] 頁面上的計數器會以 1 遞增。</span><span class="sxs-lookup"><span data-stu-id="53cb9-162">The counter on the *Counter* page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="53cb9-163">路由到元件</span><span class="sxs-lookup"><span data-stu-id="53cb9-163">Route to components</span></span>

<span data-ttu-id="53cb9-164">*Counter.cshtml* 檔案頂端的 `@page` 指示詞會指定此元件是路由端點。</span><span class="sxs-lookup"><span data-stu-id="53cb9-164">The `@page` directive at the top of the *Counter.cshtml* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="53cb9-165">「計數器」元件會處理傳送給 `/Counter` 的要求。</span><span class="sxs-lookup"><span data-stu-id="53cb9-165">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="53cb9-166">若沒有 `@page` 指示詞，此元件就不會處理路由傳送的要求，但其他元件仍可使用此元件。</span><span class="sxs-lookup"><span data-stu-id="53cb9-166">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="53cb9-167">相依性插入</span><span class="sxs-lookup"><span data-stu-id="53cb9-167">Dependency injection</span></span>

<span data-ttu-id="53cb9-168">在應用程式服務容器中註冊的服務可透過[相依性插入 (DI)](xref:fundamentals/dependency-injection) 供元件使用。</span><span class="sxs-lookup"><span data-stu-id="53cb9-168">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="53cb9-169">請使用 `@inject` 指示詞將服務插入至元件。</span><span class="sxs-lookup"><span data-stu-id="53cb9-169">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="53cb9-170">檢查 FetchData 元件 (*Pages/FetchData.cshtml*) 的指示詞。</span><span class="sxs-lookup"><span data-stu-id="53cb9-170">Examine the directives of the FetchData component (*Pages/FetchData.cshtml*).</span></span> <span data-ttu-id="53cb9-171">`@inject` 指示詞會用來將 `WeatherForecastService` 服務執行個體插入至元件：</span><span class="sxs-lookup"><span data-stu-id="53cb9-171">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=3)]

<span data-ttu-id="53cb9-172">`WeatherForecastService` 服務會註冊為[單一項目](xref:fundamentals/dependency-injection#service-lifetimes)，讓一個服務執行個體可供整個應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="53cb9-172">The `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span>

<span data-ttu-id="53cb9-173">FetchData 元件會使用插入的服務作為 `ForecastService`，以擷取 `WeatherForecast` 物件的陣列：</span><span class="sxs-lookup"><span data-stu-id="53cb9-173">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.cshtml?highlight=6)]

<span data-ttu-id="53cb9-174">[@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) 迴圈會用來將每個預測執行個體轉譯為天氣資料表中的資料列：</span><span class="sxs-lookup"><span data-stu-id="53cb9-174">A [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.cshtml?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="53cb9-175">組建待辦事項清單</span><span class="sxs-lookup"><span data-stu-id="53cb9-175">Build a todo list</span></span>

<span data-ttu-id="53cb9-176">在實作簡單待辦事項清單的應用程式中新增頁面。</span><span class="sxs-lookup"><span data-stu-id="53cb9-176">Add a new page to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="53cb9-177">將名為 *Todo.cshtml* 的空白檔案新增至 *Pages* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="53cb9-177">Add an empty file to the *Pages* folder named *Todo.cshtml*.</span></span>

1. <span data-ttu-id="53cb9-178">提供頁面的初始標記：</span><span class="sxs-lookup"><span data-stu-id="53cb9-178">Provide the initial markup for the page:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="53cb9-179">將 [待辦事項] 頁面新增至導覽列。</span><span class="sxs-lookup"><span data-stu-id="53cb9-179">Add the Todo page to the navigation bar.</span></span>

   <span data-ttu-id="53cb9-180">NavMenu 元件 (*Shared/NavMenu.csthml*) 會用於應用程式的版面配置。</span><span class="sxs-lookup"><span data-stu-id="53cb9-180">The NavMenu component (*Shared/NavMenu.csthml*) is used in the app's layout.</span></span> <span data-ttu-id="53cb9-181">版面配置是可讓您避免應用程式中內容重複的元件。</span><span class="sxs-lookup"><span data-stu-id="53cb9-181">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="53cb9-182">如需詳細資訊，請參閱<xref:razor-components/layouts>。</span><span class="sxs-lookup"><span data-stu-id="53cb9-182">For more information, see <xref:razor-components/layouts>.</span></span>

   <span data-ttu-id="53cb9-183">透過在 *Shared/NavMenu.csthml* 檔案中現有清單項目下方新增下列清單項目標記，為 [待辦事項] 頁面新增 `<NavLink>`：</span><span class="sxs-lookup"><span data-stu-id="53cb9-183">Add a `<NavLink>` for the Todo page by adding the following list item markup below the existing list items in the *Shared/NavMenu.csthml* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="53cb9-184">重建並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="53cb9-184">Rebuild and run the app.</span></span> <span data-ttu-id="53cb9-185">瀏覽新的 [待辦事項] 頁面，以確認 [待辦事項] 頁面的連結可以運作。</span><span class="sxs-lookup"><span data-stu-id="53cb9-185">Visit the new Todo page to confirm that the link to the Todo page works.</span></span>

1. <span data-ttu-id="53cb9-186">將 *TodoItem.cs* 檔案新增至專案的根目錄，以保存代表待辦事項的類別。</span><span class="sxs-lookup"><span data-stu-id="53cb9-186">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="53cb9-187">請使用下列 `TodoItem` 類別的 C# 程式碼：</span><span class="sxs-lookup"><span data-stu-id="53cb9-187">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/TodoItem.cs)]

1. <span data-ttu-id="53cb9-188">返回「待辦事項」元件 (*Todo.cshtml*)：</span><span class="sxs-lookup"><span data-stu-id="53cb9-188">Return to the Todo component (*Todo.cshtml*):</span></span>

   * <span data-ttu-id="53cb9-189">在 `@functions` 區塊中新增待辦事項的欄位。</span><span class="sxs-lookup"><span data-stu-id="53cb9-189">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="53cb9-190">「待辦事項」元件會使用此欄位來維護待辦事項清單的狀態。</span><span class="sxs-lookup"><span data-stu-id="53cb9-190">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="53cb9-191">新增未排序的清單標記和 `foreach` 迴圈，將每個待辦事項轉譯為清單項目。</span><span class="sxs-lookup"><span data-stu-id="53cb9-191">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData4.cshtml?highlight=5-10,12-14)]

1. <span data-ttu-id="53cb9-192">應用程式需要 UI 項目，才能將待辦事項新增至清單。</span><span class="sxs-lookup"><span data-stu-id="53cb9-192">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="53cb9-193">在清單下方新增文字輸入和按鈕：</span><span class="sxs-lookup"><span data-stu-id="53cb9-193">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData5.cshtml?highlight=12-13)]

1. <span data-ttu-id="53cb9-194">重建並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="53cb9-194">Rebuild and run the app.</span></span> <span data-ttu-id="53cb9-195">選取 [Add todo] \(新增待辦事項\) 按鈕時，不會發生任何情況，因為事件處理常式並未連接至這個按鈕。</span><span class="sxs-lookup"><span data-stu-id="53cb9-195">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="53cb9-196">將 `AddTodo` 方法新增至「待辦事項」元件，然後使用 `onclick` 屬性將它註冊用於按鈕點選：</span><span class="sxs-lookup"><span data-stu-id="53cb9-196">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData6.cshtml?highlight=2,7-10)]

   <span data-ttu-id="53cb9-197">當選取按鈕時，就會呼叫 `AddTodo` C# 方法。</span><span class="sxs-lookup"><span data-stu-id="53cb9-197">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="53cb9-198">若要取得新待辦事項的標題，請新增 `newTodo` 字串欄位，然後使用 `bind` 屬性將它繫結至文字輸入的值：</span><span class="sxs-lookup"><span data-stu-id="53cb9-198">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData7.cshtml?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. <span data-ttu-id="53cb9-199">更新 `AddTodo` 方法，將 `TodoItem` 與指定的標題新增至清單。</span><span class="sxs-lookup"><span data-stu-id="53cb9-199">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="53cb9-200">請將 `newTodo` 設定為空字串，以清除文字輸入的值：</span><span class="sxs-lookup"><span data-stu-id="53cb9-200">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData8.cshtml?highlight=19-26)]

1. <span data-ttu-id="53cb9-201">重建並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="53cb9-201">Rebuild and run the app.</span></span> <span data-ttu-id="53cb9-202">請將一些待辦事項新增至待辦事項清單，以測試新程式碼。</span><span class="sxs-lookup"><span data-stu-id="53cb9-202">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="53cb9-203">每個待辦事項的標題文字都可設定為可編輯，而核取方塊則可協助使用者記錄已完成的項目。</span><span class="sxs-lookup"><span data-stu-id="53cb9-203">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="53cb9-204">請為每個待辦事項新增核取方塊輸入，然後將其值繫結至 `IsDone` 屬性。</span><span class="sxs-lookup"><span data-stu-id="53cb9-204">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="53cb9-205">將 `@todo.Title` 變更為繫結至 `@todo.Title` 的 `<input>` 元素：</span><span class="sxs-lookup"><span data-stu-id="53cb9-205">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData9.cshtml?highlight=5-6)]

1. <span data-ttu-id="53cb9-206">若要確認是否已繫結這些值，請更新 `<h1>` 標頭，以顯示未完成之待辦事項 (`IsDone` 為 `false`) 的數目計數。</span><span class="sxs-lookup"><span data-stu-id="53cb9-206">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="53cb9-207">已完成的「待辦事項」元件 (*Todo.cshtml*)：</span><span class="sxs-lookup"><span data-stu-id="53cb9-207">The completed Todo component (*Todo.cshtml*):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Todo.cshtml)]

1. <span data-ttu-id="53cb9-208">重建並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="53cb9-208">Rebuild and run the app.</span></span> <span data-ttu-id="53cb9-209">請新增待辦事項，以測試新程式碼。</span><span class="sxs-lookup"><span data-stu-id="53cb9-209">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="53cb9-210">發佈及部署應用程式</span><span class="sxs-lookup"><span data-stu-id="53cb9-210">Publish and deploy the app</span></span>

<span data-ttu-id="53cb9-211">若要發佈應用程式，請參閱 <xref:host-and-deploy/razor-components/index#publish-the-app>。</span><span class="sxs-lookup"><span data-stu-id="53cb9-211">To publish the app, see <xref:host-and-deploy/razor-components/index#publish-the-app>.</span></span>
