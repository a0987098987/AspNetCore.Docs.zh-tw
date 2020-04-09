---
title: ASP.NET核心Blazor表單和驗證
author: guardrex
description: 瞭解如何在 中使用Blazor窗體 和欄位驗證方案。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- SignalR
uid: blazor/forms-validation
ms.openlocfilehash: 0359a9337860d9b8ce0b81d8833a034a898b05a5
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80218956"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a><span data-ttu-id="f5bac-103">ASP.NET核心布拉佐爾窗體和驗證</span><span class="sxs-lookup"><span data-stu-id="f5bac-103">ASP.NET Core Blazor forms and validation</span></span>

<span data-ttu-id="f5bac-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f5bac-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f5bac-105">Blazor 使用[數據註釋](xref:mvc/models/validation)支援窗體和驗證。</span><span class="sxs-lookup"><span data-stu-id="f5bac-105">Forms and validation are supported in Blazor using [data annotations](xref:mvc/models/validation).</span></span>

<span data-ttu-id="f5bac-106">以下`ExampleModel`型態使用資料註解定義認證邏輯:</span><span class="sxs-lookup"><span data-stu-id="f5bac-106">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="f5bac-107">使用`EditForm`元件定義窗體。</span><span class="sxs-lookup"><span data-stu-id="f5bac-107">A form is defined using the `EditForm` component.</span></span> <span data-ttu-id="f5bac-108">以下表單展示了典型的元素、元件和 Razor 碼:</span><span class="sxs-lookup"><span data-stu-id="f5bac-108">The following form demonstrates typical elements, components, and Razor code:</span></span>

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

<span data-ttu-id="f5bac-109">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="f5bac-109">In the preceding example:</span></span>

* <span data-ttu-id="f5bac-110">表單使用`name``ExampleModel`類型中定義的驗證驗證欄位中的使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="f5bac-110">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="f5bac-111">模型在元件的`@code`塊中創建,並位於私有欄位 ()`_exampleModel`中。</span><span class="sxs-lookup"><span data-stu-id="f5bac-111">The model is created in the component's `@code` block and held in a private field (`_exampleModel`).</span></span> <span data-ttu-id="f5bac-112">該欄位分配給元素`Model`的屬性。 `<EditForm>`</span><span class="sxs-lookup"><span data-stu-id="f5bac-112">The field is assigned to the `Model` attribute of the `<EditForm>` element.</span></span>
* <span data-ttu-id="f5bac-113">元件`InputText`的`@bind-Value`繫結:</span><span class="sxs-lookup"><span data-stu-id="f5bac-113">The `InputText` component's `@bind-Value` binds:</span></span>
  * <span data-ttu-id="f5bac-114">模型屬性`_exampleModel.Name`(`InputText`) 到元件`Value`的屬性 。</span><span class="sxs-lookup"><span data-stu-id="f5bac-114">The model property (`_exampleModel.Name`) to the `InputText` component's `Value` property.</span></span>
  * <span data-ttu-id="f5bac-115">變更事件委託給`InputText`元件的屬性`ValueChanged`。</span><span class="sxs-lookup"><span data-stu-id="f5bac-115">A change event delegate to the `InputText` component's `ValueChanged` property.</span></span>
* <span data-ttu-id="f5bac-116">元件`DataAnnotationsValidator`使用數據註釋附加驗證支援。</span><span class="sxs-lookup"><span data-stu-id="f5bac-116">The `DataAnnotationsValidator` component attaches validation support using data annotations.</span></span>
* <span data-ttu-id="f5bac-117">元件`ValidationSummary`彙總驗證消息。</span><span class="sxs-lookup"><span data-stu-id="f5bac-117">The `ValidationSummary` component summarizes validation messages.</span></span>
* <span data-ttu-id="f5bac-118">`HandleValidSubmit`當表單成功提交(通過驗證)時觸發。</span><span class="sxs-lookup"><span data-stu-id="f5bac-118">`HandleValidSubmit` is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="f5bac-119">一組內置輸入元件可用於接收和驗證使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="f5bac-119">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="f5bac-120">當更改輸入和提交表單時,將驗證輸入。</span><span class="sxs-lookup"><span data-stu-id="f5bac-120">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="f5bac-121">下表顯示了可用的輸入元件。</span><span class="sxs-lookup"><span data-stu-id="f5bac-121">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="f5bac-122">輸入元件</span><span class="sxs-lookup"><span data-stu-id="f5bac-122">Input component</span></span> | <span data-ttu-id="f5bac-123">成像為&hellip;</span><span class="sxs-lookup"><span data-stu-id="f5bac-123">Rendered as&hellip;</span></span>       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

