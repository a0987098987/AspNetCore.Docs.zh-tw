---
title: ASP.NET Core Blazor 表單和驗證
author: guardrex
description: 瞭解如何在 Blazor中使用表單和欄位驗證案例。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/forms-validation
ms.openlocfilehash: 2758bcbbc76c8a59716fe224dd2deb4ca8c06929
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726890"
---
# <a name="aspnet-core-opno-locblazor-forms-and-validation"></a><span data-ttu-id="7773e-103">ASP.NET Core Blazor 表單和驗證</span><span class="sxs-lookup"><span data-stu-id="7773e-103">ASP.NET Core Blazor forms and validation</span></span>

<span data-ttu-id="7773e-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7773e-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7773e-105">使用[資料批註](xref:mvc/models/validation)的 Blazor 中支援表單和驗證。</span><span class="sxs-lookup"><span data-stu-id="7773e-105">Forms and validation are supported in Blazor using [data annotations](xref:mvc/models/validation).</span></span>

<span data-ttu-id="7773e-106">下列 `ExampleModel` 類型會使用資料批註來定義驗證邏輯：</span><span class="sxs-lookup"><span data-stu-id="7773e-106">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="7773e-107">表單是使用 `EditForm` 元件所定義。</span><span class="sxs-lookup"><span data-stu-id="7773e-107">A form is defined using the `EditForm` component.</span></span> <span data-ttu-id="7773e-108">下列表單會示範典型的元素、元件和 Razor 程式碼：</span><span class="sxs-lookup"><span data-stu-id="7773e-108">The following form demonstrates typical elements, components, and Razor code:</span></span>

