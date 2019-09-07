---
title: ASP.NET Core Blazor 版面配置
author: guardrex
description: 瞭解如何為 Blazor 應用程式建立可重複使用的版面配置元件。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: blazor/layouts
ms.openlocfilehash: 05a38c10e18407d50422192ab1ddf3ff4b0f3a5b
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800369"
---
# <a name="aspnet-core-blazor-layouts"></a><span data-ttu-id="0ef55-103">ASP.NET Core Blazor 版面配置</span><span class="sxs-lookup"><span data-stu-id="0ef55-103">ASP.NET Core Blazor layouts</span></span>

<span data-ttu-id="0ef55-104">By [Rainer Stropek](https://www.timecockpit.com)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0ef55-104">By [Rainer Stropek](https://www.timecockpit.com) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0ef55-105">某些應用程式專案（例如功能表、著作權訊息和公司標誌）通常是應用程式整體版面配置的一部分，並由應用程式中的每個元件所使用。</span><span class="sxs-lookup"><span data-stu-id="0ef55-105">Some app elements, such as menus, copyright messages, and company logos, are usually part of app's overall layout and used by every component in the app.</span></span> <span data-ttu-id="0ef55-106">每當其中一個專案需要更新時，將這些專案的程式碼複製到應用程式&mdash;的所有元件都不是有效的方法，每個元件都必須更新。</span><span class="sxs-lookup"><span data-stu-id="0ef55-106">Copying the code of these elements into all of the components of an app isn't an efficient approach&mdash;every time one of the elements requires an update, every component must be updated.</span></span> <span data-ttu-id="0ef55-107">這種複製很容易維護，而且可能會在一段時間後導致不一致的內容。</span><span class="sxs-lookup"><span data-stu-id="0ef55-107">Such duplication is difficult to maintain and can lead to inconsistent content over time.</span></span> <span data-ttu-id="0ef55-108">*版面*配置會解決此問題。</span><span class="sxs-lookup"><span data-stu-id="0ef55-108">*Layouts* solve this problem.</span></span>

<span data-ttu-id="0ef55-109">就技術上而言，版面配置只是另一個元件。</span><span class="sxs-lookup"><span data-stu-id="0ef55-109">Technically, a layout is just another component.</span></span> <span data-ttu-id="0ef55-110">版面配置定義于 Razor 範本或程式碼中C# ，而且可以使用[資料](xref:blazor/components#data-binding)系結、相依性[插入](xref:blazor/dependency-injection)和其他元件案例。</span><span class="sxs-lookup"><span data-stu-id="0ef55-110">A layout is defined in a Razor template or in C# code and can use [data binding](xref:blazor/components#data-binding), [dependency injection](xref:blazor/dependency-injection), and other component scenarios.</span></span>

<span data-ttu-id="0ef55-111">若要將*元件*轉換成*版面*配置，元件：</span><span class="sxs-lookup"><span data-stu-id="0ef55-111">To turn a *component* into a *layout*, the component:</span></span>

* <span data-ttu-id="0ef55-112">繼承自`LayoutComponentBase`，其`Body`定義配置內呈現之內容的屬性。</span><span class="sxs-lookup"><span data-stu-id="0ef55-112">Inherits from `LayoutComponentBase`, which defines a `Body` property for the rendered content inside the layout.</span></span>
* <span data-ttu-id="0ef55-113">會使用 Razor 語法`@Body` ，在版面配置標記中，指定轉譯內容的位置。</span><span class="sxs-lookup"><span data-stu-id="0ef55-113">Uses the Razor syntax `@Body` to specify the location in the layout markup where the content is rendered.</span></span>

<span data-ttu-id="0ef55-114">下列程式碼範例顯示*MainLayout*版面配置元件的 razor 範本。</span><span class="sxs-lookup"><span data-stu-id="0ef55-114">The following code sample shows the Razor template of a layout component, *MainLayout.razor*.</span></span> <span data-ttu-id="0ef55-115">版面配置會`LayoutComponentBase`繼承並設定`@Body`導覽列和頁尾之間的：</span><span class="sxs-lookup"><span data-stu-id="0ef55-115">The layout inherits `LayoutComponentBase` and sets the `@Body` between the navigation bar and the footer:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

<span data-ttu-id="0ef55-116">在以其中一個 Blazor 應用程式範本為基礎的應用程式`MainLayout`中，元件（*MainLayout*）位於應用程式的*共用*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="0ef55-116">In an app based on one of the Blazor app templates, the `MainLayout` component (*MainLayout.razor*) is in the app's *Shared* folder.</span></span>

## <a name="default-layout"></a><span data-ttu-id="0ef55-117">預設版面配置</span><span class="sxs-lookup"><span data-stu-id="0ef55-117">Default layout</span></span>

<span data-ttu-id="0ef55-118">在應用程式的*razor*檔案中`Router` ，指定元件中的預設應用程式佈建。</span><span class="sxs-lookup"><span data-stu-id="0ef55-118">Specify the default app layout in the `Router` component in the app's *App.razor* file.</span></span> <span data-ttu-id="0ef55-119">下列`Router`元件（由預設的 Blazor 範本提供）會將預設配置設定`MainLayout`為元件：</span><span class="sxs-lookup"><span data-stu-id="0ef55-119">The following `Router` component, which is provided by the default Blazor templates, sets the default layout to the `MainLayout` component:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/App1.razor?highlight=3)]

<span data-ttu-id="0ef55-120">若要提供`NotFound`內容的預設版面配置，請`LayoutView`指定`NotFound`內容的：</span><span class="sxs-lookup"><span data-stu-id="0ef55-120">To supply a default layout for `NotFound` content, specify a `LayoutView` for `NotFound` content:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/App2.razor?highlight=6-9)]

