---
title: "配置"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/layout
ms.openlocfilehash: e268f045e39188e9cc1e759ff7e6c553662dd669
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="layout"></a><span data-ttu-id="ed363-102">配置</span><span class="sxs-lookup"><span data-stu-id="ed363-102">Layout</span></span>

<span data-ttu-id="ed363-103">由[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ed363-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ed363-104">檢視通常會共用視覺與程式設計項目。</span><span class="sxs-lookup"><span data-stu-id="ed363-104">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="ed363-105">在本文中，您將學習如何使用一般的版面配置、 共用指示詞，以及之前呈現檢視的通用程式碼中執行您的 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ed363-105">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="ed363-106">什麼是版面配置</span><span class="sxs-lookup"><span data-stu-id="ed363-106">What is a Layout</span></span>

<span data-ttu-id="ed363-107">大部分的 web 應用程式有通用的版面配置可提供使用者一致的體驗它們瀏覽 頁面。</span><span class="sxs-lookup"><span data-stu-id="ed363-107">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="ed363-108">配置通常包括一般使用者介面項目，例如應用程式標頭、 瀏覽或功能表項目和頁尾。</span><span class="sxs-lookup"><span data-stu-id="ed363-108">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![頁面版面配置範例](layout/_static/page-layout.png)

<span data-ttu-id="ed363-110">常見的 HTML 結構，例如指令碼和樣式表也經常使用的應用程式中的許多網頁。</span><span class="sxs-lookup"><span data-stu-id="ed363-110">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="ed363-111">所有這些共用項目中可能會定義*配置*然後可以在應用程式中使用任何檢視所參考的檔案。</span><span class="sxs-lookup"><span data-stu-id="ed363-111">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="ed363-112">配置減少重複的程式碼，在檢視中，協助他們遵循[不重複自行 （乾） 原則](http://deviq.com/don-t-repeat-yourself/)。</span><span class="sxs-lookup"><span data-stu-id="ed363-112">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="ed363-113">依照慣例，名為 ASP.NET 應用程式的預設版面配置`_Layout.cshtml`。</span><span class="sxs-lookup"><span data-stu-id="ed363-113">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="ed363-114">Visual Studio ASP.NET Core MVC 專案範本包含在此配置檔案`Views/Shared`資料夾：</span><span class="sxs-lookup"><span data-stu-id="ed363-114">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![在 [方案總管] 的 [檢視] 資料夾](layout/_static/web-project-views.png)

<span data-ttu-id="ed363-116">此配置命名為應用程式中定義檢視的最上層範本。</span><span class="sxs-lookup"><span data-stu-id="ed363-116">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="ed363-117">應用程式不需要配置，以及應用程式可以定義一個以上的版面配置，以指定不同的版面配置的不同檢視。</span><span class="sxs-lookup"><span data-stu-id="ed363-117">Apps don't require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="ed363-118">範例`_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="ed363-118">An example `_Layout.cshtml`:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="ed363-119">指定的配置</span><span class="sxs-lookup"><span data-stu-id="ed363-119">Specifying a Layout</span></span>

<span data-ttu-id="ed363-120">Razor 檢視有`Layout`屬性。</span><span class="sxs-lookup"><span data-stu-id="ed363-120">Razor views have a `Layout` property.</span></span> <span data-ttu-id="ed363-121">個別檢視指定版面配置設定此屬性：</span><span class="sxs-lookup"><span data-stu-id="ed363-121">Individual views specify a layout by setting this property:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="ed363-122">指定的配置可以使用完整路徑 (範例： `/Views/Shared/_Layout.cshtml`) 或部分名稱 (範例： `_Layout`)。</span><span class="sxs-lookup"><span data-stu-id="ed363-122">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="ed363-123">提供的部分名稱時，Razor 檢視引擎將搜尋配置檔使用其標準的探索程序。</span><span class="sxs-lookup"><span data-stu-id="ed363-123">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="ed363-124">控制器相關聯的資料夾中搜尋第一次，後面接著`Shared`資料夾。</span><span class="sxs-lookup"><span data-stu-id="ed363-124">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="ed363-125">此探索程序等同於用來探索[部分檢視](partial.md)。</span><span class="sxs-lookup"><span data-stu-id="ed363-125">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="ed363-126">根據預設，必須呼叫每個配置`RenderBody`。</span><span class="sxs-lookup"><span data-stu-id="ed363-126">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="ed363-127">無論在何處呼叫`RenderBody`是用來放置，檢視的內容會呈現。</span><span class="sxs-lookup"><span data-stu-id="ed363-127">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="ed363-128">章節</span><span class="sxs-lookup"><span data-stu-id="ed363-128">Sections</span></span>

<span data-ttu-id="ed363-129">配置可以選擇性地參考一個或多個*區段*，藉由呼叫`RenderSection`。</span><span class="sxs-lookup"><span data-stu-id="ed363-129">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="ed363-130">各節會提供一種方式組織特定頁面項目應放置的位置。</span><span class="sxs-lookup"><span data-stu-id="ed363-130">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="ed363-131">每次呼叫`RenderSection`可以指定該區段是必要或選擇性。</span><span class="sxs-lookup"><span data-stu-id="ed363-131">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="ed363-132">如果找不到所需的區段，將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ed363-132">If a required section isn't found, an exception will be thrown.</span></span> <span data-ttu-id="ed363-133">個別檢視指定的內容區段使用呈現`@section`Razor 語法。</span><span class="sxs-lookup"><span data-stu-id="ed363-133">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="ed363-134">如果檢視定義的區段，必須加以呈現 （或會發生錯誤）。</span><span class="sxs-lookup"><span data-stu-id="ed363-134">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="ed363-135">範例`@section`定義在檢視中：</span><span class="sxs-lookup"><span data-stu-id="ed363-135">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="ed363-136">在上述程式碼，驗證指令碼會加入至`scripts`包含表單的檢視上一節。</span><span class="sxs-lookup"><span data-stu-id="ed363-136">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="ed363-137">相同的應用程式中的其他檢視可能不需要任何額外的指令碼，並因此將不需要重新定義指令碼區段。</span><span class="sxs-lookup"><span data-stu-id="ed363-137">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="ed363-138">在檢視中所定義的是僅適用於其立即配置頁面。</span><span class="sxs-lookup"><span data-stu-id="ed363-138">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="ed363-139">無法從 partials、 檢視元件或檢視系統其他部分參考它們。</span><span class="sxs-lookup"><span data-stu-id="ed363-139">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="ed363-140">正在略過區段</span><span class="sxs-lookup"><span data-stu-id="ed363-140">Ignoring sections</span></span>

<span data-ttu-id="ed363-141">根據預設，本文和內容頁面中的所有區段必須全部轉譯的版面配置頁。</span><span class="sxs-lookup"><span data-stu-id="ed363-141">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="ed363-142">Razor 檢視引擎將會強制此追蹤是否呈現在主體和每個區段。</span><span class="sxs-lookup"><span data-stu-id="ed363-142">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="ed363-143">若要指示要略過的內容或章節的檢視引擎，請呼叫`IgnoreBody`和`IgnoreSection`方法。</span><span class="sxs-lookup"><span data-stu-id="ed363-143">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="ed363-144">在主體和 Razor 頁面中的每個區段必須轉譯或忽略。</span><span class="sxs-lookup"><span data-stu-id="ed363-144">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="ed363-145">匯入共用的指示詞</span><span class="sxs-lookup"><span data-stu-id="ed363-145">Importing Shared Directives</span></span>

<span data-ttu-id="ed363-146">檢視可以執行許多動作，例如匯入命名空間，或使用 Razor 指示詞[相依性插入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="ed363-146">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="ed363-147">由許多檢視共用的指示詞可指定在一般`_ViewImports.cshtml`檔案。</span><span class="sxs-lookup"><span data-stu-id="ed363-147">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="ed363-148">`_ViewImports`檔案支援下列指示詞：</span><span class="sxs-lookup"><span data-stu-id="ed363-148">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="ed363-149">檔案不支援其他 Razor 的功能，例如函式和區段定義。</span><span class="sxs-lookup"><span data-stu-id="ed363-149">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="ed363-150">範例`_ViewImports.cshtml`檔案：</span><span class="sxs-lookup"><span data-stu-id="ed363-150">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="ed363-151">`_ViewImports.cshtml`檔案 ASP.NET Core MVC 應用程式通常會放在`Views`資料夾。</span><span class="sxs-lookup"><span data-stu-id="ed363-151">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="ed363-152">A`_ViewImports.cshtml`檔案可以放在其中只會套用至案例檢視該資料夾以及其子資料夾內的任何資料夾內。</span><span class="sxs-lookup"><span data-stu-id="ed363-152">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="ed363-153">`_ViewImports`在根層級中，開始處理的檔案，然後針對每個備妥的資料夾檢視本身的位置，設定指定在根層級可能會覆寫資料夾層級。</span><span class="sxs-lookup"><span data-stu-id="ed363-153">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="ed363-154">比方說，如果根層級`_ViewImports.cshtml`檔案會指定`@model`和`@addTagHelper`，和另一個`_ViewImports.cshtml`控制器相關聯的檢視資料夾中的檔案指定不同`@model`並加入另一個`@addTagHelper`，檢視將能存取這兩個標記協助程式，並使用後者`@model`。</span><span class="sxs-lookup"><span data-stu-id="ed363-154">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="ed363-155">若為多個`_ViewImports.cshtml`檔案會執行檢視，結合中包含的指示詞的行為`ViewImports.cshtml`檔案會顯示如下：</span><span class="sxs-lookup"><span data-stu-id="ed363-155">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="ed363-156">`@addTagHelper``@removeTagHelper`： 所有執行，順序</span><span class="sxs-lookup"><span data-stu-id="ed363-156">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="ed363-157">`@tagHelperPrefix`： 檢視最接近的其中一個會覆寫任何其他項目</span><span class="sxs-lookup"><span data-stu-id="ed363-157">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="ed363-158">`@model`： 檢視最接近的其中一個會覆寫任何其他項目</span><span class="sxs-lookup"><span data-stu-id="ed363-158">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="ed363-159">`@inherits`： 檢視最接近的其中一個會覆寫任何其他項目</span><span class="sxs-lookup"><span data-stu-id="ed363-159">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="ed363-160">`@using`： 所有都包含在內。會忽略重複的項目</span><span class="sxs-lookup"><span data-stu-id="ed363-160">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="ed363-161">`@inject`： 每個屬性，以檢視最接近的其中一個會覆寫具有相同屬性名稱的任何其他</span><span class="sxs-lookup"><span data-stu-id="ed363-161">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="ed363-162">執行每個檢視之前的程式碼</span><span class="sxs-lookup"><span data-stu-id="ed363-162">Running Code Before Each View</span></span>

<span data-ttu-id="ed363-163">如果您的程式碼，您必須執行的每個檢視之前，這應該放在`_ViewStart.cshtml`檔案。</span><span class="sxs-lookup"><span data-stu-id="ed363-163">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="ed363-164">依照慣例，`_ViewStart.cshtml`檔案位於`Views`資料夾。</span><span class="sxs-lookup"><span data-stu-id="ed363-164">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="ed363-165">中所列的陳述式`_ViewStart.cshtml`（不版面配置和非部分檢視） 的每個完整檢視之前執行。</span><span class="sxs-lookup"><span data-stu-id="ed363-165">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="ed363-166">像[ViewImports.cshtml](xref:mvc/views/layout#viewimports)，`_ViewStart.cshtml`是階層式。</span><span class="sxs-lookup"><span data-stu-id="ed363-166">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="ed363-167">如果`_ViewStart.cshtml`檔案在控制器相關聯的檢視資料夾中所定義，它會執行之後的根目錄中定義的`Views`資料夾 （如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="ed363-167">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="ed363-168">範例`_ViewStart.cshtml`檔案：</span><span class="sxs-lookup"><span data-stu-id="ed363-168">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="ed363-169">上面的檔案可讓您指定的所有檢視將都使用`_Layout.cshtml`版面配置。</span><span class="sxs-lookup"><span data-stu-id="ed363-169">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="ed363-170">既不`_ViewStart.cshtml`也`_ViewImports.cshtml`通常會放在`/Views/Shared`資料夾。</span><span class="sxs-lookup"><span data-stu-id="ed363-170">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="ed363-171">這些檔案的應用程式層級版本應該放在`/Views`資料夾。</span><span class="sxs-lookup"><span data-stu-id="ed363-171">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
