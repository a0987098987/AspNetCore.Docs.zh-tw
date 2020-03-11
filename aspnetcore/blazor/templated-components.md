---
title: ASP.NET Core Blazor 樣板化元件
author: guardrex
description: 瞭解樣板化元件可以如何接受一或多個 UI 範本做為參數，然後用來做為元件轉譯邏輯的一部分。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/templated-components
ms.openlocfilehash: b64d6a731e540b13c50b2c6108f75efd0ac9290c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78657714"
---
# <a name="aspnet-core-opno-locblazor-templated-components"></a><span data-ttu-id="dfb9a-103">ASP.NET Core Blazor 樣板化元件</span><span class="sxs-lookup"><span data-stu-id="dfb9a-103">ASP.NET Core Blazor templated components</span></span>

<span data-ttu-id="dfb9a-104">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="dfb9a-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="dfb9a-105">樣板化元件是接受一或多個 UI 範本做為參數的元件，然後可以用來做為元件轉譯邏輯的一部分。</span><span class="sxs-lookup"><span data-stu-id="dfb9a-105">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="dfb9a-106">樣板化元件可讓您撰寫比一般元件更容易重複使用的較高層級元件。</span><span class="sxs-lookup"><span data-stu-id="dfb9a-106">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="dfb9a-107">其中有幾個範例包括：</span><span class="sxs-lookup"><span data-stu-id="dfb9a-107">A couple of examples include:</span></span>

* <span data-ttu-id="dfb9a-108">資料表元件，可讓使用者指定資料表標頭、資料列和頁尾的範本。</span><span class="sxs-lookup"><span data-stu-id="dfb9a-108">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="dfb9a-109">清單元件，可讓使用者指定範本來轉譯清單中的專案。</span><span class="sxs-lookup"><span data-stu-id="dfb9a-109">A list component that allows a user to specify a template for rendering items in a list.</span></span>

## <a name="template-parameters"></a><span data-ttu-id="dfb9a-110">範本參數</span><span class="sxs-lookup"><span data-stu-id="dfb9a-110">Template parameters</span></span>

<span data-ttu-id="dfb9a-111">樣板化元件是藉由指定 `RenderFragment` 或 `RenderFragment<T>`類型的一或多個元件參數來定義。</span><span class="sxs-lookup"><span data-stu-id="dfb9a-111">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="dfb9a-112">呈現片段代表要呈現的 UI 區段。</span><span class="sxs-lookup"><span data-stu-id="dfb9a-112">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="dfb9a-113">`RenderFragment<T>` 會採用可在叫用轉譯片段時指定的類型參數。</span><span class="sxs-lookup"><span data-stu-id="dfb9a-113">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="dfb9a-114">`TableTemplate` 元件：</span><span class="sxs-lookup"><span data-stu-id="dfb9a-114">`TableTemplate` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="dfb9a-115">使用樣板化元件時，可以使用符合參數名稱的子專案來指定範本參數（`TableHeader`，並在下列範例中 `RowTemplate`）：</span><span class="sxs-lookup"><span data-stu-id="dfb9a-115">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

## <a name="template-context-parameters"></a><span data-ttu-id="dfb9a-116">範本內容參數</span><span class="sxs-lookup"><span data-stu-id="dfb9a-116">Template context parameters</span></span>

<span data-ttu-id="dfb9a-117">當做元素傳遞 `RenderFragment<T>` 類型的元件引數具有名為 `context` 的隱含參數（例如，從前面的程式碼範例，`@context.PetId`），但您可以使用子專案上的 `Context` 屬性來變更參數名稱。</span><span class="sxs-lookup"><span data-stu-id="dfb9a-117">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="dfb9a-118">在下列範例中，`RowTemplate` 元素的 `Context` 屬性會指定 `pet` 參數：</span><span class="sxs-lookup"><span data-stu-id="dfb9a-118">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="dfb9a-119">或者，您也可以指定 component 元素上的 `Context` 屬性。</span><span class="sxs-lookup"><span data-stu-id="dfb9a-119">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="dfb9a-120">指定的 `Context` 屬性會套用至所有指定的範本參數。</span><span class="sxs-lookup"><span data-stu-id="dfb9a-120">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="dfb9a-121">當您想要指定隱含子內容的內容參數名稱時（不含任何換行的子項目），這會很有用。</span><span class="sxs-lookup"><span data-stu-id="dfb9a-121">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="dfb9a-122">在下列範例中，`Context` 屬性會出現在 `TableTemplate` 元素上，並套用至所有範本參數：</span><span class="sxs-lookup"><span data-stu-id="dfb9a-122">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

## <a name="generic-typed-components"></a><span data-ttu-id="dfb9a-123">泛型型別元件</span><span class="sxs-lookup"><span data-stu-id="dfb9a-123">Generic-typed components</span></span>

<span data-ttu-id="dfb9a-124">樣板化元件通常會以一般方式輸入。</span><span class="sxs-lookup"><span data-stu-id="dfb9a-124">Templated components are often generically typed.</span></span> <span data-ttu-id="dfb9a-125">例如，泛型 `ListViewTemplate` 元件可以用來呈現 `IEnumerable<T>` 值。</span><span class="sxs-lookup"><span data-stu-id="dfb9a-125">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="dfb9a-126">若要定義泛型元件，請使用[`@typeparam`](xref:mvc/views/razor#typeparam)指示詞來指定類型參數：</span><span class="sxs-lookup"><span data-stu-id="dfb9a-126">To define a generic component, use the [`@typeparam`](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="dfb9a-127">使用泛型型別元件時，會在可能的情況下推斷型別參數：</span><span class="sxs-lookup"><span data-stu-id="dfb9a-127">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```razor
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="dfb9a-128">否則，必須使用符合型別參數名稱的屬性來明確指定型別參數。</span><span class="sxs-lookup"><span data-stu-id="dfb9a-128">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="dfb9a-129">在下列範例中，`TItem="Pet"` 會指定類型：</span><span class="sxs-lookup"><span data-stu-id="dfb9a-129">In the following example, `TItem="Pet"` specifies the type:</span></span>

```razor
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```