```razor
<EditForm Model="@_exampleModel" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" @bind-Value="_exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

@code {
    private ExampleModel _exampleModel = new ExampleModel();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

<span data-ttu-id="7773e-109">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="7773e-109">In the preceding example:</span></span>

* <span data-ttu-id="7773e-110">表單會使用 `ExampleModel` 型別中定義的驗證，來驗證 `name` 欄位中的使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="7773e-110">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="7773e-111">模型會在元件的 `@code` 區塊中建立，並保存在私用欄位（`_exampleModel`）中。</span><span class="sxs-lookup"><span data-stu-id="7773e-111">The model is created in the component's `@code` block and held in a private field (`_exampleModel`).</span></span> <span data-ttu-id="7773e-112">欄位會指派給 `<EditForm>` 元素的 `Model` 屬性。</span><span class="sxs-lookup"><span data-stu-id="7773e-112">The field is assigned to the `Model` attribute of the `<EditForm>` element.</span></span>
* <span data-ttu-id="7773e-113">`InputText` 元件的 `@bind-Value` 系結：</span><span class="sxs-lookup"><span data-stu-id="7773e-113">The `InputText` component's `@bind-Value` binds:</span></span>
  * <span data-ttu-id="7773e-114">`InputText` 元件之 `Value` 屬性的模型屬性（`_exampleModel.Name`）。</span><span class="sxs-lookup"><span data-stu-id="7773e-114">The model property (`_exampleModel.Name`) to the `InputText` component's `Value` property.</span></span>
  * <span data-ttu-id="7773e-115">`InputText` 元件的 `ValueChanged` 屬性的變更事件委派。</span><span class="sxs-lookup"><span data-stu-id="7773e-115">A change event delegate to the `InputText` component's `ValueChanged` property.</span></span>
* <span data-ttu-id="7773e-116">`DataAnnotationsValidator` 元件會使用資料批註附加驗證支援。</span><span class="sxs-lookup"><span data-stu-id="7773e-116">The `DataAnnotationsValidator` component attaches validation support using data annotations.</span></span>
* <span data-ttu-id="7773e-117">`ValidationSummary` 元件會摘要說明驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="7773e-117">The `ValidationSummary` component summarizes validation messages.</span></span>
* <span data-ttu-id="7773e-118">當表單成功提交（通過驗證）時，就會觸發 `HandleValidSubmit`。</span><span class="sxs-lookup"><span data-stu-id="7773e-118">`HandleValidSubmit` is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="7773e-119">有一組內建的輸入元件可用來接收和驗證使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="7773e-119">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="7773e-120">當輸入變更時和送出表單時，會進行驗證。</span><span class="sxs-lookup"><span data-stu-id="7773e-120">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="7773e-121">下表顯示可用的輸入元件。</span><span class="sxs-lookup"><span data-stu-id="7773e-121">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="7773e-122">輸入元件</span><span class="sxs-lookup"><span data-stu-id="7773e-122">Input component</span></span> | <span data-ttu-id="7773e-123">呈現為&hellip;</span><span class="sxs-lookup"><span data-stu-id="7773e-123">Rendered as&hellip;</span></span>       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

<span data-ttu-id="7773e-124">所有的輸入元件（包括 `EditForm`）都支援任意屬性。</span><span class="sxs-lookup"><span data-stu-id="7773e-124">All of the input components, including `EditForm`, support arbitrary attributes.</span></span> <span data-ttu-id="7773e-125">任何不符合元件參數的屬性都會加入至轉譯的 HTML 專案。</span><span class="sxs-lookup"><span data-stu-id="7773e-125">Any attribute that doesn't match a component parameter is added to the rendered HTML element.</span></span>

<span data-ttu-id="7773e-126">輸入元件會提供預設行為，以便在編輯和變更其 CSS 類別時進行驗證，以反映欄位狀態。</span><span class="sxs-lookup"><span data-stu-id="7773e-126">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="7773e-127">某些元件包含有用的剖析邏輯。</span><span class="sxs-lookup"><span data-stu-id="7773e-127">Some components include useful parsing logic.</span></span> <span data-ttu-id="7773e-128">例如，`InputDate` 和 `InputNumber` 藉由將其註冊為驗證錯誤來適當地處理無法剖析的值。</span><span class="sxs-lookup"><span data-stu-id="7773e-128">For example, `InputDate` and `InputNumber` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="7773e-129">可以接受 null 值的類型也支援目標欄位的 null 屬性（例如，`int?`）。</span><span class="sxs-lookup"><span data-stu-id="7773e-129">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="7773e-130">下列 `Starship` 類型使用比先前 `ExampleModel`更大的屬性集和資料批註來定義驗證邏輯：</span><span class="sxs-lookup"><span data-stu-id="7773e-130">The following `Starship` type defines validation logic using a larger set of properties and data annotations than the earlier `ExampleModel`:</span></span>

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

<span data-ttu-id="7773e-131">在上述範例中，`Description` 是選擇性的，因為沒有任何資料批註存在。</span><span class="sxs-lookup"><span data-stu-id="7773e-131">In the preceding example, `Description` is optional because no data annotations are present.</span></span>

<span data-ttu-id="7773e-132">下列表單會使用 `Starship` 模型中所定義的驗證來驗證使用者輸入：</span><span class="sxs-lookup"><span data-stu-id="7773e-132">The following form validates user input using the validation defined in the `Starship` model:</span></span>

```razor
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@_starship" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label>
            Identifier:
            <InputText @bind-Value="_starship.Identifier" />
        </label>
    </p>
    <p>
        <label>
            Description (optional):
            <InputTextArea @bind-Value="_starship.Description" />
        </label>
    </p>
    <p>
        <label>
            Primary Classification:
            <InputSelect @bind-Value="_starship.Classification">
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
            <InputNumber @bind-Value="_starship.MaximumAccommodation" />
        </label>
    </p>
    <p>
        <label>
            Engineering Approval:
            <InputCheckbox @bind-Value="_starship.IsValidatedDesign" />
        </label>
    </p>
    <p>
        <label>
            Production Date:
            <InputDate @bind-Value="_starship.ProductionDate" />
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
    private Starship _starship = new Starship();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

