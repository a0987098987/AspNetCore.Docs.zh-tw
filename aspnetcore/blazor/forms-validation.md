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
# <a name="aspnet-core-blazor-forms-and-validation"></a>ASP.NET核心布拉佐爾窗體和驗證

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

Blazor 使用[數據註釋](xref:mvc/models/validation)支援窗體和驗證。

以下`ExampleModel`型態使用資料註解定義認證邏輯:

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

使用`EditForm`元件定義窗體。 以下表單展示了典型的元素、元件和 Razor 碼:

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

在上述範例中：

* 表單使用`name``ExampleModel`類型中定義的驗證驗證欄位中的使用者輸入。 模型在元件的`@code`塊中創建,並位於私有欄位 ()`_exampleModel`中。 該欄位分配給元素`Model`的屬性。 `<EditForm>`
* 元件`InputText`的`@bind-Value`繫結:
  * 模型屬性`_exampleModel.Name`(`InputText`) 到元件`Value`的屬性 。
  * 變更事件委託給`InputText`元件的屬性`ValueChanged`。
* 元件`DataAnnotationsValidator`使用數據註釋附加驗證支援。
* 元件`ValidationSummary`彙總驗證消息。
* `HandleValidSubmit`當表單成功提交(通過驗證)時觸發。

一組內置輸入元件可用於接收和驗證使用者輸入。 當更改輸入和提交表單時,將驗證輸入。 下表顯示了可用的輸入元件。

| 輸入元件 | 成像為&hellip;       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

所有輸入元件(包括`EditForm`)都支援任意屬性。 任何與元件參數不匹配的屬性都添加到呈現的 HTML 元素中。

