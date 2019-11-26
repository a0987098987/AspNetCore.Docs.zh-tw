---
title: ASP.NET Core Blazor 表單和驗證
author: guardrex
description: 瞭解如何在 Blazor中使用表單和欄位驗證案例。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- Blazor
uid: blazor/forms-validation
ms.openlocfilehash: f1df213b16bb7ecd6a771700291d834776dee475
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317173"
---
# <a name="aspnet-core-opno-locblazor-forms-and-validation"></a><span data-ttu-id="9992d-103">ASP.NET Core Blazor 表單和驗證</span><span class="sxs-lookup"><span data-stu-id="9992d-103">ASP.NET Core Blazor forms and validation</span></span>

<span data-ttu-id="9992d-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9992d-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9992d-105">使用[資料批註](xref:mvc/models/validation)的 Blazor 中支援表單和驗證。</span><span class="sxs-lookup"><span data-stu-id="9992d-105">Forms and validation are supported in Blazor using [data annotations](xref:mvc/models/validation).</span></span>

<span data-ttu-id="9992d-106">下列 `ExampleModel` 類型會使用資料批註來定義驗證邏輯：</span><span class="sxs-lookup"><span data-stu-id="9992d-106">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="9992d-107">表單是使用 `EditForm` 元件所定義。</span><span class="sxs-lookup"><span data-stu-id="9992d-107">A form is defined using the `EditForm` component.</span></span> <span data-ttu-id="9992d-108">下列表單會示範典型的元素、元件和 Razor 程式碼：</span><span class="sxs-lookup"><span data-stu-id="9992d-108">The following form demonstrates typical elements, components, and Razor code:</span></span>

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

* <span data-ttu-id="9992d-109">表單會使用 `ExampleModel` 型別中定義的驗證，來驗證 `name` 欄位中的使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="9992d-109">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="9992d-110">模型會在元件的 `@code` 區塊中建立，並保存在私用欄位（`exampleModel`）中。</span><span class="sxs-lookup"><span data-stu-id="9992d-110">The model is created in the component's `@code` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="9992d-111">欄位會指派給 `<EditForm>` 元素的 `Model` 屬性。</span><span class="sxs-lookup"><span data-stu-id="9992d-111">The field is assigned to the `Model` attribute of the `<EditForm>` element.</span></span>
* <span data-ttu-id="9992d-112">`DataAnnotationsValidator` 元件會使用資料批註附加驗證支援。</span><span class="sxs-lookup"><span data-stu-id="9992d-112">The `DataAnnotationsValidator` component attaches validation support using data annotations.</span></span>
* <span data-ttu-id="9992d-113">`ValidationSummary` 元件會摘要說明驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="9992d-113">The `ValidationSummary` component summarizes validation messages.</span></span>
* <span data-ttu-id="9992d-114">當表單成功提交（通過驗證）時，就會觸發 `HandleValidSubmit`。</span><span class="sxs-lookup"><span data-stu-id="9992d-114">`HandleValidSubmit` is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="9992d-115">有一組內建的輸入元件可用來接收和驗證使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="9992d-115">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="9992d-116">當輸入變更時和送出表單時，會進行驗證。</span><span class="sxs-lookup"><span data-stu-id="9992d-116">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="9992d-117">下表顯示可用的輸入元件。</span><span class="sxs-lookup"><span data-stu-id="9992d-117">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="9992d-118">輸入元件</span><span class="sxs-lookup"><span data-stu-id="9992d-118">Input component</span></span> | <span data-ttu-id="9992d-119">呈現為&hellip;</span><span class="sxs-lookup"><span data-stu-id="9992d-119">Rendered as&hellip;</span></span>       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

<span data-ttu-id="9992d-120">所有的輸入元件（包括 `EditForm`）都支援任意屬性。</span><span class="sxs-lookup"><span data-stu-id="9992d-120">All of the input components, including `EditForm`, support arbitrary attributes.</span></span> <span data-ttu-id="9992d-121">任何不符合元件參數的屬性都會加入至轉譯的 HTML 專案。</span><span class="sxs-lookup"><span data-stu-id="9992d-121">Any attribute that doesn't match a component parameter is added to the rendered HTML element.</span></span>

