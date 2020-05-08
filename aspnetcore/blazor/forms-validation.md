---
title: ASP.NET Core Blazor表單和驗證
author: guardrex
description: 瞭解如何在中Blazor使用表單和欄位驗證案例。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/forms-validation
ms.openlocfilehash: ec2bc2867acdd1c9be42f77cb38be36abb8c8108
ms.sourcegitcommit: 84b46594f57608f6ac4f0570172c7051df507520
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82967476"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a><span data-ttu-id="acd84-103">ASP.NET Core Blazor 表單和驗證</span><span class="sxs-lookup"><span data-stu-id="acd84-103">ASP.NET Core Blazor forms and validation</span></span>

<span data-ttu-id="acd84-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="acd84-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="acd84-105">使用[資料批註](xref:mvc/models/validation)的 Blazor 支援表單和驗證。</span><span class="sxs-lookup"><span data-stu-id="acd84-105">Forms and validation are supported in Blazor using [data annotations](xref:mvc/models/validation).</span></span>

<span data-ttu-id="acd84-106">下列`ExampleModel`型別會使用資料批註來定義驗證邏輯：</span><span class="sxs-lookup"><span data-stu-id="acd84-106">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="acd84-107">表單是使用`EditForm`元件定義的。</span><span class="sxs-lookup"><span data-stu-id="acd84-107">A form is defined using the `EditForm` component.</span></span> <span data-ttu-id="acd84-108">下列表單會示範典型的元素、元件和 Razor 程式碼：</span><span class="sxs-lookup"><span data-stu-id="acd84-108">The following form demonstrates typical elements, components, and Razor code:</span></span>

