---
title: ASP.NET Core Blazor 表單和驗證
author: guardrex
description: 瞭解如何在 Blazor中使用表單和欄位驗證案例。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/forms-validation
ms.openlocfilehash: a94a433f26e451bbadc73615e502e46d273f05c2
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828135"
---
# <a name="aspnet-core-opno-locblazor-forms-and-validation"></a>ASP.NET Core Blazor 表單和驗證

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

使用[資料批註](xref:mvc/models/validation)的 Blazor 中支援表單和驗證。

下列 `ExampleModel` 類型會使用資料批註來定義驗證邏輯：

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

表單是使用 `EditForm` 元件所定義。 下列表單會示範典型的元素、元件和 Razor 程式碼：

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

* 表單會使用 `ExampleModel` 型別中定義的驗證，來驗證 `name` 欄位中的使用者輸入。 模型會在元件的 `@code` 區塊中建立，並保存在私用欄位（`exampleModel`）中。 欄位會指派給 `<EditForm>` 元素的 `Model` 屬性。
* `DataAnnotationsValidator` 元件會使用資料批註附加驗證支援。
* `ValidationSummary` 元件會摘要說明驗證訊息。
* 當表單成功提交（通過驗證）時，就會觸發 `HandleValidSubmit`。

有一組內建的輸入元件可用來接收和驗證使用者輸入。 當輸入變更時和送出表單時，會進行驗證。 下表顯示可用的輸入元件。

| 輸入元件 | 呈現為&hellip;       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

所有的輸入元件（包括 `EditForm`）都支援任意屬性。 任何不符合元件參數的屬性都會加入至轉譯的 HTML 專案。

輸入元件會提供預設行為，以便在編輯和變更其 CSS 類別時進行驗證，以反映欄位狀態。 某些元件包含有用的剖析邏輯。 例如，`InputDate` 和 `InputNumber` 藉由將其註冊為驗證錯誤來適當地處理無法剖析的值。 可以接受 null 值的類型也支援目標欄位的 null 屬性（例如，`int?`）。

下列 `Starship` 類型使用比先前 `ExampleModel`更大的屬性集和資料批註來定義驗證邏輯：

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

在上述範例中，`Description` 是選擇性的，因為沒有任何資料批註存在。

下列表單會使用 `Starship` 模型中所定義的驗證來驗證使用者輸入：

```razor
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

會建立做`EditContext`為[階層式值](xref:blazor/components#cascading-values-and-parameters), 以追蹤編輯程式的相關中繼資料, 包括已修改的欄位和目前的驗證訊息。`EditForm` `EditForm` 也會提供有效和無效提交（`OnValidSubmit`、`OnInvalidSubmit`）的便利事件。 或者，使用 `OnSubmit` 來觸發驗證，並使用自訂驗證程式代碼來檢查域值。

## <a name="inputtext-based-on-the-input-event"></a>根據輸入事件的 InputText

使用 `InputText` 元件來建立使用 `input` 事件的自訂群組件，而不是 `change` 事件。

建立具有下列標記的元件，並使用元件，就像使用 `InputText` 一樣：

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a>驗證支援

`DataAnnotationsValidator` 元件會使用資料批註，將驗證支援附加至串聯的 `EditContext`。 若要啟用使用資料批註進行驗證的支援，則需要這個明確的手勢。 若要使用不同于資料批註的驗證系統，請將 `DataAnnotationsValidator` 取代為自訂的執行。 ASP.NET Core 的執行可用於參考來源中的檢查： [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs)。

Blazor 會執行兩種類型的驗證：

* *欄位驗證*是在使用者跳到欄位外時執行。 在欄位驗證期間，`DataAnnotationsValidator` 元件會將所有報告的驗證結果與欄位產生關聯。
* 當使用者提交表單時，就會執行*模型驗證*。 在模型驗證期間，`DataAnnotationsValidator` 元件會嘗試根據驗證結果所報告的成員名稱來決定欄位。 未與個別成員相關聯的驗證結果會與模型相關聯，而不是欄位。

### <a name="validation-summary-and-validation-message-components"></a>驗證摘要和驗證訊息元件

`ValidationSummary` 元件會匯總所有驗證訊息，類似于[驗證摘要](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)標籤協助程式：

```razor
<ValidationSummary />
```

具有 `Model` 參數的特定模型輸出驗證訊息：
  
```razor
<ValidationSummary Model="@starship" />
```

`ValidationMessage` 元件會顯示特定欄位的驗證訊息，類似于[驗證訊息標記](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)協助程式。 使用 `For` 屬性指定驗證欄位，並以 lambda 運算式命名模型屬性：

```razor
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

`ValidationMessage` 和 `ValidationSummary` 元件支援任意屬性。 任何不符合元件參數的屬性都會加入至產生的 `<div>` 或 `<ul>` 元素。

### <a name="custom-validation-attributes"></a>自訂驗證屬性

若要在使用[自訂驗證屬性](xref:mvc/models/validation#custom-attributes)時，確保驗證結果與欄位正確相關聯，請在建立 <xref:System.ComponentModel.DataAnnotations.ValidationResult>時，傳遞驗證內容的 <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName>：

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

### <a name="opno-locblazor-data-annotations-validation-package"></a>Blazor 資料批註驗證封裝

[AspNetCore.Blazor。DataAnnotations](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation)是使用 `DataAnnotationsValidator` 元件來填滿驗證體驗差距的封裝。 封裝目前為*實驗*性。

### <a name="compareproperty-attribute"></a>[CompareProperty] 屬性

<xref:System.ComponentModel.DataAnnotations.CompareAttribute> 無法與 `DataAnnotationsValidator` 元件搭配運作。 [AspNetCore.Blazor。DataAnnotations](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) *實驗*性封裝引進了額外的驗證屬性 `ComparePropertyAttribute`，這會因應這些限制。 在 Blazor 應用程式中，`[CompareProperty]` 是 `[Compare]` 屬性的直接取代。 如需詳細資訊，請參閱[CompareAttribute 已忽略 With OnValidSubmit EditForm （dotnet/AspNetCore #10643）](https://github.com/dotnet/AspNetCore/issues/10643#issuecomment-543909748)。

### <a name="nested-models-collection-types-and-complex-types"></a>嵌套模型、集合類型和複雜類型

Blazor 支援使用內建 `DataAnnotationsValidator`的資料批註來驗證表單輸入。 不過，`DataAnnotationsValidator` 只會驗證系結至不是集合或複雜型別屬性之表單之模型的最上層屬性。

若要驗證系結模型的整個物件圖形（包括集合和複雜型別屬性），請使用*實驗*性 AspNetCore 所提供的 `ObjectGraphDataAnnotationsValidator`。 [Blazor。DataAnnotations。驗證](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation)套件：

```razor
<EditForm Model="@model" OnValidSubmit="@HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

使用 `[ValidateComplexType]`標注模型屬性。 在下列模型類別中，`ShipDescription` 類別包含額外的資料批註，以在模型系結至表單時進行驗證：

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

::: moniker-end

::: moniker range="< aspnetcore-3.1"

### <a name="validation-of-complex-or-collection-type-properties"></a>複雜或集合型別屬性的驗證

當提交表單時，套用至模型屬性的驗證屬性會進行驗證。 不過，`DataAnnotationsValidator` 元件提交表單時，不會驗證集合的屬性或模型的複雜資料類型。 若要在這種情況下接受嵌套驗證屬性，請使用自訂驗證元件。 如需範例，請參閱[Blazor 驗證範例（aspnet/sample）](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation)。

::: moniker-end
