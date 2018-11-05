---
title: 檢視 ASP.NET Core 中的元件
author: rick-anderson
description: 了解如何檢視 ASP.NET Core 中使用的元件，以及如何將這些元件新增到應用程式。
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/view-components
ms.openlocfilehash: 91399acafb36f1f8759ed1783e70e59b631e3bf0
ms.sourcegitcommit: 4a6bbe84db24c2f3dd2de065de418fde952c8d40
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "50253125"
---
# <a name="view-components-in-aspnet-core"></a><span data-ttu-id="c3fee-103">檢視 ASP.NET Core 中的元件</span><span class="sxs-lookup"><span data-stu-id="c3fee-103">View components in ASP.NET Core</span></span>

<span data-ttu-id="c3fee-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c3fee-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c3fee-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c3fee-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="view-components"></a><span data-ttu-id="c3fee-106">檢視元件</span><span class="sxs-lookup"><span data-stu-id="c3fee-106">View components</span></span>

<span data-ttu-id="c3fee-107">檢視元件與部分檢視類似，但功能更強大。</span><span class="sxs-lookup"><span data-stu-id="c3fee-107">View components are similar to partial views, but they're much more powerful.</span></span> <span data-ttu-id="c3fee-108">檢視元件不會使用模型繫結，並且只取決於呼叫它時所提供的資料。</span><span class="sxs-lookup"><span data-stu-id="c3fee-108">View components don't use model binding, and only depend on the data provided when calling into it.</span></span> <span data-ttu-id="c3fee-109">撰寫本文時使用的是 ASP.NET Core MVC，但檢視元件也適用於 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="c3fee-109">This article was written using ASP.NET Core MVC, but view components also work with Razor Pages.</span></span>

<span data-ttu-id="c3fee-110">檢視元件：</span><span class="sxs-lookup"><span data-stu-id="c3fee-110">A view component:</span></span>

* <span data-ttu-id="c3fee-111">轉譯區塊，而不是整個回應。</span><span class="sxs-lookup"><span data-stu-id="c3fee-111">Renders a chunk rather than a whole response.</span></span>
* <span data-ttu-id="c3fee-112">包含控制器與檢視之間的相同關注點分離和可測試性優點。</span><span class="sxs-lookup"><span data-stu-id="c3fee-112">Includes the same separation-of-concerns and testability benefits found between a controller and view.</span></span>
* <span data-ttu-id="c3fee-113">可以有參數和商務邏輯。</span><span class="sxs-lookup"><span data-stu-id="c3fee-113">Can have parameters and business logic.</span></span>
* <span data-ttu-id="c3fee-114">它通常是從配置頁面叫用。</span><span class="sxs-lookup"><span data-stu-id="c3fee-114">Is typically invoked from a layout page.</span></span>

<span data-ttu-id="c3fee-115">如果您的可重複使用轉譯邏輯對於部分檢視而言太過複雜，則檢視元件是處理它的預定位置，例如：</span><span class="sxs-lookup"><span data-stu-id="c3fee-115">View components are intended anywhere you have reusable rendering logic that's too complex for a partial view, such as:</span></span>