```razor
<EditForm Model="@exampleModel" OnValidSubmit="HandleValidSubmit">
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

<span data-ttu-id="acd84-109">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="acd84-109">In the preceding example:</span></span>

* <span data-ttu-id="acd84-110">表單會使用在`name` `ExampleModel`型別中定義的驗證來驗證欄位中的使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="acd84-110">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="acd84-111">模型會建立在元件的`@code`區塊中，並保留在私用欄位（`exampleModel`）中。</span><span class="sxs-lookup"><span data-stu-id="acd84-111">The model is created in the component's `@code` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="acd84-112">欄位會指派給`Model` `<EditForm>`元素的屬性。</span><span class="sxs-lookup"><span data-stu-id="acd84-112">The field is assigned to the `Model` attribute of the `<EditForm>` element.</span></span>
* <span data-ttu-id="acd84-113">`InputText`元件的`@bind-Value`系結：</span><span class="sxs-lookup"><span data-stu-id="acd84-113">The `InputText` component's `@bind-Value` binds:</span></span>
  * <span data-ttu-id="acd84-114">模型屬性（`exampleModel.Name`）至`InputText`元件的`Value`屬性。</span><span class="sxs-lookup"><span data-stu-id="acd84-114">The model property (`exampleModel.Name`) to the `InputText` component's `Value` property.</span></span>
  * <span data-ttu-id="acd84-115">`InputText`元件的`ValueChanged`屬性的變更事件委派。</span><span class="sxs-lookup"><span data-stu-id="acd84-115">A change event delegate to the `InputText` component's `ValueChanged` property.</span></span>
* <span data-ttu-id="acd84-116">`DataAnnotationsValidator`元件會使用資料批註附加驗證支援。</span><span class="sxs-lookup"><span data-stu-id="acd84-116">The `DataAnnotationsValidator` component attaches validation support using data annotations.</span></span>
* <span data-ttu-id="acd84-117">`ValidationSummary`元件會摘要驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="acd84-117">The `ValidationSummary` component summarizes validation messages.</span></span>
* <span data-ttu-id="acd84-118">`HandleValidSubmit`當表單成功提交（通過驗證）時，就會觸發。</span><span class="sxs-lookup"><span data-stu-id="acd84-118">`HandleValidSubmit` is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="acd84-119">有一組內建的輸入元件可用來接收和驗證使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="acd84-119">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="acd84-120">當輸入變更時和送出表單時，會進行驗證。</span><span class="sxs-lookup"><span data-stu-id="acd84-120">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="acd84-121">下表顯示可用的輸入元件。</span><span class="sxs-lookup"><span data-stu-id="acd84-121">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="acd84-122">輸入元件</span><span class="sxs-lookup"><span data-stu-id="acd84-122">Input component</span></span> | <span data-ttu-id="acd84-123">轉譯為&hellip;</span><span class="sxs-lookup"><span data-stu-id="acd84-123">Rendered as&hellip;</span></span>       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

<span data-ttu-id="acd84-124">所有的輸入元件（包括`EditForm`）都支援任意屬性。</span><span class="sxs-lookup"><span data-stu-id="acd84-124">All of the input components, including `EditForm`, support arbitrary attributes.</span></span> <span data-ttu-id="acd84-125">任何不符合元件參數的屬性都會加入至轉譯的 HTML 專案。</span><span class="sxs-lookup"><span data-stu-id="acd84-125">Any attribute that doesn't match a component parameter is added to the rendered HTML element.</span></span>

<span data-ttu-id="acd84-126">輸入元件會提供預設行為，以便在編輯和變更其 CSS 類別時進行驗證，以反映欄位狀態。</span><span class="sxs-lookup"><span data-stu-id="acd84-126">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="acd84-127">某些元件包含有用的剖析邏輯。</span><span class="sxs-lookup"><span data-stu-id="acd84-127">Some components include useful parsing logic.</span></span> <span data-ttu-id="acd84-128">例如， `InputDate`和`InputNumber`會藉由將無法剖析的值註冊為驗證錯誤來妥善處理。</span><span class="sxs-lookup"><span data-stu-id="acd84-128">For example, `InputDate` and `InputNumber` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="acd84-129">可以接受 null 值的類型也支援目標欄位的 null 屬性（例如`int?`）。</span><span class="sxs-lookup"><span data-stu-id="acd84-129">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="acd84-130">下列`Starship`型別使用較早`ExampleModel`的屬性集和資料注釋，來定義驗證邏輯：</span><span class="sxs-lookup"><span data-stu-id="acd84-130">The following `Starship` type defines validation logic using a larger set of properties and data annotations than the earlier `ExampleModel`:</span></span>

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

<span data-ttu-id="acd84-131">在上述範例中， `Description`是選擇性的，因為沒有任何資料批註存在。</span><span class="sxs-lookup"><span data-stu-id="acd84-131">In the preceding example, `Description` is optional because no data annotations are present.</span></span>

<span data-ttu-id="acd84-132">下列表單會使用`Starship`模型中定義的驗證來驗證使用者輸入：</span><span class="sxs-lookup"><span data-stu-id="acd84-132">The following form validates user input using the validation defined in the `Starship` model:</span></span>

```razor
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@starship" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label>
            Identifier:
            <InputText @bind-Value="starship.Identifier" />
        </label>
    </p>
    <p>
        <label>
            Description (optional):
            <InputTextArea @bind-Value="starship.Description" />
        </label>
    </p>
    <p>
        <label>
            Primary Classification:
            <InputSelect @bind-Value="starship.Classification">
                <option value="">Select classification ...</option>
                <option value="Exploration">Exploration</option>
                <option value="Diplomacy">Diplomacy</option>
                <option value="Defense">Defense</option>
            </InputSelect>
        </label>
    </p>
    <p>
        <label>
            Maximum Accommodation:
            <InputNumber @bind-Value="starship.MaximumAccommodation" />
        </label>
    </p>
    <p>
        <label>
            Engineering Approval:
            <InputCheckbox @bind-Value="starship.IsValidatedDesign" />
        </label>
    </p>
    <p>
        <label>
            Production Date:
            <InputDate @bind-Value="starship.ProductionDate" />
        </label>
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