輸入元件提供預設行為,用於在編輯時驗證並更改其 CSS 類以反映欄位狀態。 某些元件包括有用的分析邏輯。 例如,`InputDate`通過`InputNumber`將不可解析的值註冊為驗證錯誤,可以正常處理這些值。 可以接受空值的類型也支援目標欄位的 null(例如`int?`,

以下`Starship`類型使用比`ExampleModel`前面 更大的屬性和資料註解集定義認證邏輯:

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

在前面的示例中,是`Description`可選的,因為不存在數據註釋。

以下表單使用`Starship`模型中定義的驗證驗證使用者輸入:

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

將`EditForm`創建`EditContext`一個級[聯值](xref:blazor/components#cascading-values-and-parameters),用於跟蹤有關編輯過程的元數據,包括已修改的欄位和當前驗證消息。 還為`EditForm`有效與不合法的提交 ()`OnValidSubmit``OnInvalidSubmit`提供方便的事件 ( 。 或者,使用`OnSubmit`用於觸發驗證並使用自定義驗證代碼檢查欄位值。

在下例中︰

* 當`HandleSubmit`選擇 **「提交」** 按鈕時,該方法將運行。
* 該表單使用表單體`EditContext`的驗證。
* 通過將 傳遞`EditContext`給 在伺服器上調用`ServerValidate`Web API 終結點的方法(*未顯示*),進一步驗證窗體。
* 根據用戶端和伺服器端驗證的結果,通過檢查`isValid`運行其他代碼。

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

## <a name="inputtext-based-on-the-input-event"></a>建基於輸入事件的輸入文字

使用元件`InputText`建立使用`input`事件`change`而不是 事件的自訂元件。

使用以下標籤建立元件,並使用元件,就像使用時一樣`InputText`:

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="work-with-radio-buttons"></a>使用單選按鈕

在表單選按鈕時,資料綁定的處理方式與其他元素不同,因為單選按鈕作為組進行計算。 每個單選按鈕的值是固定的,但單選按鈕組的值是所選單選按鈕的值。 下列範例示範如何執行：

* 處理單選按鈕組的數據綁定。
* 支援使用自定義`InputRadio`元件進行驗證。

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

以下部分`EditForm`使用`InputRadio`上述 元件從使用者取得與驗證評級:

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

## <a name="validation-support"></a>驗證支援

元件`DataAnnotationsValidator`使用資料註解將驗證支援額外`EditContext`的順序 。 啟用使用數據註釋驗證的支援需要此顯式手勢。 要使用與資料註解不同的驗證系統,請使用自訂來取代`DataAnnotationsValidator`。 ASP.NET核心可用於參考來源中的檢查:[資料註解驗證器](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotations 驗證](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs)。

Blazor執行兩種類型的驗證:

* 當使用者從欄位中選項卡時,將執行*欄位驗證*。 在欄位驗證期間,`DataAnnotationsValidator`元件將所有報告的驗證結果與欄位關聯。
* 當使用者提交表單時,將執行*模型驗證*。 在模型驗證期間,`DataAnnotationsValidator`元件嘗試根據驗證結果報告的成員名稱確定該欄位。 與單個成員不關聯的驗證結果與模型相關聯,而不是與欄位相關聯。

### <a name="validation-summary-and-validation-message-components"></a>驗證摘要及驗證訊息元件

此`ValidationSummary`元件匯總了所有認證訊息,這類似於[認證摘要標籤說明程式](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):

```razor
<ValidationSummary />
```

使用 參數輸出特定模型`Model`的 驗證訊息:
  
```razor
<ValidationSummary Model="@_starship" />
```

此`ValidationMessage`元件顯示特定欄位的驗證訊息,該訊息類似於[驗證訊息標記說明程式](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)。 使用`For`屬性指定用於驗證的欄位,並為模型屬性命名 lambda 表示式:

```razor
<ValidationMessage For="@(() => _starship.MaximumAccommodation)" />
```

`ValidationMessage`和`ValidationSummary`元件支援任意屬性。 任何與元件參數不匹配的屬性都添加到生成的`<div>`或`<ul>`元素中。

### <a name="custom-validation-attributes"></a>自訂驗證屬性

要確保在使用[自訂驗證屬性](xref:mvc/models/validation#custom-attributes)時與欄位正確關聯,在建立 時傳遞<xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName>驗證<xref:System.ComponentModel.DataAnnotations.ValidationResult>上下文 。

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

### <a name="opno-locblazor-data-annotations-validation-package"></a>Blazor資料註解驗證套件

[Microsoft.AspNetCore.元件.Data註釋.驗證](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation)是一個使用元件填充驗證體驗空白的`DataAnnotationsValidator`包。 該包裝目前正在*試驗*中。

### <a name="compareproperty-attribute"></a>【 比較屬性】 屬性

<xref:System.ComponentModel.DataAnnotations.CompareAttribute>無法很好地處理元件,`DataAnnotationsValidator`因為它不將驗證結果與特定成員相關聯。 這可能導致欄位級驗證與在提交上驗證整個模型時的行為不一致。 [Microsoft.AspNetCore.元件.DataAnnotations.驗證](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation)*實驗*包引入了一個額外的驗證屬性`ComparePropertyAttribute`, 它適用於這些限制。 在Blazor應用中,`[CompareProperty]``[Compare]`是 屬性的直接替換。

### <a name="nested-models-collection-types-and-complex-types"></a>巢狀模型、集合類型和複雜類型

Blazor支援使用內建的資料註解驗證表單`DataAnnotationsValidator`輸入 。 但是,`DataAnnotationsValidator`只有驗證綁定到非集合或複雜類型屬性的窗體的模型的頂級屬性。

要驗證繫結模型的整個物件圖(包括集合和複雜類型屬性),請使用`ObjectGraphDataAnnotationsValidator`*實驗* [Microsoft.AspNetCore.元件.Data註解.驗證](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation)套件:

```razor
<EditForm Model="@_model" OnValidSubmit="HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

使用`[ValidateComplexType]`對模型屬性進行一些用。 在以下模型類別中,`ShipDescription`類別包含其他資料註解,用於驗證模型何時綁定到窗體:

*Starship.cs*:

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

*ShipDescription.cs*:

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

### <a name="enable-the-submit-button-based-on-form-validation"></a>以表單驗證開啟的按鈕

要啟用和關閉以表單的提交按鈕,請:

* 在初始化元件時`EditContext`,使用表單分配模型。
* 在上下文`OnFieldChanged`回調中驗證表單以啟用和禁用提交按鈕。
* 取消挂鉤方法`Dispose`中的事件處理程式。 如需詳細資訊，請參閱 <xref:blazor/lifecycle#component-disposal-with-idisposable>。

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

在前面的範例中,設定為`_formInvalid``false`:

* 窗體預載入了有效的預設值。
* 您希望在表單載入時啟用提交按鈕。

上述方法的副作用是,在使用者與任何一`ValidationSummary`個字段交互後,元件填充了無效欄位。 可透過以下任一方式解決此方案:

* 不要在表單上使用`ValidationSummary`元件。
* 在選擇`ValidationSummary`提交按鈕時使元件可見(例如,在方法中`HandleValidSubmit`)。

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
