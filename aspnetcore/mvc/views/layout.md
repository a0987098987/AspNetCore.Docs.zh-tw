---
title: "配置"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 29f12d1f-9734-48bd-bf1a-cee53a8ab700
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/layout
ms.openlocfilehash: 9a7f140722548b51bbce33d44389ff5646aa6047
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="layout"></a><span data-ttu-id="b9a10-103">配置</span><span class="sxs-lookup"><span data-stu-id="b9a10-103">Layout</span></span>

<span data-ttu-id="b9a10-104">由[Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="b9a10-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="b9a10-105">檢視通常會共用視覺與程式設計項目。</span><span class="sxs-lookup"><span data-stu-id="b9a10-105">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="b9a10-106">在本文中，您將學習如何使用一般的版面配置、 共用指示詞，以及之前呈現檢視的通用程式碼中執行您的 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b9a10-106">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="b9a10-107">什麼是版面配置</span><span class="sxs-lookup"><span data-stu-id="b9a10-107">What is a Layout</span></span>

<span data-ttu-id="b9a10-108">大部分的 web 應用程式有通用的版面配置可提供使用者一致的體驗它們瀏覽 頁面。</span><span class="sxs-lookup"><span data-stu-id="b9a10-108">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="b9a10-109">配置通常包括一般使用者介面項目，例如應用程式標頭、 瀏覽或功能表項目和頁尾。</span><span class="sxs-lookup"><span data-stu-id="b9a10-109">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![頁面版面配置範例](layout/_static/page-layout.png)

<span data-ttu-id="b9a10-111">常見的 HTML 結構，例如指令碼和樣式表也經常使用的應用程式中的許多網頁。</span><span class="sxs-lookup"><span data-stu-id="b9a10-111">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="b9a10-112">所有這些共用項目中可能會定義*配置*然後可以在應用程式中使用任何檢視所參考的檔案。</span><span class="sxs-lookup"><span data-stu-id="b9a10-112">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="b9a10-113">配置減少重複的程式碼，在檢視中，協助他們遵循[不重複自行 （乾） 原則](http://deviq.com/don-t-repeat-yourself/)。</span><span class="sxs-lookup"><span data-stu-id="b9a10-113">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="b9a10-114">依照慣例，名為 ASP.NET 應用程式的預設版面配置`_Layout.cshtml`。</span><span class="sxs-lookup"><span data-stu-id="b9a10-114">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="b9a10-115">Visual Studio ASP.NET Core MVC 專案範本包含在此配置檔案`Views/Shared`資料夾：</span><span class="sxs-lookup"><span data-stu-id="b9a10-115">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![在 [方案總管] 的 [檢視] 資料夾](layout/_static/web-project-views.png)

<span data-ttu-id="b9a10-117">此配置命名為應用程式中定義檢視的最上層範本。</span><span class="sxs-lookup"><span data-stu-id="b9a10-117">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="b9a10-118">應用程式不需要配置，且應用程式可以定義一個以上的版面配置，以指定不同的版面配置的不同檢視。</span><span class="sxs-lookup"><span data-stu-id="b9a10-118">Apps do not require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="b9a10-119">範例`_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="b9a10-119">An example `_Layout.cshtml`:</span></span>

<span data-ttu-id="b9a10-120">[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]</span><span class="sxs-lookup"><span data-stu-id="b9a10-120">[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]</span></span>

## <a name="specifying-a-layout"></a><span data-ttu-id="b9a10-121">指定的配置</span><span class="sxs-lookup"><span data-stu-id="b9a10-121">Specifying a Layout</span></span>

<span data-ttu-id="b9a10-122">Razor 檢視有`Layout`屬性。</span><span class="sxs-lookup"><span data-stu-id="b9a10-122">Razor views have a `Layout` property.</span></span> <span data-ttu-id="b9a10-123">個別檢視指定版面配置設定此屬性：</span><span class="sxs-lookup"><span data-stu-id="b9a10-123">Individual views specify a layout by setting this property:</span></span>

<span data-ttu-id="b9a10-124">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="b9a10-124">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]</span></span>

<span data-ttu-id="b9a10-125">指定的配置可以使用完整路徑 (範例： `/Views/Shared/_Layout.cshtml`) 或部分名稱 (範例： `_Layout`)。</span><span class="sxs-lookup"><span data-stu-id="b9a10-125">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="b9a10-126">提供的部分名稱時，Razor 檢視引擎將搜尋配置檔使用其標準的探索程序。</span><span class="sxs-lookup"><span data-stu-id="b9a10-126">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="b9a10-127">控制器相關聯的資料夾中搜尋第一次，後面接著`Shared`資料夾。</span><span class="sxs-lookup"><span data-stu-id="b9a10-127">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="b9a10-128">此探索程序等同於用來探索[部分檢視](partial.md)。</span><span class="sxs-lookup"><span data-stu-id="b9a10-128">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="b9a10-129">根據預設，必須呼叫每個配置`RenderBody`。</span><span class="sxs-lookup"><span data-stu-id="b9a10-129">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="b9a10-130">無論在何處呼叫`RenderBody`是用來放置，檢視的內容會呈現。</span><span class="sxs-lookup"><span data-stu-id="b9a10-130">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name=layout-sections-label></a>

### <a name="sections"></a><span data-ttu-id="b9a10-131">章節</span><span class="sxs-lookup"><span data-stu-id="b9a10-131">Sections</span></span>

<span data-ttu-id="b9a10-132">配置可以選擇性地參考一個或多個*區段*，藉由呼叫`RenderSection`。</span><span class="sxs-lookup"><span data-stu-id="b9a10-132">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="b9a10-133">各節會提供一種方式組織特定頁面項目應放置的位置。</span><span class="sxs-lookup"><span data-stu-id="b9a10-133">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="b9a10-134">每次呼叫`RenderSection`可以指定該區段是必要或選擇性。</span><span class="sxs-lookup"><span data-stu-id="b9a10-134">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="b9a10-135">如果找不到所需的區段，將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b9a10-135">If a required section is not found, an exception will be thrown.</span></span> <span data-ttu-id="b9a10-136">個別檢視指定的內容區段使用呈現`@section`Razor 語法。</span><span class="sxs-lookup"><span data-stu-id="b9a10-136">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="b9a10-137">如果檢視定義的區段，必須加以呈現 （或會發生錯誤）。</span><span class="sxs-lookup"><span data-stu-id="b9a10-137">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="b9a10-138">範例`@section`定義在檢視中：</span><span class="sxs-lookup"><span data-stu-id="b9a10-138">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="b9a10-139">在上述程式碼，驗證指令碼會加入至`scripts`包含表單的檢視上一節。</span><span class="sxs-lookup"><span data-stu-id="b9a10-139">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="b9a10-140">相同的應用程式中的其他檢視可能不需要任何額外的指令碼，並因此將不需要重新定義指令碼區段。</span><span class="sxs-lookup"><span data-stu-id="b9a10-140">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="b9a10-141">在檢視中所定義的是僅適用於其立即配置頁面。</span><span class="sxs-lookup"><span data-stu-id="b9a10-141">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="b9a10-142">無法從 partials、 檢視元件或檢視系統其他部分參考它們。</span><span class="sxs-lookup"><span data-stu-id="b9a10-142">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="b9a10-143">正在略過區段</span><span class="sxs-lookup"><span data-stu-id="b9a10-143">Ignoring sections</span></span>

<span data-ttu-id="b9a10-144">根據預設，本文和內容頁面中的所有區段必須全部轉譯的版面配置頁。</span><span class="sxs-lookup"><span data-stu-id="b9a10-144">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="b9a10-145">Razor 檢視引擎將會強制此追蹤是否呈現在主體和每個區段。</span><span class="sxs-lookup"><span data-stu-id="b9a10-145">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="b9a10-146">若要指示要略過的內容或章節的檢視引擎，請呼叫`IgnoreBody`和`IgnoreSection`方法。</span><span class="sxs-lookup"><span data-stu-id="b9a10-146">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="b9a10-147">在主體和 Razor 頁面中的每個區段必須轉譯或忽略。</span><span class="sxs-lookup"><span data-stu-id="b9a10-147">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name=viewimports></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="b9a10-148">匯入共用的指示詞</span><span class="sxs-lookup"><span data-stu-id="b9a10-148">Importing Shared Directives</span></span>

<span data-ttu-id="b9a10-149">檢視可以執行許多動作，例如匯入命名空間，或使用 Razor 指示詞[相依性插入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="b9a10-149">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="b9a10-150">由許多檢視共用的指示詞可指定在一般`_ViewImports.cshtml`檔案。</span><span class="sxs-lookup"><span data-stu-id="b9a10-150">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="b9a10-151">`_ViewImports`檔案支援下列指示詞：</span><span class="sxs-lookup"><span data-stu-id="b9a10-151">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="b9a10-152">檔案不支援其他 Razor 的功能，例如函式和區段定義。</span><span class="sxs-lookup"><span data-stu-id="b9a10-152">The file does not support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="b9a10-153">範例`_ViewImports.cshtml`檔案：</span><span class="sxs-lookup"><span data-stu-id="b9a10-153">A sample `_ViewImports.cshtml` file:</span></span>

<span data-ttu-id="b9a10-154">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="b9a10-154">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]</span></span>

<span data-ttu-id="b9a10-155">`_ViewImports.cshtml`檔案 ASP.NET Core MVC 應用程式通常會放在`Views`資料夾。</span><span class="sxs-lookup"><span data-stu-id="b9a10-155">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="b9a10-156">A`_ViewImports.cshtml`檔案可以放在其中只會套用至案例檢視該資料夾以及其子資料夾內的任何資料夾內。</span><span class="sxs-lookup"><span data-stu-id="b9a10-156">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="b9a10-157">`_ViewImports`在根層級中，開始處理的檔案，然後針對每個備妥的資料夾檢視本身的位置，設定指定在根層級可能會覆寫資料夾層級。</span><span class="sxs-lookup"><span data-stu-id="b9a10-157">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="b9a10-158">比方說，如果根層級`_ViewImports.cshtml`檔案會指定`@model`和`@addTagHelper`，和另一個`_ViewImports.cshtml`控制器相關聯的檢視資料夾中的檔案指定不同`@model`並加入另一個`@addTagHelper`，檢視將能存取這兩個標記協助程式，並使用後者`@model`。</span><span class="sxs-lookup"><span data-stu-id="b9a10-158">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="b9a10-159">若為多個`_ViewImports.cshtml`檔案會執行檢視，結合中包含的指示詞的行為`ViewImports.cshtml`檔案會顯示如下：</span><span class="sxs-lookup"><span data-stu-id="b9a10-159">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="b9a10-160">`@addTagHelper``@removeTagHelper`： 所有執行，順序</span><span class="sxs-lookup"><span data-stu-id="b9a10-160">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="b9a10-161">`@tagHelperPrefix`： 檢視最接近的其中一個會覆寫任何其他項目</span><span class="sxs-lookup"><span data-stu-id="b9a10-161">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="b9a10-162">`@model`： 檢視最接近的其中一個會覆寫任何其他項目</span><span class="sxs-lookup"><span data-stu-id="b9a10-162">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="b9a10-163">`@inherits`： 檢視最接近的其中一個會覆寫任何其他項目</span><span class="sxs-lookup"><span data-stu-id="b9a10-163">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="b9a10-164">`@using`： 所有都包含在內。會忽略重複的項目</span><span class="sxs-lookup"><span data-stu-id="b9a10-164">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="b9a10-165">`@inject`： 每個屬性，以檢視最接近的其中一個會覆寫具有相同屬性名稱的任何其他</span><span class="sxs-lookup"><span data-stu-id="b9a10-165">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name=viewstart></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="b9a10-166">執行每個檢視之前的程式碼</span><span class="sxs-lookup"><span data-stu-id="b9a10-166">Running Code Before Each View</span></span>

<span data-ttu-id="b9a10-167">如果您的程式碼，您必須執行的每個檢視之前，這應該放在`_ViewStart.cshtml`檔案。</span><span class="sxs-lookup"><span data-stu-id="b9a10-167">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="b9a10-168">依照慣例，`_ViewStart.cshtml`檔案位於`Views`資料夾。</span><span class="sxs-lookup"><span data-stu-id="b9a10-168">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="b9a10-169">中所列的陳述式`_ViewStart.cshtml`（不版面配置和非部分檢視） 的每個完整檢視之前執行。</span><span class="sxs-lookup"><span data-stu-id="b9a10-169">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="b9a10-170">像[ViewImports.cshtml](xref:mvc/views/layout#viewimports)，`_ViewStart.cshtml`是階層式。</span><span class="sxs-lookup"><span data-stu-id="b9a10-170">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="b9a10-171">如果`_ViewStart.cshtml`檔案在控制器相關聯的檢視資料夾中所定義，它會執行之後的根目錄中定義的`Views`資料夾 （如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="b9a10-171">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="b9a10-172">範例`_ViewStart.cshtml`檔案：</span><span class="sxs-lookup"><span data-stu-id="b9a10-172">A sample `_ViewStart.cshtml` file:</span></span>

<span data-ttu-id="b9a10-173">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="b9a10-173">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]</span></span>

<span data-ttu-id="b9a10-174">上面的檔案可讓您指定的所有檢視將都使用`_Layout.cshtml`版面配置。</span><span class="sxs-lookup"><span data-stu-id="b9a10-174">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="b9a10-175">既不`_ViewStart.cshtml`也`_ViewImports.cshtml`通常會放在`/Views/Shared`資料夾。</span><span class="sxs-lookup"><span data-stu-id="b9a10-175">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="b9a10-176">這些檔案的應用程式層級版本應該放在`/Views`資料夾。</span><span class="sxs-lookup"><span data-stu-id="b9a10-176">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
