---
title: ASP.NET Core 中的配置
author: ardalis
description: 了解如何先使用通用配置、共用指示詞，以及執行通用程式碼，再轉譯 ASP.NET 應用程式中的檢視。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/layout
ms.openlocfilehash: 8e89c8e6cf18c47abb6bf432cdc6bb6b97e8aeb0
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
# <a name="layout-in-aspnet-core"></a><span data-ttu-id="14457-103">ASP.NET Core 中的配置</span><span class="sxs-lookup"><span data-stu-id="14457-103">Layout in ASP.NET Core</span></span>

<span data-ttu-id="14457-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="14457-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="14457-105">檢視經常會共用視覺效果和程式設計項目。</span><span class="sxs-lookup"><span data-stu-id="14457-105">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="14457-106">在本文中，您將了解如何先使用通用配置、共用指示詞，以及執行通用程式碼，再轉譯 ASP.NET 應用程式中的檢視。</span><span class="sxs-lookup"><span data-stu-id="14457-106">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="14457-107">何謂配置</span><span class="sxs-lookup"><span data-stu-id="14457-107">What is a Layout</span></span>

<span data-ttu-id="14457-108">大部分的 Web 應用程式都會有通用配置，可提供使用者一致的巡覽頁面體驗。</span><span class="sxs-lookup"><span data-stu-id="14457-108">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="14457-109">配置通常會包括一般使用者介面項目，例如應用程式標頭、導覽或功能表項目，以及頁尾。</span><span class="sxs-lookup"><span data-stu-id="14457-109">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![頁面配置範例](layout/_static/page-layout.png)

