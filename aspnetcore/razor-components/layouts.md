---
title: Razor 元件版面配置
author: guardrex
description: 了解如何建立可重複使用的配置元件 Razor 元件應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/layouts
ms.openlocfilehash: 31ed940ce40e3ae6e3744418cf241d396308f4fe
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515411"
---
# <a name="razor-components-layouts"></a><span data-ttu-id="7d03c-103">Razor 元件版面配置</span><span class="sxs-lookup"><span data-stu-id="7d03c-103">Razor Components layouts</span></span>

<span data-ttu-id="7d03c-104">藉由[Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="7d03c-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="7d03c-105">應用程式通常會包含一個以上的元件。</span><span class="sxs-lookup"><span data-stu-id="7d03c-105">Apps typically contain more than one component.</span></span> <span data-ttu-id="7d03c-106">版面配置項目，例如功能表、 著作權訊息，以及標誌，必須要有所有的元件。</span><span class="sxs-lookup"><span data-stu-id="7d03c-106">Layout elements, such as menus, copyright messages, and logos, must be present on all components.</span></span> <span data-ttu-id="7d03c-107">將這些版面配置元素的程式碼複製到所有應用程式的元件不是有效的方法。</span><span class="sxs-lookup"><span data-stu-id="7d03c-107">Copying the code of these layout elements into all of the components of an app isn't an efficient approach.</span></span> <span data-ttu-id="7d03c-108">這類重複資料刪除會很難維護，並可能會導致不一致的內容經過一段時間。</span><span class="sxs-lookup"><span data-stu-id="7d03c-108">Such duplication is hard to maintain and probably leads to inconsistent content over time.</span></span> <span data-ttu-id="7d03c-109">*版面配置*解決這個問題。</span><span class="sxs-lookup"><span data-stu-id="7d03c-109">*Layouts* solve this problem.</span></span>

<span data-ttu-id="7d03c-110">技術上來說，版面配置是只是另一個元件。</span><span class="sxs-lookup"><span data-stu-id="7d03c-110">Technically, a layout is just another component.</span></span> <span data-ttu-id="7d03c-111">版面配置定義中的 Razor 範本，或在C#程式碼，而且可以包含[資料繫結](xref:razor-components/components#data-binding)，[相依性插入](xref:razor-components/dependency-injection)，以及其他一般功能的元件。</span><span class="sxs-lookup"><span data-stu-id="7d03c-111">A layout is defined in a Razor template or in C# code and can contain [data binding](xref:razor-components/components#data-binding), [dependency injection](xref:razor-components/dependency-injection), and other ordinary features of components.</span></span>

<span data-ttu-id="7d03c-112">開啟兩個的其他層面*元件*到*版面配置*</span><span class="sxs-lookup"><span data-stu-id="7d03c-112">Two additional aspects turn a *component* into a *layout*</span></span>

* <span data-ttu-id="7d03c-113">配置元件必須繼承自`LayoutComponentBase`。</span><span class="sxs-lookup"><span data-stu-id="7d03c-113">The layout component must inherit from `LayoutComponentBase`.</span></span> `LayoutComponentBase` <span data-ttu-id="7d03c-114">定義`Body`屬性，其中包含要配置內呈現的內容。</span><span class="sxs-lookup"><span data-stu-id="7d03c-114">defines a `Body` property that contains the content to be rendered inside the layout.</span></span>
* <span data-ttu-id="7d03c-115">配置元件會使用`Body`屬性來指定應本文內容呈現使用 Razor 語法`@Body`。</span><span class="sxs-lookup"><span data-stu-id="7d03c-115">The layout component uses the `Body` property to specify where the body content should be rendered using the Razor syntax `@Body`.</span></span> <span data-ttu-id="7d03c-116">在呈現期間，`@Body`版面配置的內容所取代。</span><span class="sxs-lookup"><span data-stu-id="7d03c-116">During rendering, `@Body` is replaced by the content of the layout.</span></span>

<span data-ttu-id="7d03c-117">下列程式碼範例顯示的 Razor 範本的配置元件。</span><span class="sxs-lookup"><span data-stu-id="7d03c-117">The following code sample shows the Razor template of a layout component.</span></span> <span data-ttu-id="7d03c-118">請注意，使用`LayoutComponentBase`和`@Body`:</span><span class="sxs-lookup"><span data-stu-id="7d03c-118">Note the use of `LayoutComponentBase` and `@Body`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.cshtml)]

## <a name="use-a-layout-in-a-component"></a><span data-ttu-id="7d03c-119">元件中使用的版面配置</span><span class="sxs-lookup"><span data-stu-id="7d03c-119">Use a layout in a component</span></span>

<span data-ttu-id="7d03c-120">使用 Razor 指示詞`@layout`来套用至元件的版面配置。</span><span class="sxs-lookup"><span data-stu-id="7d03c-120">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="7d03c-121">編譯器會將轉換成這個指示詞`LayoutAttribute`，套用至元件類別。</span><span class="sxs-lookup"><span data-stu-id="7d03c-121">The compiler converts this directive into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="7d03c-122">下列程式碼範例示範概念。</span><span class="sxs-lookup"><span data-stu-id="7d03c-122">The following code sample demonstrates the concept.</span></span> <span data-ttu-id="7d03c-123">此元件的內容會插入*MasterLayout*位置的`@Body`:</span><span class="sxs-lookup"><span data-stu-id="7d03c-123">The content of this component is inserted into the *MasterLayout* at the position of `@Body`:</span></span>

