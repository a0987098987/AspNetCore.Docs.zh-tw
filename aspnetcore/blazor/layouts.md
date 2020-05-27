---
<span data-ttu-id="4f605-101">標題： ' ASP.NET Core Blazor 版面配置 ' 作者：描述：「瞭解如何建立應用程式的可重複使用的配置元件」 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="4f605-101">title: 'ASP.NET Core Blazor layouts' author: description: 'Learn how to create reusable layout components for Blazor apps.'</span></span>
<span data-ttu-id="4f605-102">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4f605-102">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="4f605-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4f605-103">'Blazor'</span></span>
- <span data-ttu-id="4f605-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4f605-104">'Identity'</span></span>
- <span data-ttu-id="4f605-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4f605-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="4f605-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4f605-106">'Razor'</span></span>
- <span data-ttu-id="4f605-107">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4f605-107">'SignalR' uid:</span></span> 

---
# <a name="aspnet-core-blazor-layouts"></a><span data-ttu-id="4f605-108">ASP.NET Core Blazor 版面配置</span><span class="sxs-lookup"><span data-stu-id="4f605-108">ASP.NET Core Blazor layouts</span></span>

<span data-ttu-id="4f605-109">By [Rainer Stropek](https://www.timecockpit.com)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4f605-109">By [Rainer Stropek](https://www.timecockpit.com) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4f605-110">某些應用程式專案（例如功能表、著作權訊息和公司標誌）通常是應用程式整體版面配置的一部分，並由應用程式中的每個元件所使用。</span><span class="sxs-lookup"><span data-stu-id="4f605-110">Some app elements, such as menus, copyright messages, and company logos, are usually part of app's overall layout and used by every component in the app.</span></span> <span data-ttu-id="4f605-111">將這些專案的程式碼複製到應用程式的所有元件，並不是有效的方法。</span><span class="sxs-lookup"><span data-stu-id="4f605-111">Copying the code of these elements into all of the components of an app isn't an efficient approach.</span></span> <span data-ttu-id="4f605-112">每當其中一個元素需要更新時，就必須更新每個元件。</span><span class="sxs-lookup"><span data-stu-id="4f605-112">Every time one of the elements requires an update, every component must be updated.</span></span> <span data-ttu-id="4f605-113">這種複製很容易維護，而且可能會在一段時間後導致不一致的內容。</span><span class="sxs-lookup"><span data-stu-id="4f605-113">Such duplication is difficult to maintain and can lead to inconsistent content over time.</span></span> <span data-ttu-id="4f605-114">*版面*配置會解決此問題。</span><span class="sxs-lookup"><span data-stu-id="4f605-114">*Layouts* solve this problem.</span></span>

<span data-ttu-id="4f605-115">就技術上而言，版面配置只是另一個元件。</span><span class="sxs-lookup"><span data-stu-id="4f605-115">Technically, a layout is just another component.</span></span> <span data-ttu-id="4f605-116">版面配置是在 Razor 範本或 c # 程式碼中定義，而且可以使用[資料](xref:blazor/data-binding)系結、相依性[插入](xref:blazor/dependency-injection)和其他元件案例。</span><span class="sxs-lookup"><span data-stu-id="4f605-116">A layout is defined in a Razor template or in C# code and can use [data binding](xref:blazor/data-binding), [dependency injection](xref:blazor/dependency-injection), and other component scenarios.</span></span>

<span data-ttu-id="4f605-117">若要將*元件*轉換成*版面*配置，元件：</span><span class="sxs-lookup"><span data-stu-id="4f605-117">To turn a *component* into a *layout*, the component:</span></span>

* <span data-ttu-id="4f605-118">繼承自 <xref:Microsoft.AspNetCore.Components.LayoutComponentBase> ，其定義配置 <xref:Microsoft.AspNetCore.Components.LayoutComponentBase.Body> 內呈現之內容的屬性。</span><span class="sxs-lookup"><span data-stu-id="4f605-118">Inherits from <xref:Microsoft.AspNetCore.Components.LayoutComponentBase>, which defines a <xref:Microsoft.AspNetCore.Components.LayoutComponentBase.Body> property for the rendered content inside the layout.</span></span>
* <span data-ttu-id="4f605-119">會使用 Razor 語法 `@Body` 來指定版面配置標記中轉譯內容的位置。</span><span class="sxs-lookup"><span data-stu-id="4f605-119">Uses the Razor syntax `@Body` to specify the location in the layout markup where the content is rendered.</span></span>

<span data-ttu-id="4f605-120">下列程式碼範例顯示 Razor 版面配置元件*MainLayout*的範本。</span><span class="sxs-lookup"><span data-stu-id="4f605-120">The following code sample shows the Razor template of a layout component, *MainLayout.razor*.</span></span> <span data-ttu-id="4f605-121">版面配置會繼承 <xref:Microsoft.AspNetCore.Components.LayoutComponentBase> 並設定 `@Body` 導覽列和頁尾之間的：</span><span class="sxs-lookup"><span data-stu-id="4f605-121">The layout inherits <xref:Microsoft.AspNetCore.Components.LayoutComponentBase> and sets the `@Body` between the navigation bar and the footer:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

<span data-ttu-id="4f605-122">在以其中一個應用程式範本為基礎的應用程式中 Blazor ， `MainLayout` 元件（*MainLayout*）位於應用程式的*共用*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="4f605-122">In an app based on one of the Blazor app templates, the `MainLayout` component (*MainLayout.razor*) is in the app's *Shared* folder.</span></span>

## <a name="default-layout"></a><span data-ttu-id="4f605-123">預設版面配置</span><span class="sxs-lookup"><span data-stu-id="4f605-123">Default layout</span></span>

<span data-ttu-id="4f605-124">在 <xref:Microsoft.AspNetCore.Components.Routing.Router> 應用程式的*razor*檔案中，指定元件中的預設應用程式佈建。</span><span class="sxs-lookup"><span data-stu-id="4f605-124">Specify the default app layout in the <xref:Microsoft.AspNetCore.Components.Routing.Router> component in the app's *App.razor* file.</span></span> <span data-ttu-id="4f605-125">下列 <xref:Microsoft.AspNetCore.Components.Routing.Router> 元件（由預設 Blazor 範本提供）會將預設版面配置設定為 `MainLayout` 元件：</span><span class="sxs-lookup"><span data-stu-id="4f605-125">The following <xref:Microsoft.AspNetCore.Components.Routing.Router> component, which is provided by the default Blazor templates, sets the default layout to the `MainLayout` component:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/App1.razor?highlight=3)]

<span data-ttu-id="4f605-126">若要提供內容的預設版面配置 <xref:Microsoft.AspNetCore.Components.Routing.Router.NotFound> ，請 <xref:Microsoft.AspNetCore.Components.LayoutView> 指定 <xref:Microsoft.AspNetCore.Components.Routing.Router.NotFound> 內容的：</span><span class="sxs-lookup"><span data-stu-id="4f605-126">To supply a default layout for <xref:Microsoft.AspNetCore.Components.Routing.Router.NotFound> content, specify a <xref:Microsoft.AspNetCore.Components.LayoutView> for <xref:Microsoft.AspNetCore.Components.Routing.Router.NotFound> content:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/App2.razor?highlight=6-9)]