<span data-ttu-id="f5bac-124">所有輸入元件(包括`EditForm`)都支援任意屬性。</span><span class="sxs-lookup"><span data-stu-id="f5bac-124">All of the input components, including `EditForm`, support arbitrary attributes.</span></span> <span data-ttu-id="f5bac-125">任何與元件參數不匹配的屬性都添加到呈現的 HTML 元素中。</span><span class="sxs-lookup"><span data-stu-id="f5bac-125">Any attribute that doesn't match a component parameter is added to the rendered HTML element.</span></span>

<span data-ttu-id="f5bac-126">輸入元件提供預設行為,用於在編輯時驗證並更改其 CSS 類以反映欄位狀態。</span><span class="sxs-lookup"><span data-stu-id="f5bac-126">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="f5bac-127">某些元件包括有用的分析邏輯。</span><span class="sxs-lookup"><span data-stu-id="f5bac-127">Some components include useful parsing logic.</span></span> <span data-ttu-id="f5bac-128">例如,`InputDate`通過`InputNumber`將不可解析的值註冊為驗證錯誤,可以正常處理這些值。</span><span class="sxs-lookup"><span data-stu-id="f5bac-128">For example, `InputDate` and `InputNumber` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="f5bac-129">可以接受空值的類型也支援目標欄位的 null(例如`int?`,</span><span class="sxs-lookup"><span data-stu-id="f5bac-129">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="f5bac-130">以下`Starship`類型使用比`ExampleModel`前面 更大的屬性和資料註解集定義認證邏輯:</span><span class="sxs-lookup"><span data-stu-id="f5bac-130">The following `Starship` type defines validation logic using a larger set of properties and data annotations than the earlier `ExampleModel`:</span></span>

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

<span data-ttu-id="f5bac-131">在前面的示例中,是`Description`可選的,因為不存在數據註釋。</span><span class="sxs-lookup"><span data-stu-id="f5bac-131">In the preceding example, `Description` is optional because no data annotations are present.</span></span>

<span data-ttu-id="f5bac-132">以下表單使用`Starship`模型中定義的驗證驗證使用者輸入:</span><span class="sxs-lookup"><span data-stu-id="f5bac-132">The following form validates user input using the validation defined in the `Starship` model:</span></span>

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

<span data-ttu-id="f5bac-133">將`EditForm`創建`EditContext`一個級[聯值](xref:blazor/components#cascading-values-and-parameters),用於跟蹤有關編輯過程的元數據,包括已修改的欄位和當前驗證消息。</span><span class="sxs-lookup"><span data-stu-id="f5bac-133">The `EditForm` creates an `EditContext` as a [cascading value](xref:blazor/components#cascading-values-and-parameters) that tracks metadata about the edit process, including which fields have been modified and the current validation messages.</span></span> <span data-ttu-id="f5bac-134">還為`EditForm`有效與不合法的提交 ()`OnValidSubmit``OnInvalidSubmit`提供方便的事件 ( 。</span><span class="sxs-lookup"><span data-stu-id="f5bac-134">The `EditForm` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="f5bac-135">或者,使用`OnSubmit`用於觸發驗證並使用自定義驗證代碼檢查欄位值。</span><span class="sxs-lookup"><span data-stu-id="f5bac-135">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

<span data-ttu-id="f5bac-136">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="f5bac-136">In the following example:</span></span>

* <span data-ttu-id="f5bac-137">當`HandleSubmit`選擇 **「提交」** 按鈕時,該方法將運行。</span><span class="sxs-lookup"><span data-stu-id="f5bac-137">The `HandleSubmit` method runs when the **Submit** button is selected.</span></span>
* <span data-ttu-id="f5bac-138">該表單使用表單體`EditContext`的驗證。</span><span class="sxs-lookup"><span data-stu-id="f5bac-138">The form is validated using the form's `EditContext`.</span></span>
* <span data-ttu-id="f5bac-139">通過將 傳遞`EditContext`給 在伺服器上調用`ServerValidate`Web API 終結點的方法(*未顯示*),進一步驗證窗體。</span><span class="sxs-lookup"><span data-stu-id="f5bac-139">The form is further validated by passing the `EditContext` to the `ServerValidate` method that calls a web API endpoint on the server (*not shown*).</span></span>
* <span data-ttu-id="f5bac-140">根據用戶端和伺服器端驗證的結果,通過檢查`isValid`運行其他代碼。</span><span class="sxs-lookup"><span data-stu-id="f5bac-140">Additional code is run depending on the result of the client- and server-side validation by checking `isValid`.</span></span>

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

## <a name="inputtext-based-on-the-input-event"></a><span data-ttu-id="f5bac-141">建基於輸入事件的輸入文字</span><span class="sxs-lookup"><span data-stu-id="f5bac-141">InputText based on the input event</span></span>

<span data-ttu-id="f5bac-142">使用元件`InputText`建立使用`input`事件`change`而不是 事件的自訂元件。</span><span class="sxs-lookup"><span data-stu-id="f5bac-142">Use the `InputText` component to create a custom component that uses the `input` event instead of the `change` event.</span></span>

<span data-ttu-id="f5bac-143">使用以下標籤建立元件,並使用元件,就像使用時一樣`InputText`:</span><span class="sxs-lookup"><span data-stu-id="f5bac-143">Create a component with the following markup, and use the component just as `InputText` is used:</span></span>

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="work-with-radio-buttons"></a><span data-ttu-id="f5bac-144">使用單選按鈕</span><span class="sxs-lookup"><span data-stu-id="f5bac-144">Work with radio buttons</span></span>

<span data-ttu-id="f5bac-145">在表單選按鈕時,資料綁定的處理方式與其他元素不同,因為單選按鈕作為組進行計算。</span><span class="sxs-lookup"><span data-stu-id="f5bac-145">When working with radio buttons in a form, data binding is handled differently than other elements because radio buttons are evaluated as a group.</span></span> <span data-ttu-id="f5bac-146">每個單選按鈕的值是固定的,但單選按鈕組的值是所選單選按鈕的值。</span><span class="sxs-lookup"><span data-stu-id="f5bac-146">The value of each radio button is fixed, but the value of the radio button group is the value of the selected radio button.</span></span> <span data-ttu-id="f5bac-147">下列範例示範如何執行：</span><span class="sxs-lookup"><span data-stu-id="f5bac-147">The following example shows how to:</span></span>

* <span data-ttu-id="f5bac-148">處理單選按鈕組的數據綁定。</span><span class="sxs-lookup"><span data-stu-id="f5bac-148">Handle data binding for a radio button group.</span></span>
* <span data-ttu-id="f5bac-149">支援使用自定義`InputRadio`元件進行驗證。</span><span class="sxs-lookup"><span data-stu-id="f5bac-149">Support validation using a custom `InputRadio` component.</span></span>

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

<span data-ttu-id="f5bac-150">以下部分`EditForm`使用`InputRadio`上述 元件從使用者取得與驗證評級:</span><span class="sxs-lookup"><span data-stu-id="f5bac-150">The following `EditForm` uses the preceding `InputRadio` component to obtain and validate a rating from the user:</span></span>

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

## <a name="validation-support"></a><span data-ttu-id="f5bac-151">驗證支援</span><span class="sxs-lookup"><span data-stu-id="f5bac-151">Validation support</span></span>

<span data-ttu-id="f5bac-152">元件`DataAnnotationsValidator`使用資料註解將驗證支援額外`EditContext`的順序 。</span><span class="sxs-lookup"><span data-stu-id="f5bac-152">The `DataAnnotationsValidator` component attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="f5bac-153">啟用使用數據註釋驗證的支援需要此顯式手勢。</span><span class="sxs-lookup"><span data-stu-id="f5bac-153">Enabling support for validation using data annotations requires this explicit gesture.</span></span> <span data-ttu-id="f5bac-154">要使用與資料註解不同的驗證系統,請使用自訂來取代`DataAnnotationsValidator`。</span><span class="sxs-lookup"><span data-stu-id="f5bac-154">To use a different validation system than data annotations, replace the `DataAnnotationsValidator` with a custom implementation.</span></span> <span data-ttu-id="f5bac-155">ASP.NET核心可用於參考來源中的檢查:[資料註解驗證器](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotations 驗證](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs)。</span><span class="sxs-lookup"><span data-stu-id="f5bac-155">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span></span>

Blazor<span data-ttu-id="f5bac-156">執行兩種類型的驗證:</span><span class="sxs-lookup"><span data-stu-id="f5bac-156"> performs two types of validation:</span></span>

* <span data-ttu-id="f5bac-157">當使用者從欄位中選項卡時,將執行*欄位驗證*。</span><span class="sxs-lookup"><span data-stu-id="f5bac-157">*Field validation* is performed when the user tabs out of a field.</span></span> <span data-ttu-id="f5bac-158">在欄位驗證期間,`DataAnnotationsValidator`元件將所有報告的驗證結果與欄位關聯。</span><span class="sxs-lookup"><span data-stu-id="f5bac-158">During field validation, the `DataAnnotationsValidator` component associates all reported validation results with the field.</span></span>
* <span data-ttu-id="f5bac-159">當使用者提交表單時,將執行*模型驗證*。</span><span class="sxs-lookup"><span data-stu-id="f5bac-159">*Model validation* is performed when the user submits the form.</span></span> <span data-ttu-id="f5bac-160">在模型驗證期間,`DataAnnotationsValidator`元件嘗試根據驗證結果報告的成員名稱確定該欄位。</span><span class="sxs-lookup"><span data-stu-id="f5bac-160">During model validation, the `DataAnnotationsValidator` component attempts to determine the field based on the member name that the validation result reports.</span></span> <span data-ttu-id="f5bac-161">與單個成員不關聯的驗證結果與模型相關聯,而不是與欄位相關聯。</span><span class="sxs-lookup"><span data-stu-id="f5bac-161">Validation results that aren't associated with an individual member are associated with the model rather than a field.</span></span>

### <a name="validation-summary-and-validation-message-components"></a><span data-ttu-id="f5bac-162">驗證摘要及驗證訊息元件</span><span class="sxs-lookup"><span data-stu-id="f5bac-162">Validation Summary and Validation Message components</span></span>

<span data-ttu-id="f5bac-163">此`ValidationSummary`元件匯總了所有認證訊息,這類似於[認證摘要標籤說明程式](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="f5bac-163">The `ValidationSummary` component summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):</span></span>

```razor
<ValidationSummary />
```

<span data-ttu-id="f5bac-164">使用 參數輸出特定模型`Model`的 驗證訊息:</span><span class="sxs-lookup"><span data-stu-id="f5bac-164">Output validation messages for a specific model with the `Model` parameter:</span></span>
  
```razor
<ValidationSummary Model="@_starship" />
```

<span data-ttu-id="f5bac-165">此`ValidationMessage`元件顯示特定欄位的驗證訊息,該訊息類似於[驗證訊息標記說明程式](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="f5bac-165">The `ValidationMessage` component displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="f5bac-166">使用`For`屬性指定用於驗證的欄位,並為模型屬性命名 lambda 表示式:</span><span class="sxs-lookup"><span data-stu-id="f5bac-166">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```razor
<ValidationMessage For="@(() => _starship.MaximumAccommodation)" />
```

<span data-ttu-id="f5bac-167">`ValidationMessage`和`ValidationSummary`元件支援任意屬性。</span><span class="sxs-lookup"><span data-stu-id="f5bac-167">The `ValidationMessage` and `ValidationSummary` components support arbitrary attributes.</span></span> <span data-ttu-id="f5bac-168">任何與元件參數不匹配的屬性都添加到生成的`<div>`或`<ul>`元素中。</span><span class="sxs-lookup"><span data-stu-id="f5bac-168">Any attribute that doesn't match a component parameter is added to the generated `<div>` or `<ul>` element.</span></span>

### <a name="custom-validation-attributes"></a><span data-ttu-id="f5bac-169">自訂驗證屬性</span><span class="sxs-lookup"><span data-stu-id="f5bac-169">Custom validation attributes</span></span>

<span data-ttu-id="f5bac-170">要確保在使用[自訂驗證屬性](xref:mvc/models/validation#custom-attributes)時與欄位正確關聯,在建立 時傳遞<xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName>驗證<xref:System.ComponentModel.DataAnnotations.ValidationResult>上下文 。</span><span class="sxs-lookup"><span data-stu-id="f5bac-170">To ensure that a validation result is correctly associated with a field when using a [custom validation attribute](xref:mvc/models/validation#custom-attributes), pass the validation context's <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> when creating the <xref:System.ComponentModel.DataAnnotations.ValidationResult>:</span></span>

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

### <a name="opno-locblazor-data-annotations-validation-package"></a>Blazor<span data-ttu-id="f5bac-171">資料註解驗證套件</span><span class="sxs-lookup"><span data-stu-id="f5bac-171"> data annotations validation package</span></span>

<span data-ttu-id="f5bac-172">[Microsoft.AspNetCore.元件.Data註釋.驗證](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation)是一個使用元件填充驗證體驗空白的`DataAnnotationsValidator`包。</span><span class="sxs-lookup"><span data-stu-id="f5bac-172">The [Microsoft.AspNetCore.Components.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation) is a package that fills validation experience gaps using the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="f5bac-173">該包裝目前正在*試驗*中。</span><span class="sxs-lookup"><span data-stu-id="f5bac-173">The package is currently *experimental*.</span></span>

### <a name="compareproperty-attribute"></a><span data-ttu-id="f5bac-174">【 比較屬性】 屬性</span><span class="sxs-lookup"><span data-stu-id="f5bac-174">[CompareProperty] attribute</span></span>

<span data-ttu-id="f5bac-175"><xref:System.ComponentModel.DataAnnotations.CompareAttribute>無法很好地處理元件,`DataAnnotationsValidator`因為它不將驗證結果與特定成員相關聯。</span><span class="sxs-lookup"><span data-stu-id="f5bac-175">The <xref:System.ComponentModel.DataAnnotations.CompareAttribute> doesn't work well with the `DataAnnotationsValidator` component because it doesn't associate the validation result with a specific member.</span></span> <span data-ttu-id="f5bac-176">這可能導致欄位級驗證與在提交上驗證整個模型時的行為不一致。</span><span class="sxs-lookup"><span data-stu-id="f5bac-176">This can result in inconsistent behavior between field-level validation and when the entire model is validated on a submit.</span></span> <span data-ttu-id="f5bac-177">[Microsoft.AspNetCore.元件.DataAnnotations.驗證](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation)*實驗*包引入了一個額外的驗證屬性`ComparePropertyAttribute`, 它適用於這些限制。</span><span class="sxs-lookup"><span data-stu-id="f5bac-177">The [Microsoft.AspNetCore.Components.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation) *experimental* package introduces an additional validation attribute, `ComparePropertyAttribute`, that works around these limitations.</span></span> <span data-ttu-id="f5bac-178">在Blazor應用中,`[CompareProperty]``[Compare]`是 屬性的直接替換。</span><span class="sxs-lookup"><span data-stu-id="f5bac-178">In a Blazor app, `[CompareProperty]` is a direct replacement for the `[Compare]` attribute.</span></span>

### <a name="nested-models-collection-types-and-complex-types"></a><span data-ttu-id="f5bac-179">巢狀模型、集合類型和複雜類型</span><span class="sxs-lookup"><span data-stu-id="f5bac-179">Nested models, collection types, and complex types</span></span>

Blazor<span data-ttu-id="f5bac-180">支援使用內建的資料註解驗證表單`DataAnnotationsValidator`輸入 。</span><span class="sxs-lookup"><span data-stu-id="f5bac-180"> provides support for validating form input using data annotations with the built-in `DataAnnotationsValidator`.</span></span> <span data-ttu-id="f5bac-181">但是,`DataAnnotationsValidator`只有驗證綁定到非集合或複雜類型屬性的窗體的模型的頂級屬性。</span><span class="sxs-lookup"><span data-stu-id="f5bac-181">However, the `DataAnnotationsValidator` only validates top-level properties of the model bound to the form that aren't collection- or complex-type properties.</span></span>

<span data-ttu-id="f5bac-182">要驗證繫結模型的整個物件圖(包括集合和複雜類型屬性),請使用`ObjectGraphDataAnnotationsValidator`*實驗* [Microsoft.AspNetCore.元件.Data註解.驗證](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation)套件:</span><span class="sxs-lookup"><span data-stu-id="f5bac-182">To validate the bound model's entire object graph, including collection- and complex-type properties, use the `ObjectGraphDataAnnotationsValidator` provided by the *experimental* [Microsoft.AspNetCore.Components.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation) package:</span></span>

```razor
<EditForm Model="@_model" OnValidSubmit="HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

<span data-ttu-id="f5bac-183">使用`[ValidateComplexType]`對模型屬性進行一些用。</span><span class="sxs-lookup"><span data-stu-id="f5bac-183">Annotate model properties with `[ValidateComplexType]`.</span></span> <span data-ttu-id="f5bac-184">在以下模型類別中,`ShipDescription`類別包含其他資料註解,用於驗證模型何時綁定到窗體:</span><span class="sxs-lookup"><span data-stu-id="f5bac-184">In the following model classes, the `ShipDescription` class contains additional data annotations to validate when the model is bound to the form:</span></span>

<span data-ttu-id="f5bac-185">*Starship.cs*:</span><span class="sxs-lookup"><span data-stu-id="f5bac-185">*Starship.cs*:</span></span>

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

<span data-ttu-id="f5bac-186">*ShipDescription.cs*:</span><span class="sxs-lookup"><span data-stu-id="f5bac-186">*ShipDescription.cs*:</span></span>

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

### <a name="enable-the-submit-button-based-on-form-validation"></a><span data-ttu-id="f5bac-187">以表單驗證開啟的按鈕</span><span class="sxs-lookup"><span data-stu-id="f5bac-187">Enable the submit button based on form validation</span></span>

<span data-ttu-id="f5bac-188">要啟用和關閉以表單的提交按鈕,請:</span><span class="sxs-lookup"><span data-stu-id="f5bac-188">To enable and disable the submit button based on form validation:</span></span>

* <span data-ttu-id="f5bac-189">在初始化元件時`EditContext`,使用表單分配模型。</span><span class="sxs-lookup"><span data-stu-id="f5bac-189">Use the form's `EditContext` to assign the model when the component is initialized.</span></span>
* <span data-ttu-id="f5bac-190">在上下文`OnFieldChanged`回調中驗證表單以啟用和禁用提交按鈕。</span><span class="sxs-lookup"><span data-stu-id="f5bac-190">Validate the form in the context's `OnFieldChanged` callback to enable and disable the submit button.</span></span>
* <span data-ttu-id="f5bac-191">取消挂鉤方法`Dispose`中的事件處理程式。</span><span class="sxs-lookup"><span data-stu-id="f5bac-191">Unhook the event handler in the `Dispose` method.</span></span> <span data-ttu-id="f5bac-192">如需詳細資訊，請參閱 <xref:blazor/lifecycle#component-disposal-with-idisposable>。</span><span class="sxs-lookup"><span data-stu-id="f5bac-192">For more information, see <xref:blazor/lifecycle#component-disposal-with-idisposable>.</span></span>

```razor
@implements IDisposable

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
        _editContext.OnFieldChanged += HandleFieldChanged;
    }

    private void HandleFieldChanged(object sender, FieldChangedEventArgs e)
    {
        _formInvalid = !_editContext.Validate();
        StateHasChanged();
    }

    public void Dispose()
    {
        _editContext.OnFieldChanged -= HandleFieldChanged;
    }
}
```

<span data-ttu-id="f5bac-193">在前面的範例中,設定為`_formInvalid``false`:</span><span class="sxs-lookup"><span data-stu-id="f5bac-193">In the preceding example, set `_formInvalid` to `false` if:</span></span>

* <span data-ttu-id="f5bac-194">窗體預載入了有效的預設值。</span><span class="sxs-lookup"><span data-stu-id="f5bac-194">The form is preloaded with valid default values.</span></span>
* <span data-ttu-id="f5bac-195">您希望在表單載入時啟用提交按鈕。</span><span class="sxs-lookup"><span data-stu-id="f5bac-195">You want the submit button enabled when the form loads.</span></span>

<span data-ttu-id="f5bac-196">上述方法的副作用是,在使用者與任何一`ValidationSummary`個字段交互後,元件填充了無效欄位。</span><span class="sxs-lookup"><span data-stu-id="f5bac-196">A side effect of the preceding approach is that a `ValidationSummary` component is populated with invalid fields after the user interacts with any one field.</span></span> <span data-ttu-id="f5bac-197">可透過以下任一方式解決此方案:</span><span class="sxs-lookup"><span data-stu-id="f5bac-197">This scenario can be addressed in either of the following ways:</span></span>

* <span data-ttu-id="f5bac-198">不要在表單上使用`ValidationSummary`元件。</span><span class="sxs-lookup"><span data-stu-id="f5bac-198">Don't use a `ValidationSummary` component on the form.</span></span>
* <span data-ttu-id="f5bac-199">在選擇`ValidationSummary`提交按鈕時使元件可見(例如,在方法中`HandleValidSubmit`)。</span><span class="sxs-lookup"><span data-stu-id="f5bac-199">Make the `ValidationSummary` component visible when the submit button is selected (for example, in a `HandleValidSubmit` method).</span></span>

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
