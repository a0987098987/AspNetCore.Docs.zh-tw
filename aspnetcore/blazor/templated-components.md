---
title: ASP.NET核心Blazor樣本化元件
author: guardrex
description: 瞭解範本化元件如何接受一個或多個 UI 範本作為參數,然後用作元件呈現邏輯的一部分。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/18/2020
no-loc:
- Blazor
- SignalR
uid: blazor/templated-components
ms.openlocfilehash: b57e3fe186402723607e90b1628062f602c77632
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "79989491"
---
# <a name="aspnet-core-opno-locblazor-templated-components"></a><span data-ttu-id="27663-103">ASP.NET核心Blazor樣本化元件</span><span class="sxs-lookup"><span data-stu-id="27663-103">ASP.NET Core Blazor templated components</span></span>

<span data-ttu-id="27663-104">由[盧克·萊瑟姆](https://github.com/guardrex)和[丹尼爾·羅斯](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="27663-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="27663-105">範本化元件是接受一個或多個 UI 範本作為參數的元件,然後可用作元件呈現邏輯的一部分。</span><span class="sxs-lookup"><span data-stu-id="27663-105">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="27663-106">樣本化元件允許您編寫比常規元件更可重用的更高級別元件。</span><span class="sxs-lookup"><span data-stu-id="27663-106">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="27663-107">幾個例子包括:</span><span class="sxs-lookup"><span data-stu-id="27663-107">A couple of examples include:</span></span>

* <span data-ttu-id="27663-108">允許使用者為表的標頭、行和頁腳指定範本的表元件。</span><span class="sxs-lookup"><span data-stu-id="27663-108">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="27663-109">允許使用者指定用於呈現清單中項的範本的清單元件。</span><span class="sxs-lookup"><span data-stu-id="27663-109">A list component that allows a user to specify a template for rendering items in a list.</span></span>

## <a name="template-parameters"></a><span data-ttu-id="27663-110">範本參數</span><span class="sxs-lookup"><span data-stu-id="27663-110">Template parameters</span></span>

<span data-ttu-id="27663-111">樣本化元件通過指定類型`RenderFragment`或`RenderFragment<T>`的一個或多個元件參數來定義。</span><span class="sxs-lookup"><span data-stu-id="27663-111">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="27663-112">渲染片段表示要呈現的 UI 段。</span><span class="sxs-lookup"><span data-stu-id="27663-112">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="27663-113">`RenderFragment<T>`採用可在調用渲染片段時指定的類型參數。</span><span class="sxs-lookup"><span data-stu-id="27663-113">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="27663-114">`TableTemplate`元件:</span><span class="sxs-lookup"><span data-stu-id="27663-114">`TableTemplate` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="27663-115">使用樣本化元件時,可以使用與參數名稱符合的子元素(`TableHeader`並在`RowTemplate`以下範例中)指定樣本參數:</span><span class="sxs-lookup"><span data-stu-id="27663-115">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```razor
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

> [!NOTE]
> <span data-ttu-id="27663-116">泛型類型約束將在將來的版本中受支援。</span><span class="sxs-lookup"><span data-stu-id="27663-116">Generic type constraints will be supported in a future release.</span></span> <span data-ttu-id="27663-117">有關詳細資訊,請參閱[允許泛型類型約束(點網/阿斯平核心#8433)](https://github.com/dotnet/aspnetcore/issues/8433)。</span><span class="sxs-lookup"><span data-stu-id="27663-117">For more information, see [Allow generic type constraints (dotnet/aspnetcore #8433)](https://github.com/dotnet/aspnetcore/issues/8433).</span></span>

## <a name="template-context-parameters"></a><span data-ttu-id="27663-118">樣本內容參數</span><span class="sxs-lookup"><span data-stu-id="27663-118">Template context parameters</span></span>

<span data-ttu-id="27663-119">`RenderFragment<T>`作為元素傳遞的類型類型的元件參數具有一個隱式參數`context`(例如,來自前面的代碼示例),`@context.PetId`但可以使用子元素上的`Context`屬性 更改參數名稱。</span><span class="sxs-lookup"><span data-stu-id="27663-119">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="27663-120">在下面的範例中,`RowTemplate`元素的`Context`屬性 指定`pet`參數:</span><span class="sxs-lookup"><span data-stu-id="27663-120">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```razor
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

<span data-ttu-id="27663-121">或者,您可以在元件元素上`Context`指定屬性。</span><span class="sxs-lookup"><span data-stu-id="27663-121">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="27663-122">指定`Context`屬性適用於所有指定的範本參數。</span><span class="sxs-lookup"><span data-stu-id="27663-122">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="27663-123">當您要為隱式子內容指定內容參數名稱(沒有任何換行子元素)時,這非常有用。</span><span class="sxs-lookup"><span data-stu-id="27663-123">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="27663-124">在下面的範例中,該`Context`屬性顯示在元素`TableTemplate`上,並應用於所有樣本參數:</span><span class="sxs-lookup"><span data-stu-id="27663-124">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```razor
<TableTemplate Items="pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

## <a name="generic-typed-components"></a><span data-ttu-id="27663-125">通用類型元件</span><span class="sxs-lookup"><span data-stu-id="27663-125">Generic-typed components</span></span>

<span data-ttu-id="27663-126">樣本元件通常採用通用類型。</span><span class="sxs-lookup"><span data-stu-id="27663-126">Templated components are often generically typed.</span></span> <span data-ttu-id="27663-127">例如,泛型`ListViewTemplate`元件可用於呈現`IEnumerable<T>`值。</span><span class="sxs-lookup"><span data-stu-id="27663-127">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="27663-128">要定義泛型元件,請使用[`@typeparam`](xref:mvc/views/razor#typeparam)指令指定類型參數:</span><span class="sxs-lookup"><span data-stu-id="27663-128">To define a generic component, use the [`@typeparam`](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="27663-129">使用泛型型態元件時,如果可能,將推斷類型參數:</span><span class="sxs-lookup"><span data-stu-id="27663-129">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```razor
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="27663-130">否則,必須使用與類型參數名稱匹配的屬性顯式指定類型參數。</span><span class="sxs-lookup"><span data-stu-id="27663-130">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="27663-131">在下面的範例中,`TItem="Pet"`指定類型:</span><span class="sxs-lookup"><span data-stu-id="27663-131">In the following example, `TItem="Pet"` specifies the type:</span></span>

```razor
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```