<span data-ttu-id="7773e-133">會建立做`EditContext`為[階層式值](xref:blazor/components#cascading-values-and-parameters), 以追蹤編輯程式的相關中繼資料, 包括已修改的欄位和目前的驗證訊息。`EditForm`</span><span class="sxs-lookup"><span data-stu-id="7773e-133">The `EditForm` creates an `EditContext` as a [cascading value](xref:blazor/components#cascading-values-and-parameters) that tracks metadata about the edit process, including which fields have been modified and the current validation messages.</span></span> <span data-ttu-id="7773e-134">`EditForm` 也會提供有效和無效提交（`OnValidSubmit`、`OnInvalidSubmit`）的便利事件。</span><span class="sxs-lookup"><span data-stu-id="7773e-134">The `EditForm` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="7773e-135">或者，使用 `OnSubmit` 來觸發驗證，並使用自訂驗證程式代碼來檢查域值。</span><span class="sxs-lookup"><span data-stu-id="7773e-135">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

<span data-ttu-id="7773e-136">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="7773e-136">In the following example:</span></span>

* <span data-ttu-id="7773e-137">選取 [**提交**] 按鈕時，就會執行 `HandleSubmit` 方法。</span><span class="sxs-lookup"><span data-stu-id="7773e-137">The `HandleSubmit` method runs when the **Submit** button is selected.</span></span>
* <span data-ttu-id="7773e-138">表單會使用表單的 `EditContext`進行驗證。</span><span class="sxs-lookup"><span data-stu-id="7773e-138">The form is validated using the form's `EditContext`.</span></span>
* <span data-ttu-id="7773e-139">藉由將 `EditContext` 傳遞至呼叫伺服器上 Web API 端點的 `ServerValidate` 方法（*未顯示*），進一步驗證表單。</span><span class="sxs-lookup"><span data-stu-id="7773e-139">The form is further validated by passing the `EditContext` to the `ServerValidate` method that calls a web API endpoint on the server (*not shown*).</span></span>
* <span data-ttu-id="7773e-140">額外的程式碼會根據用戶端和伺服器端驗證的結果來執行，方法是檢查 `isValid`。</span><span class="sxs-lookup"><span data-stu-id="7773e-140">Additional code is run depending on the result of the client- and server-side validation by checking `isValid`.</span></span>

```razor
<EditForm EditContext="@_editContext" OnSubmit="@HandleSubmit">

    ...

    <button type="submit">Submit</button>
</EditForm>

@code {
    private Starship _starship = new Starship();
    private EditContext _editContext;

    protected override void OnInitialized()
    {
        _editContext = new EditContext(_starship);
    }

    private async Task HandleSubmit()
    {
        var isValid = _editContext.Validate() && 
            await ServerValidate(_editContext);

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

## <a name="inputtext-based-on-the-input-event"></a><span data-ttu-id="7773e-141">根據輸入事件的 InputText</span><span class="sxs-lookup"><span data-stu-id="7773e-141">InputText based on the input event</span></span>

<span data-ttu-id="7773e-142">使用 `InputText` 元件來建立使用 `input` 事件的自訂群組件，而不是 `change` 事件。</span><span class="sxs-lookup"><span data-stu-id="7773e-142">Use the `InputText` component to create a custom component that uses the `input` event instead of the `change` event.</span></span>

<span data-ttu-id="7773e-143">建立具有下列標記的元件，並使用元件，就像使用 `InputText` 一樣：</span><span class="sxs-lookup"><span data-stu-id="7773e-143">Create a component with the following markup, and use the component just as `InputText` is used:</span></span>

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="work-with-radio-buttons"></a><span data-ttu-id="7773e-144">使用選項按鈕</span><span class="sxs-lookup"><span data-stu-id="7773e-144">Work with radio buttons</span></span>

<span data-ttu-id="7773e-145">使用表單中的選項按鈕時，資料系結的處理方式不同于其他元素，因為選項按鈕會評估為群組。</span><span class="sxs-lookup"><span data-stu-id="7773e-145">When working with radio buttons in a form, data binding is handled differently than other elements because radio buttons are evaluated as a group.</span></span> <span data-ttu-id="7773e-146">每個選項按鈕的值都是固定的，但是選項按鈕群組的值就是選取之選項按鈕的值。</span><span class="sxs-lookup"><span data-stu-id="7773e-146">The value of each radio button is fixed, but the value of the radio button group is the value of the selected radio button.</span></span> <span data-ttu-id="7773e-147">下列範例顯示如何：</span><span class="sxs-lookup"><span data-stu-id="7773e-147">The following example shows how to:</span></span>

* <span data-ttu-id="7773e-148">處理選項按鈕群組的資料系結。</span><span class="sxs-lookup"><span data-stu-id="7773e-148">Handle data binding for a radio button group.</span></span>
* <span data-ttu-id="7773e-149">支援使用自訂 `InputRadio` 元件進行驗證。</span><span class="sxs-lookup"><span data-stu-id="7773e-149">Support validation using a custom `InputRadio` component.</span></span>

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

<span data-ttu-id="7773e-150">下列 `EditForm` 會使用上述的 `InputRadio` 元件來取得並驗證使用者的評等：</span><span class="sxs-lookup"><span data-stu-id="7773e-150">The following `EditForm` uses the preceding `InputRadio` component to obtain and validate a rating from the user:</span></span>

```razor
@page "/RadioButtonExample"
@using System.ComponentModel.DataAnnotations

<h1>Radio Button Group Test</h1>

<EditForm Model="_model" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    @for (int i = 1; i <= 5; i++)
    {
        <label>
            <InputRadio name="rate" SelectedValue="i" @bind-Value="_model.Rating" />
            @i
        </label>
    }

    <button type="submit">Submit</button>
</EditForm>

<p>You chose: @_model.Rating</p>

@code {
    private Model _model = new Model();

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

## <a name="validation-support"></a><span data-ttu-id="7773e-151">驗證支援</span><span class="sxs-lookup"><span data-stu-id="7773e-151">Validation support</span></span>

<span data-ttu-id="7773e-152">`DataAnnotationsValidator` 元件會使用資料批註，將驗證支援附加至串聯的 `EditContext`。</span><span class="sxs-lookup"><span data-stu-id="7773e-152">The `DataAnnotationsValidator` component attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="7773e-153">若要啟用使用資料批註進行驗證的支援，則需要這個明確的手勢。</span><span class="sxs-lookup"><span data-stu-id="7773e-153">Enabling support for validation using data annotations requires this explicit gesture.</span></span> <span data-ttu-id="7773e-154">若要使用不同于資料批註的驗證系統，請將 `DataAnnotationsValidator` 取代為自訂的執行。</span><span class="sxs-lookup"><span data-stu-id="7773e-154">To use a different validation system than data annotations, replace the `DataAnnotationsValidator` with a custom implementation.</span></span> <span data-ttu-id="7773e-155">ASP.NET Core 的執行可用於參考來源中的檢查： [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs)。</span><span class="sxs-lookup"><span data-stu-id="7773e-155">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span></span>

Blazor<span data-ttu-id="7773e-156"> 會執行兩種類型的驗證：</span><span class="sxs-lookup"><span data-stu-id="7773e-156"> performs two types of validation:</span></span>

* <span data-ttu-id="7773e-157">*欄位驗證*是在使用者跳到欄位外時執行。</span><span class="sxs-lookup"><span data-stu-id="7773e-157">*Field validation* is performed when the user tabs out of a field.</span></span> <span data-ttu-id="7773e-158">在欄位驗證期間，`DataAnnotationsValidator` 元件會將所有報告的驗證結果與欄位產生關聯。</span><span class="sxs-lookup"><span data-stu-id="7773e-158">During field validation, the `DataAnnotationsValidator` component associates all reported validation results with the field.</span></span>
* <span data-ttu-id="7773e-159">當使用者提交表單時，就會執行*模型驗證*。</span><span class="sxs-lookup"><span data-stu-id="7773e-159">*Model validation* is performed when the user submits the form.</span></span> <span data-ttu-id="7773e-160">在模型驗證期間，`DataAnnotationsValidator` 元件會嘗試根據驗證結果所報告的成員名稱來決定欄位。</span><span class="sxs-lookup"><span data-stu-id="7773e-160">During model validation, the `DataAnnotationsValidator` component attempts to determine the field based on the member name that the validation result reports.</span></span> <span data-ttu-id="7773e-161">未與個別成員相關聯的驗證結果會與模型相關聯，而不是欄位。</span><span class="sxs-lookup"><span data-stu-id="7773e-161">Validation results that aren't associated with an individual member are associated with the model rather than a field.</span></span>

### <a name="validation-summary-and-validation-message-components"></a><span data-ttu-id="7773e-162">驗證摘要和驗證訊息元件</span><span class="sxs-lookup"><span data-stu-id="7773e-162">Validation Summary and Validation Message components</span></span>

<span data-ttu-id="7773e-163">`ValidationSummary` 元件會匯總所有驗證訊息，類似于[驗證摘要](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)標籤協助程式：</span><span class="sxs-lookup"><span data-stu-id="7773e-163">The `ValidationSummary` component summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):</span></span>

```razor
<ValidationSummary />
```

<span data-ttu-id="7773e-164">具有 `Model` 參數的特定模型輸出驗證訊息：</span><span class="sxs-lookup"><span data-stu-id="7773e-164">Output validation messages for a specific model with the `Model` parameter:</span></span>
  
```razor
<ValidationSummary Model="@_starship" />
```

<span data-ttu-id="7773e-165">`ValidationMessage` 元件會顯示特定欄位的驗證訊息，類似于[驗證訊息標記](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)協助程式。</span><span class="sxs-lookup"><span data-stu-id="7773e-165">The `ValidationMessage` component displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="7773e-166">使用 `For` 屬性指定驗證欄位，並以 lambda 運算式命名模型屬性：</span><span class="sxs-lookup"><span data-stu-id="7773e-166">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```razor
<ValidationMessage For="@(() => _starship.MaximumAccommodation)" />
```

<span data-ttu-id="7773e-167">`ValidationMessage` 和 `ValidationSummary` 元件支援任意屬性。</span><span class="sxs-lookup"><span data-stu-id="7773e-167">The `ValidationMessage` and `ValidationSummary` components support arbitrary attributes.</span></span> <span data-ttu-id="7773e-168">任何不符合元件參數的屬性都會加入至產生的 `<div>` 或 `<ul>` 元素。</span><span class="sxs-lookup"><span data-stu-id="7773e-168">Any attribute that doesn't match a component parameter is added to the generated `<div>` or `<ul>` element.</span></span>

### <a name="custom-validation-attributes"></a><span data-ttu-id="7773e-169">自訂驗證屬性</span><span class="sxs-lookup"><span data-stu-id="7773e-169">Custom validation attributes</span></span>

<span data-ttu-id="7773e-170">若要在使用[自訂驗證屬性](xref:mvc/models/validation#custom-attributes)時，確保驗證結果與欄位正確相關聯，請在建立 <xref:System.ComponentModel.DataAnnotations.ValidationResult>時，傳遞驗證內容的 <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName>：</span><span class="sxs-lookup"><span data-stu-id="7773e-170">To ensure that a validation result is correctly associated with a field when using a [custom validation attribute](xref:mvc/models/validation#custom-attributes), pass the validation context's <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> when creating the <xref:System.ComponentModel.DataAnnotations.ValidationResult>:</span></span>

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

### <a name="opno-locblazor-data-annotations-validation-package"></a>Blazor<span data-ttu-id="7773e-171"> 資料批註驗證封裝</span><span class="sxs-lookup"><span data-stu-id="7773e-171"> data annotations validation package</span></span>

<span data-ttu-id="7773e-172">[AspNetCore.Blazor。DataAnnotations](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation)是使用 `DataAnnotationsValidator` 元件來填滿驗證體驗差距的封裝。</span><span class="sxs-lookup"><span data-stu-id="7773e-172">The [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) is a package that fills validation experience gaps using the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="7773e-173">封裝目前為*實驗*性。</span><span class="sxs-lookup"><span data-stu-id="7773e-173">The package is currently *experimental*.</span></span>

### <a name="compareproperty-attribute"></a><span data-ttu-id="7773e-174">[CompareProperty] 屬性</span><span class="sxs-lookup"><span data-stu-id="7773e-174">[CompareProperty] attribute</span></span>

<span data-ttu-id="7773e-175"><xref:System.ComponentModel.DataAnnotations.CompareAttribute> 無法與 `DataAnnotationsValidator` 元件搭配運作，因為它不會將驗證結果與特定成員產生關聯。</span><span class="sxs-lookup"><span data-stu-id="7773e-175">The <xref:System.ComponentModel.DataAnnotations.CompareAttribute> doesn't work well with the `DataAnnotationsValidator` component because it doesn't associate the validation result with a specific member.</span></span> <span data-ttu-id="7773e-176">這可能會導致欄位層級驗證與整個模型在提交時進行驗證時的行為不一致。</span><span class="sxs-lookup"><span data-stu-id="7773e-176">This can result in inconsistent behavior between field-level validation and when the entire model is validated on a submit.</span></span> <span data-ttu-id="7773e-177">[AspNetCore.Blazor。DataAnnotations](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) *實驗*性封裝引進了額外的驗證屬性 `ComparePropertyAttribute`，這會因應這些限制。</span><span class="sxs-lookup"><span data-stu-id="7773e-177">The [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) *experimental* package introduces an additional validation attribute, `ComparePropertyAttribute`, that works around these limitations.</span></span> <span data-ttu-id="7773e-178">在 Blazor 應用程式中，`[CompareProperty]` 是 `[Compare]` 屬性的直接取代。</span><span class="sxs-lookup"><span data-stu-id="7773e-178">In a Blazor app, `[CompareProperty]` is a direct replacement for the `[Compare]` attribute.</span></span>

### <a name="nested-models-collection-types-and-complex-types"></a><span data-ttu-id="7773e-179">嵌套模型、集合類型和複雜類型</span><span class="sxs-lookup"><span data-stu-id="7773e-179">Nested models, collection types, and complex types</span></span>

Blazor<span data-ttu-id="7773e-180"> 支援使用內建 `DataAnnotationsValidator`的資料批註來驗證表單輸入。</span><span class="sxs-lookup"><span data-stu-id="7773e-180"> provides support for validating form input using data annotations with the built-in `DataAnnotationsValidator`.</span></span> <span data-ttu-id="7773e-181">不過，`DataAnnotationsValidator` 只會驗證系結至不是集合或複雜型別屬性之表單之模型的最上層屬性。</span><span class="sxs-lookup"><span data-stu-id="7773e-181">However, the `DataAnnotationsValidator` only validates top-level properties of the model bound to the form that aren't collection- or complex-type properties.</span></span>

<span data-ttu-id="7773e-182">若要驗證系結模型的整個物件圖形（包括集合和複雜型別屬性），請使用*實驗*性 AspNetCore 所提供的 `ObjectGraphDataAnnotationsValidator`。 [Blazor。DataAnnotations。驗證](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation)套件：</span><span class="sxs-lookup"><span data-stu-id="7773e-182">To validate the bound model's entire object graph, including collection- and complex-type properties, use the `ObjectGraphDataAnnotationsValidator` provided by the *experimental* [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) package:</span></span>

```razor
<EditForm Model="@_model" OnValidSubmit="HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

<span data-ttu-id="7773e-183">使用 `[ValidateComplexType]`標注模型屬性。</span><span class="sxs-lookup"><span data-stu-id="7773e-183">Annotate model properties with `[ValidateComplexType]`.</span></span> <span data-ttu-id="7773e-184">在下列模型類別中，`ShipDescription` 類別包含額外的資料批註，以在模型系結至表單時進行驗證：</span><span class="sxs-lookup"><span data-stu-id="7773e-184">In the following model classes, the `ShipDescription` class contains additional data annotations to validate when the model is bound to the form:</span></span>

<span data-ttu-id="7773e-185">*Starship.cs*：</span><span class="sxs-lookup"><span data-stu-id="7773e-185">*Starship.cs*:</span></span>

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

<span data-ttu-id="7773e-186">*ShipDescription.cs*：</span><span class="sxs-lookup"><span data-stu-id="7773e-186">*ShipDescription.cs*:</span></span>

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

### <a name="enable-the-submit-button-based-on-form-validation"></a><span data-ttu-id="7773e-187">根據表單驗證啟用 [提交] 按鈕</span><span class="sxs-lookup"><span data-stu-id="7773e-187">Enable the submit button based on form validation</span></span>

<span data-ttu-id="7773e-188">若要根據表單驗證啟用和停用 [提交] 按鈕：</span><span class="sxs-lookup"><span data-stu-id="7773e-188">To enable and disable the submit button based on form validation:</span></span>

* <span data-ttu-id="7773e-189">當元件已初始化時，請使用表單的 `EditContext` 來指派模型。</span><span class="sxs-lookup"><span data-stu-id="7773e-189">Use the form's `EditContext` to assign the model when the component is initialized.</span></span>
* <span data-ttu-id="7773e-190">在內容的 `OnFieldChanged` 回呼中驗證表單，以啟用和停用 [提交] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7773e-190">Validate the form in the context's `OnFieldChanged` callback to enable and disable the submit button.</span></span>

```razor
<EditForm EditContext="@_editContext">
    <DataAnnotationsValidator />
    <ValidationSummary />

    ...

    <button type="submit" disabled="@_formInvalid">Submit</button>
</EditForm>

@code {
    private Starship _starship = new Starship();
    private bool _formInvalid = true;
    private EditContext _editContext;

    protected override void OnInitialized()
    {
        _editContext = new EditContext(_starship);

        _editContext.OnFieldChanged += (_, __) =>
        {
            _formInvalid = !_editContext.Validate();
            StateHasChanged();
        };
    }
}
```

<span data-ttu-id="7773e-191">在上述範例中，將 `_formInvalid` 設定為 `false` 如果：</span><span class="sxs-lookup"><span data-stu-id="7773e-191">In the preceding example, set `_formInvalid` to `false` if:</span></span>

* <span data-ttu-id="7773e-192">表單會預先載入有效的預設值。</span><span class="sxs-lookup"><span data-stu-id="7773e-192">The form is preloaded with valid default values.</span></span>
* <span data-ttu-id="7773e-193">當表單載入時，您會想要啟用 [提交] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7773e-193">You want the submit button enabled when the form loads.</span></span>

<span data-ttu-id="7773e-194">先前的方法有一個副作用，就是在使用者與任何一個欄位互動之後，`ValidationSummary` 元件會填入不正確欄位。</span><span class="sxs-lookup"><span data-stu-id="7773e-194">A side effect of the preceding approach is that a `ValidationSummary` component is populated with invalid fields after the user interacts with any one field.</span></span> <span data-ttu-id="7773e-195">此案例可透過下列其中一種方式來解決：</span><span class="sxs-lookup"><span data-stu-id="7773e-195">This scenario can be addressed in either of the following ways:</span></span>

* <span data-ttu-id="7773e-196">請勿使用表單上的 `ValidationSummary` 元件。</span><span class="sxs-lookup"><span data-stu-id="7773e-196">Don't use a `ValidationSummary` component on the form.</span></span>
* <span data-ttu-id="7773e-197">選取 [提交] 按鈕時，讓 `ValidationSummary` 元件顯示（例如，在 `HandleValidSubmit` 方法中）。</span><span class="sxs-lookup"><span data-stu-id="7773e-197">Make the `ValidationSummary` component visible when the submit button is selected (for example, in a `HandleValidSubmit` method).</span></span>

```razor
<EditForm EditContext="@_editContext" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary style="@_displaySummary" />

    ...

    <button type="submit" disabled="@_formInvalid">Submit</button>
</EditForm>

@code {
    private string _displaySummary = "display:none";

    ...

    private void HandleValidSubmit()
    {
        _displaySummary = "display:block";
    }
}
```