```cshtml
@layout MasterLayout
@page "/master-list"

<h2>Master Episode List</h2>
```

## <a name="centralized-layout-selection"></a><span data-ttu-id="7d03c-124">集中式的配置選取項目</span><span class="sxs-lookup"><span data-stu-id="7d03c-124">Centralized layout selection</span></span>

<span data-ttu-id="7d03c-125">每個資料夾的應用程式可以選擇性地包含範本檔案，名為 *_ViewImports.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="7d03c-125">Every folder of a an app can optionally contain a template file named *_ViewImports.cshtml*.</span></span> <span data-ttu-id="7d03c-126">編譯器會包含在相同的資料夾中的 Razor 範本，然後遞迴地在其所有子資料夾中的所有檢視匯入檔案中指定的指示詞。</span><span class="sxs-lookup"><span data-stu-id="7d03c-126">The compiler includes the directives specified in the view imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="7d03c-127">因此， *_ViewImports.cshtml*檔案，其中`@layout MainLayout`可確保所有資料夾使用中的元件*MainLayout*版面配置。</span><span class="sxs-lookup"><span data-stu-id="7d03c-127">Therefore, a *_ViewImports.cshtml* file containing `@layout MainLayout` ensures that all of the components in a folder use the *MainLayout* layout.</span></span> <span data-ttu-id="7d03c-128">不需要重複新增`@layout`的所有 *.razor*檔案。</span><span class="sxs-lookup"><span data-stu-id="7d03c-128">There's no need to repeatedly add `@layout` to all of the *.razor* files.</span></span>

<span data-ttu-id="7d03c-129">請注意，預設範本會使用 *_ViewImports.cshtml*版面配置選擇的機制。</span><span class="sxs-lookup"><span data-stu-id="7d03c-129">Note that the default template uses the *_ViewImports.cshtml* mechanism for layout selection.</span></span> <span data-ttu-id="7d03c-130">新建立的應用程式包含 *_ViewImports.cshtml*中的檔案*元件/頁面*資料夾。</span><span class="sxs-lookup"><span data-stu-id="7d03c-130">A newly created app contains the *_ViewImports.cshtml* file in the *Components/Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="7d03c-131">巢狀版面配置</span><span class="sxs-lookup"><span data-stu-id="7d03c-131">Nested layouts</span></span>

<span data-ttu-id="7d03c-132">應用程式可以包含巢狀版面配置。</span><span class="sxs-lookup"><span data-stu-id="7d03c-132">Apps can consist of nested layouts.</span></span> <span data-ttu-id="7d03c-133">元件可以參考會參考另一個版面配置的配置。</span><span class="sxs-lookup"><span data-stu-id="7d03c-133">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="7d03c-134">例如，巢狀版面配置可用來反映多層級的功能表結構。</span><span class="sxs-lookup"><span data-stu-id="7d03c-134">For example, nesting layouts can be used to reflect a multi-level menu structure.</span></span>

<span data-ttu-id="7d03c-135">下列程式碼範例示範如何使用巢狀的版面配置。</span><span class="sxs-lookup"><span data-stu-id="7d03c-135">The following code samples show how to use nested layouts.</span></span> <span data-ttu-id="7d03c-136">*EpisodesComponent.cshtml*檔案是要顯示的元件。</span><span class="sxs-lookup"><span data-stu-id="7d03c-136">The *EpisodesComponent.cshtml* file is the component to display.</span></span> <span data-ttu-id="7d03c-137">附註的元件參考配置`MasterListLayout`。</span><span class="sxs-lookup"><span data-stu-id="7d03c-137">Note that the component references the layout `MasterListLayout`.</span></span>

<span data-ttu-id="7d03c-138">*EpisodesComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7d03c-138">*EpisodesComponent.cshtml*:</span></span>

```cshtml
@layout MasterListLayout
@page "/master-list/episodes"

<h1>Episodes</h1>
```

<span data-ttu-id="7d03c-139">*MasterListLayout.cshtml*檔案提供`MasterListLayout`。</span><span class="sxs-lookup"><span data-stu-id="7d03c-139">The *MasterListLayout.cshtml* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="7d03c-140">版面配置參考另一個版面配置， `MasterLayout`，它會內嵌。</span><span class="sxs-lookup"><span data-stu-id="7d03c-140">The layout references another layout, `MasterLayout`, where it's going to be embedded.</span></span>

<span data-ttu-id="7d03c-141">*MasterListLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7d03c-141">*MasterListLayout.cshtml*:</span></span>

```cshtml
@layout MasterLayout
@inherits LayoutComponentBase

<nav>
    <!-- Menu structure of master list -->
    ...
</nav>

@Body
```

<span data-ttu-id="7d03c-142">最後，`MasterLayout`包含最上層的版面配置項目，例如頁首、 頁尾及主功能表。</span><span class="sxs-lookup"><span data-stu-id="7d03c-142">Finally, `MasterLayout` contains the top-level layout elements, such as the header, footer, and main menu.</span></span>

<span data-ttu-id="7d03c-143">*MasterLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7d03c-143">*MasterLayout.cshtml*:</span></span>

```cshtml
@inherits LayoutComponentBase

<header>...</header>
<nav>...</nav>

@Body
```
