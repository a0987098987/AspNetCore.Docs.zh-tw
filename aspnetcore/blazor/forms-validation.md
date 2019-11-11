---
title: ASP.NET Core Blazor 表單和驗證
author: guardrex
description: 瞭解如何在 Blazor 中使用表單和欄位驗證案例。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/04/2019
uid: blazor/forms-validation
ms.openlocfilehash: 6dcc36c5133367493b476655dbdf73b75db9d168
ms.sourcegitcommit: a7bbe3890befead19440075b05b9674351f98872
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2019
ms.locfileid: "73905738"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a><span data-ttu-id="30c6a-103">ASP.NET Core Blazor 表單和驗證</span><span class="sxs-lookup"><span data-stu-id="30c6a-103">ASP.NET Core Blazor forms and validation</span></span>

<span data-ttu-id="30c6a-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="30c6a-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="30c6a-105">使用[資料批註](xref:mvc/models/validation)的 Blazor 支援表單和驗證。</span><span class="sxs-lookup"><span data-stu-id="30c6a-105">Forms and validation are supported in Blazor using [data annotations](xref:mvc/models/validation).</span></span>

<span data-ttu-id="30c6a-106">下列 `ExampleModel` 類型會使用資料批註來定義驗證邏輯：</span><span class="sxs-lookup"><span data-stu-id="30c6a-106">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="30c6a-107">表單是使用 `EditForm` 元件所定義。</span><span class="sxs-lookup"><span data-stu-id="30c6a-107">A form is defined using the `EditForm` component.</span></span> <span data-ttu-id="30c6a-108">下列表單會示範典型的元素、元件和 Razor 程式碼：</span><span class="sxs-lookup"><span data-stu-id="30c6a-108">The following form demonstrates typical elements, components, and Razor code:</span></span>

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" @bind-Value="exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

