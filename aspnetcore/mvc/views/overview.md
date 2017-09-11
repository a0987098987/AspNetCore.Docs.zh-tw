---
title: "檢視概觀"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 668c320d-c050-45e3-8161-2f460dc93b2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: 318d8832dadadd6946c7ffe58f9d89aaf68f54fc
ms.sourcegitcommit: 4693cb02d845adf2efa00e07ad432c81867bfa12
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/08/2017
---
# <a name="rendering-html-with-views-in-aspnet-core-mvc"></a><span data-ttu-id="99379-103">將 HTML 呈現使用 ASP.NET Core MVC 中的檢視</span><span class="sxs-lookup"><span data-stu-id="99379-103">Rendering HTML with views in ASP.NET Core MVC</span></span>

<span data-ttu-id="99379-104">由[Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="99379-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="99379-105">ASP.NET Core MVC 控制器可能會傳回使用的格式化的結果*檢視*。</span><span class="sxs-lookup"><span data-stu-id="99379-105">ASP.NET Core MVC controllers can return formatted results using *views*.</span></span>

## <a name="what-are-views"></a><span data-ttu-id="99379-106">檢視有哪些？</span><span class="sxs-lookup"><span data-stu-id="99379-106">What are Views?</span></span>

<span data-ttu-id="99379-107">在模型檢視控制器 (MVC) 模式中，*檢視*封裝與應用程式的使用者互動的呈現方式詳細資料。</span><span class="sxs-lookup"><span data-stu-id="99379-107">In the Model-View-Controller (MVC) pattern, the *view* encapsulates the presentation details of the user's interaction with the app.</span></span> <span data-ttu-id="99379-108">檢視是內嵌程式碼與 HTML 範本產生要傳送給用戶端內容。</span><span class="sxs-lookup"><span data-stu-id="99379-108">Views are HTML templates with embedded code that generate content to send to the client.</span></span> <span data-ttu-id="99379-109">檢視使用[Razor 語法](razor.md)，可讓程式碼與 HTML 互動最少的程式碼或儀式。</span><span class="sxs-lookup"><span data-stu-id="99379-109">Views use [Razor syntax](razor.md), which allows code to interact with HTML with minimal code or ceremony.</span></span>

<span data-ttu-id="99379-110">ASP.NET Core MVC 檢視*.cshtml*預設會在儲存檔案*檢視*應用程式中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="99379-110">ASP.NET Core MVC views are *.cshtml* files stored by default in a *Views* folder within the application.</span></span> <span data-ttu-id="99379-111">一般而言，每個控制器將具有自己的資料夾，其中都是適用於特定控制器動作的檢視。</span><span class="sxs-lookup"><span data-stu-id="99379-111">Typically, each controller will have its own folder, in which are views for specific controller actions.</span></span>

![在 [方案總管] 的 [檢視] 資料夾](overview/_static/views_solution_explorer.png)

<span data-ttu-id="99379-113">動作特定的檢視，除了[部分檢視](partial.md)，[版面配置和其他特殊的檢視檔案](layout.md)可用來協助降低重複，並允許在應用程式的檢視內的重複使用。</span><span class="sxs-lookup"><span data-stu-id="99379-113">In addition to action-specific views, [partial views](partial.md), [layouts, and other special view files](layout.md) can be used to help reduce repetition and allow for reuse within the app's views.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="99379-114">使用檢視的優點</span><span class="sxs-lookup"><span data-stu-id="99379-114">Benefits of Using Views</span></span>

<span data-ttu-id="99379-115">檢視提供[的重要性分離](http://deviq.com/separation-of-concerns/)MVC 應用程式，封裝使用者介面層級標記分開商務邏輯中。</span><span class="sxs-lookup"><span data-stu-id="99379-115">Views provide [separation of concerns](http://deviq.com/separation-of-concerns/) within an MVC app, encapsulating user interface level markup separately from business logic.</span></span> <span data-ttu-id="99379-116">ASP.NET MVC 檢視使用[Razor 語法](razor.md)，讓切換 HTML 標記和伺服器端邏輯輕鬆的方式。</span><span class="sxs-lookup"><span data-stu-id="99379-116">ASP.NET MVC views use [Razor syntax](razor.md) to make switching between HTML markup and server side logic painless.</span></span> <span data-ttu-id="99379-117">一般的重複性層面應用程式的使用者介面可輕易地重複使用的檢視之間[版面配置和共用的指示詞](layout.md)或[部分檢視](partial.md)。</span><span class="sxs-lookup"><span data-stu-id="99379-117">Common, repetitive aspects of the app's user interface can easily be reused between views using [layout and shared directives](layout.md) or [partial views](partial.md).</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="99379-118">建立檢視</span><span class="sxs-lookup"><span data-stu-id="99379-118">Creating a View</span></span>

<span data-ttu-id="99379-119">檢視特定的控制站中建立*檢視 / [ControllerName]*資料夾。</span><span class="sxs-lookup"><span data-stu-id="99379-119">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="99379-120">在控制站之間共用的檢視會放在*/檢視表/共用*資料夾。</span><span class="sxs-lookup"><span data-stu-id="99379-120">Views that are shared among controllers are placed in the */Views/Shared* folder.</span></span> <span data-ttu-id="99379-121">檢視檔案與它關聯之的控制器的動作，相同的名稱，並加入*.cshtml*檔案副檔名。</span><span class="sxs-lookup"><span data-stu-id="99379-121">Name the view file the same as its associated controller action, and add the *.cshtml* file extension.</span></span> <span data-ttu-id="99379-122">例如，若要建立的檢視*有關*動作*首頁*控制站，您就必須建立*About.cshtml*檔案 * /檢視表/首頁*資料夾。</span><span class="sxs-lookup"><span data-stu-id="99379-122">For example, to create a view for the *About* action on the *Home* controller, you would create the *About.cshtml* file in the */Views/Home* folder.</span></span>

<span data-ttu-id="99379-123">範例的檢視檔案 (*About.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="99379-123">A sample view file (*About.cshtml*):</span></span>

<span data-ttu-id="99379-124">[!code-html[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="99379-124">[!code-html[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]</span></span>

<span data-ttu-id="99379-125">*Razor*程式碼由表示`@`符號。</span><span class="sxs-lookup"><span data-stu-id="99379-125">*Razor* code is denoted by the `@` symbol.</span></span> <span data-ttu-id="99379-126">C# 陳述式會執行的 Razor 程式碼區塊，已設定 off 大括號內 (`{` `}`)，例如指派 [關於] 以`ViewData["Title"]`如上所示的項目。</span><span class="sxs-lookup"><span data-stu-id="99379-126">C# statements are run within Razor code blocks set off by curly braces (`{` `}`), such as the assignment of "About" to the `ViewData["Title"]` element shown above.</span></span> <span data-ttu-id="99379-127">Razor 可以用來顯示值在 HTML 內只參考的值與`@`符號，如中所示`<h2>`和`<h3>`上述項目。</span><span class="sxs-lookup"><span data-stu-id="99379-127">Razor can be used to display values within HTML by simply referencing the value with the `@` symbol, as shown within the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="99379-128">此檢視會著重於只輸出，它是負責的部分。</span><span class="sxs-lookup"><span data-stu-id="99379-128">This view focuses on just the portion of the output for which it is responsible.</span></span> <span data-ttu-id="99379-129">其餘的頁面配置和其他常用的部分檢視的其他地方指定。</span><span class="sxs-lookup"><span data-stu-id="99379-129">The rest of the page's layout, and other common aspects of the view, are specified elsewhere.</span></span> <span data-ttu-id="99379-130">深入了解[配置和共用的檢視邏輯](layout.md)。</span><span class="sxs-lookup"><span data-stu-id="99379-130">Learn more about [layout and shared view logic](layout.md).</span></span>

## <a name="how-do-controllers-specify-views"></a><span data-ttu-id="99379-131">控制器指定檢視如何？</span><span class="sxs-lookup"><span data-stu-id="99379-131">How do Controllers Specify Views?</span></span>

<span data-ttu-id="99379-132">檢視通常會傳回動作為`ViewResult`。</span><span class="sxs-lookup"><span data-stu-id="99379-132">Views are typically returned from actions as a `ViewResult`.</span></span> <span data-ttu-id="99379-133">動作方法可以建立並傳回`ViewResult`直接，但是通常如果您的控制器是繼承自`Controller`，只要您將使用`View`helper 方法與這個範例示範：</span><span class="sxs-lookup"><span data-stu-id="99379-133">Your action method can create and return a `ViewResult` directly, but more commonly if your controller inherits from `Controller`, you'll simply use the `View` helper method, as this example demonstrates:</span></span>

<span data-ttu-id="99379-134">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="99379-134">*HomeController.cs*</span></span>

<span data-ttu-id="99379-135">[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]</span><span class="sxs-lookup"><span data-stu-id="99379-135">[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]</span></span>

<span data-ttu-id="99379-136">`View` Helper 方法擁有數個多載來傳回檢視表更輕鬆地進行應用程式開發人員。</span><span class="sxs-lookup"><span data-stu-id="99379-136">The `View` helper method has several overloads to make returning views easier for app developers.</span></span> <span data-ttu-id="99379-137">您可以選擇性地指定要傳回的檢視，以及要傳遞至檢視的模型物件。</span><span class="sxs-lookup"><span data-stu-id="99379-137">You can optionally specify a view to return, as well as a model object to pass to the view.</span></span>

<span data-ttu-id="99379-138">這個動作會傳回*About.cshtml*呈現檢視上面所示：</span><span class="sxs-lookup"><span data-stu-id="99379-138">When this action returns, the *About.cshtml* view shown above is rendered:</span></span>

![有關頁面](overview/_static/about-page.png)

### <a name="view-discovery"></a><span data-ttu-id="99379-140">檢視探索</span><span class="sxs-lookup"><span data-stu-id="99379-140">View Discovery</span></span>

<span data-ttu-id="99379-141">當動作傳回的檢視時，這個程序稱為*檢視探索*進行。</span><span class="sxs-lookup"><span data-stu-id="99379-141">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="99379-142">此程序會決定將使用的檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="99379-142">This process determines which view file will be used.</span></span> <span data-ttu-id="99379-143">除非指定了特定的檢視檔案，則執行階段會尋找控制器特定檢視第一次，則會尋找相符的檢視名稱中*共用*資料夾。</span><span class="sxs-lookup"><span data-stu-id="99379-143">Unless a specific view file is specified, the runtime looks for a controller-specific view first, then looks for matching view name in the *Shared* folder.</span></span>

<span data-ttu-id="99379-144">當動作傳回`View`方法，就像這樣`return View();`，動作名稱當做檢視表名稱。</span><span class="sxs-lookup"><span data-stu-id="99379-144">When an action returns the `View` method, like so `return View();`, the action name is used as the view name.</span></span> <span data-ttu-id="99379-145">例如，如果這從名為"Index"的動作方法呼叫，將相當於 「 索引 」 的檢視名稱傳入。</span><span class="sxs-lookup"><span data-stu-id="99379-145">For example, if this were called from an action method named "Index", it would be equivalent to passing in a view name of "Index".</span></span> <span data-ttu-id="99379-146">檢視名稱可以明確地傳遞給方法 (`return View("SomeView");`)。</span><span class="sxs-lookup"><span data-stu-id="99379-146">A view name can be explicitly passed to the method (`return View("SomeView");`).</span></span> <span data-ttu-id="99379-147">在這兩種情況下，檢視探索會搜尋相符的檢視檔案中：</span><span class="sxs-lookup"><span data-stu-id="99379-147">In both of these cases, view discovery searches for a matching view file in:</span></span>

   1. <span data-ttu-id="99379-148">檢視 /\<ControllerName > /\<ViewName >.cshtml</span><span class="sxs-lookup"><span data-stu-id="99379-148">Views/\<ControllerName>/\<ViewName>.cshtml</span></span>

   2. <span data-ttu-id="99379-149">檢視/共用/\<ViewName >.cshtml</span><span class="sxs-lookup"><span data-stu-id="99379-149">Views/Shared/\<ViewName>.cshtml</span></span>

>[!TIP]
> <span data-ttu-id="99379-150">我們建議您遵循的慣例只傳回`View()`從可能的因為它會造成更具彈性且更容易重構程式碼時的動作。</span><span class="sxs-lookup"><span data-stu-id="99379-150">We recommend following the convention of simply returning `View()` from actions when possible, as it results in more flexible, easier to refactor code.</span></span>

<span data-ttu-id="99379-151">可以提供的檢視檔案的路徑，而不是檢視表名稱。</span><span class="sxs-lookup"><span data-stu-id="99379-151">A view file path can be provided, instead of a view name.</span></span> <span data-ttu-id="99379-152">如果使用 啟動應用程式根目錄的絕對路徑 (選擇性地起始與"/"或"~ /")、 *.cshtml*延伸模組必須指定檔案路徑的一部分。</span><span class="sxs-lookup"><span data-stu-id="99379-152">If using an absolute path starting at the application root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified as part of the file path.</span></span> <span data-ttu-id="99379-153">例如：`return View("Views/Home/About.cshtml");`。</span><span class="sxs-lookup"><span data-stu-id="99379-153">For example: `return View("Views/Home/About.cshtml");`.</span></span> <span data-ttu-id="99379-154">或者，您可以使用相對路徑內的控制站的特定目錄從*檢視*目錄中，指定在不同的目錄檢視。</span><span class="sxs-lookup"><span data-stu-id="99379-154">Alternatively, you can use a relative path from the controller-specific directory within the *Views* directory to specify views in different directories.</span></span> <span data-ttu-id="99379-155">例如：`return View("../Manage/Index");`內*首頁*控制站。</span><span class="sxs-lookup"><span data-stu-id="99379-155">For example: `return View("../Manage/Index");` inside the *Home* controller.</span></span> <span data-ttu-id="99379-156">同樣地，您便可以周遊的目前的控制器特定目錄： `return View("./About");`。</span><span class="sxs-lookup"><span data-stu-id="99379-156">Similarly, you can traverse the current controller-specific directory: `return View("./About");`.</span></span> <span data-ttu-id="99379-157">請注意，不要使用相對路徑*.cshtml*延伸模組。</span><span class="sxs-lookup"><span data-stu-id="99379-157">Note that relative paths don't use the *.cshtml* extension.</span></span> <span data-ttu-id="99379-158">如先前所述，請遵循組織檢視，以反映在控制器、 動作和可維護性和避免困擾的檢視之間的關聯性的檔案結構的最佳做法。</span><span class="sxs-lookup"><span data-stu-id="99379-158">As previously mentioned, follow the best practice of organizing the file structure for views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

> [!NOTE]
> <span data-ttu-id="99379-159">[部分檢視](partial.md)和[檢視元件](view-components.md)使用類似 （但不是完全相同） 的探索機制。</span><span class="sxs-lookup"><span data-stu-id="99379-159">[Partial views](partial.md) and [view components](view-components.md) use similar (but not identical) discovery mechanisms.</span></span>

> [!NOTE]
> <span data-ttu-id="99379-160">您可以自訂有關檢視的所在位置的應用程式中使用自訂的預設慣例`IViewLocationExpander`。</span><span class="sxs-lookup"><span data-stu-id="99379-160">You can customize the default convention regarding where views are located within the app by using a custom `IViewLocationExpander`.</span></span>

>[!TIP]
> <span data-ttu-id="99379-161">可能會區分大小寫的基礎檔案系統根據檢視名稱。</span><span class="sxs-lookup"><span data-stu-id="99379-161">View names may be case sensitive depending on the underlying file system.</span></span> <span data-ttu-id="99379-162">作業系統之間的相容性，永遠符合控制器和動作名稱與相關聯的檢視資料夾和檔案名稱之間的大小寫。</span><span class="sxs-lookup"><span data-stu-id="99379-162">For compatibility across operating systems, always match case between controller and action names and associated view folders and filenames.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="99379-163">將資料傳遞至檢視</span><span class="sxs-lookup"><span data-stu-id="99379-163">Passing Data to Views</span></span>

<span data-ttu-id="99379-164">您可以將資料傳遞至檢視使用數種機制。</span><span class="sxs-lookup"><span data-stu-id="99379-164">You can pass data to views using several mechanisms.</span></span> <span data-ttu-id="99379-165">最健全的作法是指定*模型*檢視中的類型 (通常稱為*viewmodel*，若要在區別商務網域模型類型)，然後將此類型的執行個體傳遞至檢視從動作。</span><span class="sxs-lookup"><span data-stu-id="99379-165">The most robust approach is to specify a *model* type in the view (commonly referred to as a *viewmodel*, to distinguish it from business domain model types), and then pass an instance of this type to the view from the action.</span></span> <span data-ttu-id="99379-166">我們建議您將資料傳遞至檢視使用模型或檢視模型。</span><span class="sxs-lookup"><span data-stu-id="99379-166">We recommend you use a model or view model to pass data to a view.</span></span> <span data-ttu-id="99379-167">這可讓檢視以充分利用強式型別檢查。</span><span class="sxs-lookup"><span data-stu-id="99379-167">This allows the view to take advantage of strong type checking.</span></span> <span data-ttu-id="99379-168">您可以指定的模型檢視，使用`@model`指示詞：</span><span class="sxs-lookup"><span data-stu-id="99379-168">You can specify a model for a view using the `@model` directive:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1]}} -->

```html
@model WebApplication1.ViewModels.Address
   <h2>Contact</h2>
   <address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

<span data-ttu-id="99379-169">一旦模型已指定的檢視，傳送至檢視執行個體可以存取在強類型的方式，使用`@Model`如上所示。</span><span class="sxs-lookup"><span data-stu-id="99379-169">Once a model has been specified for a view, the instance sent to the view can be accessed in a strongly-typed manner using `@Model` as shown above.</span></span> <span data-ttu-id="99379-170">若要提供檢視的模型類型的執行個體，控制器會將它做為參數：</span><span class="sxs-lookup"><span data-stu-id="99379-170">To provide an instance of the model type to the view, the controller passes it as a parameter:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

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

<span data-ttu-id="99379-171">沒有任何限制可供檢視，以做為模型的型別。</span><span class="sxs-lookup"><span data-stu-id="99379-171">There are no restrictions on the types that can be provided to a view as a model.</span></span> <span data-ttu-id="99379-172">我們建議您傳遞純舊 CLR 物件 (POCO) 檢視模型少甚至沒有行為，以便可以其他地方封裝商務邏輯，在應用程式。</span><span class="sxs-lookup"><span data-stu-id="99379-172">We recommend passing Plain Old CLR Object (POCO) view models with little or no behavior, so that business logic can be encapsulated elsewhere in the app.</span></span> <span data-ttu-id="99379-173">舉例來說，這種方法是*位址*viewmodel 上述範例中使用：</span><span class="sxs-lookup"><span data-stu-id="99379-173">An example of this approach is the *Address* viewmodel used in the example above:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

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
> <span data-ttu-id="99379-174">不會讓您為您的商務模型類型和顯示的模型型別使用相同的類別。</span><span class="sxs-lookup"><span data-stu-id="99379-174">Nothing prevents you from using the same classes as your business model types and your display model types.</span></span> <span data-ttu-id="99379-175">不過，使它們保持在個別可讓您檢視，以獨立會因您的網域或持續性的模型，並提供一些安全性優點 (針對使用者會將傳送至應用程式使用的模型[模型繫結](../models/model-binding.md))。</span><span class="sxs-lookup"><span data-stu-id="99379-175">However, keeping them separate allows your views to vary independently from your domain or persistence model, and can offer some security benefits as well (for models that users will send to the app using [model binding](../models/model-binding.md)).</span></span>

### <a name="loosely-typed-data"></a><span data-ttu-id="99379-176">鬆散型別的資料</span><span class="sxs-lookup"><span data-stu-id="99379-176">Loosely Typed Data</span></span>

<span data-ttu-id="99379-177">除了強型別檢視中，所有檢視都可以存取資料的鬆散型別集合。</span><span class="sxs-lookup"><span data-stu-id="99379-177">In addition to strongly typed views, all views have access to a loosely typed collection of data.</span></span> <span data-ttu-id="99379-178">這個相同的集合，可透過參考`ViewData`或`ViewBag`控制器和檢視上的屬性。</span><span class="sxs-lookup"><span data-stu-id="99379-178">This same collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="99379-179">`ViewBag`屬性是周圍的包裝函式`ViewData`，該集合上提供的動態檢視。</span><span class="sxs-lookup"><span data-stu-id="99379-179">The `ViewBag` property is a wrapper around `ViewData` that provides a dynamic view over that collection.</span></span> <span data-ttu-id="99379-180">它不是個別的集合。</span><span class="sxs-lookup"><span data-stu-id="99379-180">It is not a separate collection.</span></span>

<span data-ttu-id="99379-181">`ViewData`一個字典物件存取透過`string`索引鍵。</span><span class="sxs-lookup"><span data-stu-id="99379-181">`ViewData` is a dictionary object accessed through `string` keys.</span></span> <span data-ttu-id="99379-182">您可以儲存和擷取中的物件，必須先將它們轉換成特定類型，當您擷取它們。</span><span class="sxs-lookup"><span data-stu-id="99379-182">You can store and retrieve objects in it, and you'll need to cast them to a specific type when you extract them.</span></span> <span data-ttu-id="99379-183">您可以使用`ViewData`至控制器中的資料傳遞至檢視，以及檢視 （和部分檢視和配置） 內。</span><span class="sxs-lookup"><span data-stu-id="99379-183">You can use `ViewData` to pass data from a controller to views, as well as within views (and partial views and layouts).</span></span> <span data-ttu-id="99379-184">字串資料可以儲存並直接使用，而不需要轉型。</span><span class="sxs-lookup"><span data-stu-id="99379-184">String data can be stored and used directly, without the need for a cast.</span></span>

<span data-ttu-id="99379-185">設定某些值`ViewData`動作：</span><span class="sxs-lookup"><span data-stu-id="99379-185">Set some values for `ViewData` in an action:</span></span>

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

<span data-ttu-id="99379-186">使用檢視中的資料：</span><span class="sxs-lookup"><span data-stu-id="99379-186">Work with the data in a view:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3, 6]}} -->

```html
@{
       // Requires cast
       var address = ViewData["Address"] as Address;
   }

   @ViewData["Greeting"] World!

   <address>
       @address.Name<br />
       @address.Street<br />
       @address.City, @address.State @address.PostalCode
   </address>
   ```

<span data-ttu-id="99379-187">`ViewBag`物件提供儲存在物件的動態存取`ViewData`。</span><span class="sxs-lookup"><span data-stu-id="99379-187">The `ViewBag` objects provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="99379-188">這可以是更方便使用，因為它不需要進行轉型。</span><span class="sxs-lookup"><span data-stu-id="99379-188">This can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="99379-189">上面使用的相同範例`ViewBag`而不是強型別`address`檢視中的執行個體：</span><span class="sxs-lookup"><span data-stu-id="99379-189">The same example as above, using `ViewBag` instead of a strongly typed `address` instance in the view:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1, 4, 5, 6]}} -->

```html
@ViewBag.Greeting World!

   <address>
       @ViewBag.Address.Name<br />
       @ViewBag.Address.Street<br />
       @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
   </address>
   ```

> [!NOTE]
> <span data-ttu-id="99379-190">因為都會指向相同的基礎`ViewData`集合，您可以混合和比對之間`ViewData`和`ViewBag`讀取和寫入的值，如果方便時。</span><span class="sxs-lookup"><span data-stu-id="99379-190">Since both refer to the same underlying `ViewData` collection, you can mix and match between `ViewData` and `ViewBag` when reading and writing values, if convenient.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="99379-191">動態檢視</span><span class="sxs-lookup"><span data-stu-id="99379-191">Dynamic Views</span></span>

<span data-ttu-id="99379-192">檢視未宣告的模型型別，而沒有傳遞給它們的模型執行個體可以動態地參考這個執行個體。</span><span class="sxs-lookup"><span data-stu-id="99379-192">Views that do not declare a model type but have a model instance passed to them can reference this instance dynamically.</span></span> <span data-ttu-id="99379-193">例如，如果執行個體`Address`傳遞至檢視，不會宣告`@model`，檢視就仍然可以動態示參考執行個體的屬性：</span><span class="sxs-lookup"><span data-stu-id="99379-193">For example, if an instance of `Address` is passed to a view that doesn't declare an `@model`, the view would still be able to refer to the instance's properties dynamically as shown:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [13, 16, 17, 18]}} -->

```html
<address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

<span data-ttu-id="99379-194">此功能可提供一些彈性，但不是提供任何編譯保護或 IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="99379-194">This feature can offer some flexibility, but does not offer any compilation protection or IntelliSense.</span></span> <span data-ttu-id="99379-195">如果屬性不存在，頁面將會在執行階段失敗。</span><span class="sxs-lookup"><span data-stu-id="99379-195">If the property doesn't exist, the page will fail at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="99379-196">詳細的檢視功能</span><span class="sxs-lookup"><span data-stu-id="99379-196">More View Features</span></span>

<span data-ttu-id="99379-197">[標記協助程式](tag-helpers/intro.md)讓您輕鬆將伺服器端行為加入至現有的 HTML 標記，避免需要使用自訂程式碼或檢視內的協助程式。</span><span class="sxs-lookup"><span data-stu-id="99379-197">[Tag helpers](tag-helpers/intro.md) make it easy to add server-side behavior to existing HTML tags, avoiding the need to use custom code or helpers within views.</span></span> <span data-ttu-id="99379-198">標記協助程式做為屬性套用至 HTML 項目，則會忽略不熟悉，允許編輯和各種不同的工具所呈現的檢視標記的編輯器。</span><span class="sxs-lookup"><span data-stu-id="99379-198">Tag helpers are applied as attributes to HTML elements, which are ignored by editors that aren't familiar with them, allowing view markup to be edited and rendered in a variety of tools.</span></span> <span data-ttu-id="99379-199">標記協助程式有許多用途，而且特別是可以[使用表單](working-with-forms.md)容易得多。</span><span class="sxs-lookup"><span data-stu-id="99379-199">Tag helpers have many uses, and in particular can make [working with forms](working-with-forms.md) much easier.</span></span>

<span data-ttu-id="99379-200">產生自訂的 HTML 標記可達到許多內建 HTML Helper 和更複雜的 UI 邏輯 （可能具備它自己的資料需求） 可以封裝在[檢視元件](view-components.md)。</span><span class="sxs-lookup"><span data-stu-id="99379-200">Generating custom HTML markup can be achieved with many built-in HTML Helpers, and more complex UI logic (potentially with its own data requirements) can be encapsulated in [View Components](view-components.md).</span></span> <span data-ttu-id="99379-201">檢視元件提供相同重要性分離，控制器和檢視提供，並可免除動作和檢視表的需要來處理常見的 UI 項目所使用的資料。</span><span class="sxs-lookup"><span data-stu-id="99379-201">View components provide the same separation of concerns that controllers and views offer, and can eliminate the need for actions and views to deal with data used by common UI elements.</span></span>

<span data-ttu-id="99379-202">如同許多其他 ASP.NET Core 方面，檢視支援[相依性插入](../../fundamentals/dependency-injection.md)，讓服務變成[插入檢視](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="99379-202">Like many other aspects of ASP.NET Core, views support [dependency injection](../../fundamentals/dependency-injection.md), allowing services to be [injected into views](dependency-injection.md).</span></span>