<span data-ttu-id="4f605-127">如需元件的詳細資訊 <xref:Microsoft.AspNetCore.Components.Routing.Router> ，請參閱 <xref:blazor/routing> 。</span><span class="sxs-lookup"><span data-stu-id="4f605-127">For more information on the <xref:Microsoft.AspNetCore.Components.Routing.Router> component, see <xref:blazor/routing>.</span></span>

<span data-ttu-id="4f605-128">將配置指定為路由器中的預設版面配置是很有用的作法，因為它可以根據每個元件或每個資料夾來覆寫。</span><span class="sxs-lookup"><span data-stu-id="4f605-128">Specifying the layout as a default layout in the router is a useful practice because it can be overridden on a per-component or per-folder basis.</span></span> <span data-ttu-id="4f605-129">偏好使用路由器來設定應用程式的預設版面配置，因為這是最常見的技術。</span><span class="sxs-lookup"><span data-stu-id="4f605-129">Prefer using the router to set the app's default layout because it's the most general technique.</span></span>

## <a name="specify-a-layout-in-a-component"></a><span data-ttu-id="4f605-130">在元件中指定版面配置</span><span class="sxs-lookup"><span data-stu-id="4f605-130">Specify a layout in a component</span></span>

<span data-ttu-id="4f605-131">使用指示詞將版面配置套用 Razor `@layout` 至元件。</span><span class="sxs-lookup"><span data-stu-id="4f605-131">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="4f605-132">編譯器會將轉換 `@layout` 成 <xref:Microsoft.AspNetCore.Components.LayoutAttribute> ，並將其套用至元件類別。</span><span class="sxs-lookup"><span data-stu-id="4f605-132">The compiler converts `@layout` into a <xref:Microsoft.AspNetCore.Components.LayoutAttribute>, which is applied to the component class.</span></span>