<span data-ttu-id="9992d-122">輸入元件會提供預設行為，以便在編輯和變更其 CSS 類別時進行驗證，以反映欄位狀態。</span><span class="sxs-lookup"><span data-stu-id="9992d-122">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="9992d-123">某些元件包含有用的剖析邏輯。</span><span class="sxs-lookup"><span data-stu-id="9992d-123">Some components include useful parsing logic.</span></span> <span data-ttu-id="9992d-124">例如，`InputDate` 和 `InputNumber` 藉由將其註冊為驗證錯誤來適當地處理無法剖析的值。</span><span class="sxs-lookup"><span data-stu-id="9992d-124">For example, `InputDate` and `InputNumber` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="9992d-125">可以接受 null 值的類型也支援目標欄位的 null 屬性（例如，`int?`）。</span><span class="sxs-lookup"><span data-stu-id="9992d-125">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="9992d-126">下列 `Starship` 類型使用比先前 `ExampleModel`更大的屬性集和資料批註來定義驗證邏輯：</span><span class="sxs-lookup"><span data-stu-id="9992d-126">The following `Starship` type defines validation logic using a larger set of properties and data annotations than the earlier `ExampleModel`:</span></span>

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

<span data-ttu-id="9992d-127">在上述範例中，`Description` 是選擇性的，因為沒有任何資料批註存在。</span><span class="sxs-lookup"><span data-stu-id="9992d-127">In the preceding example, `Description` is optional because no data annotations are present.</span></span>

<span data-ttu-id="9992d-128">下列表單會使用 `Starship` 模型中所定義的驗證來驗證使用者輸入：</span><span class="sxs-lookup"><span data-stu-id="9992d-128">The following form validates user input using the validation defined in the `Starship` model:</span></span>

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

