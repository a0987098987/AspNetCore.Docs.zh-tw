---
title: ASP.NET Core 樣板 Blazor 化元件
author: guardrex
description: 瞭解樣板化元件可以如何接受一或多個 UI 範本做為參數，然後用來做為元件轉譯邏輯的一部分。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/18/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/components/templated-components
ms.openlocfilehash: ae591fb8280b706d568dd530e2e60a2f7955841c
ms.sourcegitcommit: 490434a700ba8c5ed24d849bd99d8489858538e3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85103617"
---
# <a name="aspnet-core-blazor-templated-components"></a><span data-ttu-id="11984-103">ASP.NET Core 樣板 Blazor 化元件</span><span class="sxs-lookup"><span data-stu-id="11984-103">ASP.NET Core Blazor templated components</span></span>

<span data-ttu-id="11984-104">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="11984-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="11984-105">樣板化元件是接受一或多個 UI 範本做為參數的元件，然後可以用來做為元件轉譯邏輯的一部分。</span><span class="sxs-lookup"><span data-stu-id="11984-105">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="11984-106">樣板化元件可讓您撰寫比一般元件更容易重複使用的較高層級元件。</span><span class="sxs-lookup"><span data-stu-id="11984-106">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="11984-107">其中有幾個範例包括：</span><span class="sxs-lookup"><span data-stu-id="11984-107">A couple of examples include:</span></span>

* <span data-ttu-id="11984-108">資料表元件，可讓使用者指定資料表標頭、資料列和頁尾的範本。</span><span class="sxs-lookup"><span data-stu-id="11984-108">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="11984-109">清單元件，可讓使用者指定範本來轉譯清單中的專案。</span><span class="sxs-lookup"><span data-stu-id="11984-109">A list component that allows a user to specify a template for rendering items in a list.</span></span>

## <a name="template-parameters"></a><span data-ttu-id="11984-110">範本參數</span><span class="sxs-lookup"><span data-stu-id="11984-110">Template parameters</span></span>

<span data-ttu-id="11984-111">樣板化元件是藉由指定一或多個類型為或的元件參數所定義 <xref:Microsoft.AspNetCore.Components.RenderFragment> <xref:Microsoft.AspNetCore.Components.RenderFragment%601> 。</span><span class="sxs-lookup"><span data-stu-id="11984-111">A templated component is defined by specifying one or more component parameters of type <xref:Microsoft.AspNetCore.Components.RenderFragment> or <xref:Microsoft.AspNetCore.Components.RenderFragment%601>.</span></span> <span data-ttu-id="11984-112">呈現片段代表要呈現的 UI 區段。</span><span class="sxs-lookup"><span data-stu-id="11984-112">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="11984-113"><xref:Microsoft.AspNetCore.Components.RenderFragment%601>採用可在叫用轉譯片段時指定的類型參數。</span><span class="sxs-lookup"><span data-stu-id="11984-113"><xref:Microsoft.AspNetCore.Components.RenderFragment%601> takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="11984-114">`TableTemplate`成分</span><span class="sxs-lookup"><span data-stu-id="11984-114">`TableTemplate` component:</span></span>

[!code-razor[](../common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="11984-115">使用樣板化元件時，可以使用符合參數名稱的子項目來指定範本參數（ `TableHeader` `RowTemplate` 在下列範例中為）：</span><span class="sxs-lookup"><span data-stu-id="11984-115">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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
> <span data-ttu-id="11984-116">在未來的版本中將會支援泛型型別條件約束。</span><span class="sxs-lookup"><span data-stu-id="11984-116">Generic type constraints will be supported in a future release.</span></span> <span data-ttu-id="11984-117">如需詳細資訊，請參閱[允許泛型型別條件約束（dotnet/aspnetcore #8433）](https://github.com/dotnet/aspnetcore/issues/8433)。</span><span class="sxs-lookup"><span data-stu-id="11984-117">For more information, see [Allow generic type constraints (dotnet/aspnetcore #8433)](https://github.com/dotnet/aspnetcore/issues/8433).</span></span>

## <a name="template-context-parameters"></a><span data-ttu-id="11984-118">範本內容參數</span><span class="sxs-lookup"><span data-stu-id="11984-118">Template context parameters</span></span>

<span data-ttu-id="11984-119">當做元素傳遞之類型的元件引數 <xref:Microsoft.AspNetCore.Components.RenderFragment%601> 具有名為的隱含參數 `context` （例如，從上述程式碼範例中 `@context.PetId` ），但您可以使用 `Context` 子項目上的屬性來變更參數名稱。</span><span class="sxs-lookup"><span data-stu-id="11984-119">Component arguments of type <xref:Microsoft.AspNetCore.Components.RenderFragment%601> passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="11984-120">在下列範例中， `RowTemplate` 元素的 `Context` 屬性會指定 `pet` 參數：</span><span class="sxs-lookup"><span data-stu-id="11984-120">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="11984-121">或者，您也可以在 `Context` component 元素上指定屬性。</span><span class="sxs-lookup"><span data-stu-id="11984-121">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="11984-122">指定的 `Context` 屬性會套用至所有指定的範本參數。</span><span class="sxs-lookup"><span data-stu-id="11984-122">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="11984-123">當您想要指定隱含子內容的內容參數名稱時（不含任何換行的子項目），這會很有用。</span><span class="sxs-lookup"><span data-stu-id="11984-123">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="11984-124">在下列範例中， `Context` 屬性會出現在元素上， `TableTemplate` 並套用至所有範本參數：</span><span class="sxs-lookup"><span data-stu-id="11984-124">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

## <a name="generic-typed-components"></a><span data-ttu-id="11984-125">泛型型別元件</span><span class="sxs-lookup"><span data-stu-id="11984-125">Generic-typed components</span></span>

<span data-ttu-id="11984-126">樣板化元件通常會以一般方式輸入。</span><span class="sxs-lookup"><span data-stu-id="11984-126">Templated components are often generically typed.</span></span> <span data-ttu-id="11984-127">例如，泛型 `ListViewTemplate` 元件可以用來呈現 `IEnumerable<T>` 值。</span><span class="sxs-lookup"><span data-stu-id="11984-127">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="11984-128">若要定義泛型元件，請使用指示詞 [`@typeparam`](xref:mvc/views/razor#typeparam) 來指定類型參數：</span><span class="sxs-lookup"><span data-stu-id="11984-128">To define a generic component, use the [`@typeparam`](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-razor[](../common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="11984-129">使用泛型型別元件時，會在可能的情況下推斷型別參數：</span><span class="sxs-lookup"><span data-stu-id="11984-129">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```razor
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="11984-130">否則，必須使用符合型別參數名稱的屬性來明確指定型別參數。</span><span class="sxs-lookup"><span data-stu-id="11984-130">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="11984-131">在下列範例中，會 `TItem="Pet"` 指定類型：</span><span class="sxs-lookup"><span data-stu-id="11984-131">In the following example, `TItem="Pet"` specifies the type:</span></span>

```razor
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```
