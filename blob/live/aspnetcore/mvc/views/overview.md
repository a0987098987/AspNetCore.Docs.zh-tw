---
title: "ASP.NET Core MVC 中的檢視"
author: ardalis
description: "了解如何檢視處理的應用程式資料的呈現與 ASP.NET Core MVC 中的使用者互動。"
keywords: "ASP.NET Core MVC、 razor、 viewmodel、 別的 viewdata、 viewbag 檢視"
ms.author: riande
manager: wpickett
ms.date: 12/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: 2562d4e5fb85159e6ccb47990f54448ddc188077
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2017
---
# <a name="views-in-aspnet-core-mvc"></a><span data-ttu-id="82611-104">ASP.NET Core MVC 中的檢視</span><span class="sxs-lookup"><span data-stu-id="82611-104">Views in ASP.NET Core MVC</span></span>

<span data-ttu-id="82611-105">由[Steve Smith](https://ardalis.com/)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="82611-105">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="82611-106">本文件說明 ASP.NET Core MVC 應用程式中使用的檢視。</span><span class="sxs-lookup"><span data-stu-id="82611-106">This document explains views used in ASP.NET Core MVC applications.</span></span> <span data-ttu-id="82611-107">在 Razor 頁面上的資訊，請參閱[Razor 頁面的簡介](xref:mvc/razor-pages/index)。</span><span class="sxs-lookup"><span data-stu-id="82611-107">For information on Razor Pages, see [Introduction to Razor Pages](xref:mvc/razor-pages/index).</span></span>

<span data-ttu-id="82611-108">在**M**模型-**V**檢視-**C**ontroller (MVC) 模式*檢視*處理應用程式的資料呈現與使用者互動。</span><span class="sxs-lookup"><span data-stu-id="82611-108">In the **M**odel-**V**iew-**C**ontroller (MVC) pattern, the *view* handles the app's data presentation and user interaction.</span></span> <span data-ttu-id="82611-109">檢視是 HTML 範本與內嵌[Razor 標記](xref:mvc/views/razor)。</span><span class="sxs-lookup"><span data-stu-id="82611-109">A view is an HTML template with embedded [Razor markup](xref:mvc/views/razor).</span></span> <span data-ttu-id="82611-110">Razor 標記是與 HTML 標記，以產生傳送至用戶端的網頁互動的程式碼。</span><span class="sxs-lookup"><span data-stu-id="82611-110">Razor markup is code that interacts with HTML markup to produce a webpage that's sent to the client.</span></span>

<span data-ttu-id="82611-111">ASP.NET Core MVC 中，檢視都*.cshtml*檔案使用[C# 程式設計語言](/dotnet/csharp/)Razor 標記中。</span><span class="sxs-lookup"><span data-stu-id="82611-111">In ASP.NET Core MVC, views are *.cshtml* files that use the [C# programming language](/dotnet/csharp/) in Razor markup.</span></span> <span data-ttu-id="82611-112">通常，檢視檔案會分組成資料夾名為每個應用程式的[控制器](xref:mvc/controllers/actions)。</span><span class="sxs-lookup"><span data-stu-id="82611-112">Usually, view files are grouped into folders named for each of the app's [controllers](xref:mvc/controllers/actions).</span></span> <span data-ttu-id="82611-113">資料夾會儲存在*檢視*根目錄中的應用程式的資料夾：</span><span class="sxs-lookup"><span data-stu-id="82611-113">The folders are stored in a *Views* folder at the root of the app:</span></span>

![Visual Studio 方案總管 中的 檢視 資料夾是主資料夾 資料夾開啟顯示 About.cshtml、 Contact.cshtml 和 Index.cshtml 檔案開啟](overview/_static/views_solution_explorer.png)

<span data-ttu-id="82611-115">*首頁*控制器由*首頁*資料夾內*檢視*資料夾。</span><span class="sxs-lookup"><span data-stu-id="82611-115">The *Home* controller is represented by a *Home* folder inside the *Views* folder.</span></span> <span data-ttu-id="82611-116">*首頁*資料夾包含檢視*有關*，*連絡人*，和*索引*（首頁） 網頁。</span><span class="sxs-lookup"><span data-stu-id="82611-116">The *Home* folder contains the views for the *About*, *Contact*, and *Index* (homepage) webpages.</span></span> <span data-ttu-id="82611-117">當使用者要求這些三個網頁中的控制器動作的其中一個*首頁*控制器決定這三個檢視用來建置和網頁傳回給使用者。</span><span class="sxs-lookup"><span data-stu-id="82611-117">When a user requests one of these three webpages, controller actions in the *Home* controller determine which of the three views is used to build and return a webpage to the user.</span></span>

<span data-ttu-id="82611-118">使用[配置](xref:mvc/views/layout)來提供一致的網頁區段，並減少重複的程式碼。</span><span class="sxs-lookup"><span data-stu-id="82611-118">Use [layouts](xref:mvc/views/layout) to provide consistent webpage sections and reduce code repetition.</span></span> <span data-ttu-id="82611-119">配置通常包含標頭、 導覽和功能表項目和頁尾。</span><span class="sxs-lookup"><span data-stu-id="82611-119">Layouts often contain the header, navigation and menu elements, and the footer.</span></span> <span data-ttu-id="82611-120">頁首和頁尾通常包含許多中繼資料的項目和連結指令碼和樣式資產的未定案標記。</span><span class="sxs-lookup"><span data-stu-id="82611-120">The header and footer usually contain boilerplate markup for many metadata elements and links to script and style assets.</span></span> <span data-ttu-id="82611-121">配置可協助您避免在您檢視這個未定案標記。</span><span class="sxs-lookup"><span data-stu-id="82611-121">Layouts help you avoid this boilerplate markup in your views.</span></span>

<span data-ttu-id="82611-122">[部分檢視](xref:mvc/views/partial)管理檢視的可重複使用的組件時，減少程式碼重複。</span><span class="sxs-lookup"><span data-stu-id="82611-122">[Partial views](xref:mvc/views/partial) reduce code duplication by managing reusable parts of views.</span></span> <span data-ttu-id="82611-123">例如，部分檢視可用於數個檢視中會出現在部落格網站上的作者自傳。</span><span class="sxs-lookup"><span data-stu-id="82611-123">For example, a partial view is useful for an author biography on a blog website that appears in several views.</span></span> <span data-ttu-id="82611-124">作者自傳是一般的檢視內容，而不需要執行，才能產生的網頁內容的程式碼。</span><span class="sxs-lookup"><span data-stu-id="82611-124">An author biography is ordinary view content and doesn't require code to execute in order to produce the content for the webpage.</span></span> <span data-ttu-id="82611-125">作者自傳內容是內容的可檢視的模型繫結，方式，因此適合用於這種類型中的部分檢視。</span><span class="sxs-lookup"><span data-stu-id="82611-125">Author biography content is available to the view by model binding alone, so using a partial view for this type of content is ideal.</span></span>

<span data-ttu-id="82611-126">[檢視元件](xref:mvc/views/view-components)，在於它們可讓您減少重複的程式碼，都設為部分類似的檢視，但是它們很適用於需要在伺服器上呈現網頁所執行的程式碼的檢視內容。</span><span class="sxs-lookup"><span data-stu-id="82611-126">[View components](xref:mvc/views/view-components) are similar to partial views in that they allow you to reduce repetitive code, but they're appropriate for view content that requires code to run on the server in order to render the webpage.</span></span> <span data-ttu-id="82611-127">呈現的內容需要資料庫互動，例如購物車的網站時，檢視元件就很有用。</span><span class="sxs-lookup"><span data-stu-id="82611-127">View components are useful when the rendered content requires database interaction, such as for a website shopping cart.</span></span> <span data-ttu-id="82611-128">不限制檢視元件，才能產生網頁輸出模型繫結。</span><span class="sxs-lookup"><span data-stu-id="82611-128">View components aren't limited to model binding in order to produce webpage output.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="82611-129">使用檢視的優點</span><span class="sxs-lookup"><span data-stu-id="82611-129">Benefits of using views</span></span>

<span data-ttu-id="82611-130">檢視可以協助建立[ **S**eparation **o**f **C**oncerns (SoC) 設計](http://deviq.com/separation-of-concerns/)分隔從使用者介面標記 MVC 應用程式中應用程式的其他部分。</span><span class="sxs-lookup"><span data-stu-id="82611-130">Views help to establish a [**S**eparation **o**f **C**oncerns (SoC) design](http://deviq.com/separation-of-concerns/) within an MVC app by separating the user interface markup from other parts of the app.</span></span> <span data-ttu-id="82611-131">遵循 SoC 設計可讓您的應用程式模組，提供數個優點：</span><span class="sxs-lookup"><span data-stu-id="82611-131">Following SoC design makes your app modular, which provides several benefits:</span></span>

* <span data-ttu-id="82611-132">應用程式很容易維護，因為組織較佳。</span><span class="sxs-lookup"><span data-stu-id="82611-132">The app is easier to maintain because it's better organized.</span></span> <span data-ttu-id="82611-133">檢視通常會依應用程式功能分組。</span><span class="sxs-lookup"><span data-stu-id="82611-133">Views are generally grouped by app feature.</span></span> <span data-ttu-id="82611-134">這可讓您更輕鬆地找到相關的檢視，使用一項功能時。</span><span class="sxs-lookup"><span data-stu-id="82611-134">This makes it easier to find related views when working on a feature.</span></span>
* <span data-ttu-id="82611-135">鬆散耦合的應用程式組件。</span><span class="sxs-lookup"><span data-stu-id="82611-135">The parts of the app are loosely coupled.</span></span> <span data-ttu-id="82611-136">您可以建置並更新商務邏輯和資料存取元件分開的應用程式的檢視。</span><span class="sxs-lookup"><span data-stu-id="82611-136">You can build and update the app's views separately from the business logic and data access components.</span></span> <span data-ttu-id="82611-137">您可以修改應用程式的檢視，而不一定需要更新應用程式的其他部分。</span><span class="sxs-lookup"><span data-stu-id="82611-137">You can modify the views of the app without necessarily having to update other parts of the app.</span></span>
* <span data-ttu-id="82611-138">它是您更輕鬆地測試應用程式的使用者介面部分，因為檢視表的個別單位。</span><span class="sxs-lookup"><span data-stu-id="82611-138">It's easier to test the user interface parts of the app because the views are separate units.</span></span>
* <span data-ttu-id="82611-139">因為較佳的組織，而較不可能是您會意外地重複的區段的使用者介面。</span><span class="sxs-lookup"><span data-stu-id="82611-139">Due to better organization, it's less likely that you'll accidently repeat sections of the user interface.</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="82611-140">建立檢視</span><span class="sxs-lookup"><span data-stu-id="82611-140">Creating a view</span></span>

<span data-ttu-id="82611-141">檢視特定的控制站中建立*檢視 / [ControllerName]*資料夾。</span><span class="sxs-lookup"><span data-stu-id="82611-141">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="82611-142">在控制站之間共用的檢視會放在*Views/Shared*資料夾。</span><span class="sxs-lookup"><span data-stu-id="82611-142">Views that are shared among controllers are placed in the *Views/Shared* folder.</span></span> <span data-ttu-id="82611-143">若要建立的檢視，將新檔案，並提供相同的名稱與它關聯之的控制器的動作*.cshtml*檔案副檔名。</span><span class="sxs-lookup"><span data-stu-id="82611-143">To create a view, add a new file and give it the same name as its associated controller action with the *.cshtml* file extension.</span></span> <span data-ttu-id="82611-144">若要建立對應於檢視*有關*中的動作*首頁*控制器，建立*About.cshtml*檔案中*Views/Home*資料夾：</span><span class="sxs-lookup"><span data-stu-id="82611-144">To create a view that corresponds with the *About* action in the *Home* controller, create an *About.cshtml* file in the *Views/Home* folder:</span></span>

[!code-cshtml[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

<span data-ttu-id="82611-145">*Razor*標記開頭`@`符號。</span><span class="sxs-lookup"><span data-stu-id="82611-145">*Razor* markup starts with the `@` symbol.</span></span> <span data-ttu-id="82611-146">執行的 C# 陳述式放入 C# 程式碼內[Razor 程式碼區塊](xref:mvc/views/razor#razor-code-blocks)設定 off，大括號括住 (`{ ... }`)。</span><span class="sxs-lookup"><span data-stu-id="82611-146">Run C# statements by placing C# code within [Razor code blocks](xref:mvc/views/razor#razor-code-blocks) set off by curly braces (`{ ... }`).</span></span> <span data-ttu-id="82611-147">例如，請參閱 [關於] 的指派`ViewData["Title"]`如上所示。</span><span class="sxs-lookup"><span data-stu-id="82611-147">For example, see the assignment of "About" to `ViewData["Title"]` shown above.</span></span> <span data-ttu-id="82611-148">您可以顯示在 HTML 中的值只參考的值與`@`符號。</span><span class="sxs-lookup"><span data-stu-id="82611-148">You can display values within HTML by simply referencing the value with the `@` symbol.</span></span> <span data-ttu-id="82611-149">請參閱的內容`<h2>`和`<h3>`上述項目。</span><span class="sxs-lookup"><span data-stu-id="82611-149">See the contents of the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="82611-150">如上所示的檢視內容是只有一部分會呈現給使用者的整個網頁。</span><span class="sxs-lookup"><span data-stu-id="82611-150">The view content shown above is only part of the entire webpage that's rendered to the user.</span></span> <span data-ttu-id="82611-151">其餘的頁面配置和其他常用的部分檢視的其他檢視檔案中指定。</span><span class="sxs-lookup"><span data-stu-id="82611-151">The rest of the page's layout and other common aspects of the view are specified in other view files.</span></span> <span data-ttu-id="82611-152">若要進一步了解，請參閱[配置主題](xref:mvc/views/layout)。</span><span class="sxs-lookup"><span data-stu-id="82611-152">To learn more, see the [Layout topic](xref:mvc/views/layout).</span></span>

## <a name="how-controllers-specify-views"></a><span data-ttu-id="82611-153">控制器指定檢視的方式</span><span class="sxs-lookup"><span data-stu-id="82611-153">How controllers specify views</span></span>

<span data-ttu-id="82611-154">檢視通常會傳回動作為[ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult)，這是一種[ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult)。</span><span class="sxs-lookup"><span data-stu-id="82611-154">Views are typically returned from actions as a [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), which is a type of [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span></span> <span data-ttu-id="82611-155">動作方法可以建立並傳回`ViewResult`直接，但是一般來說，不進行。</span><span class="sxs-lookup"><span data-stu-id="82611-155">Your action method can create and return a `ViewResult` directly, but that isn't commonly done.</span></span> <span data-ttu-id="82611-156">因為大部分的控制站繼承自[控制器](/aspnet/core/api/microsoft.aspnetcore.mvc.controller)，您只需要使用`View`helper 方法以傳回`ViewResult`:</span><span class="sxs-lookup"><span data-stu-id="82611-156">Since most controllers inherit from [Controller](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), you simply use the `View` helper method to return the `ViewResult`:</span></span>

<span data-ttu-id="82611-157">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="82611-157">*HomeController.cs*</span></span>

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

<span data-ttu-id="82611-158">這個動作會傳回*About.cshtml*的最後一節中所顯示的檢視會轉譯為下列網頁：</span><span class="sxs-lookup"><span data-stu-id="82611-158">When this action returns, the *About.cshtml* view shown in the last section is rendered as the following webpage:</span></span>

![關於在 microsoft Edge 瀏覽器中呈現的頁面](overview/_static/about-page.png)

<span data-ttu-id="82611-160">`View` Helper 方法擁有數個多載。</span><span class="sxs-lookup"><span data-stu-id="82611-160">The `View` helper method has several overloads.</span></span> <span data-ttu-id="82611-161">您可以選擇性地指定：</span><span class="sxs-lookup"><span data-stu-id="82611-161">You can optionally specify:</span></span>

* <span data-ttu-id="82611-162">要傳回明確的檢視：</span><span class="sxs-lookup"><span data-stu-id="82611-162">An explicit view to return:</span></span>

  ```csharp
  return View("Orders");
  ```
* <span data-ttu-id="82611-163">A[模型](xref:mvc/models/model-binding)来傳遞至檢視：</span><span class="sxs-lookup"><span data-stu-id="82611-163">A [model](xref:mvc/models/model-binding) to pass to the the view:</span></span>

  ```csharp
  return View(Orders);
  ```
* <span data-ttu-id="82611-164">檢視和模型：</span><span class="sxs-lookup"><span data-stu-id="82611-164">Both a view and a model:</span></span>

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a><span data-ttu-id="82611-165">檢視探索</span><span class="sxs-lookup"><span data-stu-id="82611-165">View discovery</span></span>

<span data-ttu-id="82611-166">當動作傳回的檢視時，這個程序稱為*檢視探索*進行。</span><span class="sxs-lookup"><span data-stu-id="82611-166">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="82611-167">此程序決定根據檢視名稱使用的檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="82611-167">This process determines which view file is used based on the view name.</span></span> 

<span data-ttu-id="82611-168">預設行為`View`方法 (`return View();`) 會傳回與動作方法會呼叫同名的檢視。</span><span class="sxs-lookup"><span data-stu-id="82611-168">The default behavior of the `View` method (`return View();`) is to return a view with the same name as the action method from which it's called.</span></span> <span data-ttu-id="82611-169">例如，*有關*`ActionResult`控制站的方法名稱用來搜尋名為的檢視檔案*About.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="82611-169">For example, the *About* `ActionResult` method name of the controller is used to search for a view file named *About.cshtml*.</span></span> <span data-ttu-id="82611-170">首先，執行階段會尋找*檢視 / [ControllerName]*檢視的資料夾。</span><span class="sxs-lookup"><span data-stu-id="82611-170">First, the runtime looks in the *Views/[ControllerName]* folder for the view.</span></span> <span data-ttu-id="82611-171">如果找不到那里相符的檢視，它會搜尋*共用*檢視的資料夾。</span><span class="sxs-lookup"><span data-stu-id="82611-171">If it doesn't find a matching view there, it searches the *Shared* folder for the view.</span></span>

<span data-ttu-id="82611-172">如果您以隱含方式傳回，它並不重要`ViewResult`與`return View();`或明確地將傳遞至檢視表名稱`View`方法`return View("<ViewName>");`。</span><span class="sxs-lookup"><span data-stu-id="82611-172">It doesn't matter if you implicitly return the `ViewResult` with `return View();` or explicitly pass the view name to the `View` method with `return View("<ViewName>");`.</span></span> <span data-ttu-id="82611-173">在這兩種情況下，檢視探索會搜尋相符的檢視檔案，依此順序：</span><span class="sxs-lookup"><span data-stu-id="82611-173">In both cases, view discovery searches for a matching view file in this order:</span></span>

   1. <span data-ttu-id="82611-174">*檢視 /\[ControllerName]\[ViewName].cshtml*</span><span class="sxs-lookup"><span data-stu-id="82611-174">*Views/\[ControllerName]\[ViewName].cshtml*</span></span>
   1. <span data-ttu-id="82611-175">*檢視/共用/\[ViewName].cshtml*</span><span class="sxs-lookup"><span data-stu-id="82611-175">*Views/Shared/\[ViewName].cshtml*</span></span>

<span data-ttu-id="82611-176">可以提供的檢視檔案的路徑，而不是檢視表名稱。</span><span class="sxs-lookup"><span data-stu-id="82611-176">A view file path can be provided instead of a view name.</span></span> <span data-ttu-id="82611-177">如果使用 啟動應用程式根目錄的絕對路徑 (選擇性地從"/"或"~ /")，則*.cshtml*必須指定延伸模組：</span><span class="sxs-lookup"><span data-stu-id="82611-177">If using an absolute path starting at the app root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified:</span></span>

```csharp
return View("Views/Home/About.cshtml");
```

<span data-ttu-id="82611-178">您也可以使用相對路徑來指定不同的目錄，而檢視*.cshtml*延伸模組。</span><span class="sxs-lookup"><span data-stu-id="82611-178">You can also use a relative path to specify views in different directories without the *.cshtml* extension.</span></span> <span data-ttu-id="82611-179">內部`HomeController`，您可以傳回*索引*檢視您*管理*檢視具有相對路徑：</span><span class="sxs-lookup"><span data-stu-id="82611-179">Inside the `HomeController`, you can return the *Index* view of your *Manage* views with a relative path:</span></span>

```csharp
return View("../Manage/Index");
```

<span data-ttu-id="82611-180">同樣地，您可以指出與目前的控制站的特定目錄 」。 /"前置詞：</span><span class="sxs-lookup"><span data-stu-id="82611-180">Similarly, you can indicate the current controller-specific directory with the "./" prefix:</span></span>

```csharp
return View("./About");
```

<span data-ttu-id="82611-181">[部分檢視](xref:mvc/views/partial)和[檢視元件](xref:mvc/views/view-components)使用類似 （但不是完全相同） 的探索機制。</span><span class="sxs-lookup"><span data-stu-id="82611-181">[Partial views](xref:mvc/views/partial) and [view components](xref:mvc/views/view-components) use similar (but not identical) discovery mechanisms.</span></span>

<span data-ttu-id="82611-182">您可以自訂如何檢視位於應用程式中使用自訂的預設慣例[IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander)。</span><span class="sxs-lookup"><span data-stu-id="82611-182">You can customize the default convention for how views are located within the app by using a custom [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span></span>

<span data-ttu-id="82611-183">檢視探索依賴檔案名稱來尋找檢視的檔案。</span><span class="sxs-lookup"><span data-stu-id="82611-183">View discovery relies on finding view files by file name.</span></span> <span data-ttu-id="82611-184">如果基礎檔案系統區分大小寫，檢視名稱會可能區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="82611-184">If the underlying file system is case sensitive, view names are probably case sensitive.</span></span> <span data-ttu-id="82611-185">作業系統之間的相容性，請控制器和動作名稱與相關聯的檢視資料夾和檔案名稱之間的大小寫須相符。</span><span class="sxs-lookup"><span data-stu-id="82611-185">For compatibility across operating systems, match case between controller and action names and associated view folders and file names.</span></span> <span data-ttu-id="82611-186">如果您遇到的錯誤，使用區分大小寫的檔案系統時找不到檢視檔案，確認其大小寫符合要求的檢視檔案和實際的檢視檔案名稱之間。</span><span class="sxs-lookup"><span data-stu-id="82611-186">If you encounter an error that a view file can't be found while working with a case-sensitive file system, confirm that the casing matches between the requested view file and the actual view file name.</span></span>

<span data-ttu-id="82611-187">請遵循最佳作法來組織您的檢視，以反映控制器、 動作和可維護性和避免困擾的檢視之間的關聯性的檔案結構。</span><span class="sxs-lookup"><span data-stu-id="82611-187">Follow the best practice of organizing the file structure for your views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="82611-188">將資料傳遞至檢視</span><span class="sxs-lookup"><span data-stu-id="82611-188">Passing data to views</span></span>

<span data-ttu-id="82611-189">您可以將資料傳遞至使用數種方法的檢視。</span><span class="sxs-lookup"><span data-stu-id="82611-189">You can pass data to views using several approaches.</span></span> <span data-ttu-id="82611-190">最健全的作法是指定[模型](xref:mvc/models/model-binding)檢視中的型別。</span><span class="sxs-lookup"><span data-stu-id="82611-190">The most robust approach is to specify a [model](xref:mvc/models/model-binding) type in the view.</span></span> <span data-ttu-id="82611-191">這種模型通常稱為*viewmodel*。</span><span class="sxs-lookup"><span data-stu-id="82611-191">This model is commonly referred to as a *viewmodel*.</span></span> <span data-ttu-id="82611-192">您傳遞至檢視的 viewmodel 類型的執行個體的動作。</span><span class="sxs-lookup"><span data-stu-id="82611-192">You pass an instance of the viewmodel type to the view from the action.</span></span>

<span data-ttu-id="82611-193">將資料傳遞至檢視中使用 viewmodel 可讓檢視來善用*強式*類型檢查。</span><span class="sxs-lookup"><span data-stu-id="82611-193">Using a viewmodel to pass data to a view allows the view to take advantage of *strong* type checking.</span></span> <span data-ttu-id="82611-194">*強式類型*(或*強型別*) 表示每個變數和常數有明確定義的型別 (例如， `string`， `int`，或`DateTime`)。</span><span class="sxs-lookup"><span data-stu-id="82611-194">*Strong typing* (or *strongly-typed*) means that every variable and constant has an explicitly defined type (for example, `string`, `int`, or `DateTime`).</span></span> <span data-ttu-id="82611-195">在編譯時期檢查有效性的檢視中使用的類型。</span><span class="sxs-lookup"><span data-stu-id="82611-195">The validity of types used in a view is checked at compile time.</span></span>

<span data-ttu-id="82611-196">[Visual Studio](https://www.visualstudio.com/vs/)和[Visual Studio Code](https://code.visualstudio.com/)列出強型別類別成員使用一項功能稱為[IntelliSense](/visualstudio/ide/using-intellisense)。</span><span class="sxs-lookup"><span data-stu-id="82611-196">[Visual Studio](https://www.visualstudio.com/vs/) and [Visual Studio Code](https://code.visualstudio.com/) list strongly-typed class members using a feature called [IntelliSense](/visualstudio/ide/using-intellisense).</span></span> <span data-ttu-id="82611-197">當您想要查看的 viewmodel 屬性時，輸入變數名稱，後面接著句號 viewmodel (`.`)。</span><span class="sxs-lookup"><span data-stu-id="82611-197">When you want to see the properties of a viewmodel, type the variable name for the viewmodel followed by a period (`.`).</span></span> <span data-ttu-id="82611-198">這有助於減少錯誤更快撰寫程式碼。</span><span class="sxs-lookup"><span data-stu-id="82611-198">This helps you write code faster with fewer errors.</span></span>

<span data-ttu-id="82611-199">指定模型使用`@model`指示詞。</span><span class="sxs-lookup"><span data-stu-id="82611-199">Specify a model using the `@model` directive.</span></span> <span data-ttu-id="82611-200">使用模型與`@Model`:</span><span class="sxs-lookup"><span data-stu-id="82611-200">Use the model with `@Model`:</span></span>

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="82611-201">若要提供模型給檢視，控制器會將它做為參數：</span><span class="sxs-lookup"><span data-stu-id="82611-201">To provide the model to the view, the controller passes it as a parameter:</span></span>

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

<span data-ttu-id="82611-202">沒有任何限制，您可以提供給檢視的模型型別。</span><span class="sxs-lookup"><span data-stu-id="82611-202">There are no restrictions on the model types that you can provide to a view.</span></span> <span data-ttu-id="82611-203">我們建議您使用**P**lain **O**ld **C**LR **O**物件 (POCO) viewmodels 少量或沒有定義的行為 （方法）。</span><span class="sxs-lookup"><span data-stu-id="82611-203">We recommend using **P**lain **O**ld **C**LR **O**bject (POCO) viewmodels with little or no behavior (methods) defined.</span></span> <span data-ttu-id="82611-204">通常，viewmodel 類別可能會儲存在*模型*資料夾或個別*ViewModels*根目錄中的應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="82611-204">Usually, viewmodel classes are either stored in the *Models* folder or a separate *ViewModels* folder at the root of the app.</span></span> <span data-ttu-id="82611-205">*位址*viewmodel 使用上述範例中是儲存在名為 POCO viewmodel *Address.cs*:</span><span class="sxs-lookup"><span data-stu-id="82611-205">The *Address* viewmodel used in the example above is a POCO viewmodel stored in a file named *Address.cs*:</span></span>

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

> [!NOTE]
> <span data-ttu-id="82611-206">會造成任何問題 viewmodel 類型和您的商務模型類型使用相同的類別。</span><span class="sxs-lookup"><span data-stu-id="82611-206">Nothing prevents you from using the same classes for both your viewmodel types and your business model types.</span></span> <span data-ttu-id="82611-207">不過，使用不同的模型可讓您的檢視而異獨立的商務邏輯和資料存取應用程式部分。</span><span class="sxs-lookup"><span data-stu-id="82611-207">However, using separate models allows your views to vary independently from the business logic and data access parts of your app.</span></span> <span data-ttu-id="82611-208">模型和 viewmodels 分離也提供安全性優點，當模型使用時[模型繫結](xref:mvc/models/model-binding)和[驗證](xref:mvc/models/validation)資料傳送到應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="82611-208">Separation of models and viewmodels also offers security benefits when models use [model binding](xref:mvc/models/model-binding) and [validation](xref:mvc/models/validation) for data sent to the app by the user.</span></span>


<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-and-viewbag"></a><span data-ttu-id="82611-209">弱型別資料 （別的 ViewData 和 ViewBag）</span><span class="sxs-lookup"><span data-stu-id="82611-209">Weakly-typed data (ViewData and ViewBag)</span></span>

<span data-ttu-id="82611-210">注意：`ViewBag`不適用於 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="82611-210">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="82611-211">除了強型別檢視表檢視可以存取*弱型別*(也稱為*鬆散型別*) 的資料集合。</span><span class="sxs-lookup"><span data-stu-id="82611-211">In addition to strongly-typed views, views have access to a *weakly-typed* (also called *loosely-typed*) collection of data.</span></span> <span data-ttu-id="82611-212">不同於強式類型，*弱式類型*(或*鬆散類型*) 表示您沒有明確宣告的您所使用的資料類型。</span><span class="sxs-lookup"><span data-stu-id="82611-212">Unlike strong types, *weak types* (or *loose types*) means that you don't explicitly declare the type of data you're using.</span></span> <span data-ttu-id="82611-213">您可以使用弱式型別資料的集合，用來傳遞資料移轉入和控制器和檢視的資訊量很少。</span><span class="sxs-lookup"><span data-stu-id="82611-213">You can use the collection of weakly-typed data for passing small amounts of data in and out of controllers and views.</span></span>

| <span data-ttu-id="82611-214">傳遞之間的資料...</span><span class="sxs-lookup"><span data-stu-id="82611-214">Passing data between a ...</span></span>                        | <span data-ttu-id="82611-215">範例</span><span class="sxs-lookup"><span data-stu-id="82611-215">Example</span></span>                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="82611-216">在控制器與檢視</span><span class="sxs-lookup"><span data-stu-id="82611-216">Controller and a view</span></span>                             | <span data-ttu-id="82611-217">填入下拉式清單的資料。</span><span class="sxs-lookup"><span data-stu-id="82611-217">Populating a dropdown list with data.</span></span>                                          |
| <span data-ttu-id="82611-218">檢視和[版面配置檢視](xref:mvc/views/layout)</span><span class="sxs-lookup"><span data-stu-id="82611-218">View and a [layout view](xref:mvc/views/layout)</span></span>   | <span data-ttu-id="82611-219">設定**\<標題 >**版面配置檢視的檢視檔案中的項目內容。</span><span class="sxs-lookup"><span data-stu-id="82611-219">Setting the **\<title>** element content in the layout view from a view file.</span></span>  |
| <span data-ttu-id="82611-220">[部分檢視](xref:mvc/views/partial)和檢視</span><span class="sxs-lookup"><span data-stu-id="82611-220">[Partial view](xref:mvc/views/partial) and a view</span></span> | <span data-ttu-id="82611-221">一種 widget，會根據使用者要求網頁顯示資料。</span><span class="sxs-lookup"><span data-stu-id="82611-221">A widget that displays data based on the webpage that the user requested.</span></span>      |

<span data-ttu-id="82611-222">此集合可透過參考`ViewData`或`ViewBag`控制器和檢視上的屬性。</span><span class="sxs-lookup"><span data-stu-id="82611-222">This collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="82611-223">`ViewData`屬性是弱型別物件的字典。</span><span class="sxs-lookup"><span data-stu-id="82611-223">The `ViewData` property is a dictionary of weakly-typed objects.</span></span> <span data-ttu-id="82611-224">`ViewBag`屬性是周圍的包裝函式`ViewData`基礎提供動態內容`ViewData`集合。</span><span class="sxs-lookup"><span data-stu-id="82611-224">The `ViewBag` property is a wrapper around `ViewData` that provides dynamic properties for the underlying `ViewData` collection.</span></span>

<span data-ttu-id="82611-225">`ViewData`和`ViewBag`以動態方式在執行階段解析。</span><span class="sxs-lookup"><span data-stu-id="82611-225">`ViewData` and `ViewBag` are dynamically resolved at runtime.</span></span> <span data-ttu-id="82611-226">由於它們不提供編譯時間類型檢查，因此兩者都是通常更容易發生錯誤比使用 viewmodel。</span><span class="sxs-lookup"><span data-stu-id="82611-226">Since they don't offer compile-time type checking, both are generally more error-prone than using a viewmodel.</span></span> <span data-ttu-id="82611-227">基於這個原因，有些開發人員想要使用最少或從不`ViewData`和`ViewBag`。</span><span class="sxs-lookup"><span data-stu-id="82611-227">For that reason, some developers prefer to minimally or never use `ViewData` and `ViewBag`.</span></span>


<a name="VD"></a>

<span data-ttu-id="82611-228">**別的 viewData**</span><span class="sxs-lookup"><span data-stu-id="82611-228">**ViewData**</span></span>

<span data-ttu-id="82611-229">`ViewData`是[ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)物件透過存取`string`索引鍵。</span><span class="sxs-lookup"><span data-stu-id="82611-229">`ViewData` is a [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) object accessed through `string` keys.</span></span> <span data-ttu-id="82611-230">字串資料可以儲存和使用直接，而不需要轉型，但您必須轉換為其他`ViewData`物件特定類型的值，當您擷取它們。</span><span class="sxs-lookup"><span data-stu-id="82611-230">String data can be stored and used directly without the need for a cast, but you must cast other `ViewData` object values to specific types when you extract them.</span></span> <span data-ttu-id="82611-231">您可以使用`ViewData`來控制站中的資料傳遞至檢視和檢視，包括內[部分檢視](xref:mvc/views/partial)和[配置](xref:mvc/views/layout)。</span><span class="sxs-lookup"><span data-stu-id="82611-231">You can use `ViewData` to pass data from controllers to views and within views, including [partial views](xref:mvc/views/partial) and [layouts](xref:mvc/views/layout).</span></span>

<span data-ttu-id="82611-232">以下是設定問候語和位址使用的值範例`ViewData`動作：</span><span class="sxs-lookup"><span data-stu-id="82611-232">The following is an example that sets values for a greeting and an address using `ViewData` in an action:</span></span>

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

<span data-ttu-id="82611-233">使用檢視中的資料：</span><span class="sxs-lookup"><span data-stu-id="82611-233">Work with the data in a view:</span></span>

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

<span data-ttu-id="82611-234">**ViewBag**</span><span class="sxs-lookup"><span data-stu-id="82611-234">**ViewBag**</span></span>

<span data-ttu-id="82611-235">注意：`ViewBag`不適用於 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="82611-235">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="82611-236">`ViewBag`是[DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata)物件，提供儲存在物件的動態存取`ViewData`。</span><span class="sxs-lookup"><span data-stu-id="82611-236">`ViewBag` is a [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) object that provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="82611-237">`ViewBag`可能更方便使用，因為它不需要進行轉型。</span><span class="sxs-lookup"><span data-stu-id="82611-237">`ViewBag` can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="82611-238">下列範例示範如何使用`ViewBag`與使用相同的結果與`ViewData`上方：</span><span class="sxs-lookup"><span data-stu-id="82611-238">The following example shows how to use `ViewBag` with the same result as using `ViewData` above:</span></span>

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

<span data-ttu-id="82611-239">**同時使用別的 ViewData ViewBag**</span><span class="sxs-lookup"><span data-stu-id="82611-239">**Using ViewData and ViewBag simultaneously**</span></span>

<span data-ttu-id="82611-240">注意：`ViewBag`不適用於 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="82611-240">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="82611-241">因為`ViewData`和`ViewBag`參考相同的基礎`ViewData`集合，您可以同時使用`ViewData`和`ViewBag`並混用，而且符合之間讀取和寫入的值時。</span><span class="sxs-lookup"><span data-stu-id="82611-241">Since `ViewData` and `ViewBag` refer to the same underlying `ViewData` collection, you can use both `ViewData` and `ViewBag` and mix and match between them when reading and writing values.</span></span>

<span data-ttu-id="82611-242">設定標題使用`ViewBag`和描述使用`ViewData`頂端*About.cshtml*檢視：</span><span class="sxs-lookup"><span data-stu-id="82611-242">Set the title using `ViewBag` and the description using `ViewData` at the top of an *About.cshtml* view:</span></span>

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

<span data-ttu-id="82611-243">讀取內容，但是反向使用`ViewData`和`ViewBag`。</span><span class="sxs-lookup"><span data-stu-id="82611-243">Read the properties but reverse the use of `ViewData` and `ViewBag`.</span></span> <span data-ttu-id="82611-244">在*_Layout.cshtml*檔案中，取得標題使用`ViewData`並取得描述使用`ViewBag`:</span><span class="sxs-lookup"><span data-stu-id="82611-244">In the *_Layout.cshtml* file, obtain the title using `ViewData` and obtain the description using `ViewBag`:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

<span data-ttu-id="82611-245">請記住，字串不需要轉型為`ViewData`。</span><span class="sxs-lookup"><span data-stu-id="82611-245">Remember that strings don't require a cast for `ViewData`.</span></span> <span data-ttu-id="82611-246">您可以使用`@ViewData["Title"]`而不用轉型。</span><span class="sxs-lookup"><span data-stu-id="82611-246">You can use `@ViewData["Title"]` without casting.</span></span>

<span data-ttu-id="82611-247">使用這兩個`ViewData`和`ViewBag`在相同時間運作方式，為沒有混合和比對讀取和寫入的屬性。</span><span class="sxs-lookup"><span data-stu-id="82611-247">Using both `ViewData` and `ViewBag` at the same time works, as does mixing and matching reading and writing the properties.</span></span> <span data-ttu-id="82611-248">會呈現下列標記：</span><span class="sxs-lookup"><span data-stu-id="82611-248">The following markup is rendered:</span></span>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

<span data-ttu-id="82611-249">**ViewBag 別的 ViewData 之間差異的摘要**</span><span class="sxs-lookup"><span data-stu-id="82611-249">**Summary of the differences between ViewData and ViewBag**</span></span>

 <span data-ttu-id="82611-250">`ViewBag`無法使用 Razor 頁面中。</span><span class="sxs-lookup"><span data-stu-id="82611-250">`ViewBag` is not available in the Razor Pages.</span></span>

* `ViewData`
  * <span data-ttu-id="82611-251">衍生自[ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)，因此它的字典屬性會很有用，例如`ContainsKey`， `Add`， `Remove`，和`Clear`。</span><span class="sxs-lookup"><span data-stu-id="82611-251">Derives from [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), so it has dictionary properties that can be useful, such as `ContainsKey`, `Add`, `Remove`, and `Clear`.</span></span>
  * <span data-ttu-id="82611-252">字典中的索引鍵是字串，因此允許空白字元。</span><span class="sxs-lookup"><span data-stu-id="82611-252">Keys in the dictionary are strings, so whitespace is allowed.</span></span> <span data-ttu-id="82611-253">範例：`ViewData["Some Key With Whitespace"]`</span><span class="sxs-lookup"><span data-stu-id="82611-253">Example: `ViewData["Some Key With Whitespace"]`</span></span>
  * <span data-ttu-id="82611-254">任何型別以外`string`必須轉換中要使用的檢視`ViewData`。</span><span class="sxs-lookup"><span data-stu-id="82611-254">Any type other than a `string` must be cast in the view to use `ViewData`.</span></span>
* `ViewBag`
  * <span data-ttu-id="82611-255">衍生自[DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata)，因此它可以建立動態屬性使用點標記法 (`@ViewBag.SomeKey = <value or object>`)，而且不會執行轉換需要。</span><span class="sxs-lookup"><span data-stu-id="82611-255">Derives from [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), so it allows the creation of dynamic properties using dot notation (`@ViewBag.SomeKey = <value or object>`), and no casting is required.</span></span> <span data-ttu-id="82611-256">語法`ViewBag`可更快速地將加入至控制器和檢視。</span><span class="sxs-lookup"><span data-stu-id="82611-256">The syntax of `ViewBag` makes it quicker to add to controllers and views.</span></span>
  * <span data-ttu-id="82611-257">容易檢查 null 值。</span><span class="sxs-lookup"><span data-stu-id="82611-257">Simpler to check for null values.</span></span> <span data-ttu-id="82611-258">範例：`@ViewBag.Person?.Name`</span><span class="sxs-lookup"><span data-stu-id="82611-258">Example: `@ViewBag.Person?.Name`</span></span>

<span data-ttu-id="82611-259">**何時使用別的 ViewData 或 ViewBag**</span><span class="sxs-lookup"><span data-stu-id="82611-259">**When to use ViewData or ViewBag**</span></span>

<span data-ttu-id="82611-260">同時`ViewData`和`ViewBag`同樣會傳遞小量的資料在控制器和檢視之間的有效方法。</span><span class="sxs-lookup"><span data-stu-id="82611-260">Both `ViewData` and `ViewBag` are equally valid approaches for passing small amounts of data among controllers and views.</span></span> <span data-ttu-id="82611-261">若要使用哪一個選擇根據喜好設定。</span><span class="sxs-lookup"><span data-stu-id="82611-261">The choice of which one to use is based on preference.</span></span> <span data-ttu-id="82611-262">您可以混合和比對`ViewData`和`ViewBag`物件，不過，程式碼是容易讀取與維護與使用一致的其中一個方法。</span><span class="sxs-lookup"><span data-stu-id="82611-262">You can mix and match `ViewData` and `ViewBag` objects, however, the code is easier to read and maintain with one approach used consistently.</span></span> <span data-ttu-id="82611-263">這兩種方法是在執行階段以動態方式解決，因此容易導致執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="82611-263">Both approaches are dynamically resolved at runtime and thus prone to causing runtime errors.</span></span> <span data-ttu-id="82611-264">某些開發團隊予以避免。</span><span class="sxs-lookup"><span data-stu-id="82611-264">Some development teams avoid them.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="82611-265">動態檢視</span><span class="sxs-lookup"><span data-stu-id="82611-265">Dynamic views</span></span>

<span data-ttu-id="82611-266">不要宣告模型的檢視類型使用`@model`但可以是傳遞給它們的模型執行個體 (例如， `return View(Address);`) 可以動態地參考執行個體的屬性：</span><span class="sxs-lookup"><span data-stu-id="82611-266">Views that don't declare a model type using `@model` but that have a model instance passed to them (for example, `return View(Address);`) can reference the instance's properties dynamically:</span></span>

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="82611-267">此功能，可提供彈性，但並未提供編譯保護或 IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="82611-267">This feature offers flexibility but doesn't offer compilation protection or IntelliSense.</span></span> <span data-ttu-id="82611-268">如果屬性不存在，網頁產生在執行階段失敗。</span><span class="sxs-lookup"><span data-stu-id="82611-268">If the property doesn't exist, webpage generation fails at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="82611-269">詳細的檢視功能</span><span class="sxs-lookup"><span data-stu-id="82611-269">More view features</span></span>

<span data-ttu-id="82611-270">[標記協助程式](xref:mvc/views/tag-helpers/intro)讓您輕鬆將伺服器端行為加入至現有的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="82611-270">[Tag Helpers](xref:mvc/views/tag-helpers/intro) make it easy to add server-side behavior to existing HTML tags.</span></span> <span data-ttu-id="82611-271">使用標記協助程式，可避免需要撰寫自訂程式碼或在檢視內的協助程式。</span><span class="sxs-lookup"><span data-stu-id="82611-271">Using Tag Helpers avoids the need to write custom code or helpers within your views.</span></span> <span data-ttu-id="82611-272">標記協助程式會做為屬性套用至 HTML 元素，而且會忽略由無法處理它們的編輯器。</span><span class="sxs-lookup"><span data-stu-id="82611-272">Tag helpers are applied as attributes to HTML elements and are ignored by editors that can't process them.</span></span> <span data-ttu-id="82611-273">這可讓您編輯並呈現檢視標記中的各種工具。</span><span class="sxs-lookup"><span data-stu-id="82611-273">This allows you to edit and render view markup in a variety of tools.</span></span>

<span data-ttu-id="82611-274">許多內建的 HTML Helper 可達到產生自訂的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="82611-274">Generating custom HTML markup can be achieved with many built-in HTML Helpers.</span></span> <span data-ttu-id="82611-275">更複雜的使用者介面邏輯可以處理由[檢視元件](xref:mvc/views/view-components)。</span><span class="sxs-lookup"><span data-stu-id="82611-275">More complex user interface logic can be handled by [View Components](xref:mvc/views/view-components).</span></span> <span data-ttu-id="82611-276">檢視元件提供的相同 SoC 該控制站，並檢視所提供。</span><span class="sxs-lookup"><span data-stu-id="82611-276">View components provide the same SoC that controllers and views offer.</span></span> <span data-ttu-id="82611-277">它們不需要動作和一般使用者介面項目所使用的資料處理的檢視。</span><span class="sxs-lookup"><span data-stu-id="82611-277">They can eliminate the need for actions and views that deal with data used by common user interface elements.</span></span>

<span data-ttu-id="82611-278">如同許多其他 ASP.NET Core 方面，檢視支援[相依性插入](xref:fundamentals/dependency-injection)，讓服務變成[插入檢視](xref:mvc/views/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="82611-278">Like many other aspects of ASP.NET Core, views support [dependency injection](xref:fundamentals/dependency-injection), allowing services to be [injected into views](xref:mvc/views/dependency-injection).</span></span>
