---
title: ASP.NET Core Blazor 表單和驗證
author: guardrex
description: 瞭解如何在 Blazor 中使用表單和欄位驗證案例。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/15/2019
uid: blazor/forms-validation
ms.openlocfilehash: 2fd76db90a53e328cd2ac8f452fba58365db0384
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011064"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a>ASP.NET Core Blazor 表單和驗證

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

使用[資料批註](xref:mvc/models/validation)的 Blazor 支援表單和驗證。

> [!NOTE]
> 每個預覽版本可能會變更表單和驗證案例。

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

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" @bind-Value="@exampleModel.Name" />

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

* 表單會使用在`name` `ExampleModel`型別中定義的驗證來驗證欄位中的使用者輸入。 模型會建立在元件的`@code`區塊中，並保留在私用欄位（`exampleModel`）中。 欄位會指派`Model`給`<EditForm>`元素的屬性。
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

輸入元件會提供預設行為，以便在編輯和變更其 CSS 類別時進行驗證，以反映欄位狀態。 某些元件包含有用的剖析邏輯。 例如， `InputDate`和`InputNumber`會藉由將無法剖析的值註冊為驗證錯誤來妥善處理。 可以接受 null 值的類型也支援目標欄位的 null 屬性（ `int?`例如）。

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

會建立做`EditContext`為[階層式值](xref:blazor/components#cascading-values-and-parameters), 以追蹤編輯程式的相關中繼資料, 包括已修改的欄位和目前的驗證訊息。`EditForm` 也提供有效和無效提交的便利事件（`OnValidSubmit`、 `OnInvalidSubmit`）。 `EditForm` 或者，使用`OnSubmit`來觸發驗證，並使用自訂驗證程式代碼來檢查域值。

## <a name="inputtext-based-on-the-input-event"></a>根據輸入事件的 InputText

使用元件來建立`input`使用事件的自訂群組件，而不是`change`事件。 `InputText`

建立具有下列標記的元件，並使用如同使用的元件`InputText` ：

```cshtml
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a>驗證支援

元件會使用資料批註，將驗證支援附加至`EditContext`串聯的。 `DataAnnotationsValidator` 若要啟用使用資料批註進行驗證的支援，則需要這個明確的手勢。 若要使用不同于資料批註的驗證系統，請`DataAnnotationsValidator`將取代為自訂的執行。 ASP.NET Core 的執行可用於參考來源中的檢查：[DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs)。

此`ValidationSummary`元件會摘要所有驗證訊息，類似于[驗證摘要](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)標籤協助程式。

元件會顯示特定欄位的驗證訊息，類似于[驗證訊息](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)標籤協助程式。 `ValidationMessage` 使用`For`屬性指定驗證欄位，並以 lambda 運算式命名模型屬性：

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

`ValidationMessage` 和`ValidationSummary`元件支援任意屬性。 任何不符合元件參數的屬性都會加入至產生`<div>`的或`<ul>`元素。

### <a name="validation-of-complex-or-collection-type-properties"></a>複雜或集合型別屬性的驗證

當提交表單時，套用至模型屬性的驗證屬性會進行驗證。 不過，在表單提交時，不會驗證集合的屬性或模型的複雜資料類型。 若要在這種情況下接受嵌套驗證屬性，請使用自訂驗證元件。 如需範例，請參閱[aspnet/Samples GitHub 存放庫中的 Blazor 驗證範例](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation)。
