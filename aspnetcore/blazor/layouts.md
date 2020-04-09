---
title: ASP.NET核心Blazor佈局
author: guardrex
description: 瞭解如何為應用創建可重用的Blazor佈局元件。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/layouts
ms.openlocfilehash: 5b6e1c7ceb4a6e41230e31bbe379bde1bb0a8286
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78660409"
---
# <a name="aspnet-core-opno-locblazor-layouts"></a><span data-ttu-id="810d9-103">ASP.NET核心Blazor佈局</span><span class="sxs-lookup"><span data-stu-id="810d9-103">ASP.NET Core Blazor layouts</span></span>

<span data-ttu-id="810d9-104">由[雷納·斯特羅佩克](https://www.timecockpit.com)和[盧克·萊瑟姆](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="810d9-104">By [Rainer Stropek](https://www.timecockpit.com) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="810d9-105">某些應用元素(如功能表、版權消息和公司徽標)通常是應用整體佈局的一部分,並且由應用中的每個元件使用。</span><span class="sxs-lookup"><span data-stu-id="810d9-105">Some app elements, such as menus, copyright messages, and company logos, are usually part of app's overall layout and used by every component in the app.</span></span> <span data-ttu-id="810d9-106">每次其中一個元素需要更新時,將這些元素的代碼複製到應用的所有元件中並不是一種有效&mdash;的方法,因此必須更新每個元件。</span><span class="sxs-lookup"><span data-stu-id="810d9-106">Copying the code of these elements into all of the components of an app isn't an efficient approach&mdash;every time one of the elements requires an update, every component must be updated.</span></span> <span data-ttu-id="810d9-107">這種重複很難維護,並且隨著時間的推移可能導致內容不一致。</span><span class="sxs-lookup"><span data-stu-id="810d9-107">Such duplication is difficult to maintain and can lead to inconsistent content over time.</span></span> <span data-ttu-id="810d9-108">*佈局*解決了這個問題。</span><span class="sxs-lookup"><span data-stu-id="810d9-108">*Layouts* solve this problem.</span></span>

<span data-ttu-id="810d9-109">從技術上講,佈局只是另一個元件。</span><span class="sxs-lookup"><span data-stu-id="810d9-109">Technically, a layout is just another component.</span></span> <span data-ttu-id="810d9-110">佈局在 Razor 樣本或 C# 代碼中定義,可以使用[資料綁定](xref:blazor/data-binding)、[依賴項注入](xref:blazor/dependency-injection)和其他元件方案。</span><span class="sxs-lookup"><span data-stu-id="810d9-110">A layout is defined in a Razor template or in C# code and can use [data binding](xref:blazor/data-binding), [dependency injection](xref:blazor/dependency-injection), and other component scenarios.</span></span>

<span data-ttu-id="810d9-111">要將*元件*轉換為*佈局*,該元件:</span><span class="sxs-lookup"><span data-stu-id="810d9-111">To turn a *component* into a *layout*, the component:</span></span>

* <span data-ttu-id="810d9-112">繼承,`LayoutComponentBase`該繼承定義佈局`Body`中 呈現的內容的屬性。</span><span class="sxs-lookup"><span data-stu-id="810d9-112">Inherits from `LayoutComponentBase`, which defines a `Body` property for the rendered content inside the layout.</span></span>
* <span data-ttu-id="810d9-113">使用 Razor`@Body`語法指定呈現內容的布局標記中的位置。</span><span class="sxs-lookup"><span data-stu-id="810d9-113">Uses the Razor syntax `@Body` to specify the location in the layout markup where the content is rendered.</span></span>

<span data-ttu-id="810d9-114">以下代碼示例顯示了佈局元件*MainLayout.razor*的 Razor 範本。</span><span class="sxs-lookup"><span data-stu-id="810d9-114">The following code sample shows the Razor template of a layout component, *MainLayout.razor*.</span></span> <span data-ttu-id="810d9-115">佈局繼承`LayoutComponentBase`並設定導覽`@Body`列和 頁腳之間的設定:</span><span class="sxs-lookup"><span data-stu-id="810d9-115">The layout inherits `LayoutComponentBase` and sets the `@Body` between the navigation bar and the footer:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

<span data-ttu-id="810d9-116">在Blazor基於其中一個應用樣本的應用中,`MainLayout`元件(*MainLayout.razor)* 位於應用的 *「共用」* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="810d9-116">In an app based on one of the Blazor app templates, the `MainLayout` component (*MainLayout.razor*) is in the app's *Shared* folder.</span></span>

## <a name="default-layout"></a><span data-ttu-id="810d9-117">預設佈局</span><span class="sxs-lookup"><span data-stu-id="810d9-117">Default layout</span></span>

<span data-ttu-id="810d9-118">在應用的`Router`*App.razor*檔中的元件中指定預設應用佈局。</span><span class="sxs-lookup"><span data-stu-id="810d9-118">Specify the default app layout in the `Router` component in the app's *App.razor* file.</span></span> <span data-ttu-id="810d9-119">預設`Router`Blazor樣本提供的以下元件將預設佈局設定到`MainLayout`元件:</span><span class="sxs-lookup"><span data-stu-id="810d9-119">The following `Router` component, which is provided by the default Blazor templates, sets the default layout to the `MainLayout` component:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/App1.razor?highlight=3)]

<span data-ttu-id="810d9-120">要為內容提供預設佈局`NotFound`,請`NotFound`為`LayoutView`內容指定 a:</span><span class="sxs-lookup"><span data-stu-id="810d9-120">To supply a default layout for `NotFound` content, specify a `LayoutView` for `NotFound` content:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/App2.razor?highlight=6-9)]

