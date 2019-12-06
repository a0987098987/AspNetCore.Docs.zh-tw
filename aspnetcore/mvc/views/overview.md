---
title: ASP.NET Core MVC 中的檢視
author: ardalis
description: 了解檢視如何處理 ASP.NET Core MVC 中的應用程式資料呈現和使用者互動。
ms.author: riande
ms.date: 12/05/2019
uid: mvc/views/overview
ms.openlocfilehash: f636908ee36d0af6e92875876240cb8712dd2ccc
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881026"
---
# <a name="views-in-aspnet-core-mvc"></a><span data-ttu-id="18458-103">ASP.NET Core MVC 中的檢視</span><span class="sxs-lookup"><span data-stu-id="18458-103">Views in ASP.NET Core MVC</span></span>

<span data-ttu-id="18458-104">作者：[Steve Smith](https://ardalis.com/) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="18458-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="18458-105">本文件說明 ASP.NET Core MVC 應用程式中所使用的檢視。</span><span class="sxs-lookup"><span data-stu-id="18458-105">This document explains views used in ASP.NET Core MVC applications.</span></span> <span data-ttu-id="18458-106">如需 Razor 頁面的資訊，請參閱 [Razor 頁面簡介](xref:razor-pages/index)。</span><span class="sxs-lookup"><span data-stu-id="18458-106">For information on Razor Pages, see [Introduction to Razor Pages](xref:razor-pages/index).</span></span>

<span data-ttu-id="18458-107">在模型檢視控制器 (MVC) 模式中，「檢視」會處理應用程式的資料呈現和使用者互動。</span><span class="sxs-lookup"><span data-stu-id="18458-107">In the Model-View-Controller (MVC) pattern, the *view* handles the app's data presentation and user interaction.</span></span> <span data-ttu-id="18458-108">檢視是具有內嵌 [Razor 標記](xref:mvc/views/razor)的 HTML 範本。</span><span class="sxs-lookup"><span data-stu-id="18458-108">A view is an HTML template with embedded [Razor markup](xref:mvc/views/razor).</span></span> <span data-ttu-id="18458-109">Razor 標記是與 HTML 標記互動的程式碼，可以產生傳送至用戶端的網頁。</span><span class="sxs-lookup"><span data-stu-id="18458-109">Razor markup is code that interacts with HTML markup to produce a webpage that's sent to the client.</span></span>

<span data-ttu-id="18458-110">在 ASP.NET Core MVC 中，檢視是在 Razor 標記中使用 [C# 程式設計語言](/dotnet/csharp/)的 *.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18458-110">In ASP.NET Core MVC, views are *.cshtml* files that use the [C# programming language](/dotnet/csharp/) in Razor markup.</span></span> <span data-ttu-id="18458-111">通常，檢視檔案會分組成針對每個應用程式之[控制器](xref:mvc/controllers/actions)而命名的資料夾。</span><span class="sxs-lookup"><span data-stu-id="18458-111">Usually, view files are grouped into folders named for each of the app's [controllers](xref:mvc/controllers/actions).</span></span> <span data-ttu-id="18458-112">資料夾會儲存至應用程式根目錄的 *Views* 資料夾中：</span><span class="sxs-lookup"><span data-stu-id="18458-112">The folders are stored in a *Views* folder at the root of the app:</span></span>

![Visual Studio 方案總管中的 Views 資料夾是與 Home 資料夾一起開啟，以顯示 About.cshtml、Contact.cshtml 和 Index.cshtml 檔案](overview/_static/views_solution_explorer.png)

<span data-ttu-id="18458-114">*Home* 控制器是由 *Views* 資料夾內的 *Home* 資料夾所呈現。</span><span class="sxs-lookup"><span data-stu-id="18458-114">The *Home* controller is represented by a *Home* folder inside the *Views* folder.</span></span> <span data-ttu-id="18458-115">*Home* 資料夾包含 *About*、*Contact* 和 *Index* (首頁) 網頁的檢視。</span><span class="sxs-lookup"><span data-stu-id="18458-115">The *Home* folder contains the views for the *About*, *Contact*, and *Index* (homepage) webpages.</span></span> <span data-ttu-id="18458-116">使用者要求這三個網頁中的其中一個時，*Home* 控制器中的控制器動作可決定使用這三個檢視的哪一個來建置網頁並將其傳回給使用者。</span><span class="sxs-lookup"><span data-stu-id="18458-116">When a user requests one of these three webpages, controller actions in the *Home* controller determine which of the three views is used to build and return a webpage to the user.</span></span>

<span data-ttu-id="18458-117">使用[配置](xref:mvc/views/layout)，提供一致的網頁區段，並減少程式碼重複。</span><span class="sxs-lookup"><span data-stu-id="18458-117">Use [layouts](xref:mvc/views/layout) to provide consistent webpage sections and reduce code repetition.</span></span> <span data-ttu-id="18458-118">配置通常包含標頭、導覽和功能表項目，以及頁尾。</span><span class="sxs-lookup"><span data-stu-id="18458-118">Layouts often contain the header, navigation and menu elements, and the footer.</span></span> <span data-ttu-id="18458-119">標頭和頁尾通常會包含許多中繼資料項目以及指令碼和樣式表連結的未定案標記。</span><span class="sxs-lookup"><span data-stu-id="18458-119">The header and footer usually contain boilerplate markup for many metadata elements and links to script and style assets.</span></span> <span data-ttu-id="18458-120">配置可協助您避免在檢視中使用此未定案標記。</span><span class="sxs-lookup"><span data-stu-id="18458-120">Layouts help you avoid this boilerplate markup in your views.</span></span>

<span data-ttu-id="18458-121">[部分檢視](xref:mvc/views/partial)透過管理檢視的可重複使用部分，來減少程式碼重複。</span><span class="sxs-lookup"><span data-stu-id="18458-121">[Partial views](xref:mvc/views/partial) reduce code duplication by managing reusable parts of views.</span></span> <span data-ttu-id="18458-122">例如，部分檢視可用於數個檢視中出現之部落格網站上的作者自傳。</span><span class="sxs-lookup"><span data-stu-id="18458-122">For example, a partial view is useful for an author biography on a blog website that appears in several views.</span></span> <span data-ttu-id="18458-123">作者自傳是一般檢視內容，不需要執行程式碼就能產生網頁內容。</span><span class="sxs-lookup"><span data-stu-id="18458-123">An author biography is ordinary view content and doesn't require code to execute in order to produce the content for the webpage.</span></span> <span data-ttu-id="18458-124">檢視單獨透過模型繫結就可以使用作者自傳內容，因此最適合使用這類型內容的部分檢視。</span><span class="sxs-lookup"><span data-stu-id="18458-124">Author biography content is available to the view by model binding alone, so using a partial view for this type of content is ideal.</span></span>

<span data-ttu-id="18458-125">[檢視元件](xref:mvc/views/view-components)與部分檢視類似，可讓您減少重複的程式碼，但它們適用於需要程式碼在伺服器上執行才能轉譯網頁的檢視內容。</span><span class="sxs-lookup"><span data-stu-id="18458-125">[View components](xref:mvc/views/view-components) are similar to partial views in that they allow you to reduce repetitive code, but they're appropriate for view content that requires code to run on the server in order to render the webpage.</span></span> <span data-ttu-id="18458-126">轉譯的內容需要資料庫互動時 (例如網站購物車)，檢視元件十分有用。</span><span class="sxs-lookup"><span data-stu-id="18458-126">View components are useful when the rendered content requires database interaction, such as for a website shopping cart.</span></span> <span data-ttu-id="18458-127">檢視元件不會限制為模型繫結，以產生網頁輸出。</span><span class="sxs-lookup"><span data-stu-id="18458-127">View components aren't limited to model binding in order to produce webpage output.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="18458-128">使用檢視的優點</span><span class="sxs-lookup"><span data-stu-id="18458-128">Benefits of using views</span></span>

<span data-ttu-id="18458-129">檢視可協助在 MVC 應用程式內建立[關注點分離](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns)，方法是區隔使用者介面標記與應用程式的其他部分。</span><span class="sxs-lookup"><span data-stu-id="18458-129">Views help to establish [separation of concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) within an MVC app by separating the user interface markup from other parts of the app.</span></span> <span data-ttu-id="18458-130">遵循 SoC 設計可讓您的應用程式模組化，以提供數個優點：</span><span class="sxs-lookup"><span data-stu-id="18458-130">Following SoC design makes your app modular, which provides several benefits:</span></span>

* <span data-ttu-id="18458-131">應用程式較容易維護，因為其組織性較佳。</span><span class="sxs-lookup"><span data-stu-id="18458-131">The app is easier to maintain because it's better organized.</span></span> <span data-ttu-id="18458-132">檢視一般會依應用程式功能分組。</span><span class="sxs-lookup"><span data-stu-id="18458-132">Views are generally grouped by app feature.</span></span> <span data-ttu-id="18458-133">這可讓您在處理功能時更輕鬆地找到相關檢視。</span><span class="sxs-lookup"><span data-stu-id="18458-133">This makes it easier to find related views when working on a feature.</span></span>
* <span data-ttu-id="18458-134">應用程式的組件是鬆散耦合的。</span><span class="sxs-lookup"><span data-stu-id="18458-134">The parts of the app are loosely coupled.</span></span> <span data-ttu-id="18458-135">您可以分別從商務邏輯和資料存取元件來建置和更新應用程式的檢視。</span><span class="sxs-lookup"><span data-stu-id="18458-135">You can build and update the app's views separately from the business logic and data access components.</span></span> <span data-ttu-id="18458-136">您可以修改應用程式的檢視，而不一定需要更新應用程式的其他部分。</span><span class="sxs-lookup"><span data-stu-id="18458-136">You can modify the views of the app without necessarily having to update other parts of the app.</span></span>
* <span data-ttu-id="18458-137">因為檢視是個別單位，所以可以更輕鬆地測試應用程式的使用者介面部分。</span><span class="sxs-lookup"><span data-stu-id="18458-137">It's easier to test the user interface parts of the app because the views are separate units.</span></span>
* <span data-ttu-id="18458-138">為達到更佳的編排情況，較不希望使用者介面的區段意外地重複。</span><span class="sxs-lookup"><span data-stu-id="18458-138">Due to better organization, it's less likely that you'll accidentally repeat sections of the user interface.</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="18458-139">建立檢視</span><span class="sxs-lookup"><span data-stu-id="18458-139">Creating a view</span></span>

<span data-ttu-id="18458-140">會在 *Views/[ControllerName]* 資料夾中建立控制器特有的檢視。</span><span class="sxs-lookup"><span data-stu-id="18458-140">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="18458-141">在控制器之間共用的檢視會放在 *Views/Shared* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="18458-141">Views that are shared among controllers are placed in the *Views/Shared* folder.</span></span> <span data-ttu-id="18458-142">若要建立檢視，請新增檔案，並讓它與建立關聯的控制器動作同名，且副檔名為 *.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="18458-142">To create a view, add a new file and give it the same name as its associated controller action with the *.cshtml* file extension.</span></span> <span data-ttu-id="18458-143">若要在 *Home* 控制器中建立與 *About* 動作對應的檢視，請在 *Views/Home* 資料夾中建立 *About.cshtml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="18458-143">To create a view that corresponds with the *About* action in the *Home* controller, create an *About.cshtml* file in the *Views/Home* folder:</span></span>

[!code-cshtml[](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

<span data-ttu-id="18458-144">*Razor* 標記的開頭為 `@` 符號。</span><span class="sxs-lookup"><span data-stu-id="18458-144">*Razor* markup starts with the `@` symbol.</span></span> <span data-ttu-id="18458-145">在大括弧 (`{ ... }`) 所設定的 [Razor 程式碼區塊](xref:mvc/views/razor#razor-code-blocks)內放入 C# 程式碼，即可執行 C# 陳述式。</span><span class="sxs-lookup"><span data-stu-id="18458-145">Run C# statements by placing C# code within [Razor code blocks](xref:mvc/views/razor#razor-code-blocks) set off by curly braces (`{ ... }`).</span></span> <span data-ttu-id="18458-146">例如，請參閱上方將 "About" 指派給 `ViewData["Title"]`。</span><span class="sxs-lookup"><span data-stu-id="18458-146">For example, see the assignment of "About" to `ViewData["Title"]` shown above.</span></span> <span data-ttu-id="18458-147">只要使用 `@` 符號參考值，就可以在 HTML 內顯示值。</span><span class="sxs-lookup"><span data-stu-id="18458-147">You can display values within HTML by simply referencing the value with the `@` symbol.</span></span> <span data-ttu-id="18458-148">請參閱上方 `<h2>` 和 `<h3>` 項目的內容。</span><span class="sxs-lookup"><span data-stu-id="18458-148">See the contents of the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="18458-149">上述檢視內容只是轉譯給使用者之整個網頁的一部分。</span><span class="sxs-lookup"><span data-stu-id="18458-149">The view content shown above is only part of the entire webpage that's rendered to the user.</span></span> <span data-ttu-id="18458-150">其餘的頁面配置以及檢視的其他通用層面指定於其他檢視檔案中。</span><span class="sxs-lookup"><span data-stu-id="18458-150">The rest of the page's layout and other common aspects of the view are specified in other view files.</span></span> <span data-ttu-id="18458-151">若要深入了解，請參閱[配置主題](xref:mvc/views/layout)。</span><span class="sxs-lookup"><span data-stu-id="18458-151">To learn more, see the [Layout topic](xref:mvc/views/layout).</span></span>

## <a name="how-controllers-specify-views"></a><span data-ttu-id="18458-152">控制器指定檢視的方式</span><span class="sxs-lookup"><span data-stu-id="18458-152">How controllers specify views</span></span>

<span data-ttu-id="18458-153">檢視通常會從動作傳回為 [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult)，這是 [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) 的類型。</span><span class="sxs-lookup"><span data-stu-id="18458-153">Views are typically returned from actions as a [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult), which is a type of [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult).</span></span> <span data-ttu-id="18458-154">動作方法可以直接建立並傳回 `ViewResult`，但這通常不會進行。</span><span class="sxs-lookup"><span data-stu-id="18458-154">Your action method can create and return a `ViewResult` directly, but that isn't commonly done.</span></span> <span data-ttu-id="18458-155">因為大部分的控制器都是繼承自[控制器](/dotnet/api/microsoft.aspnetcore.mvc.controller)，所以您只需要使用 `View` 協助程式方法傳回 `ViewResult`：</span><span class="sxs-lookup"><span data-stu-id="18458-155">Since most controllers inherit from [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller), you simply use the `View` helper method to return the `ViewResult`:</span></span>

<span data-ttu-id="18458-156">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="18458-156">*HomeController.cs*</span></span>

[!code-csharp[](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

<span data-ttu-id="18458-157">傳回此動作時，最後一個區段中所顯示的 *About.cshtml* 檢視會轉譯為下列網頁：</span><span class="sxs-lookup"><span data-stu-id="18458-157">When this action returns, the *About.cshtml* view shown in the last section is rendered as the following webpage:</span></span>

![關於 Edge 瀏覽器中所轉譯的頁面](overview/_static/about-page.png)

<span data-ttu-id="18458-159">`View` 協助程式方法具有數個多載。</span><span class="sxs-lookup"><span data-stu-id="18458-159">The `View` helper method has several overloads.</span></span> <span data-ttu-id="18458-160">您可以選擇性地指定：</span><span class="sxs-lookup"><span data-stu-id="18458-160">You can optionally specify:</span></span>

* <span data-ttu-id="18458-161">要傳回的明確檢視：</span><span class="sxs-lookup"><span data-stu-id="18458-161">An explicit view to return:</span></span>

  ```csharp
  return View("Orders");
  ```

* <span data-ttu-id="18458-162">要傳遞至檢視的[模型](xref:mvc/models/model-binding)：</span><span class="sxs-lookup"><span data-stu-id="18458-162">A [model](xref:mvc/models/model-binding) to pass to the view:</span></span>

  ```csharp
  return View(Orders);
  ```

* <span data-ttu-id="18458-163">檢視和模型：</span><span class="sxs-lookup"><span data-stu-id="18458-163">Both a view and a model:</span></span>

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a><span data-ttu-id="18458-164">檢視探索</span><span class="sxs-lookup"><span data-stu-id="18458-164">View discovery</span></span>

<span data-ttu-id="18458-165">動作傳回檢視時，會進行稱為「檢視探索」的程序。</span><span class="sxs-lookup"><span data-stu-id="18458-165">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="18458-166">此程序根據檢視名稱來決定使用的檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="18458-166">This process determines which view file is used based on the view name.</span></span> 

<span data-ttu-id="18458-167">`View` 方法的預設行為 (`return View();`) 是傳回的檢視與從中呼叫它的動作方法同名。</span><span class="sxs-lookup"><span data-stu-id="18458-167">The default behavior of the `View` method (`return View();`) is to return a view with the same name as the action method from which it's called.</span></span> <span data-ttu-id="18458-168">例如，使用控制器的 *About* `ActionResult` 方法名稱來搜尋名為 *About.cshtml* 的檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="18458-168">For example, the *About* `ActionResult` method name of the controller is used to search for a view file named *About.cshtml*.</span></span> <span data-ttu-id="18458-169">首先，執行階段會在 *Views/[ControllerName]* 資料夾中尋找檢視。</span><span class="sxs-lookup"><span data-stu-id="18458-169">First, the runtime looks in the *Views/[ControllerName]* folder for the view.</span></span> <span data-ttu-id="18458-170">如果在這裡找不到相符的檢視，則會搜尋檢視的 *Shared* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="18458-170">If it doesn't find a matching view there, it searches the *Shared* folder for the view.</span></span>

<span data-ttu-id="18458-171">如果您使用 `return View();` 以隱含方式傳回 `ViewResult`，或使用 `return View("<ViewName>");` 將檢視名稱明確地傳遞至 `View` 方法，則不重要。</span><span class="sxs-lookup"><span data-stu-id="18458-171">It doesn't matter if you implicitly return the `ViewResult` with `return View();` or explicitly pass the view name to the `View` method with `return View("<ViewName>");`.</span></span> <span data-ttu-id="18458-172">在這兩種情況下，檢視探索會依此順序搜尋相符的檢視檔案：</span><span class="sxs-lookup"><span data-stu-id="18458-172">In both cases, view discovery searches for a matching view file in this order:</span></span>

   1. <span data-ttu-id="18458-173">*Views/\[ControllerName]/\[ViewName].cshtml*</span><span class="sxs-lookup"><span data-stu-id="18458-173">*Views/\[ControllerName]/\[ViewName].cshtml*</span></span>
   1. <span data-ttu-id="18458-174">*Views/Shared/\[ViewName].cshtml*</span><span class="sxs-lookup"><span data-stu-id="18458-174">*Views/Shared/\[ViewName].cshtml*</span></span>

<span data-ttu-id="18458-175">可以提供檢視檔案路徑，而不是檢視名稱。</span><span class="sxs-lookup"><span data-stu-id="18458-175">A view file path can be provided instead of a view name.</span></span> <span data-ttu-id="18458-176">如果使用從應用程式根目錄開始的絕對路徑 (選擇性地開始於 "/" 或 "~/")，則必須指定 *.cshtml* 副檔名：</span><span class="sxs-lookup"><span data-stu-id="18458-176">If using an absolute path starting at the app root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified:</span></span>

```csharp
return View("Views/Home/About.cshtml");
```

<span data-ttu-id="18458-177">您也可以使用相對路徑來指定不同目錄中的檢視，但沒有 *.cshtml* 副檔名。</span><span class="sxs-lookup"><span data-stu-id="18458-177">You can also use a relative path to specify views in different directories without the *.cshtml* extension.</span></span> <span data-ttu-id="18458-178">在 `HomeController` 內，您可以使用相對路徑傳回 *Manage* 檢視的 *Index* 檢視：</span><span class="sxs-lookup"><span data-stu-id="18458-178">Inside the `HomeController`, you can return the *Index* view of your *Manage* views with a relative path:</span></span>

```csharp
return View("../Manage/Index");
```

<span data-ttu-id="18458-179">同樣地，您可以使用 "./" 前置詞來指出目前的控制器特定目錄：</span><span class="sxs-lookup"><span data-stu-id="18458-179">Similarly, you can indicate the current controller-specific directory with the "./" prefix:</span></span>

```csharp
return View("./About");
```

<span data-ttu-id="18458-180">[部分檢視](xref:mvc/views/partial)和[檢視元件](xref:mvc/views/view-components)使用類似 (但不同) 的探索機制。</span><span class="sxs-lookup"><span data-stu-id="18458-180">[Partial views](xref:mvc/views/partial) and [view components](xref:mvc/views/view-components) use similar (but not identical) discovery mechanisms.</span></span>

<span data-ttu-id="18458-181">您可以使用自訂 [IViewLocationExpander](/dotnet/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander)，來自訂檢視如何位在應用程式內的預設慣例。</span><span class="sxs-lookup"><span data-stu-id="18458-181">You can customize the default convention for how views are located within the app by using a custom [IViewLocationExpander](/dotnet/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span></span>

<span data-ttu-id="18458-182">檢視探索依賴如何依檔案名稱來尋找檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="18458-182">View discovery relies on finding view files by file name.</span></span> <span data-ttu-id="18458-183">如果基礎檔案系統區分大小寫，則檢視名稱可能會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="18458-183">If the underlying file system is case sensitive, view names are probably case sensitive.</span></span> <span data-ttu-id="18458-184">基於作業系統之間的相容性，控制器與動作名稱和建立關聯的檢視資料夾和檔案名稱之間的大小寫必須相符。</span><span class="sxs-lookup"><span data-stu-id="18458-184">For compatibility across operating systems, match case between controller and action names and associated view folders and file names.</span></span> <span data-ttu-id="18458-185">如果您遇到使用區分大小寫的檔案系統時找不到檢視檔案的錯誤，請確認所要求檢視檔案與實際檢視檔案名稱之間的大小寫符合。</span><span class="sxs-lookup"><span data-stu-id="18458-185">If you encounter an error that a view file can't be found while working with a case-sensitive file system, confirm that the casing matches between the requested view file and the actual view file name.</span></span>

<span data-ttu-id="18458-186">請遵循最佳做法來組織檢視的檔案結構，以反映控制器、動作與檢視之間的關聯性來進行維護和避免混淆。</span><span class="sxs-lookup"><span data-stu-id="18458-186">Follow the best practice of organizing the file structure for your views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="18458-187">將資料傳遞至檢視</span><span class="sxs-lookup"><span data-stu-id="18458-187">Passing data to views</span></span>

<span data-ttu-id="18458-188">使用數種方法將資料傳遞至檢視：</span><span class="sxs-lookup"><span data-stu-id="18458-188">Pass data to views using several approaches:</span></span>

* <span data-ttu-id="18458-189">強型別資料：viewmodel</span><span class="sxs-lookup"><span data-stu-id="18458-189">Strongly typed data: viewmodel</span></span>
* <span data-ttu-id="18458-190">弱型別資料</span><span class="sxs-lookup"><span data-stu-id="18458-190">Weakly typed data</span></span>
  * <span data-ttu-id="18458-191">`ViewData` (`ViewDataAttribute`)</span><span class="sxs-lookup"><span data-stu-id="18458-191">`ViewData` (`ViewDataAttribute`)</span></span>
  * `ViewBag`

### <a name="strongly-typed-data-viewmodel"></a><span data-ttu-id="18458-192">強型別資料 (viewmodel)</span><span class="sxs-lookup"><span data-stu-id="18458-192">Strongly typed data (viewmodel)</span></span>

<span data-ttu-id="18458-193">最穩健的方法是在檢視中指定 [model](xref:mvc/models/model-binding) 類型。</span><span class="sxs-lookup"><span data-stu-id="18458-193">The most robust approach is to specify a [model](xref:mvc/models/model-binding) type in the view.</span></span> <span data-ttu-id="18458-194">此模型通常稱為 *viewmodel*。</span><span class="sxs-lookup"><span data-stu-id="18458-194">This model is commonly referred to as a *viewmodel*.</span></span> <span data-ttu-id="18458-195">您可以透過動作將 viewmodel 類型執行個體傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="18458-195">You pass an instance of the viewmodel type to the view from the action.</span></span>

<span data-ttu-id="18458-196">使用 viewmodel 將資料傳遞至檢視，讓檢視利用「強式」檢查。</span><span class="sxs-lookup"><span data-stu-id="18458-196">Using a viewmodel to pass data to a view allows the view to take advantage of *strong* type checking.</span></span> <span data-ttu-id="18458-197">*強式型別* (或*強型別*) 代表每個變數與常數都有明確定義的類型 (例如，`string`、`int` 或 `DateTime`)。</span><span class="sxs-lookup"><span data-stu-id="18458-197">*Strong typing* (or *strongly typed*) means that every variable and constant has an explicitly defined type (for example, `string`, `int`, or `DateTime`).</span></span> <span data-ttu-id="18458-198">在編譯時期檢查檢視中所使用之類型的有效性。</span><span class="sxs-lookup"><span data-stu-id="18458-198">The validity of types used in a view is checked at compile time.</span></span>

<span data-ttu-id="18458-199">[Visual Studio](https://visualstudio.microsoft.com) 與 [Visual Studio Code](https://code.visualstudio.com/) 會使用稱為 [IntelliSense](/visualstudio/ide/using-intellisense) 的功能，列出強型別類別成員。</span><span class="sxs-lookup"><span data-stu-id="18458-199">[Visual Studio](https://visualstudio.microsoft.com) and [Visual Studio Code](https://code.visualstudio.com/) list strongly typed class members using a feature called [IntelliSense](/visualstudio/ide/using-intellisense).</span></span> <span data-ttu-id="18458-200">當您想要查看 viewmodel 的屬性時，請鍵入 viewmodel 的變數名稱，後面接著句號 (`.`)。</span><span class="sxs-lookup"><span data-stu-id="18458-200">When you want to see the properties of a viewmodel, type the variable name for the viewmodel followed by a period (`.`).</span></span> <span data-ttu-id="18458-201">這有助於更快速地撰寫程式碼，並且減少錯誤。</span><span class="sxs-lookup"><span data-stu-id="18458-201">This helps you write code faster with fewer errors.</span></span>

<span data-ttu-id="18458-202">使用 `@model` 指示詞來指定模型。</span><span class="sxs-lookup"><span data-stu-id="18458-202">Specify a model using the `@model` directive.</span></span> <span data-ttu-id="18458-203">搭配使用模型與 `@Model`：</span><span class="sxs-lookup"><span data-stu-id="18458-203">Use the model with `@Model`:</span></span>

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="18458-204">若要將模型提供給檢視，控制器會將它當成參數傳遞：</span><span class="sxs-lookup"><span data-stu-id="18458-204">To provide the model to the view, the controller passes it as a parameter:</span></span>

```csharp
public IActionResult Contact()
{
    ViewData["Message"] = "Your contact page.";

    var viewModel = new Address()
    {
        Name = "Microsoft",
        Street = "One Microsoft Way",
        City = "Redmond",
        State = "WA",
        PostalCode = "98052-6399"
    };

    return View(viewModel);
}
```

<span data-ttu-id="18458-205">您可以提供給檢視的模型類型沒有任何限制。</span><span class="sxs-lookup"><span data-stu-id="18458-205">There are no restrictions on the model types that you can provide to a view.</span></span> <span data-ttu-id="18458-206">建議您使用定義最少行為或沒有定義行為 (方法) 的簡單的 CLR 物件 (POCO) viewmodel。</span><span class="sxs-lookup"><span data-stu-id="18458-206">We recommend using Plain Old CLR Object (POCO) viewmodels with little or no behavior (methods) defined.</span></span> <span data-ttu-id="18458-207">通常，viewmodel 類別會儲存至應用程式根目錄的 *Models* 資料夾或個別 *ViewModels* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="18458-207">Usually, viewmodel classes are either stored in the *Models* folder or a separate *ViewModels* folder at the root of the app.</span></span> <span data-ttu-id="18458-208">上述範例中所使用的 *Address* viewmodel 是儲存至名為 *Address.cs* 之檔案中的 POCO viewmodel：</span><span class="sxs-lookup"><span data-stu-id="18458-208">The *Address* viewmodel used in the example above is a POCO viewmodel stored in a file named *Address.cs*:</span></span>

```csharp
namespace WebApplication1.ViewModels
{
    public class Address
    {
        public string Name { get; set; }
        public string Street { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string PostalCode { get; set; }
    }
}
```

<span data-ttu-id="18458-209">您可以將相同的類別用於 viewmodel 類型和商務模型類型。</span><span class="sxs-lookup"><span data-stu-id="18458-209">Nothing prevents you from using the same classes for both your viewmodel types and your business model types.</span></span> <span data-ttu-id="18458-210">不過，使用不同的模型可讓您的檢視因應用程式的商務邏輯和資料存取部分而不同。</span><span class="sxs-lookup"><span data-stu-id="18458-210">However, using separate models allows your views to vary independently from the business logic and data access parts of your app.</span></span> <span data-ttu-id="18458-211">模型將[模型繫結](xref:mvc/models/model-binding)和[驗證](xref:mvc/models/validation)用於依使用者傳送至應用程式的資料時，模型與 viewmodel 的區隔也提供安全性優點。</span><span class="sxs-lookup"><span data-stu-id="18458-211">Separation of models and viewmodels also offers security benefits when models use [model binding](xref:mvc/models/model-binding) and [validation](xref:mvc/models/validation) for data sent to the app by the user.</span></span>

<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-viewdata-attribute-and-viewbag"></a><span data-ttu-id="18458-212">弱型別資料 (ViewData、ViewData 屬性與 ViewBag)</span><span class="sxs-lookup"><span data-stu-id="18458-212">Weakly typed data (ViewData, ViewData attribute, and ViewBag)</span></span>

<span data-ttu-id="18458-213">Razor 頁面中沒有 `ViewBag`。</span><span class="sxs-lookup"><span data-stu-id="18458-213">`ViewBag` *isn't available in Razor Pages.*</span></span>

<span data-ttu-id="18458-214">除了強型別檢視之外，檢視還可以存取*弱型別* (也稱為*鬆散型別*) 資料集合。</span><span class="sxs-lookup"><span data-stu-id="18458-214">In addition to strongly typed views, views have access to a *weakly typed* (also called *loosely typed*) collection of data.</span></span> <span data-ttu-id="18458-215">與強式型別不同，「弱式型別」 (或「鬆散型別」) 表示您未明確宣告所使用資料的類型。</span><span class="sxs-lookup"><span data-stu-id="18458-215">Unlike strong types, *weak types* (or *loose types*) means that you don't explicitly declare the type of data you're using.</span></span> <span data-ttu-id="18458-216">您可以使用弱型別資料的集合，對控制器與檢視傳遞將少量的資料進出。</span><span class="sxs-lookup"><span data-stu-id="18458-216">You can use the collection of weakly typed data for passing small amounts of data in and out of controllers and views.</span></span>

| <span data-ttu-id="18458-217">在 ... 之間傳遞資料</span><span class="sxs-lookup"><span data-stu-id="18458-217">Passing data between a ...</span></span>                        | <span data-ttu-id="18458-218">範例</span><span class="sxs-lookup"><span data-stu-id="18458-218">Example</span></span>                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="18458-219">控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="18458-219">Controller and a view</span></span>                             | <span data-ttu-id="18458-220">使用資料填入下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="18458-220">Populating a dropdown list with data.</span></span>                                          |
| <span data-ttu-id="18458-221">檢視和[配置檢視](xref:mvc/views/layout)</span><span class="sxs-lookup"><span data-stu-id="18458-221">View and a [layout view](xref:mvc/views/layout)</span></span>   | <span data-ttu-id="18458-222">透過檢視檔案，在配置檢視中設定 **\<title>** 項目內容。</span><span class="sxs-lookup"><span data-stu-id="18458-222">Setting the **\<title>** element content in the layout view from a view file.</span></span>  |
| <span data-ttu-id="18458-223">[部分檢視](xref:mvc/views/partial)和檢視</span><span class="sxs-lookup"><span data-stu-id="18458-223">[Partial view](xref:mvc/views/partial) and a view</span></span> | <span data-ttu-id="18458-224">一種小工具，可根據使用者所要求的網頁來顯示資料。</span><span class="sxs-lookup"><span data-stu-id="18458-224">A widget that displays data based on the webpage that the user requested.</span></span>      |

<span data-ttu-id="18458-225">此集合可以透過控制器和檢視上的 `ViewData` 或 `ViewBag` 屬性進行參考。</span><span class="sxs-lookup"><span data-stu-id="18458-225">This collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="18458-226">`ViewData` 屬性是弱型別物件的字典。</span><span class="sxs-lookup"><span data-stu-id="18458-226">The `ViewData` property is a dictionary of weakly typed objects.</span></span> <span data-ttu-id="18458-227">`ViewBag` 屬性是 `ViewData` 中提供基礎 `ViewData` 集合之動態屬性的包裝函式。</span><span class="sxs-lookup"><span data-stu-id="18458-227">The `ViewBag` property is a wrapper around `ViewData` that provides dynamic properties for the underlying `ViewData` collection.</span></span> <span data-ttu-id="18458-228">注意： `ViewData` 和 `ViewBag`的索引鍵查閱都不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="18458-228">Note: Key lookups are case-insensitive for both `ViewData` and `ViewBag`.</span></span>

<span data-ttu-id="18458-229">`ViewData` 和 `ViewBag` 是在執行階段動態解析。</span><span class="sxs-lookup"><span data-stu-id="18458-229">`ViewData` and `ViewBag` are dynamically resolved at runtime.</span></span> <span data-ttu-id="18458-230">因為它們未提供編譯時間類型檢查，所以兩者通常會比使用 viewmodel 更容易發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="18458-230">Since they don't offer compile-time type checking, both are generally more error-prone than using a viewmodel.</span></span> <span data-ttu-id="18458-231">因此，有些開發人員會盡量少使用或不使用 `ViewData` 和 `ViewBag`。</span><span class="sxs-lookup"><span data-stu-id="18458-231">For that reason, some developers prefer to minimally or never use `ViewData` and `ViewBag`.</span></span>

<a name="VD"></a>

<span data-ttu-id="18458-232">**ViewData**</span><span class="sxs-lookup"><span data-stu-id="18458-232">**ViewData**</span></span>

<span data-ttu-id="18458-233">`ViewData` 是透過 `string` 索引鍵存取的 [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) 物件。</span><span class="sxs-lookup"><span data-stu-id="18458-233">`ViewData` is a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) object accessed through `string` keys.</span></span> <span data-ttu-id="18458-234">字串資料可以直接儲存和使用，而不需要轉換；但必須在擷取其他 `ViewData` 物件值時將其轉換為特定類型。</span><span class="sxs-lookup"><span data-stu-id="18458-234">String data can be stored and used directly without the need for a cast, but you must cast other `ViewData` object values to specific types when you extract them.</span></span> <span data-ttu-id="18458-235">您可以使用 `ViewData` 將資料從控制器傳遞至檢視以及在檢視內傳遞資料，包括[部分檢視](xref:mvc/views/partial)和[配置](xref:mvc/views/layout)。</span><span class="sxs-lookup"><span data-stu-id="18458-235">You can use `ViewData` to pass data from controllers to views and within views, including [partial views](xref:mvc/views/partial) and [layouts](xref:mvc/views/layout).</span></span>

<span data-ttu-id="18458-236">下列範例使用運作中 `ViewData` 來設定問候語和地址的值：</span><span class="sxs-lookup"><span data-stu-id="18458-236">The following is an example that sets values for a greeting and an address using `ViewData` in an action:</span></span>

```csharp
public IActionResult SomeAction()
{
    ViewData["Greeting"] = "Hello";
    ViewData["Address"]  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

<span data-ttu-id="18458-237">使用檢視中的資料：</span><span class="sxs-lookup"><span data-stu-id="18458-237">Work with the data in a view:</span></span>

```cshtml
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="18458-238">**ViewData 屬性**</span><span class="sxs-lookup"><span data-stu-id="18458-238">**ViewData attribute**</span></span>

<span data-ttu-id="18458-239">使用 [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) 的另一種方法是 [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute)。</span><span class="sxs-lookup"><span data-stu-id="18458-239">Another approach that uses the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) is [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="18458-240">以 `[ViewData]` 屬性標示之控制器或 Razor 頁面模型上的屬性，其值會從字典儲存和載入。</span><span class="sxs-lookup"><span data-stu-id="18458-240">Properties on controllers or Razor Page models marked with the `[ViewData]` attribute have their values stored and loaded from the dictionary.</span></span>

<span data-ttu-id="18458-241">在下列範例中，Home 控制器包含標記為 `[ViewData]`的 `Title` 屬性。</span><span class="sxs-lookup"><span data-stu-id="18458-241">In the following example, the Home controller contains a `Title` property marked with `[ViewData]`.</span></span> <span data-ttu-id="18458-242">`About` 方法會設定 [About] 檢視的標題：</span><span class="sxs-lookup"><span data-stu-id="18458-242">The `About` method sets the title for the About view:</span></span>

```csharp
public class HomeController : Controller
{
    [ViewData]
    public string Title { get; set; }

    public IActionResult About()
    {
        Title = "About Us";
        ViewData["Message"] = "Your application description page.";

        return View();
    }
}
```

<span data-ttu-id="18458-243">在 [About] 檢視上，存取 `Title` 屬性作為模型屬性：</span><span class="sxs-lookup"><span data-stu-id="18458-243">In the About view, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="18458-244">在此配置中，標題會從 ViewData 字典中讀取：</span><span class="sxs-lookup"><span data-stu-id="18458-244">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

::: moniker-end

<span data-ttu-id="18458-245">**ViewBag**</span><span class="sxs-lookup"><span data-stu-id="18458-245">**ViewBag**</span></span>

<span data-ttu-id="18458-246">Razor 頁面中沒有 `ViewBag`。</span><span class="sxs-lookup"><span data-stu-id="18458-246">`ViewBag` *isn't available in Razor Pages.*</span></span>

<span data-ttu-id="18458-247">`ViewBag` 是可動態存取 `ViewData` 中所儲存物件的 [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) 物件。</span><span class="sxs-lookup"><span data-stu-id="18458-247">`ViewBag` is a [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) object that provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="18458-248">`ViewBag` 的使用更為方便，因為它不需要進行轉換。</span><span class="sxs-lookup"><span data-stu-id="18458-248">`ViewBag` can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="18458-249">下列範例示範如何使用 `ViewBag`，而其結果與上方使用 `ViewData` 相同：</span><span class="sxs-lookup"><span data-stu-id="18458-249">The following example shows how to use `ViewBag` with the same result as using `ViewData` above:</span></span>

```csharp
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

```cshtml
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

<span data-ttu-id="18458-250">**同時使用 ViewData 和 ViewBag**</span><span class="sxs-lookup"><span data-stu-id="18458-250">**Using ViewData and ViewBag simultaneously**</span></span>

<span data-ttu-id="18458-251">Razor 頁面中沒有 `ViewBag`。</span><span class="sxs-lookup"><span data-stu-id="18458-251">`ViewBag` *isn't available in Razor Pages.*</span></span>

<span data-ttu-id="18458-252">因為 `ViewData` 和 `ViewBag` 參照相同的基礎 `ViewData` 集合，所以您可以同時使用 `ViewData` 和 `ViewBag`，並在讀取和寫入值時於其間混合使用和比對。</span><span class="sxs-lookup"><span data-stu-id="18458-252">Since `ViewData` and `ViewBag` refer to the same underlying `ViewData` collection, you can use both `ViewData` and `ViewBag` and mix and match between them when reading and writing values.</span></span>

<span data-ttu-id="18458-253">使用 `ViewBag` 設定標題，並使用 *About.cshtml* 檢視頂端的 `ViewData` 來設定描述：</span><span class="sxs-lookup"><span data-stu-id="18458-253">Set the title using `ViewBag` and the description using `ViewData` at the top of an *About.cshtml* view:</span></span>

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

<span data-ttu-id="18458-254">讀取屬性，但反向使用 `ViewData` 和 `ViewBag`。</span><span class="sxs-lookup"><span data-stu-id="18458-254">Read the properties but reverse the use of `ViewData` and `ViewBag`.</span></span> <span data-ttu-id="18458-255">在 *_Layout.cshtml* 檔案中，使用 `ViewData` 取得標題，並使用 `ViewBag` 取得描述：</span><span class="sxs-lookup"><span data-stu-id="18458-255">In the *_Layout.cshtml* file, obtain the title using `ViewData` and obtain the description using `ViewBag`:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

<span data-ttu-id="18458-256">請記住，字串不要求針對 `ViewData` 進行轉換。</span><span class="sxs-lookup"><span data-stu-id="18458-256">Remember that strings don't require a cast for `ViewData`.</span></span> <span data-ttu-id="18458-257">您可以使用 `@ViewData["Title"]`，而不需要轉換。</span><span class="sxs-lookup"><span data-stu-id="18458-257">You can use `@ViewData["Title"]` without casting.</span></span>

<span data-ttu-id="18458-258">可以同時使用 `ViewData` 和 `ViewBag`，與混合使用和比對讀取與寫入屬性一樣。</span><span class="sxs-lookup"><span data-stu-id="18458-258">Using both `ViewData` and `ViewBag` at the same time works, as does mixing and matching reading and writing the properties.</span></span> <span data-ttu-id="18458-259">會轉譯下列標記：</span><span class="sxs-lookup"><span data-stu-id="18458-259">The following markup is rendered:</span></span>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

<span data-ttu-id="18458-260">**ViewData 與 ViewBag 之間的差異摘要**</span><span class="sxs-lookup"><span data-stu-id="18458-260">**Summary of the differences between ViewData and ViewBag**</span></span>

 <span data-ttu-id="18458-261">Razor 頁面中沒有 `ViewBag`。</span><span class="sxs-lookup"><span data-stu-id="18458-261">`ViewBag` isn't available in the Razor Pages.</span></span>

* `ViewData`
  * <span data-ttu-id="18458-262">衍生自 [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)，因此它的字典屬性十分有用，例如 `ContainsKey`、`Add`、`Remove` 和 `Clear`。</span><span class="sxs-lookup"><span data-stu-id="18458-262">Derives from [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), so it has dictionary properties that can be useful, such as `ContainsKey`, `Add`, `Remove`, and `Clear`.</span></span>
  * <span data-ttu-id="18458-263">字典中的索引鍵是字串，因此允許空白字元。</span><span class="sxs-lookup"><span data-stu-id="18458-263">Keys in the dictionary are strings, so whitespace is allowed.</span></span> <span data-ttu-id="18458-264">範例：`ViewData["Some Key With Whitespace"]`</span><span class="sxs-lookup"><span data-stu-id="18458-264">Example: `ViewData["Some Key With Whitespace"]`</span></span>
  * <span data-ttu-id="18458-265">在檢視中必須轉換任何 `string` 以外的類型，才能使用 `ViewData`。</span><span class="sxs-lookup"><span data-stu-id="18458-265">Any type other than a `string` must be cast in the view to use `ViewData`.</span></span>
* `ViewBag`
  * <span data-ttu-id="18458-266">衍生自 [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata)，因此允許使用點標記法 (`@ViewBag.SomeKey = <value or object>`) 來建立動態屬性，而不需要轉換。</span><span class="sxs-lookup"><span data-stu-id="18458-266">Derives from [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), so it allows the creation of dynamic properties using dot notation (`@ViewBag.SomeKey = <value or object>`), and no casting is required.</span></span> <span data-ttu-id="18458-267">`ViewBag` 的語法可以更快速地新增至控制器和檢視。</span><span class="sxs-lookup"><span data-stu-id="18458-267">The syntax of `ViewBag` makes it quicker to add to controllers and views.</span></span>
  * <span data-ttu-id="18458-268">檢查 Null 值更簡單。</span><span class="sxs-lookup"><span data-stu-id="18458-268">Simpler to check for null values.</span></span> <span data-ttu-id="18458-269">範例：`@ViewBag.Person?.Name`</span><span class="sxs-lookup"><span data-stu-id="18458-269">Example: `@ViewBag.Person?.Name`</span></span>

<span data-ttu-id="18458-270">**何時使用 ViewData 或 ViewBag**</span><span class="sxs-lookup"><span data-stu-id="18458-270">**When to use ViewData or ViewBag**</span></span>

<span data-ttu-id="18458-271">`ViewData` 和 `ViewBag` 是同樣有效的方式，都可以在控制器與檢視之間傳遞少量資料。</span><span class="sxs-lookup"><span data-stu-id="18458-271">Both `ViewData` and `ViewBag` are equally valid approaches for passing small amounts of data among controllers and views.</span></span> <span data-ttu-id="18458-272">根據喜好設定來選擇使用哪一個。</span><span class="sxs-lookup"><span data-stu-id="18458-272">The choice of which one to use is based on preference.</span></span> <span data-ttu-id="18458-273">您可以混合使用與比對 `ViewData` 和 `ViewBag` 物件；不過，使用一致的方法，較容易讀取和維護程式碼。</span><span class="sxs-lookup"><span data-stu-id="18458-273">You can mix and match `ViewData` and `ViewBag` objects, however, the code is easier to read and maintain with one approach used consistently.</span></span> <span data-ttu-id="18458-274">這兩種方法是在執行階段動態解析，因此容易導致執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="18458-274">Both approaches are dynamically resolved at runtime and thus prone to causing runtime errors.</span></span> <span data-ttu-id="18458-275">有些開發小組會避免使用它們。</span><span class="sxs-lookup"><span data-stu-id="18458-275">Some development teams avoid them.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="18458-276">動態檢視</span><span class="sxs-lookup"><span data-stu-id="18458-276">Dynamic views</span></span>

<span data-ttu-id="18458-277">如果檢視未使用 `@model` 來宣告模型類型，但具有傳遞給它們的模型執行個體 (例如，`return View(Address);`)，則可以動態參考執行個體的屬性：</span><span class="sxs-lookup"><span data-stu-id="18458-277">Views that don't declare a model type using `@model` but that have a model instance passed to them (for example, `return View(Address);`) can reference the instance's properties dynamically:</span></span>

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="18458-278">此功能提供彈性，但未提供編譯保護或 IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="18458-278">This feature offers flexibility but doesn't offer compilation protection or IntelliSense.</span></span> <span data-ttu-id="18458-279">如果屬性不存在，則網頁產生會在執行階段失敗。</span><span class="sxs-lookup"><span data-stu-id="18458-279">If the property doesn't exist, webpage generation fails at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="18458-280">其他檢視功能</span><span class="sxs-lookup"><span data-stu-id="18458-280">More view features</span></span>

<span data-ttu-id="18458-281">[標籤協助程式](xref:mvc/views/tag-helpers/intro)可讓您輕鬆地將伺服器端行為新增至現有 HTML 標籤。</span><span class="sxs-lookup"><span data-stu-id="18458-281">[Tag Helpers](xref:mvc/views/tag-helpers/intro) make it easy to add server-side behavior to existing HTML tags.</span></span> <span data-ttu-id="18458-282">使用標籤協助程式就不需要在檢視內撰寫自訂程式碼或協助程式。</span><span class="sxs-lookup"><span data-stu-id="18458-282">Using Tag Helpers avoids the need to write custom code or helpers within your views.</span></span> <span data-ttu-id="18458-283">標籤協助程式會以屬性形式套用至 HTML 項目，而且無法處理它們的編輯器會忽略它們。</span><span class="sxs-lookup"><span data-stu-id="18458-283">Tag helpers are applied as attributes to HTML elements and are ignored by editors that can't process them.</span></span> <span data-ttu-id="18458-284">這可讓您在各種工具中編輯和轉譯檢視標記。</span><span class="sxs-lookup"><span data-stu-id="18458-284">This allows you to edit and render view markup in a variety of tools.</span></span>

<span data-ttu-id="18458-285">許多內建 HTML 協助程式都可以產生自訂 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="18458-285">Generating custom HTML markup can be achieved with many built-in HTML Helpers.</span></span> <span data-ttu-id="18458-286">[檢視元件](xref:mvc/views/view-components)可以處理更複雜的使用者介面邏輯。</span><span class="sxs-lookup"><span data-stu-id="18458-286">More complex user interface logic can be handled by [View Components](xref:mvc/views/view-components).</span></span> <span data-ttu-id="18458-287">檢視元件所提供的 SoC 與該控制器和檢視所提供的 SoC 相同。</span><span class="sxs-lookup"><span data-stu-id="18458-287">View components provide the same SoC that controllers and views offer.</span></span> <span data-ttu-id="18458-288">它們不需要動作和檢視來處理一般使用者介面項目所使用的資料。</span><span class="sxs-lookup"><span data-stu-id="18458-288">They can eliminate the need for actions and views that deal with data used by common user interface elements.</span></span>

<span data-ttu-id="18458-289">與許多其他 ASP.NET Core 層面一樣，檢視支援[相依性插入](xref:fundamentals/dependency-injection)，允許將服務[插入檢視](xref:mvc/views/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="18458-289">Like many other aspects of ASP.NET Core, views support [dependency injection](xref:fundamentals/dependency-injection), allowing services to be [injected into views](xref:mvc/views/dependency-injection).</span></span>