<span data-ttu-id="9992d-129">`EditForm` 會建立一個 `EditContext` 做為階層式[值](xref:blazor/components#cascading-values-and-parameters)，以追蹤編輯程式的相關中繼資料，包括已修改的欄位和目前的驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="9992d-129">The `EditForm` creates an `EditContext` as a [cascading value](xref:blazor/components#cascading-values-and-parameters) that tracks metadata about the edit process, including which fields have been modified and the current validation messages.</span></span> <span data-ttu-id="9992d-130">`EditForm` 也會提供有效和無效提交（`OnValidSubmit`、`OnInvalidSubmit`）的便利事件。</span><span class="sxs-lookup"><span data-stu-id="9992d-130">The `EditForm` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="9992d-131">或者，使用 `OnSubmit` 來觸發驗證，並使用自訂驗證程式代碼來檢查域值。</span><span class="sxs-lookup"><span data-stu-id="9992d-131">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

## <a name="inputtext-based-on-the-input-event"></a><span data-ttu-id="9992d-132">根據輸入事件的 InputText</span><span class="sxs-lookup"><span data-stu-id="9992d-132">InputText based on the input event</span></span>

<span data-ttu-id="9992d-133">使用 `InputText` 元件來建立使用 `input` 事件的自訂群組件，而不是 `change` 事件。</span><span class="sxs-lookup"><span data-stu-id="9992d-133">Use the `InputText` component to create a custom component that uses the `input` event instead of the `change` event.</span></span>

<span data-ttu-id="9992d-134">建立具有下列標記的元件，並使用元件，就像使用 `InputText` 一樣：</span><span class="sxs-lookup"><span data-stu-id="9992d-134">Create a component with the following markup, and use the component just as `InputText` is used:</span></span>

```cshtml
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a><span data-ttu-id="9992d-135">驗證支援</span><span class="sxs-lookup"><span data-stu-id="9992d-135">Validation support</span></span>

<span data-ttu-id="9992d-136">`DataAnnotationsValidator` 元件會使用資料批註，將驗證支援附加至串聯的 `EditContext`。</span><span class="sxs-lookup"><span data-stu-id="9992d-136">The `DataAnnotationsValidator` component attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="9992d-137">若要啟用使用資料批註進行驗證的支援，則需要這個明確的手勢。</span><span class="sxs-lookup"><span data-stu-id="9992d-137">Enabling support for validation using data annotations requires this explicit gesture.</span></span> <span data-ttu-id="9992d-138">若要使用不同于資料批註的驗證系統，請將 `DataAnnotationsValidator` 取代為自訂的執行。</span><span class="sxs-lookup"><span data-stu-id="9992d-138">To use a different validation system than data annotations, replace the `DataAnnotationsValidator` with a custom implementation.</span></span> <span data-ttu-id="9992d-139">ASP.NET Core 的執行可用於參考來源中的檢查： [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs)。</span><span class="sxs-lookup"><span data-stu-id="9992d-139">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span></span>

Blazor<span data-ttu-id="9992d-140"> 會執行兩種類型的驗證：</span><span class="sxs-lookup"><span data-stu-id="9992d-140"> performs two types of validation:</span></span>

* <span data-ttu-id="9992d-141">*欄位驗證*是在使用者跳到欄位外時執行。</span><span class="sxs-lookup"><span data-stu-id="9992d-141">*Field validation* is performed when the user tabs out of a field.</span></span> <span data-ttu-id="9992d-142">在欄位驗證期間，`DataAnnotationsValidator` 元件會將所有報告的驗證結果與欄位產生關聯。</span><span class="sxs-lookup"><span data-stu-id="9992d-142">During field validation, the `DataAnnotationsValidator` component associates all reported validation results with the field.</span></span>
* <span data-ttu-id="9992d-143">當使用者提交表單時，就會執行*模型驗證*。</span><span class="sxs-lookup"><span data-stu-id="9992d-143">*Model validation* is performed when the user submits the form.</span></span> <span data-ttu-id="9992d-144">在模型驗證期間，`DataAnnotationsValidator` 元件會嘗試根據驗證結果所報告的成員名稱來決定欄位。</span><span class="sxs-lookup"><span data-stu-id="9992d-144">During model validation, the `DataAnnotationsValidator` component attempts to determine the field based on the member name that the validation result reports.</span></span> <span data-ttu-id="9992d-145">未與個別成員相關聯的驗證結果會與模型相關聯，而不是欄位。</span><span class="sxs-lookup"><span data-stu-id="9992d-145">Validation results that aren't associated with an individual member are associated with the model rather than a field.</span></span>

### <a name="validation-summary-and-validation-message-components"></a><span data-ttu-id="9992d-146">驗證摘要和驗證訊息元件</span><span class="sxs-lookup"><span data-stu-id="9992d-146">Validation Summary and Validation Message components</span></span>

<span data-ttu-id="9992d-147">`ValidationSummary` 元件會匯總所有驗證訊息，類似于[驗證摘要](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)標籤協助程式：</span><span class="sxs-lookup"><span data-stu-id="9992d-147">The `ValidationSummary` component summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):</span></span>

```csthml
<ValidationSummary />
```

<span data-ttu-id="9992d-148">具有 `Model` 參數的特定模型輸出驗證訊息：</span><span class="sxs-lookup"><span data-stu-id="9992d-148">Output validation messages for a specific model with the `Model` parameter:</span></span>
  
```csthml
<ValidationSummary Model="@starship" />
```

<span data-ttu-id="9992d-149">`ValidationMessage` 元件會顯示特定欄位的驗證訊息，類似于[驗證訊息標記](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)協助程式。</span><span class="sxs-lookup"><span data-stu-id="9992d-149">The `ValidationMessage` component displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="9992d-150">使用 `For` 屬性指定驗證欄位，並以 lambda 運算式命名模型屬性：</span><span class="sxs-lookup"><span data-stu-id="9992d-150">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

<span data-ttu-id="9992d-151">`ValidationMessage` 和 `ValidationSummary` 元件支援任意屬性。</span><span class="sxs-lookup"><span data-stu-id="9992d-151">The `ValidationMessage` and `ValidationSummary` components support arbitrary attributes.</span></span> <span data-ttu-id="9992d-152">任何不符合元件參數的屬性都會加入至產生的 `<div>` 或 `<ul>` 元素。</span><span class="sxs-lookup"><span data-stu-id="9992d-152">Any attribute that doesn't match a component parameter is added to the generated `<div>` or `<ul>` element.</span></span>

### <a name="custom-validation-attributes"></a><span data-ttu-id="9992d-153">自訂驗證屬性</span><span class="sxs-lookup"><span data-stu-id="9992d-153">Custom validation attributes</span></span>

<span data-ttu-id="9992d-154">若要在使用[自訂驗證屬性](xref:mvc/models/validation#custom-attributes)時，確保驗證結果與欄位正確相關聯，請在建立 <xref:System.ComponentModel.DataAnnotations.ValidationResult>時，傳遞驗證內容的 <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName>：</span><span class="sxs-lookup"><span data-stu-id="9992d-154">To ensure that a validation result is correctly associated with a field when using a [custom validation attribute](xref:mvc/models/validation#custom-attributes), pass the validation context's <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> when creating the <xref:System.ComponentModel.DataAnnotations.ValidationResult>:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

private class MyCustomValidator : ValidationAttribute
{
    protected override ValidationResult IsValid(object value, 
        ValidationContext validationContext)
    {
        ...

        return new ValidationResult("Validation message to user.",
            new[] { validationContext.MemberName });
    }
}
```

::: moniker range=">= aspnetcore-3.1"

### <a name="opno-locblazor-data-annotations-validation-package"></a>Blazor<span data-ttu-id="9992d-155"> 資料批註驗證封裝</span><span class="sxs-lookup"><span data-stu-id="9992d-155"> data annotations validation package</span></span>

<span data-ttu-id="9992d-156">[AspNetCore.Blazor。DataAnnotations](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation)是使用 `DataAnnotationsValidator` 元件來填滿驗證體驗差距的封裝。</span><span class="sxs-lookup"><span data-stu-id="9992d-156">The [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) is a package that fills validation experience gaps using the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="9992d-157">封裝目前為*實驗*性。</span><span class="sxs-lookup"><span data-stu-id="9992d-157">The package is currently *experimental*.</span></span>

### <a name="compareproperty-attribute"></a><span data-ttu-id="9992d-158">[CompareProperty] 屬性</span><span class="sxs-lookup"><span data-stu-id="9992d-158">[CompareProperty] attribute</span></span>

<span data-ttu-id="9992d-159"><xref:System.ComponentModel.DataAnnotations.CompareAttribute> 無法與 `DataAnnotationsValidator` 元件搭配運作。</span><span class="sxs-lookup"><span data-stu-id="9992d-159">The <xref:System.ComponentModel.DataAnnotations.CompareAttribute> doesn't work well with the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="9992d-160">[AspNetCore.Blazor。DataAnnotations](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) *實驗*性封裝引進了額外的驗證屬性 `ComparePropertyAttribute`，這會因應這些限制。</span><span class="sxs-lookup"><span data-stu-id="9992d-160">The [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) *experimental* package introduces an additional validation attribute, `ComparePropertyAttribute`, that works around these limitations.</span></span> <span data-ttu-id="9992d-161">在 Blazor 應用程式中，`[CompareProperty]` 是 `[Compare]` 屬性的直接取代。</span><span class="sxs-lookup"><span data-stu-id="9992d-161">In a Blazor app, `[CompareProperty]` is a direct replacement for the `[Compare]` attribute.</span></span> <span data-ttu-id="9992d-162">如需詳細資訊，請參閱[CompareAttribute 已忽略 With OnValidSubmit EditForm （aspnet/AspNetCore #10643）](https://github.com/aspnet/AspNetCore/issues/10643#issuecomment-543909748)。</span><span class="sxs-lookup"><span data-stu-id="9992d-162">For more information, see [CompareAttribute ignored with OnValidSubmit EditForm (aspnet/AspNetCore #10643)](https://github.com/aspnet/AspNetCore/issues/10643#issuecomment-543909748).</span></span>

### <a name="nested-models-collection-types-and-complex-types"></a><span data-ttu-id="9992d-163">嵌套模型、集合類型和複雜類型</span><span class="sxs-lookup"><span data-stu-id="9992d-163">Nested models, collection types, and complex types</span></span>

Blazor<span data-ttu-id="9992d-164"> 支援使用內建 `DataAnnotationsValidator`的資料批註來驗證表單輸入。</span><span class="sxs-lookup"><span data-stu-id="9992d-164"> provides support for validating form input using data annotations with the built-in `DataAnnotationsValidator`.</span></span> <span data-ttu-id="9992d-165">不過，`DataAnnotationsValidator` 只會驗證系結至不是集合或複雜型別屬性之表單之模型的最上層屬性。</span><span class="sxs-lookup"><span data-stu-id="9992d-165">However, the `DataAnnotationsValidator` only validates top-level properties of the model bound to the form that aren't collection- or complex-type properties.</span></span>

<span data-ttu-id="9992d-166">若要驗證系結模型的整個物件圖形（包括集合和複雜型別屬性），請使用*實驗*性 AspNetCore 所提供的 `ObjectGraphDataAnnotationsValidator`。 [Blazor。DataAnnotations。驗證](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation)套件：</span><span class="sxs-lookup"><span data-stu-id="9992d-166">To validate the bound model's entire object graph, including collection- and complex-type properties, use the `ObjectGraphDataAnnotationsValidator` provided by the *experimental* [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) package:</span></span>

```cshtml
<EditForm Model="@model" OnValidSubmit="@HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

<span data-ttu-id="9992d-167">使用 `[ValidateComplexType]`標注模型屬性。</span><span class="sxs-lookup"><span data-stu-id="9992d-167">Annotate model properties with `[ValidateComplexType]`.</span></span> <span data-ttu-id="9992d-168">在下列模型類別中，`ShipDescription` 類別包含額外的資料批註，以在模型系結至表單時進行驗證：</span><span class="sxs-lookup"><span data-stu-id="9992d-168">In the following model classes, the `ShipDescription` class contains additional data annotations to validate when the model is bound to the form:</span></span>

<span data-ttu-id="9992d-169">*Starship.cs*：</span><span class="sxs-lookup"><span data-stu-id="9992d-169">*Starship.cs*:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    ...

    [ValidateComplexType]
    public ShipDescription ShipDescription { get; set; }

    ...
}
```

<span data-ttu-id="9992d-170">*ShipDescription.cs*：</span><span class="sxs-lookup"><span data-stu-id="9992d-170">*ShipDescription.cs*:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class ShipDescription
{
    [Required]
    [StringLength(40, ErrorMessage = "Description too long (40 char).")]
    public string ShortDescription { get; set; }
    
    [Required]
    [StringLength(240, ErrorMessage = "Description too long (240 char).")]
    public string LongDescription { get; set; }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

### <a name="validation-of-complex-or-collection-type-properties"></a><span data-ttu-id="9992d-171">複雜或集合型別屬性的驗證</span><span class="sxs-lookup"><span data-stu-id="9992d-171">Validation of complex or collection type properties</span></span>

<span data-ttu-id="9992d-172">當提交表單時，套用至模型屬性的驗證屬性會進行驗證。</span><span class="sxs-lookup"><span data-stu-id="9992d-172">Validation attributes applied to the properties of a model validate when the form is submitted.</span></span> <span data-ttu-id="9992d-173">不過，`DataAnnotationsValidator` 元件提交表單時，不會驗證集合的屬性或模型的複雜資料類型。</span><span class="sxs-lookup"><span data-stu-id="9992d-173">However, the properties of collections or complex data types of a model aren't validated on form submission by the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="9992d-174">若要在這種情況下接受嵌套驗證屬性，請使用自訂驗證元件。</span><span class="sxs-lookup"><span data-stu-id="9992d-174">To honor the nested validation attributes in this scenario, use a custom validation component.</span></span> <span data-ttu-id="9992d-175">如需範例，請參閱[Blazor 驗證範例（aspnet/sample）](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation)。</span><span class="sxs-lookup"><span data-stu-id="9992d-175">For an example, see the [Blazor Validation sample (aspnet/samples)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span></span>

::: moniker-end