<span data-ttu-id="acd84-133">會`EditForm`建立做`EditContext`為階層式[值](xref:blazor/components#cascading-values-and-parameters)，以追蹤編輯程式的相關中繼資料，包括已修改的欄位和目前的驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="acd84-133">The `EditForm` creates an `EditContext` as a [cascading value](xref:blazor/components#cascading-values-and-parameters) that tracks metadata about the edit process, including which fields have been modified and the current validation messages.</span></span> <span data-ttu-id="acd84-134">`EditForm`也提供有效和無效提交的便利事件（`OnValidSubmit`、 `OnInvalidSubmit`）。</span><span class="sxs-lookup"><span data-stu-id="acd84-134">The `EditForm` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="acd84-135">或者，使用`OnSubmit`來觸發驗證，並使用自訂驗證程式代碼來檢查域值。</span><span class="sxs-lookup"><span data-stu-id="acd84-135">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

<span data-ttu-id="acd84-136">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="acd84-136">In the following example:</span></span>

* <span data-ttu-id="acd84-137">當`HandleSubmit`選取 [**提交**] 按鈕時，就會執行方法。</span><span class="sxs-lookup"><span data-stu-id="acd84-137">The `HandleSubmit` method runs when the **Submit** button is selected.</span></span>
* <span data-ttu-id="acd84-138">表單會使用表單的`EditContext`進行驗證。</span><span class="sxs-lookup"><span data-stu-id="acd84-138">The form is validated using the form's `EditContext`.</span></span>
* <span data-ttu-id="acd84-139">藉由將傳遞`EditContext`至在伺服器上呼叫 Web API `ServerValidate`端點的方法（*未顯示*），進一步驗證表單。</span><span class="sxs-lookup"><span data-stu-id="acd84-139">The form is further validated by passing the `EditContext` to the `ServerValidate` method that calls a web API endpoint on the server (*not shown*).</span></span>
* <span data-ttu-id="acd84-140">額外的程式碼會根據用戶端和伺服器端驗證的結果來執行（藉由`isValid`檢查）。</span><span class="sxs-lookup"><span data-stu-id="acd84-140">Additional code is run depending on the result of the client- and server-side validation by checking `isValid`.</span></span>

```razor
<EditForm EditContext="@editContext" OnSubmit="@HandleSubmit">

    ...

    <button type="submit">Submit</button>
</EditForm>

@code {
    private Starship starship = new Starship();
    private EditContext editContext;

    protected override void OnInitialized()
    {
        editContext = new EditContext(starship);
    }

    private async Task HandleSubmit()
    {
        var isValid = editContext.Validate() && 
            await ServerValidate(editContext);

        if (isValid)
        {
            ...
        }
        else
        {
            ...
        }
    }

    private async Task<bool> ServerValidate(EditContext editContext)
    {
        var serverChecksValid = ...

        return serverChecksValid;
    }
}
```

## <a name="inputtext-based-on-the-input-event"></a><span data-ttu-id="acd84-141">根據輸入事件的 InputText</span><span class="sxs-lookup"><span data-stu-id="acd84-141">InputText based on the input event</span></span>

<span data-ttu-id="acd84-142">使用`InputText`元件來建立使用`input`事件的自訂群組件，而不是`change`事件。</span><span class="sxs-lookup"><span data-stu-id="acd84-142">Use the `InputText` component to create a custom component that uses the `input` event instead of the `change` event.</span></span>

<span data-ttu-id="acd84-143">建立具有下列標記的元件，並使用如同使用的元件`InputText` ：</span><span class="sxs-lookup"><span data-stu-id="acd84-143">Create a component with the following markup, and use the component just as `InputText` is used:</span></span>

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="work-with-radio-buttons"></a><span data-ttu-id="acd84-144">使用選項按鈕</span><span class="sxs-lookup"><span data-stu-id="acd84-144">Work with radio buttons</span></span>

<span data-ttu-id="acd84-145">使用表單中的選項按鈕時，資料系結的處理方式不同于其他元素，因為選項按鈕會評估為群組。</span><span class="sxs-lookup"><span data-stu-id="acd84-145">When working with radio buttons in a form, data binding is handled differently than other elements because radio buttons are evaluated as a group.</span></span> <span data-ttu-id="acd84-146">每個選項按鈕的值都是固定的，但是選項按鈕群組的值就是選取之選項按鈕的值。</span><span class="sxs-lookup"><span data-stu-id="acd84-146">The value of each radio button is fixed, but the value of the radio button group is the value of the selected radio button.</span></span> <span data-ttu-id="acd84-147">下列範例示範如何執行：</span><span class="sxs-lookup"><span data-stu-id="acd84-147">The following example shows how to:</span></span>

* <span data-ttu-id="acd84-148">處理選項按鈕群組的資料系結。</span><span class="sxs-lookup"><span data-stu-id="acd84-148">Handle data binding for a radio button group.</span></span>
* <span data-ttu-id="acd84-149">支援使用自訂`InputRadio`元件的驗證。</span><span class="sxs-lookup"><span data-stu-id="acd84-149">Support validation using a custom `InputRadio` component.</span></span>

```razor
@using System.Globalization
@typeparam TValue
@inherits InputBase<TValue>

<input @attributes="AdditionalAttributes" type="radio" value="@SelectedValue" 
       checked="@(SelectedValue.Equals(Value))" @onchange="OnChange" />

@code {
    [Parameter]
    public TValue SelectedValue { get; set; }

    private void OnChange(ChangeEventArgs args)
    {
        CurrentValueAsString = args.Value.ToString();
    }

    protected override bool TryParseValueFromString(string value, 
        out TValue result, out string errorMessage)
    {
        var success = BindConverter.TryConvertTo<TValue>(
            value, CultureInfo.CurrentCulture, out var parsedValue);
        if (success)
        {
            result = parsedValue;
            errorMessage = null;

            return true;
        }
        else
        {
            result = default;
            errorMessage = $"{FieldIdentifier.FieldName} field isn't valid.";

            return false;
        }
    }
}
```

<span data-ttu-id="acd84-150">下列`EditForm`使用上述`InputRadio`元件來取得並驗證使用者的評等：</span><span class="sxs-lookup"><span data-stu-id="acd84-150">The following `EditForm` uses the preceding `InputRadio` component to obtain and validate a rating from the user:</span></span>

```razor
@page "/RadioButtonExample"
@using System.ComponentModel.DataAnnotations

<h1>Radio Button Group Test</h1>

<EditForm Model="model" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    @for (int i = 1; i <= 5; i++)
    {
        <label>
            <InputRadio name="rate" SelectedValue="i" @bind-Value="model.Rating" />
            @i
        </label>
    }

    <button type="submit">Submit</button>
</EditForm>

<p>You chose: @model.Rating</p>

@code {
    private Model model = new Model();

    private void HandleValidSubmit()
    {
        Console.WriteLine("valid");
    }

    public class Model
    {
        [Range(1, 5)]
        public int Rating { get; set; }
    }
}
```

## <a name="validation-support"></a><span data-ttu-id="acd84-151">驗證支援</span><span class="sxs-lookup"><span data-stu-id="acd84-151">Validation support</span></span>

<span data-ttu-id="acd84-152">`DataAnnotationsValidator`元件會使用資料批註，將驗證支援附加至`EditContext`串聯的。</span><span class="sxs-lookup"><span data-stu-id="acd84-152">The `DataAnnotationsValidator` component attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="acd84-153">若要啟用使用資料批註進行驗證的支援，則需要這個明確的手勢。</span><span class="sxs-lookup"><span data-stu-id="acd84-153">Enabling support for validation using data annotations requires this explicit gesture.</span></span> <span data-ttu-id="acd84-154">若要使用不同于資料批註的驗證系統，請`DataAnnotationsValidator`將取代為自訂的執行。</span><span class="sxs-lookup"><span data-stu-id="acd84-154">To use a different validation system than data annotations, replace the `DataAnnotationsValidator` with a custom implementation.</span></span> <span data-ttu-id="acd84-155">ASP.NET Core 的執行可用於參考來源中的檢查： [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs)。</span><span class="sxs-lookup"><span data-stu-id="acd84-155">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span></span>

Blazor<span data-ttu-id="acd84-156">會執行兩種類型的驗證：</span><span class="sxs-lookup"><span data-stu-id="acd84-156"> performs two types of validation:</span></span>

* <span data-ttu-id="acd84-157">*欄位驗證*是在使用者跳到欄位外時執行。</span><span class="sxs-lookup"><span data-stu-id="acd84-157">*Field validation* is performed when the user tabs out of a field.</span></span> <span data-ttu-id="acd84-158">在欄位驗證期間， `DataAnnotationsValidator`元件會將所有報告的驗證結果與欄位產生關聯。</span><span class="sxs-lookup"><span data-stu-id="acd84-158">During field validation, the `DataAnnotationsValidator` component associates all reported validation results with the field.</span></span>
* <span data-ttu-id="acd84-159">當使用者提交表單時，就會執行*模型驗證*。</span><span class="sxs-lookup"><span data-stu-id="acd84-159">*Model validation* is performed when the user submits the form.</span></span> <span data-ttu-id="acd84-160">在模型驗證期間， `DataAnnotationsValidator`元件會嘗試根據驗證結果所報告的成員名稱來決定欄位。</span><span class="sxs-lookup"><span data-stu-id="acd84-160">During model validation, the `DataAnnotationsValidator` component attempts to determine the field based on the member name that the validation result reports.</span></span> <span data-ttu-id="acd84-161">未與個別成員相關聯的驗證結果會與模型相關聯，而不是欄位。</span><span class="sxs-lookup"><span data-stu-id="acd84-161">Validation results that aren't associated with an individual member are associated with the model rather than a field.</span></span>

### <a name="validation-summary-and-validation-message-components"></a><span data-ttu-id="acd84-162">驗證摘要和驗證訊息元件</span><span class="sxs-lookup"><span data-stu-id="acd84-162">Validation Summary and Validation Message components</span></span>

<span data-ttu-id="acd84-163">此`ValidationSummary`元件會摘要所有驗證訊息，類似于[驗證摘要](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)標籤協助程式：</span><span class="sxs-lookup"><span data-stu-id="acd84-163">The `ValidationSummary` component summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):</span></span>

