---
title: ASP.NET Core 中的配置
author: ardalis
description: 了解如何先使用通用配置、共用指示詞，以及執行通用程式碼，再轉譯 ASP.NET 應用程式中的檢視。
ms.author: riande
ms.date: 07/30/2019
uid: mvc/views/layout
ms.openlocfilehash: db8c6c30397593c1a8375ebc800c1c0e34d241cb
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667899"
---
# <a name="layout-in-aspnet-core"></a><span data-ttu-id="b699c-103">ASP.NET Core 中的配置</span><span class="sxs-lookup"><span data-stu-id="b699c-103">Layout in ASP.NET Core</span></span>

<span data-ttu-id="b699c-104">作者：[Steve Smith](https://ardalis.com/) 和 [Dave Brock](https://twitter.com/daveabrock)</span><span class="sxs-lookup"><span data-stu-id="b699c-104">By [Steve Smith](https://ardalis.com/) and [Dave Brock](https://twitter.com/daveabrock)</span></span>

<span data-ttu-id="b699c-105">頁面和檢視經常會共用視覺效果和程式設計項目。</span><span class="sxs-lookup"><span data-stu-id="b699c-105">Pages and views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="b699c-106">本文示範如何：</span><span class="sxs-lookup"><span data-stu-id="b699c-106">This article demonstrates how to:</span></span>

* <span data-ttu-id="b699c-107">使用常見配置。</span><span class="sxs-lookup"><span data-stu-id="b699c-107">Use common layouts.</span></span>
* <span data-ttu-id="b699c-108">共用指示詞。</span><span class="sxs-lookup"><span data-stu-id="b699c-108">Share directives.</span></span>
* <span data-ttu-id="b699c-109">執行常見的程式碼，再轉譯頁面或檢視。</span><span class="sxs-lookup"><span data-stu-id="b699c-109">Run common code before rendering pages or views.</span></span>

<span data-ttu-id="b699c-110">本文件將探討 ASP.NET Core MVC 兩種不同方法的配置：Razor Pages 和包含檢視的控制器。</span><span class="sxs-lookup"><span data-stu-id="b699c-110">This document discusses layouts for the two different approaches to ASP.NET Core MVC: Razor Pages and controllers with views.</span></span> <span data-ttu-id="b699c-111">針對本主題，差異很小：</span><span class="sxs-lookup"><span data-stu-id="b699c-111">For this topic, the differences are minimal:</span></span>

* <span data-ttu-id="b699c-112">Razor Pages 位於 *Pages* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b699c-112">Razor Pages are in the *Pages* folder.</span></span>
* <span data-ttu-id="b699c-113">包含檢視的控制器，使用 *Views* 資料夾進行檢視。</span><span class="sxs-lookup"><span data-stu-id="b699c-113">Controllers with views uses a *Views* folder for views.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="b699c-114">何謂配置</span><span class="sxs-lookup"><span data-stu-id="b699c-114">What is a Layout</span></span>

<span data-ttu-id="b699c-115">大部分的 Web 應用程式都會有通用配置，可提供使用者一致的巡覽頁面體驗。</span><span class="sxs-lookup"><span data-stu-id="b699c-115">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="b699c-116">配置通常會包括一般使用者介面項目，例如應用程式標頭、導覽或功能表項目，以及頁尾。</span><span class="sxs-lookup"><span data-stu-id="b699c-116">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![頁面配置範例](layout/_static/page-layout.png)

<span data-ttu-id="b699c-118">應用程式內的許多頁面也經常使用指令碼和樣式表這類一般 HTML 結構。</span><span class="sxs-lookup"><span data-stu-id="b699c-118">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="b699c-119">所有這些共用項目可能都定義在 *layout* 檔案中，而之後應用程式內使用的任何檢視都可以參考該檔案。</span><span class="sxs-lookup"><span data-stu-id="b699c-119">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="b699c-120">配置可減少檢視中的重複程式碼。</span><span class="sxs-lookup"><span data-stu-id="b699c-120">Layouts reduce duplicate code in views.</span></span>

<span data-ttu-id="b699c-121">依照慣例，ASP.NET Core 應用程式的預設配置命名為 *_Layout.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="b699c-121">By convention, the default layout for an ASP.NET Core app is named *_Layout.cshtml*.</span></span> <span data-ttu-id="b699c-122">使用範本建立的新 ASP.NET Core 專案配置檔案為：</span><span class="sxs-lookup"><span data-stu-id="b699c-122">The layout files for new ASP.NET Core projects created with the templates are:</span></span>

* <span data-ttu-id="b699c-123">Razor Pages：*Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b699c-123">Razor Pages: *Pages/Shared/_Layout.cshtml*</span></span>

  ![方案總管中的 Pages 資料夾](layout/_static/rp-web-project-views.png)

* <span data-ttu-id="b699c-125">包含檢視的控制器：*Views/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b699c-125">Controller with views: *Views/Shared/_Layout.cshtml*</span></span>

  ![方案總管中的 Views 資料夾](layout/_static/mvc-web-project-views.png)

<span data-ttu-id="b699c-127">此配置會定義應用程式中檢視的最上層範本。</span><span class="sxs-lookup"><span data-stu-id="b699c-127">The layout defines a top level template for views in the app.</span></span> <span data-ttu-id="b699c-128">應用程式不需要配置。</span><span class="sxs-lookup"><span data-stu-id="b699c-128">Apps don't require a layout.</span></span> <span data-ttu-id="b699c-129">應用程式可以定義數個配置，即不同的檢視指定不同的配置。</span><span class="sxs-lookup"><span data-stu-id="b699c-129">Apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="b699c-130">下列程式碼顯示使用範本建立之專案 (包含控制器和檢視) 的配置檔案：</span><span class="sxs-lookup"><span data-stu-id="b699c-130">The following code shows the layout file for a template created project with a controller and views:</span></span>

[!code-cshtml[](~/common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=44,72)]

## <a name="specifying-a-layout"></a><span data-ttu-id="b699c-131">指定配置</span><span class="sxs-lookup"><span data-stu-id="b699c-131">Specifying a Layout</span></span>

<span data-ttu-id="b699c-132">Razor 檢視具有 `Layout` 屬性。</span><span class="sxs-lookup"><span data-stu-id="b699c-132">Razor views have a `Layout` property.</span></span> <span data-ttu-id="b699c-133">個別檢視透過設定此屬性來指定配置：</span><span class="sxs-lookup"><span data-stu-id="b699c-133">Individual views specify a layout by setting this property:</span></span>

[!code-cshtml[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="b699c-134">指定的配置可以使用完整路徑 (例如 */Pages/Shared/_Layout.cshtml* 或 */Views/Shared/_Layout.cshtml*) 或部分名稱 (例如：`_Layout`)。</span><span class="sxs-lookup"><span data-stu-id="b699c-134">The layout specified can use a full path (for example, */Pages/Shared/_Layout.cshtml* or */Views/Shared/_Layout.cshtml*) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="b699c-135">提供部分名稱時，Razor 檢視引擎會使用其標準探索程序來搜尋配置檔案。</span><span class="sxs-lookup"><span data-stu-id="b699c-135">When a partial name is provided, the Razor view engine searches for the layout file using its standard discovery process.</span></span> <span data-ttu-id="b699c-136">首先搜尋處理常式方法 (或控制器) 所在的資料夾，接著搜尋 *Shared* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b699c-136">The folder where the handler method (or controller) exists is searched first, followed by the *Shared* folder.</span></span> <span data-ttu-id="b699c-137">此探索程序相當於用來探索[部分檢視](xref:mvc/views/partial#partial-view-discovery)的程序。</span><span class="sxs-lookup"><span data-stu-id="b699c-137">This discovery process is identical to the process used to discover [partial views](xref:mvc/views/partial#partial-view-discovery).</span></span>

<span data-ttu-id="b699c-138">根據預設，每個配置都必須呼叫 `RenderBody`。</span><span class="sxs-lookup"><span data-stu-id="b699c-138">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="b699c-139">不論在何處呼叫 `RenderBody`，都會轉譯檢視內容。</span><span class="sxs-lookup"><span data-stu-id="b699c-139">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>
<!-- https://stackoverflow.com/questions/23327578 -->
### <a name="sections"></a><span data-ttu-id="b699c-140">章節</span><span class="sxs-lookup"><span data-stu-id="b699c-140">Sections</span></span>

<span data-ttu-id="b699c-141">配置可以選擇性地呼叫 *，以參考一或多個「區段」* `RenderSection`。</span><span class="sxs-lookup"><span data-stu-id="b699c-141">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="b699c-142">區段提供一種方式，來組織特定頁面項目應放置的位置。</span><span class="sxs-lookup"><span data-stu-id="b699c-142">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="b699c-143">每次呼叫 `RenderSection`，都可以指定該區段是必要區段或是選擇性區段：</span><span class="sxs-lookup"><span data-stu-id="b699c-143">Each call to `RenderSection` can specify whether that section is required or optional:</span></span>

```html
<script type="text/javascript" src="~/scripts/global.js"></script>

@RenderSection("Scripts", required: false)
```

<span data-ttu-id="b699c-144">如果找不到必要區段，將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b699c-144">If a required section isn't found, an exception is thrown.</span></span> <span data-ttu-id="b699c-145">個別檢視指定要使用 `@section` Razor 語法在區段內轉譯的內容。</span><span class="sxs-lookup"><span data-stu-id="b699c-145">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="b699c-146">如果頁面或檢視定義區段，則必須進行轉譯 (否則會發生錯誤)。</span><span class="sxs-lookup"><span data-stu-id="b699c-146">If a page or view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="b699c-147">Razor Pages 檢視中的範例 `@section` 定義：</span><span class="sxs-lookup"><span data-stu-id="b699c-147">An example `@section` definition in Razor Pages view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="~/scripts/main.js"></script>
}
```

<span data-ttu-id="b699c-148">在前述程式碼中，*scripts/main.js* 會新增至頁面或檢視上的 `scripts` 區段。</span><span class="sxs-lookup"><span data-stu-id="b699c-148">In the preceding code, *scripts/main.js* is added to the `scripts` section on a page or view.</span></span> <span data-ttu-id="b699c-149">相同應用程式中的其他頁面或檢視可能不需要此指令碼，也不會定義指令碼區段。</span><span class="sxs-lookup"><span data-stu-id="b699c-149">Other pages or views in the same app might not require this script and wouldn't define a scripts section.</span></span>

<span data-ttu-id="b699c-150">下列標記會使用 [部分標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) 來轉譯 *_ValidationScriptsPartial.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="b699c-150">The following markup uses the [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) to render  *_ValidationScriptsPartial.cshtml*:</span></span>

```html
@section Scripts {
    <partial name="_ValidationScriptsPartial" />
}
```

<span data-ttu-id="b699c-151">前述標記由 [Scaffolding 識別](xref:security/authentication/scaffold-identity) 產生。</span><span class="sxs-lookup"><span data-stu-id="b699c-151">The preceding markup was generated by [scaffolding Identity](xref:security/authentication/scaffold-identity).</span></span>

<span data-ttu-id="b699c-152">頁面或檢視中所定義的區段僅適用於其立即配置頁面。</span><span class="sxs-lookup"><span data-stu-id="b699c-152">Sections defined in a page or view are available only in its immediate layout page.</span></span> <span data-ttu-id="b699c-153">無法從部分、檢視元件或檢視系統的其他部分參考它們。</span><span class="sxs-lookup"><span data-stu-id="b699c-153">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="b699c-154">忽略區段</span><span class="sxs-lookup"><span data-stu-id="b699c-154">Ignoring sections</span></span>

<span data-ttu-id="b699c-155">根據預設，內容頁面中的本文和所有區段都必須透過配置頁面進行轉譯。</span><span class="sxs-lookup"><span data-stu-id="b699c-155">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="b699c-156">Razor 檢視引擎會強制執行此作業，方法是追蹤是否已轉譯本文和每個區段。</span><span class="sxs-lookup"><span data-stu-id="b699c-156">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="b699c-157">若要指示檢視引擎略過本文或區段，請呼叫 `IgnoreBody` 和 `IgnoreSection` 方法。</span><span class="sxs-lookup"><span data-stu-id="b699c-157">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="b699c-158">Razor 頁面中的本文和每個區段都必須進行轉譯或忽略。</span><span class="sxs-lookup"><span data-stu-id="b699c-158">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="b699c-159">匯入共用指示詞</span><span class="sxs-lookup"><span data-stu-id="b699c-159">Importing Shared Directives</span></span>

<span data-ttu-id="b699c-160">Views 和 pages 可以使用 Razor 指示詞來匯入命名空間，並使用相依性[插入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="b699c-160">Views and pages can use Razor directives to import namespaces and use [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="b699c-161">許多檢視所共用的指示詞可能指定於通用 *_ViewImports.cshtml* 檔案中。</span><span class="sxs-lookup"><span data-stu-id="b699c-161">Directives shared by many views may be specified in a common *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="b699c-162">`_ViewImports` 檔案支援下列指示詞：</span><span class="sxs-lookup"><span data-stu-id="b699c-162">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`
* `@removeTagHelper`
* `@tagHelperPrefix`
* `@using`
* `@model`
* `@inherits`
* `@inject`

<span data-ttu-id="b699c-163">檔案不支援其他 Razor 功能，例如函式和區段定義。</span><span class="sxs-lookup"><span data-stu-id="b699c-163">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="b699c-164">範例 `_ViewImports.cshtml` 檔案：</span><span class="sxs-lookup"><span data-stu-id="b699c-164">A sample `_ViewImports.cshtml` file:</span></span>

[!code-cshtml[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="b699c-165">ASP.NET Core MVC 應用程式的 *_ViewImports.cshtml* 檔案通常會放在 *Pages* (或 *Views*) 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="b699c-165">The *_ViewImports.cshtml* file for an ASP.NET Core MVC app is typically placed in the *Pages* (or *Views*) folder.</span></span> <span data-ttu-id="b699c-166">*_ViewImports.cshtml* 檔案可以放在任何資料夾內；在此案例中，該檔案只會套用至該資料夾和其子資料夾內的頁面或檢視。</span><span class="sxs-lookup"><span data-stu-id="b699c-166">A *_ViewImports.cshtml* file can be placed within any folder, in which case it will only be applied to pages or views within that folder and its subfolders.</span></span> <span data-ttu-id="b699c-167">`_ViewImports` 檔案會從根層級開始處理，然後處理導向頁面或檢視本身位置的每個資料夾。</span><span class="sxs-lookup"><span data-stu-id="b699c-167">`_ViewImports` files are processed starting at the root level and then for each folder leading up to the location of the page or view itself.</span></span> <span data-ttu-id="b699c-168">根層級指定的 `_ViewImports` 設定可以在資料夾層級覆寫。</span><span class="sxs-lookup"><span data-stu-id="b699c-168">`_ViewImports` settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="b699c-169">例如，假設：</span><span class="sxs-lookup"><span data-stu-id="b699c-169">For example, suppose:</span></span>

* <span data-ttu-id="b699c-170">根層級 *_ViewImports.cshtml* 檔案包含 `@model MyModel1` 和 `@addTagHelper *, MyTagHelper1`。</span><span class="sxs-lookup"><span data-stu-id="b699c-170">The  root level *_ViewImports.cshtml* file includes `@model MyModel1` and `@addTagHelper *, MyTagHelper1`.</span></span>
* <span data-ttu-id="b699c-171">子資料夾 *_ViewImports.cshtml* 檔案包含 `@model MyModel2` 和 `@addTagHelper *, MyTagHelper2`。</span><span class="sxs-lookup"><span data-stu-id="b699c-171">A subfolder  *_ViewImports.cshtml* file includes `@model MyModel2` and `@addTagHelper *, MyTagHelper2`.</span></span>

<span data-ttu-id="b699c-172">子資料夾中的頁面和檢視可以存取標籤協助程式和 `MyModel2` 模型。</span><span class="sxs-lookup"><span data-stu-id="b699c-172">Pages and views in the subfolder will have access to both Tag Helpers and the `MyModel2` model.</span></span>

<span data-ttu-id="b699c-173">如果在檔案階層有多個 *_ViewImports.cshtml* 檔案，指示詞的合併行為是：</span><span class="sxs-lookup"><span data-stu-id="b699c-173">If multiple *_ViewImports.cshtml* files are found in the file hierarchy, the combined behavior of the directives are:</span></span>

* <span data-ttu-id="b699c-174">`@addTagHelper`、`@removeTagHelper`：全部依序執行</span><span class="sxs-lookup"><span data-stu-id="b699c-174">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>
* <span data-ttu-id="b699c-175">`@tagHelperPrefix`：最接近檢視的項目會覆寫任何其他項目</span><span class="sxs-lookup"><span data-stu-id="b699c-175">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>
* <span data-ttu-id="b699c-176">`@model`：最接近檢視的項目會覆寫任何其他項目</span><span class="sxs-lookup"><span data-stu-id="b699c-176">`@model`: the closest one to the view overrides any others</span></span>
* <span data-ttu-id="b699c-177">`@inherits`：最接近檢視的項目會覆寫任何其他項目</span><span class="sxs-lookup"><span data-stu-id="b699c-177">`@inherits`: the closest one to the view overrides any others</span></span>
* <span data-ttu-id="b699c-178">`@using`：全部包括，但忽略重複項目</span><span class="sxs-lookup"><span data-stu-id="b699c-178">`@using`: all are included; duplicates are ignored</span></span>
* <span data-ttu-id="b699c-179">`@inject`：針對每個屬性，最接近檢視的項目會覆寫任何其他具有相同屬性名稱的項目</span><span class="sxs-lookup"><span data-stu-id="b699c-179">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="b699c-180">在每個檢視之前執行程式碼</span><span class="sxs-lookup"><span data-stu-id="b699c-180">Running Code Before Each View</span></span>

<span data-ttu-id="b699c-181">需要在每個檢視或頁面之前執行的程式碼應放在 *_ViewStart.cshtml* 檔案中。</span><span class="sxs-lookup"><span data-stu-id="b699c-181">Code that needs to run before each view or page should be placed in the *_ViewStart.cshtml* file.</span></span> <span data-ttu-id="b699c-182">依照慣例， *_ViewStart.cshtml* 檔案會位於*頁面* (或*檢視*) 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="b699c-182">By convention, the *_ViewStart.cshtml* file is located in the *Pages* (or *Views*) folder.</span></span> <span data-ttu-id="b699c-183">*_ViewStart.cshtml* 中所列的陳述式會在每個完整檢視之前執行 (不是配置，也不是部分檢視)。</span><span class="sxs-lookup"><span data-stu-id="b699c-183">The statements listed in *_ViewStart.cshtml* are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="b699c-184">與 [ViewImports.cshtml](xref:mvc/views/layout#viewimports) 類似， *_ViewStart.cshtml* 是階層式的。</span><span class="sxs-lookup"><span data-stu-id="b699c-184">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), *_ViewStart.cshtml* is hierarchical.</span></span> <span data-ttu-id="b699c-185">如果 *_ViewStart.cshtml* 檔案是定義於檢視或頁面資料夾中，則會在 *Pages* (或 *Views*) 資料夾 (如果有的話) 根目錄中所定義的檔案後面執行。</span><span class="sxs-lookup"><span data-stu-id="b699c-185">If a *_ViewStart.cshtml* file is defined in the view or pages folder, it will be run after the one defined in the root of the *Pages* (or *Views*) folder (if any).</span></span>

<span data-ttu-id="b699c-186">範例 *_ViewStart.cshtml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="b699c-186">A sample *_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="b699c-187">上面的檔案指定所有檢視都會使用 *_Layout.cshtml* 配置。</span><span class="sxs-lookup"><span data-stu-id="b699c-187">The file above specifies that all views will use the *_Layout.cshtml* layout.</span></span>

<span data-ttu-id="b699c-188">*_ViewStart.cshtml* 和 *_ViewImports.cshtml* 通常**不會**放在 */Pages/Shared* (或 */Views/Shared*) 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="b699c-188">*_ViewStart.cshtml* and *_ViewImports.cshtml* are **not** typically placed in the */Pages/Shared* (or */Views/Shared*) folder.</span></span> <span data-ttu-id="b699c-189">這些檔案的應用程式層級版本應該直接放在 */Pages* (或 */Views*) 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="b699c-189">The app-level versions of these files should be placed directly in the */Pages* (or */Views*) folder.</span></span>
