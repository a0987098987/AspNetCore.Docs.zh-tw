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
ms.openlocfilehash: 9ffcacc404aa868d533196e5c1bb52d9acdeb337
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82768977"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a>ASP.NET Core Blazor 表單和驗證

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

使用[資料批註](xref:mvc/models/validation)的 Blazor 支援表單和驗證。

下列`ExampleModel`型別會使用資料批註來定義驗證邏輯：

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

表單是使用`EditForm`元件定義的。 下列表單會示範典型的元素、元件和 Razor 程式碼：

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

* 表單會使用在`name` `ExampleModel`型別中定義的驗證來驗證欄位中的使用者輸入。 模型會建立在元件的`@code`區塊中，並保留在私用欄位（`_exampleModel`）中。 欄位會指派給`Model` `<EditForm>`元素的屬性。
* `InputText`元件的`@bind-Value`系結：
  * 模型屬性（`_exampleModel.Name`）至`InputText`元件的`Value`屬性。
  * `InputText`元件的`ValueChanged`屬性的變更事件委派。
* `DataAnnotationsValidator`元件會使用資料批註附加驗證支援。
* `ValidationSummary`元件會摘要驗證訊息。
* `HandleValidSubmit`當表單成功提交（通過驗證）時，就會觸發。

有一組內建的輸入元件可用來接收和驗證使用者輸入。 當輸入變更時和送出表單時，會進行驗證。 下表顯示可用的輸入元件。

| 輸入元件 | 轉譯為&hellip;       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

所有的輸入元件（包括`EditForm`）都支援任意屬性。 任何不符合元件參數的屬性都會加入至轉譯的 HTML 專案。

輸入元件會提供預設行為，以便在編輯和變更其 CSS 類別時進行驗證，以反映欄位狀態。 某些元件包含有用的剖析邏輯。 例如， `InputDate`和`InputNumber`會藉由將無法剖析的值註冊為驗證錯誤來妥善處理。 可以接受 null 值的類型也支援目標欄位的 null 屬性（例如`int?`）。

下列`Starship`型別使用較早`ExampleModel`的屬性集和資料注釋，來定義驗證邏輯：

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

在上述範例中， `Description`是選擇性的，因為沒有任何資料批註存在。

下列表單會使用`Starship`模型中定義的驗證來驗證使用者輸入：

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