<span data-ttu-id="0ef55-121">如需`Router`元件的詳細資訊，請<xref:blazor/routing>參閱。</span><span class="sxs-lookup"><span data-stu-id="0ef55-121">For more information on the `Router` component, see <xref:blazor/routing>.</span></span>

## <a name="specify-a-layout-in-a-component"></a><span data-ttu-id="0ef55-122">在元件中指定版面配置</span><span class="sxs-lookup"><span data-stu-id="0ef55-122">Specify a layout in a component</span></span>

<span data-ttu-id="0ef55-123">使用 Razor `@layout`指示詞將版面配置套用至元件。</span><span class="sxs-lookup"><span data-stu-id="0ef55-123">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="0ef55-124">編譯器會將`@layout`轉換`LayoutAttribute`成，並將其套用至元件類別。</span><span class="sxs-lookup"><span data-stu-id="0ef55-124">The compiler converts `@layout` into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="0ef55-125">下列`MasterList`元件的內容會插入`MasterLayout`至的位置`@Body`：</span><span class="sxs-lookup"><span data-stu-id="0ef55-125">The content of the following `MasterList` component is inserted into the `MasterLayout` at the position of `@Body`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

## <a name="centralized-layout-selection"></a><span data-ttu-id="0ef55-126">集中式版面配置選取</span><span class="sxs-lookup"><span data-stu-id="0ef55-126">Centralized layout selection</span></span>

<span data-ttu-id="0ef55-127">應用程式的每個資料夾都可以選擇性地包含名為 *_Imports*的範本檔案。</span><span class="sxs-lookup"><span data-stu-id="0ef55-127">Every folder of an app can optionally contain a template file named *_Imports.razor*.</span></span> <span data-ttu-id="0ef55-128">編譯器包含相同資料夾中所有 Razor 範本的匯入檔案中所指定的指示詞，並且會以遞迴方式在其所有子資料夾中進行。</span><span class="sxs-lookup"><span data-stu-id="0ef55-128">The compiler includes the directives specified in the imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="0ef55-129">因此，包含`@layout MyCoolLayout`的 *_Imports razor*檔案可確保資料夾中的所有元件都使用。 `MyCoolLayout`</span><span class="sxs-lookup"><span data-stu-id="0ef55-129">Therefore, an *_Imports.razor* file containing `@layout MyCoolLayout` ensures that all of the components in a folder use `MyCoolLayout`.</span></span> <span data-ttu-id="0ef55-130">不需要重複新增`@layout MyCoolLayout`至資料夾和子資料夾內的所有*razor*檔案。</span><span class="sxs-lookup"><span data-stu-id="0ef55-130">There's no need to repeatedly add `@layout MyCoolLayout` to all of the *.razor* files within the folder and subfolders.</span></span> <span data-ttu-id="0ef55-131">`@using`指示詞也會以同樣的方式套用至元件。</span><span class="sxs-lookup"><span data-stu-id="0ef55-131">`@using` directives are also applied to components in the same way.</span></span>