<span data-ttu-id="14457-111">應用程式內的許多頁面也經常使用指令碼和樣式表這類一般 HTML 結構。</span><span class="sxs-lookup"><span data-stu-id="14457-111">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="14457-112">所有這些共用項目可能都定義在 *layout* 檔案中，而之後應用程式內使用的任何檢視都可以參考該檔案。</span><span class="sxs-lookup"><span data-stu-id="14457-112">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="14457-113">配置可減少檢視中的重複程式碼，協助它們遵循[不重複 (DRY) 原則](http://deviq.com/don-t-repeat-yourself/)。</span><span class="sxs-lookup"><span data-stu-id="14457-113">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="14457-114">依照慣例，ASP.NET 應用程式的預設配置命名為 `_Layout.cshtml`。</span><span class="sxs-lookup"><span data-stu-id="14457-114">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="14457-115">Visual Studio ASP.NET Core MVC 專案範本會將此配置檔案包含在 `Views/Shared` 資料夾中：</span><span class="sxs-lookup"><span data-stu-id="14457-115">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![方案總管中的 views 資料夾](layout/_static/web-project-views.png)

<span data-ttu-id="14457-117">此配置定義應用程式中檢視的最上層範本。</span><span class="sxs-lookup"><span data-stu-id="14457-117">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="14457-118">應用程式不需要配置，而且應用程式可以定義數個配置，即不同的檢視指定不同的配置。</span><span class="sxs-lookup"><span data-stu-id="14457-118">Apps don't require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="14457-119">範例 `_Layout.cshtml`：</span><span class="sxs-lookup"><span data-stu-id="14457-119">An example `_Layout.cshtml`:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="14457-120">指定配置</span><span class="sxs-lookup"><span data-stu-id="14457-120">Specifying a Layout</span></span>

<span data-ttu-id="14457-121">Razor 檢視具有 `Layout` 屬性。</span><span class="sxs-lookup"><span data-stu-id="14457-121">Razor views have a `Layout` property.</span></span> <span data-ttu-id="14457-122">個別檢視透過設定此屬性來指定配置：</span><span class="sxs-lookup"><span data-stu-id="14457-122">Individual views specify a layout by setting this property:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="14457-123">指定的配置可以使用完整路徑 (範例：`/Views/Shared/_Layout.cshtml`) 或部分名稱 (範例：`_Layout`)。</span><span class="sxs-lookup"><span data-stu-id="14457-123">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="14457-124">提供部分名稱時，Razor 檢視引擎將使用其標準探索程序來搜尋配置檔案。</span><span class="sxs-lookup"><span data-stu-id="14457-124">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="14457-125">會先搜尋控制器相關聯資料夾，接著再搜尋 `Shared` 資料夾。</span><span class="sxs-lookup"><span data-stu-id="14457-125">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="14457-126">此探索程序相當於用來探索[部分檢視](partial.md)的程序。</span><span class="sxs-lookup"><span data-stu-id="14457-126">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="14457-127">根據預設，每個配置都必須呼叫 `RenderBody`。</span><span class="sxs-lookup"><span data-stu-id="14457-127">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="14457-128">不論在何處呼叫 `RenderBody`，都會轉譯檢視內容。</span><span class="sxs-lookup"><span data-stu-id="14457-128">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="14457-129">章節</span><span class="sxs-lookup"><span data-stu-id="14457-129">Sections</span></span>

<span data-ttu-id="14457-130">配置可以選擇性地呼叫 `RenderSection`，以參考一或多個「區段」。</span><span class="sxs-lookup"><span data-stu-id="14457-130">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="14457-131">區段提供一種方式，來組織特定頁面項目應放置的位置。</span><span class="sxs-lookup"><span data-stu-id="14457-131">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="14457-132">每次呼叫 `RenderSection` 都可以指定該區段是必要區段還是選擇性區段。</span><span class="sxs-lookup"><span data-stu-id="14457-132">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="14457-133">如果找不到必要區段，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="14457-133">If a required section isn't found, an exception will be thrown.</span></span> <span data-ttu-id="14457-134">個別檢視指定要使用 `@section` Razor 語法在區段內轉譯的內容。</span><span class="sxs-lookup"><span data-stu-id="14457-134">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="14457-135">如果檢視定義區段，則必須進行轉譯 (或發生錯誤)。</span><span class="sxs-lookup"><span data-stu-id="14457-135">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="14457-136">檢視中的範例 `@section` 定義：</span><span class="sxs-lookup"><span data-stu-id="14457-136">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="14457-137">在上述程式碼中，會將驗證指令碼新增至包含表單之檢視上的 `scripts` 區段。</span><span class="sxs-lookup"><span data-stu-id="14457-137">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="14457-138">相同應用程式中的其他檢視可能不需要任何額外的指令碼，因此不需要定義 scripts 區段。</span><span class="sxs-lookup"><span data-stu-id="14457-138">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="14457-139">檢視中所定義的區段僅適用於其立即配置頁面。</span><span class="sxs-lookup"><span data-stu-id="14457-139">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="14457-140">無法從部分、檢視元件或檢視系統的其他部分參考它們。</span><span class="sxs-lookup"><span data-stu-id="14457-140">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="14457-141">忽略區段</span><span class="sxs-lookup"><span data-stu-id="14457-141">Ignoring sections</span></span>

<span data-ttu-id="14457-142">根據預設，內容頁面中的本文和所有區段都必須透過配置頁面進行轉譯。</span><span class="sxs-lookup"><span data-stu-id="14457-142">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="14457-143">Razor 檢視引擎會強制執行此作業，方法是追蹤是否已轉譯本文和每個區段。</span><span class="sxs-lookup"><span data-stu-id="14457-143">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="14457-144">若要指示檢視引擎略過本文或區段，請呼叫 `IgnoreBody` 和 `IgnoreSection` 方法。</span><span class="sxs-lookup"><span data-stu-id="14457-144">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="14457-145">Razor 頁面中的本文和每個區段都必須進行轉譯或忽略。</span><span class="sxs-lookup"><span data-stu-id="14457-145">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="14457-146">匯入共用指示詞</span><span class="sxs-lookup"><span data-stu-id="14457-146">Importing Shared Directives</span></span>

<span data-ttu-id="14457-147">檢視可以使用 Razor 指示詞來執行許多動作，例如匯入命名空間或執行[相依性插入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="14457-147">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="14457-148">許多檢視所共用的指示詞可能指定於通用 `_ViewImports.cshtml` 檔案中。</span><span class="sxs-lookup"><span data-stu-id="14457-148">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="14457-149">`_ViewImports` 檔案支援下列指示詞：</span><span class="sxs-lookup"><span data-stu-id="14457-149">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="14457-150">檔案不支援其他 Razor 功能，例如函式和區段定義。</span><span class="sxs-lookup"><span data-stu-id="14457-150">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="14457-151">範例 `_ViewImports.cshtml` 檔案：</span><span class="sxs-lookup"><span data-stu-id="14457-151">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="14457-152">ASP.NET Core MVC 應用程式的 `_ViewImports.cshtml` 檔案通常會放在 `Views` 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="14457-152">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="14457-153">`_ViewImports.cshtml` 檔案可以放在任何資料夾內；在此情況下，它只會套用至該資料夾和其子資料夾內的檢視。</span><span class="sxs-lookup"><span data-stu-id="14457-153">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="14457-154">`_ViewImports` 檔案的處理是從根層級開始，然後針對到檢視本身位置的所有資料夾；因此，在資料夾層級，可能會覆寫根層級上指定的設定。</span><span class="sxs-lookup"><span data-stu-id="14457-154">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="14457-155">例如，如果根層級 `_ViewImports.cshtml` 檔案指定 `@model` 和 `@addTagHelper`，但檢視之控制器相關聯資料夾中的另一個 `_ViewImports.cshtml` 檔案指定不同的 `@model` 並新增另一個 `@addTagHelper`，則檢視可以存取這兩個標籤協助程式，並使用後面這個 `@model`。</span><span class="sxs-lookup"><span data-stu-id="14457-155">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="14457-156">如果為檢視執行多個 `_ViewImports.cshtml` 檔案，則 `ViewImports.cshtml` 檔案中所含指示詞的結合行為將如下：</span><span class="sxs-lookup"><span data-stu-id="14457-156">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="14457-157">`@addTagHelper`、`@removeTagHelper`：全部依序執行</span><span class="sxs-lookup"><span data-stu-id="14457-157">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="14457-158">`@tagHelperPrefix`：最接近檢視的項目會覆寫任何其他項目</span><span class="sxs-lookup"><span data-stu-id="14457-158">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="14457-159">`@model`：最接近檢視的項目會覆寫任何其他項目</span><span class="sxs-lookup"><span data-stu-id="14457-159">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="14457-160">`@inherits`：最接近檢視的項目會覆寫任何其他項目</span><span class="sxs-lookup"><span data-stu-id="14457-160">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="14457-161">`@using`：全部包括，但忽略重複項目</span><span class="sxs-lookup"><span data-stu-id="14457-161">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="14457-162">`@inject`：針對每個屬性，最接近檢視的項目會覆寫任何其他具有相同屬性名稱的項目</span><span class="sxs-lookup"><span data-stu-id="14457-162">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="14457-163">在每個檢視之前執行程式碼</span><span class="sxs-lookup"><span data-stu-id="14457-163">Running Code Before Each View</span></span>

<span data-ttu-id="14457-164">如果您的程式碼需要在每個檢視之前執行，則這應該放在 `_ViewStart.cshtml` 檔案中。</span><span class="sxs-lookup"><span data-stu-id="14457-164">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="14457-165">依照慣例，`_ViewStart.cshtml` 檔案位於 `Views` 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="14457-165">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="14457-166">`_ViewStart.cshtml` 中所列的陳述式會在每個完整檢視之前執行 (不是配置，也不是部分檢視)。</span><span class="sxs-lookup"><span data-stu-id="14457-166">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="14457-167">與 [ViewImports.cshtml](xref:mvc/views/layout#viewimports) 類似，`_ViewStart.cshtml` 是階層式的。</span><span class="sxs-lookup"><span data-stu-id="14457-167">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="14457-168">如果 `_ViewStart.cshtml` 檔案是定義於控制器相關聯檢視資料夾，則會在 `Views` 資料夾 (如果有的話) 根目錄中所定義的檔案後面進行執行。</span><span class="sxs-lookup"><span data-stu-id="14457-168">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="14457-169">範例 `_ViewStart.cshtml` 檔案：</span><span class="sxs-lookup"><span data-stu-id="14457-169">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="14457-170">上面的檔案指定所有檢視都會使用 `_Layout.cshtml` 配置。</span><span class="sxs-lookup"><span data-stu-id="14457-170">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="14457-171">`_ViewStart.cshtml` 和 `_ViewImports.cshtml` 通常都不是放在 `/Views/Shared` 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="14457-171">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="14457-172">這些檔案的應用程式層級版本應該直接放在 `/Views` 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="14457-172">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
