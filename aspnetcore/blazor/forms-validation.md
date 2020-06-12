---
title: ASP.NET Core Blazor 表單和驗證
author: guardrex
description: 瞭解如何在中使用表單和欄位驗證案例 Blazor 。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/04/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/forms-validation
ms.openlocfilehash: 707686cdc4ceb6605d6214eeeee0f6dd2e121c90
ms.sourcegitcommit: 6371114344a5f4fbc5d4a119b0be1ad3762e0216
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/10/2020
ms.locfileid: "84679523"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a>ASP.NET Core Blazor 表單和驗證

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

Blazor使用[資料批註](xref:mvc/models/validation)可支援表單和驗證。

下列型別會 `ExampleModel` 使用資料批註來定義驗證邏輯：

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

表單是使用元件定義的 <xref:Microsoft.AspNetCore.Components.Forms.EditForm> 。 下列表單會示範典型的元素、元件和程式 Razor 代碼：

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

在上述範例中：

* 表單會使用在型別中 `name` 定義的驗證來驗證欄位中的使用者輸入 `ExampleModel` 。 模型會建立在元件的區塊中 `@code` ，並保留在私用欄位（ `exampleModel` ）中。 欄位會指派給元素的 `Model` 屬性 `<EditForm>` 。
* <xref:Microsoft.AspNetCore.Components.Forms.InputText>元件的系結 `@bind-Value` ：
  * 模型屬性（ `exampleModel.Name` ）至 <xref:Microsoft.AspNetCore.Components.Forms.InputText> 元件的 `Value` 屬性。 如需屬性系結的詳細資訊，請參閱 <xref:blazor/data-binding#parent-to-child-binding-with-component-parameters> 。
  * 元件的屬性的變更事件委派 <xref:Microsoft.AspNetCore.Components.Forms.InputText> `ValueChanged` 。
* <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator>元件會使用資料批註附加驗證支援。
* <xref:Microsoft.AspNetCore.Components.Forms.ValidationSummary>元件會摘要驗證訊息。
* `HandleValidSubmit`當表單成功提交（通過驗證）時，就會觸發。

有一組內建的輸入元件可用來接收和驗證使用者輸入。 當輸入變更時和送出表單時，會進行驗證。 下表顯示可用的輸入元件。

| 輸入元件 | 轉譯為&hellip; |
| --------------- | ------------------- |
| <xref:Microsoft.AspNetCore.Components.Forms.InputText> | `<input>` |
| <xref:Microsoft.AspNetCore.Components.Forms.InputTextArea> | `<textarea>` |
| <xref:Microsoft.AspNetCore.Components.Forms.InputSelect%601> | `<select>` |
| <xref:Microsoft.AspNetCore.Components.Forms.InputNumber%601> | `<input type="number">` |
| <xref:Microsoft.AspNetCore.Components.Forms.InputCheckbox> | `<input type="checkbox">` |
| <xref:Microsoft.AspNetCore.Components.Forms.InputDate%601> | `<input type="date">` |

所有的輸入元件（包括 <xref:Microsoft.AspNetCore.Components.Forms.EditForm> ）都支援任意屬性。 任何不符合元件參數的屬性都會加入至轉譯的 HTML 專案。

輸入元件會提供預設行為，以便在編輯和變更其 CSS 類別時進行驗證，以反映欄位狀態。 某些元件包含有用的剖析邏輯。 例如，和會藉由將無法剖析 <xref:Microsoft.AspNetCore.Components.Forms.InputDate%601> <xref:Microsoft.AspNetCore.Components.Forms.InputNumber%601> 的值註冊為驗證錯誤來妥善處理。 可以接受 null 值的類型也支援目標欄位的 null 屬性（例如 `int?` ）。

下列 `Starship` 型別使用較早的屬性集和資料注釋，來定義驗證邏輯 `ExampleModel` ：

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

在上述範例中， `Description` 是選擇性的，因為沒有任何資料批註存在。

下列表單會使用模型中定義的驗證來驗證使用者輸入 `Starship` ：

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