<span data-ttu-id="0ef55-132">下列 *_Imports razor*檔案匯入：</span><span class="sxs-lookup"><span data-stu-id="0ef55-132">The following *_Imports.razor* file imports:</span></span>

* <span data-ttu-id="0ef55-133">`MyCoolLayout`.</span><span class="sxs-lookup"><span data-stu-id="0ef55-133">`MyCoolLayout`.</span></span>
* <span data-ttu-id="0ef55-134">相同資料夾和任何子資料夾中的所有 Razor 元件。</span><span class="sxs-lookup"><span data-stu-id="0ef55-134">All Razor components in the same folder and any subfolders.</span></span>
* <span data-ttu-id="0ef55-135">`BlazorApp1.Data` 命名空間。</span><span class="sxs-lookup"><span data-stu-id="0ef55-135">The `BlazorApp1.Data` namespace.</span></span>
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

<span data-ttu-id="0ef55-136">*_Imports* razor 檔案類似[razor 視圖和頁面的 _ViewImports](xref:mvc/views/layout#importing-shared-directives)檔案，但特別適用于 razor 元件檔案。</span><span class="sxs-lookup"><span data-stu-id="0ef55-136">The *_Imports.razor* file is similar to the [_ViewImports.cshtml file for Razor views and pages](xref:mvc/views/layout#importing-shared-directives) but applied specifically to Razor component files.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="0ef55-137">嵌套版面配置</span><span class="sxs-lookup"><span data-stu-id="0ef55-137">Nested layouts</span></span>

<span data-ttu-id="0ef55-138">應用程式可以包含嵌套的版面配置。</span><span class="sxs-lookup"><span data-stu-id="0ef55-138">Apps can consist of nested layouts.</span></span> <span data-ttu-id="0ef55-139">元件可以參考配置，而該配置會轉而參考另一個版面配置。</span><span class="sxs-lookup"><span data-stu-id="0ef55-139">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="0ef55-140">例如，用來建立多層級功能表結構的嵌套配置。</span><span class="sxs-lookup"><span data-stu-id="0ef55-140">For example, nesting layouts are used to create a multi-level menu structure.</span></span>

<span data-ttu-id="0ef55-141">下列範例顯示如何使用嵌套的配置。</span><span class="sxs-lookup"><span data-stu-id="0ef55-141">The following example shows how to use nested layouts.</span></span> <span data-ttu-id="0ef55-142">*EpisodesComponent. razor*檔案是要顯示的元件。</span><span class="sxs-lookup"><span data-stu-id="0ef55-142">The *EpisodesComponent.razor* file is the component to display.</span></span> <span data-ttu-id="0ef55-143">元件會參考`MasterListLayout`：</span><span class="sxs-lookup"><span data-stu-id="0ef55-143">The component references the `MasterListLayout`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

<span data-ttu-id="0ef55-144">*MasterListLayout razor*檔案提供`MasterListLayout`。</span><span class="sxs-lookup"><span data-stu-id="0ef55-144">The *MasterListLayout.razor* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="0ef55-145">版面配置會參考另一個`MasterLayout`版面配置，也就是其呈現位置。</span><span class="sxs-lookup"><span data-stu-id="0ef55-145">The layout references another layout, `MasterLayout`, where it's rendered.</span></span> <span data-ttu-id="0ef55-146">`EpisodesComponent`會在出現`@Body`的位置呈現：</span><span class="sxs-lookup"><span data-stu-id="0ef55-146">`EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

<span data-ttu-id="0ef55-147">最後， `MasterLayout`在*MasterLayout*中，包含最上層的版面配置元素，例如頁首、主功能表和頁尾。</span><span class="sxs-lookup"><span data-stu-id="0ef55-147">Finally, `MasterLayout` in *MasterLayout.razor* contains the top-level layout elements, such as the header, main menu, and footer.</span></span> <span data-ttu-id="0ef55-148">`MasterListLayout``EpisodesComponent` 當`@Body`出現時，會呈現：</span><span class="sxs-lookup"><span data-stu-id="0ef55-148">`MasterListLayout` with the `EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a><span data-ttu-id="0ef55-149">其他資源</span><span class="sxs-lookup"><span data-stu-id="0ef55-149">Additional resources</span></span>

* <xref:mvc/views/layout>