<span data-ttu-id="810d9-121">有關元件的詳細資訊,`Router`請參閱。 <xref:blazor/routing></span><span class="sxs-lookup"><span data-stu-id="810d9-121">For more information on the `Router` component, see <xref:blazor/routing>.</span></span>

<span data-ttu-id="810d9-122">在路由器中指定布局作為默認佈局是一種有用的做法,因為它可以按元件或每個資料夾覆蓋。</span><span class="sxs-lookup"><span data-stu-id="810d9-122">Specifying the layout as a default layout in the router is a useful practice because it can be overridden on a per-component or per-folder basis.</span></span> <span data-ttu-id="810d9-123">更喜歡使用路由器來設置應用的默認佈局,因為它是最通用的技術。</span><span class="sxs-lookup"><span data-stu-id="810d9-123">Prefer using the router to set the app's default layout because it's the most general technique.</span></span>

## <a name="specify-a-layout-in-a-component"></a><span data-ttu-id="810d9-124">在元件中指定佈局</span><span class="sxs-lookup"><span data-stu-id="810d9-124">Specify a layout in a component</span></span>

<span data-ttu-id="810d9-125">使用 Razor`@layout`指令將佈局應用於元件。</span><span class="sxs-lookup"><span data-stu-id="810d9-125">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="810d9-126">編譯器轉換`@layout`為套用元件`LayoutAttribute`類別類別的 。</span><span class="sxs-lookup"><span data-stu-id="810d9-126">The compiler converts `@layout` into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="810d9-127">以下`MasterList`元件的內容插入 :`MasterLayout`的位置`@Body`:</span><span class="sxs-lookup"><span data-stu-id="810d9-127">The content of the following `MasterList` component is inserted into the `MasterLayout` at the position of `@Body`:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

<span data-ttu-id="810d9-128">直接在元件中指定佈局將覆蓋路由器中的*默認佈局*集或`@layout`從 *_Imports.razor*導入的指令。</span><span class="sxs-lookup"><span data-stu-id="810d9-128">Specifying the layout directly in a component overrides a *default layout* set in the router or an `@layout` directive imported from *_Imports.razor*.</span></span>

## <a name="centralized-layout-selection"></a><span data-ttu-id="810d9-129">集中佈局選擇</span><span class="sxs-lookup"><span data-stu-id="810d9-129">Centralized layout selection</span></span>

<span data-ttu-id="810d9-130">應用的每個資料夾可以選擇包含名為 *_Imports.razor 的*範本檔。</span><span class="sxs-lookup"><span data-stu-id="810d9-130">Every folder of an app can optionally contain a template file named *_Imports.razor*.</span></span> <span data-ttu-id="810d9-131">編譯器包括導入檔中指定的指令,這些指令位於同一資料夾中的所有 Razor 範本中,並遞歸於其所有子資料夾中。</span><span class="sxs-lookup"><span data-stu-id="810d9-131">The compiler includes the directives specified in the imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="810d9-132">因此,`@layout MyCoolLayout`包含 *_Imports.razor*檔案可確保資料夾中的所有元件`MyCoolLayout`都使用 。</span><span class="sxs-lookup"><span data-stu-id="810d9-132">Therefore, an *_Imports.razor* file containing `@layout MyCoolLayout` ensures that all of the components in a folder use `MyCoolLayout`.</span></span> <span data-ttu-id="810d9-133">無需重複加入`@layout MyCoolLayout`資料夾或子資料夾中的所有 *.razor*檔案。</span><span class="sxs-lookup"><span data-stu-id="810d9-133">There's no need to repeatedly add `@layout MyCoolLayout` to all of the *.razor* files within the folder and subfolders.</span></span> <span data-ttu-id="810d9-134">`@using`指令也以同樣的方式應用於元件。</span><span class="sxs-lookup"><span data-stu-id="810d9-134">`@using` directives are also applied to components in the same way.</span></span>

<span data-ttu-id="810d9-135">以下 *_Imports.razor*檔案匯入:</span><span class="sxs-lookup"><span data-stu-id="810d9-135">The following *_Imports.razor* file imports:</span></span>

