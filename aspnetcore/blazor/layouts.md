---
title: ASP.NET Core Blazor 版面配置
author: guardrex
description: 瞭解如何為 Blazor 應用程式建立可重複使用的版面配置元件。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/layouts
ms.openlocfilehash: 2d652e149381f0a93e3135da978ab5737d47c6f1
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "68948218"
---
# <a name="aspnet-core-blazor-layouts"></a><span data-ttu-id="a5be2-103">ASP.NET Core Blazor 版面配置</span><span class="sxs-lookup"><span data-stu-id="a5be2-103">ASP.NET Core Blazor layouts</span></span>

<span data-ttu-id="a5be2-104">依[Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="a5be2-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="a5be2-105">某些應用程式專案 (例如功能表、著作權訊息和公司標誌) 通常是應用程式整體版面配置的一部分, 並由應用程式中的每個元件所使用。</span><span class="sxs-lookup"><span data-stu-id="a5be2-105">Some app elements, such as menus, copyright messages, and company logos, are usually part of app's overall layout and used by every component in the app.</span></span> <span data-ttu-id="a5be2-106">每當其中一個專案需要更新時, 將這些專案的程式碼複製到應用程式&mdash;的所有元件都不是有效的方法, 每個元件都必須更新。</span><span class="sxs-lookup"><span data-stu-id="a5be2-106">Copying the code of these elements into all of the components of an app isn't an efficient approach&mdash;every time one of the elements requires an update, every component must be updated.</span></span> <span data-ttu-id="a5be2-107">這種複製很容易維護, 而且可能會在一段時間後導致不一致的內容。</span><span class="sxs-lookup"><span data-stu-id="a5be2-107">Such duplication is difficult to maintain and can lead to inconsistent content over time.</span></span> <span data-ttu-id="a5be2-108">*版面*配置會解決此問題。</span><span class="sxs-lookup"><span data-stu-id="a5be2-108">*Layouts* solve this problem.</span></span>

<span data-ttu-id="a5be2-109">就技術上而言, 版面配置只是另一個元件。</span><span class="sxs-lookup"><span data-stu-id="a5be2-109">Technically, a layout is just another component.</span></span> <span data-ttu-id="a5be2-110">版面配置定義于 Razor 範本或程式碼中C# , 而且可以使用[資料](xref:blazor/components#data-binding)系結、相依性[插入](xref:blazor/dependency-injection)和其他元件案例。</span><span class="sxs-lookup"><span data-stu-id="a5be2-110">A layout is defined in a Razor template or in C# code and can use [data binding](xref:blazor/components#data-binding), [dependency injection](xref:blazor/dependency-injection), and other component scenarios.</span></span>

<span data-ttu-id="a5be2-111">若要將*元件*轉換成*版面*配置, 元件:</span><span class="sxs-lookup"><span data-stu-id="a5be2-111">To turn a *component* into a *layout*, the component:</span></span>

* <span data-ttu-id="a5be2-112">繼承自`LayoutComponentBase`, 其`Body`定義配置內呈現之內容的屬性。</span><span class="sxs-lookup"><span data-stu-id="a5be2-112">Inherits from `LayoutComponentBase`, which defines a `Body` property for the rendered content inside the layout.</span></span>
* <span data-ttu-id="a5be2-113">會使用 Razor 語法`@Body` , 在版面配置標記中, 指定轉譯內容的位置。</span><span class="sxs-lookup"><span data-stu-id="a5be2-113">Uses the Razor syntax `@Body` to specify the location in the layout markup where the content is rendered.</span></span>

<span data-ttu-id="a5be2-114">下列程式碼範例顯示*MainLayout*版面配置元件的 razor 範本。</span><span class="sxs-lookup"><span data-stu-id="a5be2-114">The following code sample shows the Razor template of a layout component, *MainLayout.razor*.</span></span> <span data-ttu-id="a5be2-115">版面配置會`LayoutComponentBase`繼承並設定`@Body`導覽列和頁尾之間的:</span><span class="sxs-lookup"><span data-stu-id="a5be2-115">The layout inherits `LayoutComponentBase` and sets the `@Body` between the navigation bar and the footer:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

## <a name="specify-a-layout-in-a-component"></a><span data-ttu-id="a5be2-116">在元件中指定版面配置</span><span class="sxs-lookup"><span data-stu-id="a5be2-116">Specify a layout in a component</span></span>

<span data-ttu-id="a5be2-117">使用 Razor `@layout`指示詞將版面配置套用至元件。</span><span class="sxs-lookup"><span data-stu-id="a5be2-117">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="a5be2-118">編譯器會將`@layout`轉換`LayoutAttribute`成, 並將其套用至元件類別。</span><span class="sxs-lookup"><span data-stu-id="a5be2-118">The compiler converts `@layout` into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="a5be2-119">下列元件 ( *MasterList*) 的內容會插入至`MainLayout`的位置: `@Body`</span><span class="sxs-lookup"><span data-stu-id="a5be2-119">The content of the following component, *MasterList.razor*, is inserted into the `MainLayout` at the position of `@Body`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

## <a name="centralized-layout-selection"></a><span data-ttu-id="a5be2-120">集中式版面配置選取</span><span class="sxs-lookup"><span data-stu-id="a5be2-120">Centralized layout selection</span></span>

<span data-ttu-id="a5be2-121">應用程式的每個資料夾都可以選擇性地包含名為 *_Imports*的範本檔案。</span><span class="sxs-lookup"><span data-stu-id="a5be2-121">Every folder of an app can optionally contain a template file named *_Imports.razor*.</span></span> <span data-ttu-id="a5be2-122">編譯器包含相同資料夾中所有 Razor 範本的匯入檔案中所指定的指示詞, 並且會以遞迴方式在其所有子資料夾中進行。</span><span class="sxs-lookup"><span data-stu-id="a5be2-122">The compiler includes the directives specified in the imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="a5be2-123">因此, 包含`@layout MainLayout`的 *_Imports razor*檔案可確保資料夾中的所有元件都使用。 `MainLayout`</span><span class="sxs-lookup"><span data-stu-id="a5be2-123">Therefore, an *_Imports.razor* file containing `@layout MainLayout` ensures that all of the components in a folder use `MainLayout`.</span></span> <span data-ttu-id="a5be2-124">不需要重複新增`@layout MainLayout`至資料夾和子資料夾內的所有*razor*檔案。</span><span class="sxs-lookup"><span data-stu-id="a5be2-124">There's no need to repeatedly add `@layout MainLayout` to all of the *.razor* files within the folder and subfolders.</span></span> <span data-ttu-id="a5be2-125">`@using`指示詞也會以同樣的方式套用至元件。</span><span class="sxs-lookup"><span data-stu-id="a5be2-125">`@using` directives are also applied to components in the same way.</span></span>

<span data-ttu-id="a5be2-126">下列 *_Imports razor*檔案匯入:</span><span class="sxs-lookup"><span data-stu-id="a5be2-126">The following *_Imports.razor* file imports:</span></span>

* <span data-ttu-id="a5be2-127">`MainLayout`.</span><span class="sxs-lookup"><span data-stu-id="a5be2-127">`MainLayout`.</span></span>
* <span data-ttu-id="a5be2-128">相同資料夾和任何子資料夾中的所有 Razor 元件。</span><span class="sxs-lookup"><span data-stu-id="a5be2-128">All Razor components in the same folder and any subfolders.</span></span>
* <span data-ttu-id="a5be2-129">`BlazorApp1.Data` 命名空間。</span><span class="sxs-lookup"><span data-stu-id="a5be2-129">The `BlazorApp1.Data` namespace.</span></span>
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

<span data-ttu-id="a5be2-130">*_Imports* razor 檔案類似[razor 視圖和頁面的 _ViewImports](xref:mvc/views/layout#importing-shared-directives)檔案, 但特別適用于 razor 元件檔案。</span><span class="sxs-lookup"><span data-stu-id="a5be2-130">The *_Imports.razor* file is similar to the [_ViewImports.cshtml file for Razor views and pages](xref:mvc/views/layout#importing-shared-directives) but applied specifically to Razor component files.</span></span>

<span data-ttu-id="a5be2-131">Blazor 範本會使用 *_Imports*檔案進行版面配置選擇。</span><span class="sxs-lookup"><span data-stu-id="a5be2-131">The Blazor templates use *_Imports.razor* files for layout selection.</span></span> <span data-ttu-id="a5be2-132">從 Blazor 範本建立的應用程式包含專案根目錄和*Pages*資料夾中的 *_Imports razor*檔案。</span><span class="sxs-lookup"><span data-stu-id="a5be2-132">An app created from a Blazor template contains the *_Imports.razor* file in the root of the project and in the *Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="a5be2-133">嵌套版面配置</span><span class="sxs-lookup"><span data-stu-id="a5be2-133">Nested layouts</span></span>

<span data-ttu-id="a5be2-134">應用程式可以包含嵌套的版面配置。</span><span class="sxs-lookup"><span data-stu-id="a5be2-134">Apps can consist of nested layouts.</span></span> <span data-ttu-id="a5be2-135">元件可以參考配置, 而該配置會轉而參考另一個版面配置。</span><span class="sxs-lookup"><span data-stu-id="a5be2-135">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="a5be2-136">例如, 用來建立多層級功能表結構的嵌套配置。</span><span class="sxs-lookup"><span data-stu-id="a5be2-136">For example, nesting layouts are used to create a multi-level menu structure.</span></span>

<span data-ttu-id="a5be2-137">下列範例顯示如何使用嵌套的配置。</span><span class="sxs-lookup"><span data-stu-id="a5be2-137">The following example shows how to use nested layouts.</span></span> <span data-ttu-id="a5be2-138">*EpisodesComponent. razor*檔案是要顯示的元件。</span><span class="sxs-lookup"><span data-stu-id="a5be2-138">The *EpisodesComponent.razor* file is the component to display.</span></span> <span data-ttu-id="a5be2-139">元件會參考`MasterListLayout`:</span><span class="sxs-lookup"><span data-stu-id="a5be2-139">The component references the `MasterListLayout`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

<span data-ttu-id="a5be2-140">*MasterListLayout razor*檔案提供`MasterListLayout`。</span><span class="sxs-lookup"><span data-stu-id="a5be2-140">The *MasterListLayout.razor* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="a5be2-141">版面配置會參考另一個`MasterLayout`版面配置, 也就是其呈現位置。</span><span class="sxs-lookup"><span data-stu-id="a5be2-141">The layout references another layout, `MasterLayout`, where it's rendered.</span></span> <span data-ttu-id="a5be2-142">`EpisodesComponent`會在出現`@Body`的位置呈現:</span><span class="sxs-lookup"><span data-stu-id="a5be2-142">`EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

<span data-ttu-id="a5be2-143">最後, `MasterLayout`在*MasterLayout*中, 包含最上層的版面配置元素, 例如頁首、主功能表和頁尾。</span><span class="sxs-lookup"><span data-stu-id="a5be2-143">Finally, `MasterLayout` in *MasterLayout.razor* contains the top-level layout elements, such as the header, main menu, and footer.</span></span> <span data-ttu-id="a5be2-144">`MasterListLayout`會`EpisodesComponent`在出現的`@Body`處呈現:</span><span class="sxs-lookup"><span data-stu-id="a5be2-144">`MasterListLayout` with `EpisodesComponent` are rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a><span data-ttu-id="a5be2-145">其他資源</span><span class="sxs-lookup"><span data-stu-id="a5be2-145">Additional resources</span></span>

* <xref:mvc/views/layout>