```razor
<ValidationSummary />
```

<span data-ttu-id="acd84-164">使用`Model`參數輸出特定模型的驗證訊息：</span><span class="sxs-lookup"><span data-stu-id="acd84-164">Output validation messages for a specific model with the `Model` parameter:</span></span>
  
```razor
<ValidationSummary Model="@starship" />
```

<span data-ttu-id="acd84-165">`ValidationMessage`元件會顯示特定欄位的驗證訊息，類似于[驗證訊息](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="acd84-165">The `ValidationMessage` component displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="acd84-166">使用`For`屬性指定驗證欄位，並以 lambda 運算式命名模型屬性：</span><span class="sxs-lookup"><span data-stu-id="acd84-166">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```razor
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

<span data-ttu-id="acd84-167">`ValidationMessage`和`ValidationSummary`元件支援任意屬性。</span><span class="sxs-lookup"><span data-stu-id="acd84-167">The `ValidationMessage` and `ValidationSummary` components support arbitrary attributes.</span></span> <span data-ttu-id="acd84-168">任何不符合元件參數的屬性都會加入至產生`<div>`的或`<ul>`元素。</span><span class="sxs-lookup"><span data-stu-id="acd84-168">Any attribute that doesn't match a component parameter is added to the generated `<div>` or `<ul>` element.</span></span>

### <a name="custom-validation-attributes"></a><span data-ttu-id="acd84-169">自訂驗證屬性</span><span class="sxs-lookup"><span data-stu-id="acd84-169">Custom validation attributes</span></span>

<span data-ttu-id="acd84-170">若要在使用[自訂驗證屬性](xref:mvc/models/validation#custom-attributes)時，確保驗證結果與欄位正確相關聯，請<xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName>在建立時傳遞驗證內容： <xref:System.ComponentModel.DataAnnotations.ValidationResult></span><span class="sxs-lookup"><span data-stu-id="acd84-170">To ensure that a validation result is correctly associated with a field when using a [custom validation attribute](xref:mvc/models/validation#custom-attributes), pass the validation context's <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> when creating the <xref:System.ComponentModel.DataAnnotations.ValidationResult>:</span></span>

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

### <a name="blazor-data-annotations-validation-package"></a>Blazor<span data-ttu-id="acd84-171">資料批註驗證封裝</span><span class="sxs-lookup"><span data-stu-id="acd84-171"> data annotations validation package</span></span>

<span data-ttu-id="acd84-172">[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation)是使用`DataAnnotationsValidator`元件來填滿驗證體驗缺口的套件。</span><span class="sxs-lookup"><span data-stu-id="acd84-172">The [Microsoft.AspNetCore.Components.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation) is a package that fills validation experience gaps using the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="acd84-173">封裝目前為*實驗*性。</span><span class="sxs-lookup"><span data-stu-id="acd84-173">The package is currently *experimental*.</span></span>

### <a name="compareproperty-attribute"></a><span data-ttu-id="acd84-174">[CompareProperty] 屬性</span><span class="sxs-lookup"><span data-stu-id="acd84-174">[CompareProperty] attribute</span></span>

<span data-ttu-id="acd84-175"><xref:System.ComponentModel.DataAnnotations.CompareAttribute>無法與`DataAnnotationsValidator`元件搭配運作，因為它不會將驗證結果與特定成員產生關聯。</span><span class="sxs-lookup"><span data-stu-id="acd84-175">The <xref:System.ComponentModel.DataAnnotations.CompareAttribute> doesn't work well with the `DataAnnotationsValidator` component because it doesn't associate the validation result with a specific member.</span></span> <span data-ttu-id="acd84-176">這可能會導致欄位層級驗證與整個模型在提交時進行驗證時的行為不一致。</span><span class="sxs-lookup"><span data-stu-id="acd84-176">This can result in inconsistent behavior between field-level validation and when the entire model is validated on a submit.</span></span> <span data-ttu-id="acd84-177">[DataAnnotations](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation) *實驗*性封裝引進了額外的驗證屬性， `ComparePropertyAttribute`其運作方式會因應這些限制。</span><span class="sxs-lookup"><span data-stu-id="acd84-177">The [Microsoft.AspNetCore.Components.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation) *experimental* package introduces an additional validation attribute, `ComparePropertyAttribute`, that works around these limitations.</span></span> <span data-ttu-id="acd84-178">在Blazor應用程式中`[CompareProperty]` ，是`[Compare]`屬性的直接取代。</span><span class="sxs-lookup"><span data-stu-id="acd84-178">In a Blazor app, `[CompareProperty]` is a direct replacement for the `[Compare]` attribute.</span></span>

### <a name="nested-models-collection-types-and-complex-types"></a><span data-ttu-id="acd84-179">嵌套模型、集合類型和複雜類型</span><span class="sxs-lookup"><span data-stu-id="acd84-179">Nested models, collection types, and complex types</span></span>

Blazor<span data-ttu-id="acd84-180">使用內`DataAnnotationsValidator`建的資料批註，提供驗證表單輸入的支援。</span><span class="sxs-lookup"><span data-stu-id="acd84-180"> provides support for validating form input using data annotations with the built-in `DataAnnotationsValidator`.</span></span> <span data-ttu-id="acd84-181">不過， `DataAnnotationsValidator`只會驗證系結至不是集合或複雜型別屬性之表單之模型的最上層屬性。</span><span class="sxs-lookup"><span data-stu-id="acd84-181">However, the `DataAnnotationsValidator` only validates top-level properties of the model bound to the form that aren't collection- or complex-type properties.</span></span>

<span data-ttu-id="acd84-182">若要驗證系結模型的整個物件圖形（包括集合型和複雜型別屬性）， `ObjectGraphDataAnnotationsValidator`請使用*實驗*性的 DataAnnotations 所提供的。[驗證](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation)套件：</span><span class="sxs-lookup"><span data-stu-id="acd84-182">To validate the bound model's entire object graph, including collection- and complex-type properties, use the `ObjectGraphDataAnnotationsValidator` provided by the *experimental* [Microsoft.AspNetCore.Components.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation) package:</span></span>

```razor
<EditForm Model="@model" OnValidSubmit="HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

<span data-ttu-id="acd84-183">使用`[ValidateComplexType]`標注模型屬性。</span><span class="sxs-lookup"><span data-stu-id="acd84-183">Annotate model properties with `[ValidateComplexType]`.</span></span> <span data-ttu-id="acd84-184">在下列模型類別中， `ShipDescription`類別會包含其他資料批註，以在模型系結至表單時進行驗證：</span><span class="sxs-lookup"><span data-stu-id="acd84-184">In the following model classes, the `ShipDescription` class contains additional data annotations to validate when the model is bound to the form:</span></span>

<span data-ttu-id="acd84-185">*Starship.cs*：</span><span class="sxs-lookup"><span data-stu-id="acd84-185">*Starship.cs*:</span></span>

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

<span data-ttu-id="acd84-186">*ShipDescription.cs*：</span><span class="sxs-lookup"><span data-stu-id="acd84-186">*ShipDescription.cs*:</span></span>

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

### <a name="enable-the-submit-button-based-on-form-validation"></a><span data-ttu-id="acd84-187">根據表單驗證啟用 [提交] 按鈕</span><span class="sxs-lookup"><span data-stu-id="acd84-187">Enable the submit button based on form validation</span></span>

<span data-ttu-id="acd84-188">若要根據表單驗證啟用和停用 [提交] 按鈕：</span><span class="sxs-lookup"><span data-stu-id="acd84-188">To enable and disable the submit button based on form validation:</span></span>

* <span data-ttu-id="acd84-189">當元件初始化時`EditContext` ，請使用表單的來指派模型。</span><span class="sxs-lookup"><span data-stu-id="acd84-189">Use the form's `EditContext` to assign the model when the component is initialized.</span></span>
* <span data-ttu-id="acd84-190">在內容的`OnFieldChanged`回呼中驗證表單，以啟用和停用 [提交] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="acd84-190">Validate the form in the context's `OnFieldChanged` callback to enable and disable the submit button.</span></span>
* <span data-ttu-id="acd84-191">解除掛接`Dispose`方法中的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="acd84-191">Unhook the event handler in the `Dispose` method.</span></span> <span data-ttu-id="acd84-192">如需詳細資訊，請參閱<xref:blazor/lifecycle#component-disposal-with-idisposable>。</span><span class="sxs-lookup"><span data-stu-id="acd84-192">For more information, see <xref:blazor/lifecycle#component-disposal-with-idisposable>.</span></span>

```razor
@implements IDisposable

<EditForm EditContext="@editContext">
    <DataAnnotationsValidator />
    <ValidationSummary />

    ...

    <button type="submit" disabled="@formInvalid">Submit</button>
</EditForm>

@code {
    private Starship starship = new Starship();
    private bool formInvalid = true;
    private EditContext editContext;

    protected override void OnInitialized()
    {
        editContext = new EditContext(starship);
        editContext.OnFieldChanged += HandleFieldChanged;
    }

    private void HandleFieldChanged(object sender, FieldChangedEventArgs e)
    {
        formInvalid = !editContext.Validate();
        StateHasChanged();
    }

    public void Dispose()
    {
        editContext.OnFieldChanged -= HandleFieldChanged;
    }
}
```

<span data-ttu-id="acd84-193">在上述範例中，將`formInvalid`設定`false`為（如果：</span><span class="sxs-lookup"><span data-stu-id="acd84-193">In the preceding example, set `formInvalid` to `false` if:</span></span>

* <span data-ttu-id="acd84-194">表單會預先載入有效的預設值。</span><span class="sxs-lookup"><span data-stu-id="acd84-194">The form is preloaded with valid default values.</span></span>
* <span data-ttu-id="acd84-195">當表單載入時，您會想要啟用 [提交] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="acd84-195">You want the submit button enabled when the form loads.</span></span>

<span data-ttu-id="acd84-196">上述方法的副作用在於，在使用者與任何`ValidationSummary`一個欄位互動之後，元件會填入不正確欄位。</span><span class="sxs-lookup"><span data-stu-id="acd84-196">A side effect of the preceding approach is that a `ValidationSummary` component is populated with invalid fields after the user interacts with any one field.</span></span> <span data-ttu-id="acd84-197">此案例可透過下列其中一種方式來解決：</span><span class="sxs-lookup"><span data-stu-id="acd84-197">This scenario can be addressed in either of the following ways:</span></span>

* <span data-ttu-id="acd84-198">請勿使用窗`ValidationSummary`體上的元件。</span><span class="sxs-lookup"><span data-stu-id="acd84-198">Don't use a `ValidationSummary` component on the form.</span></span>
* <span data-ttu-id="acd84-199">選取 [ `ValidationSummary`提交] 按鈕時，讓元件顯示（例如，在`HandleValidSubmit`方法中）。</span><span class="sxs-lookup"><span data-stu-id="acd84-199">Make the `ValidationSummary` component visible when the submit button is selected (for example, in a `HandleValidSubmit` method).</span></span>

```razor
<EditForm EditContext="@editContext" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary style="@displaySummary" />

    ...

    <button type="submit" disabled="@formInvalid">Submit</button>
</EditForm>

@code {
    private string displaySummary = "display:none";

    ...

    private void HandleValidSubmit()
    {
        displaySummary = "display:block";
    }
}
```