@code {
    private ExampleModel exampleModel = new ExampleModel();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

* <span data-ttu-id="30c6a-109">表單會使用 `ExampleModel` 型別中定義的驗證，來驗證 `name` 欄位中的使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="30c6a-109">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="30c6a-110">模型會在元件的 `@code` 區塊中建立，並保存在私用欄位（`exampleModel`）中。</span><span class="sxs-lookup"><span data-stu-id="30c6a-110">The model is created in the component's `@code` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="30c6a-111">欄位會指派給 `<EditForm>` 元素的 `Model` 屬性。</span><span class="sxs-lookup"><span data-stu-id="30c6a-111">The field is assigned to the `Model` attribute of the `<EditForm>` element.</span></span>
* <span data-ttu-id="30c6a-112">`DataAnnotationsValidator` 元件會使用資料批註附加驗證支援。</span><span class="sxs-lookup"><span data-stu-id="30c6a-112">The `DataAnnotationsValidator` component attaches validation support using data annotations.</span></span>
* <span data-ttu-id="30c6a-113">`ValidationSummary` 元件會摘要說明驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="30c6a-113">The `ValidationSummary` component summarizes validation messages.</span></span>
* <span data-ttu-id="30c6a-114">當表單成功提交（通過驗證）時，就會觸發 `HandleValidSubmit`。</span><span class="sxs-lookup"><span data-stu-id="30c6a-114">`HandleValidSubmit` is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="30c6a-115">有一組內建的輸入元件可用來接收和驗證使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="30c6a-115">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="30c6a-116">當輸入變更時和送出表單時，會進行驗證。</span><span class="sxs-lookup"><span data-stu-id="30c6a-116">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="30c6a-117">下表顯示可用的輸入元件。</span><span class="sxs-lookup"><span data-stu-id="30c6a-117">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="30c6a-118">輸入元件</span><span class="sxs-lookup"><span data-stu-id="30c6a-118">Input component</span></span> | <span data-ttu-id="30c6a-119">呈現為&hellip;</span><span class="sxs-lookup"><span data-stu-id="30c6a-119">Rendered as&hellip;</span></span>       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

<span data-ttu-id="30c6a-120">所有的輸入元件（包括 `EditForm`）都支援任意屬性。</span><span class="sxs-lookup"><span data-stu-id="30c6a-120">All of the input components, including `EditForm`, support arbitrary attributes.</span></span> <span data-ttu-id="30c6a-121">任何不符合元件參數的屬性都會加入至轉譯的 HTML 專案。</span><span class="sxs-lookup"><span data-stu-id="30c6a-121">Any attribute that doesn't match a component parameter is added to the rendered HTML element.</span></span>

<span data-ttu-id="30c6a-122">輸入元件會提供預設行為，以便在編輯和變更其 CSS 類別時進行驗證，以反映欄位狀態。</span><span class="sxs-lookup"><span data-stu-id="30c6a-122">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="30c6a-123">某些元件包含有用的剖析邏輯。</span><span class="sxs-lookup"><span data-stu-id="30c6a-123">Some components include useful parsing logic.</span></span> <span data-ttu-id="30c6a-124">例如，`InputDate` 和 `InputNumber` 藉由將其註冊為驗證錯誤來適當地處理無法剖析的值。</span><span class="sxs-lookup"><span data-stu-id="30c6a-124">For example, `InputDate` and `InputNumber` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="30c6a-125">可以接受 null 值的類型也支援目標欄位的 null 屬性（例如，`int?`）。</span><span class="sxs-lookup"><span data-stu-id="30c6a-125">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="30c6a-126">下列 `Starship` 類型使用比先前 `ExampleModel`更大的屬性集和資料批註來定義驗證邏輯：</span><span class="sxs-lookup"><span data-stu-id="30c6a-126">The following `Starship` type defines validation logic using a larger set of properties and data annotations than the earlier `ExampleModel`:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    [Required]
    [StringLength(16, ErrorMessage = "Identifier too long (16 character limit).")]
    public string Identifier { get; set; }

    public string Description { get; set; }

    [Required]
    public string Classification { get; set; }

    [Range(1, 100000, ErrorMessage = "Accommodation invalid (1-100000).")]
    public int MaximumAccommodation { get; set; }

    [Required]
    [Range(typeof(bool), "true", "true", 
        ErrorMessage = "This form disallows unapproved ships.")]
    public bool IsValidatedDesign { get; set; }

    [Required]
    public DateTime ProductionDate { get; set; }
}
```

<span data-ttu-id="30c6a-127">在上述範例中，`Description` 是選擇性的，因為沒有任何資料批註存在。</span><span class="sxs-lookup"><span data-stu-id="30c6a-127">In the preceding example, `Description` is optional because no data annotations are present.</span></span>

<span data-ttu-id="30c6a-128">下列表單會使用 `Starship` 模型中所定義的驗證來驗證使用者輸入：</span><span class="sxs-lookup"><span data-stu-id="30c6a-128">The following form validates user input using the validation defined in the `Starship` model:</span></span>

```cshtml
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@starship" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label for="identifier">Identifier: </label>
        <InputText id="identifier" @bind-Value="starship.Identifier" />
    </p>
    <p>
        <label for="description">Description (optional): </label>
        <InputTextArea id="description" @bind-Value="starship.Description" />
    </p>
    <p>
        <label for="classification">Primary Classification: </label>
        <InputSelect id="classification" @bind-Value="starship.Classification">
            <option value="">Select classification ...</option>
            <option value="Exploration">Exploration</option>
            <option value="Diplomacy">Diplomacy</option>
            <option value="Defense">Defense</option>
        </InputSelect>
    </p>
    <p>
        <label for="accommodation">Maximum Accommodation: </label>
        <InputNumber id="accommodation" 
            @bind-Value="starship.MaximumAccommodation" />
    </p>
    <p>
        <label for="valid">Engineering Approval: </label>
        <InputCheckbox id="valid" @bind-Value="starship.IsValidatedDesign" />
    </p>
    <p>
        <label for="productionDate">Production Date: </label>
        <InputDate id="productionDate" @bind-Value="starship.ProductionDate" />
    </p>

    <button type="submit">Submit</button>

    <p>
        <a href="http://www.startrek.com/">Star Trek</a>, 
        &copy;1966-2019 CBS Studios, Inc. and 
        <a href="https://www.paramount.com">Paramount Pictures</a>
    </p>
</EditForm>