會 <xref:Microsoft.AspNetCore.Components.Forms.EditForm> 建立 <xref:Microsoft.AspNetCore.Components.Forms.EditContext> 做為階層式[值](xref:blazor/components#cascading-values-and-parameters)，以追蹤編輯程式的相關中繼資料，包括已修改的欄位和目前的驗證訊息。 <xref:Microsoft.AspNetCore.Components.Forms.EditForm>也提供有效和無效提交的便利事件（ <xref:Microsoft.AspNetCore.Components.Forms.EditForm.OnValidSubmit> 、 <xref:Microsoft.AspNetCore.Components.Forms.EditForm.OnInvalidSubmit> ）。 或者，使用 <xref:Microsoft.AspNetCore.Components.Forms.EditForm.OnSubmit> 來觸發驗證，並使用自訂驗證程式代碼來檢查域值。

在下例中︰

* `HandleSubmit`當選取 [**提交**] 按鈕時，就會執行方法。
* 表單會使用表單的進行驗證 <xref:Microsoft.AspNetCore.Components.Forms.EditContext> 。
* 藉由將傳遞 <xref:Microsoft.AspNetCore.Components.Forms.EditContext> 至在 `ServerValidate` 伺服器上呼叫 Web API 端點的方法（*未顯示*），進一步驗證表單。
* 額外的程式碼會根據用戶端和伺服器端驗證的結果來執行（藉由檢查） `isValid` 。

```razor
<EditForm EditContext="@editContext" OnSubmit="HandleSubmit">

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

## <a name="inputtext-based-on-the-input-event"></a>根據輸入事件的 InputText

使用 <xref:Microsoft.AspNetCore.Components.Forms.InputText> 元件來建立使用事件的自訂群組件， `input` 而不是 `change` 事件。

在下列範例中， `CustomInputText` 元件會繼承架構的 `InputText` 元件，並將事件系結（ <xref:Microsoft.AspNetCore.Components.EventCallbackFactoryBinderExtensions.CreateBinder%2A> ）設定為 `oninput` 事件。

*Shared/CustomInputText. razor*：

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue"
    @oninput="EventCallback.Factory.CreateBinder<string>(
         this, __value => CurrentValueAsString = __value, 
         CurrentValueAsString)" />
```

`CustomInputText`元件可以在使用的任何位置使用 <xref:Microsoft.AspNetCore.Components.Forms.InputText> ：

*Pages/TestForm. razor*：

```razor
@page  "/testform"
@using System.ComponentModel.DataAnnotations;

<EditForm Model="@exampleModel" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <CustomInputText @bind-Value="exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

<p>
    CurrentValue: @exampleModel.Name
</p>

@code {
    private ExampleModel exampleModel = new ExampleModel();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }

    public class ExampleModel
    {
        [Required]
        [StringLength(10, ErrorMessage = "Name is too long.")]
        public string Name { get; set; }
    }
}
```

## <a name="work-with-radio-buttons"></a>使用選項按鈕

使用表單中的選項按鈕時，資料系結的處理方式不同于其他元素，因為選項按鈕會評估為群組。 每個選項按鈕的值都是固定的，但是選項按鈕群組的值就是選取之選項按鈕的值。 下列範例示範如何執行：

* 處理選項按鈕群組的資料系結。
* 支援使用自訂群組件的驗證 `InputRadio` 。

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

下列 <xref:Microsoft.AspNetCore.Components.Forms.EditForm> 使用上述 `InputRadio` 元件來取得並驗證使用者的評等：

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

## <a name="validation-support"></a>驗證支援

<xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator>元件會使用資料批註，將驗證支援附加至串聯的 <xref:Microsoft.AspNetCore.Components.Forms.EditContext> 。 若要啟用使用資料批註進行驗證的支援，則需要這個明確的手勢。 若要使用不同于資料批註的驗證系統，請將取代為 <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> 自訂的執行。 ASP.NET Core 的執行可用於參考來源中的檢查： [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs) / [AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs)。

Blazor會執行兩種類型的驗證：

* *欄位驗證*是在使用者跳到欄位外時執行。 在欄位驗證期間， <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> 元件會將所有報告的驗證結果與欄位產生關聯。
* 當使用者提交表單時，就會執行*模型驗證*。 在模型驗證期間， <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> 元件會嘗試根據驗證結果所報告的成員名稱來決定欄位。 未與個別成員相關聯的驗證結果會與模型相關聯，而不是欄位。

### <a name="validation-summary-and-validation-message-components"></a>驗證摘要和驗證訊息元件

此 <xref:Microsoft.AspNetCore.Components.Forms.ValidationSummary> 元件會摘要所有驗證訊息，類似于[驗證摘要](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)標籤協助程式：

```razor
<ValidationSummary />
```

使用參數輸出特定模型的驗證訊息 `Model` ：
  
```razor
<ValidationSummary Model="@starship" />
```

<xref:Microsoft.AspNetCore.Components.Forms.ValidationMessage%601>元件會顯示特定欄位的驗證訊息，類似于[驗證訊息](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)標籤協助程式。 使用屬性指定驗證欄位 `For` ，並以 lambda 運算式命名模型屬性：

```razor
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

<xref:Microsoft.AspNetCore.Components.Forms.ValidationMessage%601>和 <xref:Microsoft.AspNetCore.Components.Forms.ValidationSummary> 元件支援任意屬性。 任何不符合元件參數的屬性都會加入至產生的 `<div>` 或 `<ul>` 元素。

### <a name="custom-validation-attributes"></a>自訂驗證屬性

若要在使用[自訂驗證屬性](xref:mvc/models/validation#custom-attributes)時，確保驗證結果與欄位正確相關聯，請 <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> 在建立時傳遞驗證內容 <xref:System.ComponentModel.DataAnnotations.ValidationResult> ：

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

[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation)是使用元件來填滿驗證體驗缺口的套件 <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> 。 封裝目前為*實驗*性。

### <a name="compareproperty-attribute"></a>[CompareProperty] 屬性

<xref:System.ComponentModel.DataAnnotations.CompareAttribute>無法與元件搭配運作， <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> 因為它不會將驗證結果與特定成員產生關聯。 這可能會導致欄位層級驗證與整個模型在提交時進行驗證時的行為不一致。 [DataAnnotations](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation) *實驗*性封裝引進了額外的驗證屬性，其 `ComparePropertyAttribute` 運作方式會因應這些限制。 在 Blazor 應用程式中， `[CompareProperty]` 是屬性的直接取代 [`[Compare]`](xref:System.ComponentModel.DataAnnotations.CompareAttribute) 。

### <a name="nested-models-collection-types-and-complex-types"></a>嵌套模型、集合類型和複雜類型

Blazor使用內建的資料批註，提供驗證表單輸入的支援 <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> 。 不過，只會驗證系結 <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> 至不是集合或複雜型別屬性之表單之模型的最上層屬性。

若要驗證系結模型的整個物件圖形（包括集合型和複雜型別屬性），請使用實驗性的 DataAnnotations 所提供的。 `ObjectGraphDataAnnotationsValidator` *experimental* [驗證](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation)套件：

```razor
<EditForm Model="@model" OnValidSubmit="HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

使用標注模型屬性 `[ValidateComplexType]` 。 在下列模型類別中， `ShipDescription` 類別會包含其他資料批註，以在模型系結至表單時進行驗證：

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

* 當元件初始化時，請使用表單的 <xref:Microsoft.AspNetCore.Components.Forms.EditContext> 來指派模型。
* 在內容的回呼中驗證表單 <xref:Microsoft.AspNetCore.Components.Forms.EditContext.OnFieldChanged> ，以啟用和停用 [提交] 按鈕。
* 解除掛接方法中的事件處理常式 `Dispose` 。 如需詳細資訊，請參閱 <xref:blazor/lifecycle#component-disposal-with-idisposable> 。

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

在上述範例中，將設定 `formInvalid` 為（ `false` 如果：

* 表單會預先載入有效的預設值。
* 當表單載入時，您會想要啟用 [提交] 按鈕。

上述方法的副作用在於，在 <xref:Microsoft.AspNetCore.Components.Forms.ValidationSummary> 使用者與任何一個欄位互動之後，元件會填入不正確欄位。 此案例可透過下列其中一種方式來解決：

* 請勿使用 <xref:Microsoft.AspNetCore.Components.Forms.ValidationSummary> 表單上的元件。
* <xref:Microsoft.AspNetCore.Components.Forms.ValidationSummary>選取 [提交] 按鈕時，讓元件顯示（例如，在方法中 `HandleValidSubmit` ）。

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

## <a name="troubleshoot"></a>疑難排解

> InvalidOperationException： EditForm 需要模型參數或 EditCoNtext 參數，但不能同時使用兩者。

確認 <xref:Microsoft.AspNetCore.Components.Forms.EditForm> 具有 <xref:Microsoft.AspNetCore.Components.Forms.EditForm.Model> 或 <xref:Microsoft.AspNetCore.Components.Forms.EditContext> 。

將指派 <xref:Microsoft.AspNetCore.Components.Forms.EditForm.Model> 給表單時，請確認模型類型已具現化，如下列範例所示：

```csharp
private ExampleModel exampleModel = new ExampleModel();
```