* <span data-ttu-id="c3fee-116">動態導覽功能表</span><span class="sxs-lookup"><span data-stu-id="c3fee-116">Dynamic navigation menus</span></span>
* <span data-ttu-id="c3fee-117">標籤雲端 (可在其中查詢資料庫)</span><span class="sxs-lookup"><span data-stu-id="c3fee-117">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="c3fee-118">登入面板</span><span class="sxs-lookup"><span data-stu-id="c3fee-118">Login panel</span></span>
* <span data-ttu-id="c3fee-119">購物車</span><span class="sxs-lookup"><span data-stu-id="c3fee-119">Shopping cart</span></span>
* <span data-ttu-id="c3fee-120">最近發行的文章</span><span class="sxs-lookup"><span data-stu-id="c3fee-120">Recently published articles</span></span>
* <span data-ttu-id="c3fee-121">一般部落格上的資訊看板內容</span><span class="sxs-lookup"><span data-stu-id="c3fee-121">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="c3fee-122">登入面板，將在每個頁面上轉譯並根據使用者登入狀態來示範登出或登入連結</span><span class="sxs-lookup"><span data-stu-id="c3fee-122">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="c3fee-123">檢視元件是由兩個部分所組成：類別 (通常衍生自 [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) 以及它所傳回的結果 (通常是檢視)。</span><span class="sxs-lookup"><span data-stu-id="c3fee-123">A view component consists of two parts: the class (typically derived from [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="c3fee-124">與控制器類似，檢視元件可以是 POCO，但大部分開發人員會想要利用透過衍生自 `ViewComponent` 而取得的方法和屬性。</span><span class="sxs-lookup"><span data-stu-id="c3fee-124">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="c3fee-125">建立檢視元件</span><span class="sxs-lookup"><span data-stu-id="c3fee-125">Creating a view component</span></span>

<span data-ttu-id="c3fee-126">本節包含建立檢視元件的高階需求。</span><span class="sxs-lookup"><span data-stu-id="c3fee-126">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="c3fee-127">在本文稍後，我們會詳細檢查每個步驟，並建立檢視元件。</span><span class="sxs-lookup"><span data-stu-id="c3fee-127">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="c3fee-128">檢視元件類別</span><span class="sxs-lookup"><span data-stu-id="c3fee-128">The view component class</span></span>

<span data-ttu-id="c3fee-129">您可以透過下列任一項來建立檢視元件類別：</span><span class="sxs-lookup"><span data-stu-id="c3fee-129">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="c3fee-130">衍生自 *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="c3fee-130">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="c3fee-131">以 `[ViewComponent]` 屬性裝飾類別，或衍生自具有 `[ViewComponent]` 屬性的類別</span><span class="sxs-lookup"><span data-stu-id="c3fee-131">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="c3fee-132">建立名稱結尾為尾碼 *ViewComponent* 的類別</span><span class="sxs-lookup"><span data-stu-id="c3fee-132">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="c3fee-133">與控制器類似，檢視元件必須是公用、非巢狀和非抽象類別。</span><span class="sxs-lookup"><span data-stu-id="c3fee-133">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="c3fee-134">檢視元件名稱是移除 "ViewComponent" 尾碼的類別名稱。</span><span class="sxs-lookup"><span data-stu-id="c3fee-134">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="c3fee-135">它也可以使用 `ViewComponentAttribute.Name` 屬性明確地指定。</span><span class="sxs-lookup"><span data-stu-id="c3fee-135">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="c3fee-136">檢視元件類別：</span><span class="sxs-lookup"><span data-stu-id="c3fee-136">A view component class:</span></span>

* <span data-ttu-id="c3fee-137">完全支援建構函式[相依性插入](../../fundamentals/dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="c3fee-137">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="c3fee-138">不參與控制器生命週期，這表示您無法在檢視元件中使用[篩選](../controllers/filters.md)</span><span class="sxs-lookup"><span data-stu-id="c3fee-138">Doesn't take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="c3fee-139">檢視元件方法</span><span class="sxs-lookup"><span data-stu-id="c3fee-139">View component methods</span></span>

<span data-ttu-id="c3fee-140">檢視元件會在傳回 `IViewComponentResult` 的 `InvokeAsync` 方法中定義其邏輯。</span><span class="sxs-lookup"><span data-stu-id="c3fee-140">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="c3fee-141">參數直接來自檢視元件的引動過程，而不是來自模型繫結。</span><span class="sxs-lookup"><span data-stu-id="c3fee-141">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="c3fee-142">檢視元件絕不會直接處理要求。</span><span class="sxs-lookup"><span data-stu-id="c3fee-142">A view component never directly handles a request.</span></span> <span data-ttu-id="c3fee-143">通常，檢視元件會初始化模型，並呼叫 `View` 方法將其傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="c3fee-143">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="c3fee-144">簡要來說，檢視元件方法：</span><span class="sxs-lookup"><span data-stu-id="c3fee-144">In summary, view component methods:</span></span>

* <span data-ttu-id="c3fee-145">定義可傳回 `IViewComponentResult` 的 `InvokeAsync` 方法</span><span class="sxs-lookup"><span data-stu-id="c3fee-145">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="c3fee-146">通常會初始化模型，並呼叫 `ViewComponent` `View` 方法將其傳遞至檢視</span><span class="sxs-lookup"><span data-stu-id="c3fee-146">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="c3fee-147">參數來自呼叫端方法，而非 HTTP，而且沒有模型繫結</span><span class="sxs-lookup"><span data-stu-id="c3fee-147">Parameters come from the calling method, not HTTP, there's no model binding</span></span>
* <span data-ttu-id="c3fee-148">無法直接當成 HTTP 端點連接，它們是透過您的程式碼所叫用 (通常是在檢視中)。</span><span class="sxs-lookup"><span data-stu-id="c3fee-148">Are not reachable directly as an HTTP endpoint, they're invoked from your code (usually in a view).</span></span> <span data-ttu-id="c3fee-149">檢視元件絕不會處理要求</span><span class="sxs-lookup"><span data-stu-id="c3fee-149">A view component never handles a request</span></span>
* <span data-ttu-id="c3fee-150">已多載在簽章上，而非目前 HTTP 要求中的任何詳細資料</span><span class="sxs-lookup"><span data-stu-id="c3fee-150">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="c3fee-151">檢視搜尋路徑</span><span class="sxs-lookup"><span data-stu-id="c3fee-151">View search path</span></span>

<span data-ttu-id="c3fee-152">執行階段會搜尋下列路徑中的檢視：</span><span class="sxs-lookup"><span data-stu-id="c3fee-152">The runtime searches for the view in the following paths:</span></span>

* <span data-ttu-id="c3fee-153">/Pages/Components/{檢視元件名稱}/{檢視名稱}</span><span class="sxs-lookup"><span data-stu-id="c3fee-153">/Pages/Components/{View Component Name}/{View Name}</span></span>
* <span data-ttu-id="c3fee-154">/Views/{控制器名稱}/Components/{檢視元件名稱}/{檢視名稱}</span><span class="sxs-lookup"><span data-stu-id="c3fee-154">/Views/{Controller Name}/Components/{View Component Name}/{View Name}</span></span>
* <span data-ttu-id="c3fee-155">/Views/Shared/Components/{檢視元件名稱}/{檢視名稱}</span><span class="sxs-lookup"><span data-stu-id="c3fee-155">/Views/Shared/Components/{View Component Name}/{View Name}</span></span>

<span data-ttu-id="c3fee-156">檢視元件的預設檢視名稱是 *Default*，這表示您的檢視檔案通常會命名為 *Default.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="c3fee-156">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="c3fee-157">建立檢視元件結果時，或呼叫 `View` 方法時，可以指定不同的檢視名稱。</span><span class="sxs-lookup"><span data-stu-id="c3fee-157">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="c3fee-158">建議您將檢視檔案命名為 *Default.cshtml*，並使用 *Views/Shared/Components/{View Component Name}/{View Name}* 路徑。</span><span class="sxs-lookup"><span data-stu-id="c3fee-158">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/{View Component Name}/{View Name}* path.</span></span> <span data-ttu-id="c3fee-159">此範例中所使用的 `PriorityList` 檢視元件會將 *Views/Shared/Components/PriorityList/Default.cshtml* 用於檢視元件檢視。</span><span class="sxs-lookup"><span data-stu-id="c3fee-159">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="c3fee-160">叫用檢視元件</span><span class="sxs-lookup"><span data-stu-id="c3fee-160">Invoking a view component</span></span>

<span data-ttu-id="c3fee-161">若要使用檢視元件，請在檢視內呼叫下列項目：</span><span class="sxs-lookup"><span data-stu-id="c3fee-161">To use the view component, call the following inside a view:</span></span>

```cshtml
@await Component.InvokeAsync("Name of view component", {Anonymous Type Containing Parameters})
```

<span data-ttu-id="c3fee-162">參數將傳遞給 `InvokeAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="c3fee-162">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="c3fee-163">本文中所開發的 `PriorityList` 檢視元件是透過 *Views/Todo/Index.cshtml* 檢視檔案所叫用。</span><span class="sxs-lookup"><span data-stu-id="c3fee-163">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="c3fee-164">在下列範例中，`InvokeAsync` 方法是使用兩個參數所呼叫：</span><span class="sxs-lookup"><span data-stu-id="c3fee-164">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

::: moniker range=">= aspnetcore-1.1"

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="c3fee-165">叫用檢視元件作為標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="c3fee-165">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="c3fee-166">針對 ASP.NET Core 1.1 和更新版本，您可以叫用檢視元件作為[標籤協助程式](xref:mvc/views/tag-helpers/intro)：</span><span class="sxs-lookup"><span data-stu-id="c3fee-166">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="c3fee-167">標籤協助程式依照 Pascal 命名法大小寫慣例的類別和方法參數會轉譯成其[小寫 Kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)。</span><span class="sxs-lookup"><span data-stu-id="c3fee-167">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="c3fee-168">用來叫用檢視元件的標籤協助程式會使用 `<vc></vc>` 項目。</span><span class="sxs-lookup"><span data-stu-id="c3fee-168">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="c3fee-169">檢視元件指定如下：</span><span class="sxs-lookup"><span data-stu-id="c3fee-169">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="c3fee-170">若要使用檢視元件作為標籤協助程式，請使用 `@addTagHelper` 指示詞註冊包含檢視元件的組件。</span><span class="sxs-lookup"><span data-stu-id="c3fee-170">To use a view component as a Tag Helper, register the assembly containing the view component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="c3fee-171">如果檢視元件位於稱為 `MyWebApp` 的組件中，則請將下列指示詞新增至 *_ViewImports.cshtml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="c3fee-171">If your view component is in an assembly called `MyWebApp`, add the following directive to the *_ViewImports.cshtml* file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="c3fee-172">您可以將檢視元件註冊為任何參考檢視元件的檔案標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="c3fee-172">You can register a view component as a Tag Helper to any file that references the view component.</span></span> <span data-ttu-id="c3fee-173">如需如何註冊標籤協助程式的詳細資訊，請參閱[管理標籤協助程式範圍](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope)。</span><span class="sxs-lookup"><span data-stu-id="c3fee-173">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="c3fee-174">本教學課程中使用的 `InvokeAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="c3fee-174">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="c3fee-175">在標籤 (tag) 協助程式標籤 (markup) 中：</span><span class="sxs-lookup"><span data-stu-id="c3fee-175">In Tag Helper markup:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="c3fee-176">在上述範例中，`PriorityList` 檢視元件會變成 `priority-list`。</span><span class="sxs-lookup"><span data-stu-id="c3fee-176">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="c3fee-177">檢視元件的參數會以小寫 Kebab 形式傳遞為屬性。</span><span class="sxs-lookup"><span data-stu-id="c3fee-177">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

::: moniker-end

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="c3fee-178">直接從控制器叫用檢視元件</span><span class="sxs-lookup"><span data-stu-id="c3fee-178">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="c3fee-179">檢視元件通常是從檢視中進行叫用，但您可以直接從控制器方法叫用它們。</span><span class="sxs-lookup"><span data-stu-id="c3fee-179">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="c3fee-180">雖然檢視元件不會定義控制器這類端點，但您可以輕鬆地實作控制器動作，以傳回 `ViewComponentResult` 的內容。</span><span class="sxs-lookup"><span data-stu-id="c3fee-180">While view components don't define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="c3fee-181">在此範例中，是直接從控制器呼叫檢視元件：</span><span class="sxs-lookup"><span data-stu-id="c3fee-181">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="c3fee-182">逐步解說：建立簡單檢視元件</span><span class="sxs-lookup"><span data-stu-id="c3fee-182">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="c3fee-183">[下載](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample)、建置和測試起始程式碼。</span><span class="sxs-lookup"><span data-stu-id="c3fee-183">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="c3fee-184">它是具有 `Todo` 控制器的簡單專案，而此控制器顯示 *Todo* 項目清單。</span><span class="sxs-lookup"><span data-stu-id="c3fee-184">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![ToDos 清單](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="c3fee-186">新增 ViewComponent 類別</span><span class="sxs-lookup"><span data-stu-id="c3fee-186">Add a ViewComponent class</span></span>

<span data-ttu-id="c3fee-187">建立 *ViewComponents* 資料夾，並新增下列 `PriorityListViewComponent` 類別：</span><span class="sxs-lookup"><span data-stu-id="c3fee-187">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="c3fee-188">程式碼的注意事項：</span><span class="sxs-lookup"><span data-stu-id="c3fee-188">Notes on the code:</span></span>

* <span data-ttu-id="c3fee-189">檢視元件類別可以包含在專案的**任何**資料夾中。</span><span class="sxs-lookup"><span data-stu-id="c3fee-189">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="c3fee-190">因為類別名稱 PriorityList**ViewComponent** 結尾為尾碼 **ViewComponent**，所以從檢視參考類別元件時，執行階段會使用字串 "PriorityList"。</span><span class="sxs-lookup"><span data-stu-id="c3fee-190">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="c3fee-191">我稍後將更詳細地進行說明。</span><span class="sxs-lookup"><span data-stu-id="c3fee-191">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="c3fee-192">`[ViewComponent]` 屬性可以變更用來參考檢視元件的名稱。</span><span class="sxs-lookup"><span data-stu-id="c3fee-192">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="c3fee-193">例如，我們無法將類別命名為 `XYZ` 以及套用 `ViewComponent` 屬性：</span><span class="sxs-lookup"><span data-stu-id="c3fee-193">For example, we could've named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="c3fee-194">上面的 `[ViewComponent]` 屬性會告知檢視元件選取器在尋找與元件建立關聯的檢視時使用名稱 `PriorityList`，以及在從檢視參考類別元件時使用字串 "PriorityList"。</span><span class="sxs-lookup"><span data-stu-id="c3fee-194">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="c3fee-195">我稍後將更詳細地進行說明。</span><span class="sxs-lookup"><span data-stu-id="c3fee-195">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="c3fee-196">元件會使用[相依性插入](../../fundamentals/dependency-injection.md)，讓資料內容可供使用。</span><span class="sxs-lookup"><span data-stu-id="c3fee-196">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="c3fee-197">`InvokeAsync` 會公開可以從檢視中呼叫的方法，而且可以採用任意數目的引數。</span><span class="sxs-lookup"><span data-stu-id="c3fee-197">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="c3fee-198">`InvokeAsync` 方法會傳回一組符合 `isDone` 和 `maxPriority` 參數的 `ToDo` 項目。</span><span class="sxs-lookup"><span data-stu-id="c3fee-198">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="c3fee-199">建立檢視元件 Razor 檢視</span><span class="sxs-lookup"><span data-stu-id="c3fee-199">Create the view component Razor view</span></span>

* <span data-ttu-id="c3fee-200">建立 *Views/Shared/Components* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="c3fee-200">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="c3fee-201">此資料夾**必須**命名為 *Components*。</span><span class="sxs-lookup"><span data-stu-id="c3fee-201">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="c3fee-202">建立 *Views/Shared/Components/PriorityList* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="c3fee-202">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="c3fee-203">此資料夾名稱必須符合檢視元件類別的名稱，或去掉尾碼的類別名稱 (如果我們遵循慣例，並在類別名稱中使用 *ViewComponent* 尾碼)。</span><span class="sxs-lookup"><span data-stu-id="c3fee-203">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="c3fee-204">如果您已使用 `ViewComponent` 屬性，則類別名稱需要符合屬性指定。</span><span class="sxs-lookup"><span data-stu-id="c3fee-204">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="c3fee-205">建立 *Views/Shared/Components/PriorityList/Default.cshtml* Razor 檢視：[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="c3fee-205">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="c3fee-206">Razor 檢視採用 `TodoItem` 清單，並加以顯示。</span><span class="sxs-lookup"><span data-stu-id="c3fee-206">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="c3fee-207">如果檢視元件 `InvokeAsync` 方法未傳遞檢視名稱 (如我們的範例所示)，則依照慣例會使用 *Default* 作為檢視名稱。</span><span class="sxs-lookup"><span data-stu-id="c3fee-207">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="c3fee-208">在教學課程稍後，我將示範如何傳遞檢視的名稱。</span><span class="sxs-lookup"><span data-stu-id="c3fee-208">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="c3fee-209">若要覆寫特定控制器的預設樣式，請在控制器特定檢視資料夾中新增檢視 (例如 *Views/Todo/Components/PriorityList/Default.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="c3fee-209">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="c3fee-210">如果檢視元件是控制器特有的，則可以將它新增至控制器特定資料夾 (*Views/Todo/Components/PriorityList/Default.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="c3fee-210">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="c3fee-211">將包含優先順序清單元件呼叫的 `div` 新增至 *Views/Todo/index.cshtml* 檔案底端：</span><span class="sxs-lookup"><span data-stu-id="c3fee-211">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="c3fee-212">`@await Component.InvokeAsync` 標記顯示呼叫檢視元件的語法。</span><span class="sxs-lookup"><span data-stu-id="c3fee-212">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="c3fee-213">第一個引數是我們想要叫用或呼叫之元件的名稱。</span><span class="sxs-lookup"><span data-stu-id="c3fee-213">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="c3fee-214">後續參數會傳遞至元件。</span><span class="sxs-lookup"><span data-stu-id="c3fee-214">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="c3fee-215">`InvokeAsync` 可以採用任意數目的引數。</span><span class="sxs-lookup"><span data-stu-id="c3fee-215">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="c3fee-216">測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="c3fee-216">Test the app.</span></span> <span data-ttu-id="c3fee-217">下圖顯示 ToDo 清單和優先順序項目：</span><span class="sxs-lookup"><span data-stu-id="c3fee-217">The following image shows the ToDo list and the priority items:</span></span>

![ToDo 清單和優先順序項目](view-components/_static/pi.png)

<span data-ttu-id="c3fee-219">您也可以直接從控制器呼叫檢視元件：</span><span class="sxs-lookup"><span data-stu-id="c3fee-219">You can also call the view component directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![IndexVC 動作中的優先順序項目](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="c3fee-221">指定檢視名稱</span><span class="sxs-lookup"><span data-stu-id="c3fee-221">Specifying a view name</span></span>

<span data-ttu-id="c3fee-222">在某些情況下，可能需要複雜的檢視元件，才能指定非預設檢視。</span><span class="sxs-lookup"><span data-stu-id="c3fee-222">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="c3fee-223">下列程式碼示範如何從 `InvokeAsync` 方法指定 "PVC" 檢視。</span><span class="sxs-lookup"><span data-stu-id="c3fee-223">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="c3fee-224">更新 `PriorityListViewComponent` 類別中的 `InvokeAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="c3fee-224">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="c3fee-225">將 *Views/Shared/Components/PriorityList/Default.cshtml* 檔案複製至名為 *Views/Shared/Components/PriorityList/PVC.cshtml* 的檢視。</span><span class="sxs-lookup"><span data-stu-id="c3fee-225">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="c3fee-226">新增標題，以指出正在使用 PVC 檢視。</span><span class="sxs-lookup"><span data-stu-id="c3fee-226">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="c3fee-227">更新 *Views/TodoList/Index.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="c3fee-227">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="c3fee-228">執行應用程式，並驗證 PVC 檢視。</span><span class="sxs-lookup"><span data-stu-id="c3fee-228">Run the app and verify PVC view.</span></span>

![設定檢視元件優先順序](view-components/_static/pvc.png)

<span data-ttu-id="c3fee-230">如果未轉譯 PVC 檢視，請驗證您要呼叫的檢視元件優先順序為 4 或以上。</span><span class="sxs-lookup"><span data-stu-id="c3fee-230">If the PVC view isn't rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="c3fee-231">檢查檢視路徑</span><span class="sxs-lookup"><span data-stu-id="c3fee-231">Examine the view path</span></span>

* <span data-ttu-id="c3fee-232">將優先順序參數變更為 3 或更小，以不傳回優先順序檢視。</span><span class="sxs-lookup"><span data-stu-id="c3fee-232">Change the priority parameter to three or less so the priority view isn't returned.</span></span>
* <span data-ttu-id="c3fee-233">將 *Views/Todo/Components/PriorityList/Default.cshtml* 暫時重新命名為 *1Default.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="c3fee-233">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="c3fee-234">測試應用程式，您會收到下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="c3fee-234">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="c3fee-235">將 *Views/Todo/Components/PriorityList/1Default.cshtml* 複製至 *Views/Shared/Components/PriorityList/Default.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="c3fee-235">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="c3fee-236">將某個標記新增至 *Shared* Todo 檢視元件檢視，指出檢視來自 *Shared* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="c3fee-236">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="c3fee-237">測試 **Shared** 元件檢視。</span><span class="sxs-lookup"><span data-stu-id="c3fee-237">Test the **Shared** component view.</span></span>

![含 Shared 元件檢視的 ToDo 輸出](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="c3fee-239">避免魔術字串</span><span class="sxs-lookup"><span data-stu-id="c3fee-239">Avoiding magic strings</span></span>

<span data-ttu-id="c3fee-240">如果您想要編譯時間安全，則可以將寫在程式碼中的檢視元件名稱取代為類別名稱。</span><span class="sxs-lookup"><span data-stu-id="c3fee-240">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="c3fee-241">建立不含 "ViewComponent" 尾碼的檢視元件：</span><span class="sxs-lookup"><span data-stu-id="c3fee-241">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="c3fee-242">將 `using` 陳述式新增至 Razor 檢視檔案，並使用 `nameof` 運算子：</span><span class="sxs-lookup"><span data-stu-id="c3fee-242">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="perform-synchronous-work"></a><span data-ttu-id="c3fee-243">執行同步工作</span><span class="sxs-lookup"><span data-stu-id="c3fee-243">Perform synchronous work</span></span>

<span data-ttu-id="c3fee-244">如果您不需要執行非同步工作，架構會處理叫用同步 `Invoke` 方法。</span><span class="sxs-lookup"><span data-stu-id="c3fee-244">The framework handles invoking a synchronous `Invoke` method if you don't need to perform asynchronous work.</span></span> <span data-ttu-id="c3fee-245">下列方法會建立同步 `Invoke` 檢視元件：</span><span class="sxs-lookup"><span data-stu-id="c3fee-245">The following method creates a synchronous `Invoke` view component:</span></span>

```csharp
public class PriorityList : ViewComponent
{
    public IViewComponentResult Invoke(int maxPriority, bool isDone)
    {
        var items = new List<string> { $"maxPriority: {maxPriority}", $"isDone: {isDone}" };
        return View(items);
    }
}
```

<span data-ttu-id="c3fee-246">檢視元件的 Razor 檔案，會列出傳遞至 `Invoke` 方法 (*Views/Home/Components/PriorityList/Default.cshtml*) 的字串：</span><span class="sxs-lookup"><span data-stu-id="c3fee-246">The view component's Razor file lists the strings passed to the `Invoke` method (*Views/Home/Components/PriorityList/Default.cshtml*):</span></span>

```cshtml
@model List<string>

<h3>Priority Items</h3>
<ul>
    @foreach (var item in Model)
    {
        <li>@item</li>
    }
</ul>
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="c3fee-247">使用下列其中一項方式，在 Razor 檔案中叫用檢視元件 (例如 *Views/Home/Index.cshtml*)：</span><span class="sxs-lookup"><span data-stu-id="c3fee-247">The view component is invoked in a Razor file (for example, *Views/Home/Index.cshtml*) using one of the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>
* [<span data-ttu-id="c3fee-248">標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="c3fee-248">Tag Helper</span></span>](xref:mvc/views/tag-helpers/intro)

<span data-ttu-id="c3fee-249">若要使用 <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper> 方法，請呼叫 `Component.InvokeAsync`：</span><span class="sxs-lookup"><span data-stu-id="c3fee-249">To use the <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper> approach, call `Component.InvokeAsync`:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-1.1"

<span data-ttu-id="c3fee-250">使用 <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper> 在 Razor 檔案中叫用檢視元件 (例如 *Views/Home/Index.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="c3fee-250">The view component is invoked in a Razor file (for example, *Views/Home/Index.cshtml*) with <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>.</span></span>

<span data-ttu-id="c3fee-251">呼叫 `Component.InvokeAsync`：</span><span class="sxs-lookup"><span data-stu-id="c3fee-251">Call `Component.InvokeAsync`:</span></span>

::: moniker-end

```cshtml
@await Component.InvokeAsync(nameof(PriorityList), new { maxPriority = 4, isDone = true })
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="c3fee-252">若要使用標籤協助程式，請使用 `@addTagHelper` 指示詞註冊包含檢視元件的組件 (檢視元件位於稱為 `MyWebApp` 的組件中)：</span><span class="sxs-lookup"><span data-stu-id="c3fee-252">To use the Tag Helper, register the assembly containing the View Component using the `@addTagHelper` directive (the view component is in an assembly called `MyWebApp`):</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="c3fee-253">使用 Razor 標記檔案中的檢視元件標籤協助程式：</span><span class="sxs-lookup"><span data-stu-id="c3fee-253">Use the view component Tag Helper in the Razor markup file:</span></span>

```cshtml
<vc:priority-list max-priority="999" is-done="false">
</vc:priority-list>
```
::: moniker-end

<span data-ttu-id="c3fee-254">`PriorityList.Invoke` 的方法簽章為同步，但 Razor 會在標記檔案中找到並使用 `Component.InvokeAsync` 呼叫該方法。</span><span class="sxs-lookup"><span data-stu-id="c3fee-254">The method signature of `PriorityList.Invoke` is synchronous, but Razor finds and calls the method with `Component.InvokeAsync` in the markup file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c3fee-255">其他資源</span><span class="sxs-lookup"><span data-stu-id="c3fee-255">Additional resources</span></span>

* [<span data-ttu-id="c3fee-256">在檢視中插入相依性</span><span class="sxs-lookup"><span data-stu-id="c3fee-256">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