<span data-ttu-id="4f605-133">下列元件的內容 `MasterList` 會插入至的 `MasterLayout` 位置 `@Body` ：</span><span class="sxs-lookup"><span data-stu-id="4f605-133">The content of the following `MasterList` component is inserted into the `MasterLayout` at the position of `@Body`:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

<span data-ttu-id="4f605-134">直接在元件中指定版面配置會覆寫路由器中的*預設*組態集，或 `@layout` 從 *_Imports razor*匯入的指示詞。</span><span class="sxs-lookup"><span data-stu-id="4f605-134">Specifying the layout directly in a component overrides a *default layout* set in the router or an `@layout` directive imported from *_Imports.razor*.</span></span>

## <a name="centralized-layout-selection"></a><span data-ttu-id="4f605-135">集中式版面配置選取</span><span class="sxs-lookup"><span data-stu-id="4f605-135">Centralized layout selection</span></span>

<span data-ttu-id="4f605-136">應用程式的每個資料夾都可以選擇性地包含名為 *_Imports*的範本檔案。</span><span class="sxs-lookup"><span data-stu-id="4f605-136">Every folder of an app can optionally contain a template file named *_Imports.razor*.</span></span> <span data-ttu-id="4f605-137">編譯器會將匯入檔案中的指示詞，包含在 Razor 相同資料夾的所有範本中，並以遞迴方式在其所有子資料夾中指定。</span><span class="sxs-lookup"><span data-stu-id="4f605-137">The compiler includes the directives specified in the imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="4f605-138">因此，包含的 *_Imports razor*檔案 `@layout MyCoolLayout` 可確保資料夾中的所有元件都使用 `MyCoolLayout` 。</span><span class="sxs-lookup"><span data-stu-id="4f605-138">Therefore, an *_Imports.razor* file containing `@layout MyCoolLayout` ensures that all of the components in a folder use `MyCoolLayout`.</span></span> <span data-ttu-id="4f605-139">不需要重複新增 `@layout MyCoolLayout` 至資料夾和子資料夾內的所有*razor*檔案。</span><span class="sxs-lookup"><span data-stu-id="4f605-139">There's no need to repeatedly add `@layout MyCoolLayout` to all of the *.razor* files within the folder and subfolders.</span></span> <span data-ttu-id="4f605-140">`@using`指示詞也會以同樣的方式套用至元件。</span><span class="sxs-lookup"><span data-stu-id="4f605-140">`@using` directives are also applied to components in the same way.</span></span>

<span data-ttu-id="4f605-141">下列 *_Imports razor*檔案匯入：</span><span class="sxs-lookup"><span data-stu-id="4f605-141">The following *_Imports.razor* file imports:</span></span>

* <span data-ttu-id="4f605-142">`MyCoolLayout`.</span><span class="sxs-lookup"><span data-stu-id="4f605-142">`MyCoolLayout`.</span></span>
* <span data-ttu-id="4f605-143">Razor相同資料夾和任何子資料夾中的所有元件。</span><span class="sxs-lookup"><span data-stu-id="4f605-143">All Razor components in the same folder and any subfolders.</span></span>
* <span data-ttu-id="4f605-144">`BlazorApp1.Data` 命名空間。</span><span class="sxs-lookup"><span data-stu-id="4f605-144">The `BlazorApp1.Data` namespace.</span></span>
 
[!code-razor[](layouts/sample_snapshot/3.x/_Imports.razor)]

