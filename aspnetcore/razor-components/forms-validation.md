---
title: Razor 元件表單和驗證
author: guardrex
description: 了解如何使用 Razor 元件中的表單和欄位的驗證案例。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/forms-validation
ms.openlocfilehash: a4ed75efc6b5a733ce4ff4e82f354a8e2fd48500
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515423"
---
# <a name="razor-components-forms-and-validation"></a>Razor 元件表單和驗證

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

使用 Razor 元件支援的表單和驗證[資料註解](xref:mvc/models/validation)。

> [!NOTE]
> 表單和驗證案例可能會變更每個預覽版本。

下列`ExampleModel`類型會定義使用資料註解的驗證邏輯：

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

使用定義表單`<EditForm>`元件。 下列的表單會示範一般的項目、 元件和 Razor 程式碼：

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" bind-Value="@exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

@functions {
    private ExampleModel exampleModel = new ExampleModel();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

* 表單驗證中的使用者輸入`name`欄位使用中定義的驗證`ExampleModel`型別。 中的元件建立模型時會`@functions`封鎖並保存在私用欄位 (`exampleModel`)。 欄位指派給`Model`屬性的`<EditForm>`。
* 資料註解驗證器元件 (`<DataAnnotationsValidator>`) 附加驗證支援使用資料註解。
* 驗證摘要元件 (`<ValidationSummary>`) 摘要說明驗證訊息。
* `HandleValidSubmit` 將表單已成功提交 （通過驗證） 時，會觸發。

一組內建的輸入元件可用來接收並驗證使用者輸入。 他們正在變更時，並送出表單時，會驗證輸入。 下表顯示可用的輸入的元件。

| 輸入的元件   | 轉譯為&hellip;       |
| ----------------- | ------------------------- |
| `<InputText>`     | `<input>`                 |
| `<InputTextArea>` | `<textarea>`              |
| `<InputSelect>`   | `<select>`                |
| `<InputNumber>`   | `<input type="number">`   |
| `<InputCheckbox>` | `<input type="checkbox">` |
| `<InputDate>`     | `<input type="date">`     |

輸入的元件會提供 編輯驗證及變更欄位狀態會反映其 CSS 類別的預設行為。 某些元件會包含有用的剖析邏輯。 例如，`<InputDate>`和`<InputNumber>`註冊它們做為驗證錯誤的正常處理無法剖析值。 可接受 null 值的類型也支援 [目標] 欄位的 null 屬性 (例如`int?`)。

下列`Starship`型別定義的驗證邏輯使用較大的一組屬性和[資料註解](xref:mvc/models/validation)比舊版`ExampleModel`:

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    [Required]
    [StringLength(16, 
        ErrorMessage = "Identifier too long (16 character limit).")]
    public string Identifier { get; set; }

    // Optional (no data annotations)
    public string Description { get; set; }

    [Required]
    public string Classification { get; set; }

    [Range(1, 100000, ErrorMessage = "Accommodation invalid (1-100000).")]
    public int MaximumAccommodation { get; set; }

    [Required]
    [Range(typeof(bool), "true", "true", 
        ErrorMessage = "Form disallowed for unapproved ships.")]
    public bool IsValidatedDesign { get; set; }

    [Required]
    public DateTime ProductionDate { get; set; }
}
```

下列的表單驗證使用者輸入使用中定義的驗證`Starship`模型：

```cshtml
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@starship" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label for="identifier">Identifier: </label>
        <InputText id="identifier" bind-Value="@starship.Identifier" />
    </p>
    <p>
        <label for="description">Description (optional): </label>
        <InputTextArea Id="description" bind-Value="@starship.Description" />
    </p>
    <p>
        <label for="classification">Primary Classification: </label>
        <InputSelect id="classification" bind-Value="@starship.Classification">
            <option value"">Select classification ...</option>
            <option value="Defense">Defense</option>
            <option value="Exploration">Exploration</option>
            <option value="Diplomacy">Diplomacy</option>
        </InputSelect>
    </p>
    <p>
        <label for="accommodation">Maximum Accommodation: </label>
        <InputNumber id="accommodation" 
            bind-Value="@starship.MaximumAccommodation" />
    </p>
    <p>
        <label for="valid">Engineering Approval: </label>
        <InputCheckbox id="valid" bind-Value="@starship.IsValidatedDesign" />
    </p>
    <p>
        <label for="productionDate">Production Date: </label>
        <InputDate Id="productionDate" bind-Value="@starship.ProductionDate" />
    </p>

    <button type="submit">Submit</button>

    <p>
        <a href="http://www.startrek.com/">Star Trek</a>, 
        &copy;1966-2019 CBS Studios, Inc. and 
        <a href="http://www.paramount.com">Paramount Pictures</a>
    </p>
</EditForm>

@functions {
    private Starship starship = new Starship();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

`<EditForm>`會建立`EditContext`作為[串聯值](xref:razor-components/components#cascading-values-and-parameters)追蹤編輯程序，包括哪些欄位已修改的相關中繼資料和目前的驗證訊息。 `<EditForm>`也提供方便的事件的有效和無效提交 (`OnValidSubmit`， `OnInvalidSubmit`)。 或者，使用`OnSubmit`來觸發驗證，並檢查使用自訂驗證程式碼的欄位值。

資料註解驗證器元件 (`<DataAnnotationsValidator>`) 將驗證支援使用資料註解來串聯`EditContext`。 啟用支援目前使用資料註解的驗證需要這個明確的筆勢，但我們正在考慮進行此預設行為，您可以接著覆寫。 若要使用不同的驗證系統雖然比資料註解，請將資料註解驗證程式取代的自訂實作。 ASP.NET Core 實作會提供參考來源中的檢查：[DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs)。 *ASP.NET Core 實作受限於快速更新在預覽版本期間。*

驗證摘要元件 (`<ValidationSummary>`) 摘要說明所有的驗證訊息，類似於[驗證摘要標記協助程式](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)。

驗證訊息元件 (`<ValidationMessage>`) 會顯示特定欄位中，也就是類似的驗證訊息[驗證訊息標籤協助程式](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)。 指定的欄位具有之驗證`For`屬性及命名模型屬性的 lambda 運算式：

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

> [!NOTE]
> 輸入的內建元件有我們預計會解決在未來版本的限制。 例如，您無法指定任意屬性上產生`<input>`標記。 建置您自己的元件子類別，來處理無法使用的案例。