* <span data-ttu-id="810d9-136">`MyCoolLayout`.</span><span class="sxs-lookup"><span data-stu-id="810d9-136">`MyCoolLayout`.</span></span>
* <span data-ttu-id="810d9-137">同一資料夾中的所有 Razor 元件和任何子資料夾。</span><span class="sxs-lookup"><span data-stu-id="810d9-137">All Razor components in the same folder and any subfolders.</span></span>
* <span data-ttu-id="810d9-138">`BlazorApp1.Data` 命名空間。</span><span class="sxs-lookup"><span data-stu-id="810d9-138">The `BlazorApp1.Data` namespace.</span></span>
 
[!code-razor[](layouts/sample_snapshot/3.x/_Imports.razor)]

<span data-ttu-id="810d9-139">*_Imports.razor*檔案類似於Razor[檢視和頁面的_ViewImports.cshtml檔](xref:mvc/views/layout#importing-shared-directives),但專門應用於Razor元件檔。</span><span class="sxs-lookup"><span data-stu-id="810d9-139">The *_Imports.razor* file is similar to the [_ViewImports.cshtml file for Razor views and pages](xref:mvc/views/layout#importing-shared-directives) but applied specifically to Razor component files.</span></span>

<span data-ttu-id="810d9-140">在 *_Imports.razor*中指定佈局將覆蓋指定為路由器*預設佈局的佈局*。</span><span class="sxs-lookup"><span data-stu-id="810d9-140">Specifying a layout in *_Imports.razor* overrides a layout specified as the router's *default layout*.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="810d9-141">巢狀佈局</span><span class="sxs-lookup"><span data-stu-id="810d9-141">Nested layouts</span></span>

<span data-ttu-id="810d9-142">應用可以由嵌套布局組成。</span><span class="sxs-lookup"><span data-stu-id="810d9-142">Apps can consist of nested layouts.</span></span> <span data-ttu-id="810d9-143">元件可以引用佈局,而佈局又引用另一個佈局。</span><span class="sxs-lookup"><span data-stu-id="810d9-143">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="810d9-144">例如,嵌套布局用於創建多級功能表結構。</span><span class="sxs-lookup"><span data-stu-id="810d9-144">For example, nesting layouts are used to create a multi-level menu structure.</span></span>

<span data-ttu-id="810d9-145">下面的範例示範如何使用嵌套佈局。</span><span class="sxs-lookup"><span data-stu-id="810d9-145">The following example shows how to use nested layouts.</span></span> <span data-ttu-id="810d9-146">*「單集元件.razor」* 檔案是要顯示的元件。</span><span class="sxs-lookup"><span data-stu-id="810d9-146">The *EpisodesComponent.razor* file is the component to display.</span></span> <span data-ttu-id="810d9-147">元件參照`MasterListLayout`:</span><span class="sxs-lookup"><span data-stu-id="810d9-147">The component references the `MasterListLayout`:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

<span data-ttu-id="810d9-148">*主清單佈局.razor*檔案`MasterListLayout`提供 。</span><span class="sxs-lookup"><span data-stu-id="810d9-148">The *MasterListLayout.razor* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="810d9-149">佈局引用另一個佈局`MasterLayout`,在其中呈現佈局。</span><span class="sxs-lookup"><span data-stu-id="810d9-149">The layout references another layout, `MasterLayout`, where it's rendered.</span></span> <span data-ttu-id="810d9-150">`EpisodesComponent`呈現的位置`@Body`出現:</span><span class="sxs-lookup"><span data-stu-id="810d9-150">`EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

<span data-ttu-id="810d9-151">最後,`MasterLayout`在*MasterLayout.razor*中包含頂級佈局元素,如標題、主選單和頁腳。</span><span class="sxs-lookup"><span data-stu-id="810d9-151">Finally, `MasterLayout` in *MasterLayout.razor* contains the top-level layout elements, such as the header, main menu, and footer.</span></span> <span data-ttu-id="810d9-152">`MasterListLayout``EpisodesComponent`與 顯示`@Body`位置顯示:</span><span class="sxs-lookup"><span data-stu-id="810d9-152">`MasterListLayout` with the `EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="share-a-razor-pages-layout-with-integrated-components"></a><span data-ttu-id="810d9-153">與整合元件分享剃刀頁面配置</span><span class="sxs-lookup"><span data-stu-id="810d9-153">Share a Razor Pages layout with integrated components</span></span>

<span data-ttu-id="810d9-154">當可路由元件集成到 Razor Pages 應用中時,該應用的共用佈局可與元件一起使用。</span><span class="sxs-lookup"><span data-stu-id="810d9-154">When routable components are integrated into a Razor Pages app, the app's shared layout can be used with the components.</span></span> <span data-ttu-id="810d9-155">如需詳細資訊，請參閱 <xref:blazor/integrate-components>。</span><span class="sxs-lookup"><span data-stu-id="810d9-155">For more information, see <xref:blazor/integrate-components>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="810d9-156">其他資源</span><span class="sxs-lookup"><span data-stu-id="810d9-156">Additional resources</span></span>

* <xref:mvc/views/layout>