會`EditForm`建立做`EditContext`為階層式[值](xref:blazor/components#cascading-values-and-parameters)，以追蹤編輯程式的相關中繼資料，包括已修改的欄位和目前的驗證訊息。 `EditForm`也提供有效和無效提交的便利事件（`OnValidSubmit`、 `OnInvalidSubmit`）。 或者，使用`OnSubmit`來觸發驗證，並使用自訂驗證程式代碼來檢查域值。

在下例中︰

* 當`HandleSubmit`選取 [**提交**] 按鈕時，就會執行方法。
* 表單會使用表單的`EditContext`進行驗證。
* 藉由將傳遞`EditContext`至在伺服器上呼叫 Web API `ServerValidate`端點的方法（*未顯示*），進一步驗證表單。
* 額外的程式碼會根據用戶端和伺服器端驗證的結果來執行（藉由`isValid`檢查）。

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

## <a name="inputtext-based-on-the-input-event"></a>根據輸入事件的 InputText

使用`InputText`元件來建立使用`input`事件的自訂群組件，而不是`change`事件。

建立具有下列標記的元件，並使用如同使用的元件`InputText` ：

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="work-with-radio-buttons"></a>使用選項按鈕

使用表單中的選項按鈕時，資料系結的處理方式不同于其他元素，因為選項按鈕會評估為群組。 每個選項按鈕的值都是固定的，但是選項按鈕群組的值就是選取之選項按鈕的值。 下列範例示範如何執行：

* 處理選項按鈕群組的資料系結。
* 支援使用自訂`InputRadio`元件的驗證。

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

下列`EditForm`使用上述`InputRadio`元件來取得並驗證使用者的評等：

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

`DataAnnotationsValidator`元件會使用資料批註，將驗證支援附加至`EditContext`串聯的。 若要啟用使用資料批註進行驗證的支援，則需要這個明確的手勢。 若要使用不同于資料批註的驗證系統，請`DataAnnotationsValidator`將取代為自訂的執行。 ASP.NET Core 的執行可用於參考來源中的檢查： [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs)。

Blazor會執行兩種類型的驗證：

* *欄位驗證*是在使用者跳到欄位外時執行。 在欄位驗證期間， `DataAnnotationsValidator`元件會將所有報告的驗證結果與欄位產生關聯。
* 當使用者提交表單時，就會執行*模型驗證*。 在模型驗證期間， `DataAnnotationsValidator`元件會嘗試根據驗證結果所報告的成員名稱來決定欄位。 未與個別成員相關聯的驗證結果會與模型相關聯，而不是欄位。

### <a name="validation-summary-and-validation-message-components"></a>驗證摘要和驗證訊息元件

此`ValidationSummary`元件會摘要所有驗證訊息，類似于[驗證摘要](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)標籤協助程式：

```razor
<ValidationSummary />
```

使用`Model`參數輸出特定模型的驗證訊息：
  
```razor
<ValidationSummary Model="@_starship" />
```

`ValidationMessage`元件會顯示特定欄位的驗證訊息，類似于[驗證訊息](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)標籤協助程式。 使用`For`屬性指定驗證欄位，並以 lambda 運算式命名模型屬性：

```razor
<ValidationMessage For="@(() => _starship.MaximumAccommodation)" />
```

`ValidationMessage`和`ValidationSummary`元件支援任意屬性。 任何不符合元件參數的屬性都會加入至產生`<div>`的或`<ul>`元素。

### <a name="custom-validation-attributes"></a>自訂驗證屬性

若要在使用[自訂驗證屬性](xref:mvc/models/validation#custom-attributes)時，確保驗證結果與欄位正確相關聯，請<xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName>在建立時傳遞驗證內容： <xref:System.ComponentModel.DataAnnotations.ValidationResult>

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

### <a name="blazor-data-annotations-validation-package"></a>Blazor資料批註驗證封裝

[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation)是使用`DataAnnotationsValidator`元件來填滿驗證體驗缺口的套件。 封裝目前為*實驗*性。

### <a name="compareproperty-attribute"></a>[CompareProperty] 屬性

<xref:System.ComponentModel.DataAnnotations.CompareAttribute>無法與`DataAnnotationsValidator`元件搭配運作，因為它不會將驗證結果與特定成員產生關聯。 這可能會導致欄位層級驗證與整個模型在提交時進行驗證時的行為不一致。 [DataAnnotations](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation) *實驗*性封裝引進了額外的驗證屬性， `ComparePropertyAttribute`其運作方式會因應這些限制。 在Blazor應用程式中`[CompareProperty]` ，是`[Compare]`屬性的直接取代。

### <a name="nested-models-collection-types-and-complex-types"></a>嵌套模型、集合類型和複雜類型

Blazor使用內`DataAnnotationsValidator`建的資料批註，提供驗證表單輸入的支援。 不過， `DataAnnotationsValidator`只會驗證系結至不是集合或複雜型別屬性之表單之模型的最上層屬性。

若要驗證系結模型的整個物件圖形（包括集合型和複雜型別屬性）， `ObjectGraphDataAnnotationsValidator`請使用*實驗*性的 DataAnnotations 所提供的。[驗證](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation)套件：

```razor
<EditForm Model="@_model" OnValidSubmit="HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

使用`[ValidateComplexType]`標注模型屬性。 在下列模型類別中， `ShipDescription`類別會包含其他資料批註，以在模型系結至表單時進行驗證：

*Starship.cs*：

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

*ShipDescription.cs*：

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

### <a name="enable-the-submit-button-based-on-form-validation"></a>根據表單驗證啟用 [提交] 按鈕

若要根據表單驗證啟用和停用 [提交] 按鈕：

* 當元件初始化時`EditContext` ，請使用表單的來指派模型。
* 在內容的`OnFieldChanged`回呼中驗證表單，以啟用和停用 [提交] 按鈕。
* 解除掛接`Dispose`方法中的事件處理常式。 如需詳細資訊，請參閱<xref:blazor/lifecycle#component-disposal-with-idisposable>。

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

在上述範例中，將`_formInvalid`設定`false`為（如果：

* 表單會預先載入有效的預設值。
* 當表單載入時，您會想要啟用 [提交] 按鈕。

上述方法的副作用在於，在使用者與任何`ValidationSummary`一個欄位互動之後，元件會填入不正確欄位。 此案例可透過下列其中一種方式來解決：

* 請勿使用窗`ValidationSummary`體上的元件。
* 選取 [ `ValidationSummary`提交] 按鈕時，讓元件顯示（例如，在`HandleValidSubmit`方法中）。

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