<span data-ttu-id="4f605-145">*_Imports razor*檔案類似于[ Razor views 和 pages 的 _ViewImports. cshtml](xref:mvc/views/layout#importing-shared-directives)檔案，但特別適用于 Razor 元件檔案。</span><span class="sxs-lookup"><span data-stu-id="4f605-145">The *_Imports.razor* file is similar to the [_ViewImports.cshtml file for Razor views and pages](xref:mvc/views/layout#importing-shared-directives) but applied specifically to Razor component files.</span></span>

<span data-ttu-id="4f605-146">在 *_Imports*中指定版面配置，會覆寫指定為路由器*預設版面*配置的版面配置。</span><span class="sxs-lookup"><span data-stu-id="4f605-146">Specifying a layout in *_Imports.razor* overrides a layout specified as the router's *default layout*.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="4f605-147">嵌套版面配置</span><span class="sxs-lookup"><span data-stu-id="4f605-147">Nested layouts</span></span>

<span data-ttu-id="4f605-148">應用程式可以包含嵌套的版面配置。</span><span class="sxs-lookup"><span data-stu-id="4f605-148">Apps can consist of nested layouts.</span></span> <span data-ttu-id="4f605-149">元件可以參考配置，而該配置會轉而參考另一個版面配置。</span><span class="sxs-lookup"><span data-stu-id="4f605-149">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="4f605-150">例如，用來建立多層級功能表結構的嵌套配置。</span><span class="sxs-lookup"><span data-stu-id="4f605-150">For example, nesting layouts are used to create a multi-level menu structure.</span></span>

<span data-ttu-id="4f605-151">下列範例顯示如何使用嵌套的配置。</span><span class="sxs-lookup"><span data-stu-id="4f605-151">The following example shows how to use nested layouts.</span></span> <span data-ttu-id="4f605-152">*EpisodesComponent. razor*檔案是要顯示的元件。</span><span class="sxs-lookup"><span data-stu-id="4f605-152">The *EpisodesComponent.razor* file is the component to display.</span></span> <span data-ttu-id="4f605-153">元件會參考 `MasterListLayout` ：</span><span class="sxs-lookup"><span data-stu-id="4f605-153">The component references the `MasterListLayout`:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

<span data-ttu-id="4f605-154">*MasterListLayout razor*檔案提供 `MasterListLayout` 。</span><span class="sxs-lookup"><span data-stu-id="4f605-154">The *MasterListLayout.razor* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="4f605-155">版面配置會參考另一個版面配置， `MasterLayout` 也就是其呈現位置。</span><span class="sxs-lookup"><span data-stu-id="4f605-155">The layout references another layout, `MasterLayout`, where it's rendered.</span></span> <span data-ttu-id="4f605-156">`EpisodesComponent`會在出現的位置呈現 `@Body` ：</span><span class="sxs-lookup"><span data-stu-id="4f605-156">`EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

<span data-ttu-id="4f605-157">最後， `MasterLayout` 在*MasterLayout*中，包含最上層的版面配置元素，例如頁首、主功能表和頁尾。</span><span class="sxs-lookup"><span data-stu-id="4f605-157">Finally, `MasterLayout` in *MasterLayout.razor* contains the top-level layout elements, such as the header, main menu, and footer.</span></span> <span data-ttu-id="4f605-158">`MasterListLayout``EpisodesComponent`當出現時，會呈現 `@Body` ：</span><span class="sxs-lookup"><span data-stu-id="4f605-158">`MasterListLayout` with the `EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="share-a-razor-pages-layout-with-integrated-components"></a><span data-ttu-id="4f605-159">Razor與整合式元件共用頁面配置</span><span class="sxs-lookup"><span data-stu-id="4f605-159">Share a Razor Pages layout with integrated components</span></span>

<span data-ttu-id="4f605-160">當可路由的元件整合至 Razor 頁面應用程式時，應用程式的共用版面配置可以與元件搭配使用。</span><span class="sxs-lookup"><span data-stu-id="4f605-160">When routable components are integrated into a Razor Pages app, the app's shared layout can be used with the components.</span></span> <span data-ttu-id="4f605-161">如需詳細資訊，請參閱<xref:blazor/integrate-components>。</span><span class="sxs-lookup"><span data-stu-id="4f605-161">For more information, see <xref:blazor/integrate-components>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4f605-162">其他資源</span><span class="sxs-lookup"><span data-stu-id="4f605-162">Additional resources</span></span>

* <xref:mvc/views/layout>