@code {
    private Starship starship = new Starship();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

<span data-ttu-id="30c6a-129">`EditForm` 會建立一個 `EditContext` 做為階層式[值](xref:blazor/components#cascading-values-and-parameters)，以追蹤編輯程式的相關中繼資料，包括已修改的欄位和目前的驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="30c6a-129">The `EditForm` creates an `EditContext` as a [cascading value](xref:blazor/components#cascading-values-and-parameters) that tracks metadata about the edit process, including which fields have been modified and the current validation messages.</span></span> <span data-ttu-id="30c6a-130">`EditForm` 也會提供有效和無效提交（`OnValidSubmit`、`OnInvalidSubmit`）的便利事件。</span><span class="sxs-lookup"><span data-stu-id="30c6a-130">The `EditForm` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="30c6a-131">或者，使用 `OnSubmit` 來觸發驗證，並使用自訂驗證程式代碼來檢查域值。</span><span class="sxs-lookup"><span data-stu-id="30c6a-131">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

## <a name="inputtext-based-on-the-input-event"></a><span data-ttu-id="30c6a-132">根據輸入事件的 InputText</span><span class="sxs-lookup"><span data-stu-id="30c6a-132">InputText based on the input event</span></span>

<span data-ttu-id="30c6a-133">使用 `InputText` 元件來建立使用 `input` 事件的自訂群組件，而不是 `change` 事件。</span><span class="sxs-lookup"><span data-stu-id="30c6a-133">Use the `InputText` component to create a custom component that uses the `input` event instead of the `change` event.</span></span>

<span data-ttu-id="30c6a-134">建立具有下列標記的元件，並使用元件，就像使用 `InputText` 一樣：</span><span class="sxs-lookup"><span data-stu-id="30c6a-134">Create a component with the following markup, and use the component just as `InputText` is used:</span></span>

```cshtml
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a><span data-ttu-id="30c6a-135">驗證支援</span><span class="sxs-lookup"><span data-stu-id="30c6a-135">Validation support</span></span>

<span data-ttu-id="30c6a-136">`DataAnnotationsValidator` 元件會使用資料批註，將驗證支援附加至串聯的 `EditContext`。</span><span class="sxs-lookup"><span data-stu-id="30c6a-136">The `DataAnnotationsValidator` component attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="30c6a-137">若要啟用使用資料批註進行驗證的支援，則需要這個明確的手勢。</span><span class="sxs-lookup"><span data-stu-id="30c6a-137">Enabling support for validation using data annotations requires this explicit gesture.</span></span> <span data-ttu-id="30c6a-138">若要使用不同于資料批註的驗證系統，請將 `DataAnnotationsValidator` 取代為自訂的執行。</span><span class="sxs-lookup"><span data-stu-id="30c6a-138">To use a different validation system than data annotations, replace the `DataAnnotationsValidator` with a custom implementation.</span></span> <span data-ttu-id="30c6a-139">ASP.NET Core 的執行可用於參考來源中的檢查： [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs)。</span><span class="sxs-lookup"><span data-stu-id="30c6a-139">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span></span>

<span data-ttu-id="30c6a-140">`ValidationSummary` 元件會匯總所有驗證訊息，類似于[驗證摘要](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="30c6a-140">The `ValidationSummary` component summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span></span>

<span data-ttu-id="30c6a-141">`ValidationMessage` 元件會顯示特定欄位的驗證訊息，類似于[驗證訊息標記](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)協助程式。</span><span class="sxs-lookup"><span data-stu-id="30c6a-141">The `ValidationMessage` component displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="30c6a-142">使用 `For` 屬性指定驗證欄位，並以 lambda 運算式命名模型屬性：</span><span class="sxs-lookup"><span data-stu-id="30c6a-142">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

<span data-ttu-id="30c6a-143">`ValidationMessage` 和 `ValidationSummary` 元件支援任意屬性。</span><span class="sxs-lookup"><span data-stu-id="30c6a-143">The `ValidationMessage` and `ValidationSummary` components support arbitrary attributes.</span></span> <span data-ttu-id="30c6a-144">任何不符合元件參數的屬性都會加入至產生的 `<div>` 或 `<ul>` 元素。</span><span class="sxs-lookup"><span data-stu-id="30c6a-144">Any attribute that doesn't match a component parameter is added to the generated `<div>` or `<ul>` element.</span></span>

::: moniker range=">= aspnetcore-3.1"

<span data-ttu-id="30c6a-145">**AspNetCore. Blazor. DataAnnotations. 驗證套件**</span><span class="sxs-lookup"><span data-stu-id="30c6a-145">**Microsoft.AspNetCore.Blazor.DataAnnotations.Validation package**</span></span>

<span data-ttu-id="30c6a-146">[AspNetCore. Blazor. 驗證](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation)是使用 `DataAnnotationsValidator` 元件填滿驗證體驗缺口的套件。</span><span class="sxs-lookup"><span data-stu-id="30c6a-146">The [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) is a package that fills validation experience gaps using the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="30c6a-147">封裝目前為*實驗*性，我們計畫在未來的版本中將這些案例加入至 ASP.NET Core 架構。</span><span class="sxs-lookup"><span data-stu-id="30c6a-147">The package is currently *experimental*, and we plan to add these scenarios into the ASP.NET Core framework in a future release.</span></span>

<span data-ttu-id="30c6a-148">`DataAnnotationsValidator` 元件不會驗證驗證模型上複雜屬性的子屬性。</span><span class="sxs-lookup"><span data-stu-id="30c6a-148">The `DataAnnotationsValidator` component doesn't validate subproperties of complex properties on a validating model.</span></span> <span data-ttu-id="30c6a-149">不會驗證集合類型屬性的專案。</span><span class="sxs-lookup"><span data-stu-id="30c6a-149">Items of collection-type properties aren't validated.</span></span> <span data-ttu-id="30c6a-150">若要驗證這些類型，`Microsoft.AspNetCore.Blazor.DataAnnotations.Validation` 封裝會引進可與 `ObjectGraphDataAnnotationsValidator` 元件一起運作的 `ValidateComplexType` 驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="30c6a-150">To validate these types, the `Microsoft.AspNetCore.Blazor.DataAnnotations.Validation` package introduces the `ValidateComplexType` validation attribute that works in tandem with the `ObjectGraphDataAnnotationsValidator` component.</span></span> <span data-ttu-id="30c6a-151">如需這些使用中類型的範例，請參閱[aspnet/Samples GitHub 存放庫中的 Blazor 驗證範例](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation)。</span><span class="sxs-lookup"><span data-stu-id="30c6a-151">For an example of these types in use, see the [Blazor Validation sample in the aspnet/samples GitHub repository ](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span></span>

<span data-ttu-id="30c6a-152"><xref:System.ComponentModel.DataAnnotations.CompareAttribute> 無法與 `DataAnnotationsValidator` 元件搭配運作。</span><span class="sxs-lookup"><span data-stu-id="30c6a-152">The <xref:System.ComponentModel.DataAnnotations.CompareAttribute> doesn't work well with the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="30c6a-153">`Microsoft.AspNetCore.Blazor.DataAnnotations.Validation` 套件導入了額外的驗證屬性（`ComparePropertyAttribute`），其運作方式會因應這些限制。</span><span class="sxs-lookup"><span data-stu-id="30c6a-153">The `Microsoft.AspNetCore.Blazor.DataAnnotations.Validation` package introduces an additional validation attribute, `ComparePropertyAttribute`, that works around these limitations.</span></span> <span data-ttu-id="30c6a-154">在 Blazor 應用程式中，`ComparePropertyAttribute` 是 `CompareAttribute`的直接取代。</span><span class="sxs-lookup"><span data-stu-id="30c6a-154">In a Blazor app, `ComparePropertyAttribute` is a direct replacement for the `CompareAttribute`.</span></span> <span data-ttu-id="30c6a-155">如需詳細資訊，請參閱[CompareAttribute 已忽略 With OnValidSubmit EditForm （aspnet/AspNetCore \#10643）](https://github.com/aspnet/AspNetCore/issues/10643#issuecomment-543909748)。</span><span class="sxs-lookup"><span data-stu-id="30c6a-155">For more information, see [CompareAttribute ignored with OnValidSubmit EditForm (aspnet/AspNetCore \#10643)](https://github.com/aspnet/AspNetCore/issues/10643#issuecomment-543909748).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.1"

### <a name="validation-of-complex-or-collection-type-properties"></a><span data-ttu-id="30c6a-156">複雜或集合型別屬性的驗證</span><span class="sxs-lookup"><span data-stu-id="30c6a-156">Validation of complex or collection type properties</span></span>

<span data-ttu-id="30c6a-157">當提交表單時，套用至模型屬性的驗證屬性會進行驗證。</span><span class="sxs-lookup"><span data-stu-id="30c6a-157">Validation attributes applied to the properties of a model validate when the form is submitted.</span></span> <span data-ttu-id="30c6a-158">不過，`DataAnnotationsValidator` 元件提交表單時，不會驗證集合的屬性或模型的複雜資料類型。</span><span class="sxs-lookup"><span data-stu-id="30c6a-158">However, the properties of collections or complex data types of a model aren't validated on form submission by the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="30c6a-159">若要在這種情況下接受嵌套驗證屬性，請使用自訂驗證元件。</span><span class="sxs-lookup"><span data-stu-id="30c6a-159">To honor the nested validation attributes in this scenario, use a custom validation component.</span></span> <span data-ttu-id="30c6a-160">如需範例，請參閱[aspnet/Samples GitHub 存放庫中的 Blazor 驗證範例](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation)。</span><span class="sxs-lookup"><span data-stu-id="30c6a-160">For an example, see the [Blazor Validation sample in the aspnet/samples GitHub repository](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span></span>

::: moniker-end
