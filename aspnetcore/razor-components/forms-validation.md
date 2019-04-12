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
# <a name="razor-components-forms-and-validation"></a><span data-ttu-id="3486e-103">Razor 元件表單和驗證</span><span class="sxs-lookup"><span data-stu-id="3486e-103">Razor Components forms and validation</span></span>

<span data-ttu-id="3486e-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3486e-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3486e-105">使用 Razor 元件支援的表單和驗證[資料註解](xref:mvc/models/validation)。</span><span class="sxs-lookup"><span data-stu-id="3486e-105">Forms and validation are supported in Razor Components using [data annotations](xref:mvc/models/validation).</span></span>

> [!NOTE]
> <span data-ttu-id="3486e-106">表單和驗證案例可能會變更每個預覽版本。</span><span class="sxs-lookup"><span data-stu-id="3486e-106">Forms and validation scenarios are likely to change with each preview release.</span></span>

<span data-ttu-id="3486e-107">下列`ExampleModel`類型會定義使用資料註解的驗證邏輯：</span><span class="sxs-lookup"><span data-stu-id="3486e-107">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="3486e-108">使用定義表單`<EditForm>`元件。</span><span class="sxs-lookup"><span data-stu-id="3486e-108">A form is defined using the `<EditForm>` component.</span></span> <span data-ttu-id="3486e-109">下列的表單會示範一般的項目、 元件和 Razor 程式碼：</span><span class="sxs-lookup"><span data-stu-id="3486e-109">The following form demonstrates typical elements, components, and Razor code:</span></span>

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

* <span data-ttu-id="3486e-110">表單驗證中的使用者輸入`name`欄位使用中定義的驗證`ExampleModel`型別。</span><span class="sxs-lookup"><span data-stu-id="3486e-110">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="3486e-111">中的元件建立模型時會`@functions`封鎖並保存在私用欄位 (`exampleModel`)。</span><span class="sxs-lookup"><span data-stu-id="3486e-111">The model is created in the component's `@functions` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="3486e-112">欄位指派給`Model`屬性的`<EditForm>`。</span><span class="sxs-lookup"><span data-stu-id="3486e-112">The field is assigned to the `Model` attribute of the `<EditForm>`.</span></span>
* <span data-ttu-id="3486e-113">資料註解驗證器元件 (`<DataAnnotationsValidator>`) 附加驗證支援使用資料註解。</span><span class="sxs-lookup"><span data-stu-id="3486e-113">The Data Annotations Validator component (`<DataAnnotationsValidator>`) attaches validation support using data annotations.</span></span>
* <span data-ttu-id="3486e-114">驗證摘要元件 (`<ValidationSummary>`) 摘要說明驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="3486e-114">The Validation Summary component (`<ValidationSummary>`) summarizes validation messages.</span></span>
* `HandleValidSubmit` <span data-ttu-id="3486e-115">將表單已成功提交 （通過驗證） 時，會觸發。</span><span class="sxs-lookup"><span data-stu-id="3486e-115">is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="3486e-116">一組內建的輸入元件可用來接收並驗證使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="3486e-116">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="3486e-117">他們正在變更時，並送出表單時，會驗證輸入。</span><span class="sxs-lookup"><span data-stu-id="3486e-117">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="3486e-118">下表顯示可用的輸入的元件。</span><span class="sxs-lookup"><span data-stu-id="3486e-118">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="3486e-119">輸入的元件</span><span class="sxs-lookup"><span data-stu-id="3486e-119">Input component</span></span>   | <span data-ttu-id="3486e-120">轉譯為&hellip;</span><span class="sxs-lookup"><span data-stu-id="3486e-120">Rendered as&hellip;</span></span>       |
| ----------------- | ------------------------- |
| `<InputText>`     | `<input>`                 |
| `<InputTextArea>` | `<textarea>`              |
| `<InputSelect>`   | `<select>`                |
| `<InputNumber>`   | `<input type="number">`   |
| `<InputCheckbox>` | `<input type="checkbox">` |
| `<InputDate>`     | `<input type="date">`     |

<span data-ttu-id="3486e-121">輸入的元件會提供 編輯驗證及變更欄位狀態會反映其 CSS 類別的預設行為。</span><span class="sxs-lookup"><span data-stu-id="3486e-121">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="3486e-122">某些元件會包含有用的剖析邏輯。</span><span class="sxs-lookup"><span data-stu-id="3486e-122">Some components include useful parsing logic.</span></span> <span data-ttu-id="3486e-123">例如，`<InputDate>`和`<InputNumber>`註冊它們做為驗證錯誤的正常處理無法剖析值。</span><span class="sxs-lookup"><span data-stu-id="3486e-123">For example, `<InputDate>` and `<InputNumber>` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="3486e-124">可接受 null 值的類型也支援 [目標] 欄位的 null 屬性 (例如`int?`)。</span><span class="sxs-lookup"><span data-stu-id="3486e-124">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="3486e-125">下列`Starship`型別定義的驗證邏輯使用較大的一組屬性和[資料註解](xref:mvc/models/validation)比舊版`ExampleModel`:</span><span class="sxs-lookup"><span data-stu-id="3486e-125">The following `Starship` type defines validation logic using a larger set of properties and [data annotations](xref:mvc/models/validation) than the earlier `ExampleModel`:</span></span>

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

<span data-ttu-id="3486e-126">下列的表單驗證使用者輸入使用中定義的驗證`Starship`模型：</span><span class="sxs-lookup"><span data-stu-id="3486e-126">The following form validates user input using the validation defined in the `Starship` model:</span></span>

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

<span data-ttu-id="3486e-127">`<EditForm>`會建立`EditContext`作為[串聯值](xref:razor-components/components#cascading-values-and-parameters)追蹤編輯程序，包括哪些欄位已修改的相關中繼資料和目前的驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="3486e-127">The `<EditForm>` creates an `EditContext` as a [cascading value](xref:razor-components/components#cascading-values-and-parameters) that tracks metadata about the edit process, including what fields have been modified and the current validation messages.</span></span> <span data-ttu-id="3486e-128">`<EditForm>`也提供方便的事件的有效和無效提交 (`OnValidSubmit`， `OnInvalidSubmit`)。</span><span class="sxs-lookup"><span data-stu-id="3486e-128">The `<EditForm>` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="3486e-129">或者，使用`OnSubmit`來觸發驗證，並檢查使用自訂驗證程式碼的欄位值。</span><span class="sxs-lookup"><span data-stu-id="3486e-129">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

<span data-ttu-id="3486e-130">資料註解驗證器元件 (`<DataAnnotationsValidator>`) 將驗證支援使用資料註解來串聯`EditContext`。</span><span class="sxs-lookup"><span data-stu-id="3486e-130">The Data Annotations Validator component (`<DataAnnotationsValidator>`) attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="3486e-131">啟用支援目前使用資料註解的驗證需要這個明確的筆勢，但我們正在考慮進行此預設行為，您可以接著覆寫。</span><span class="sxs-lookup"><span data-stu-id="3486e-131">Enabling support for validation using data annotations currently requires this explicit gesture, but we're considering making this the default behavior that you can then override.</span></span> <span data-ttu-id="3486e-132">若要使用不同的驗證系統雖然比資料註解，請將資料註解驗證程式取代的自訂實作。</span><span class="sxs-lookup"><span data-stu-id="3486e-132">To use a different validation system than data annotations, replace the Data Annotations Validator with a custom implementation.</span></span> <span data-ttu-id="3486e-133">ASP.NET Core 實作會提供參考來源中的檢查：[DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs)。</span><span class="sxs-lookup"><span data-stu-id="3486e-133">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs).</span></span> *<span data-ttu-id="3486e-134">ASP.NET Core 實作受限於快速更新在預覽版本期間。</span><span class="sxs-lookup"><span data-stu-id="3486e-134">The ASP.NET Core implementation is subject to rapid updates during the preview release period.</span></span>*

<span data-ttu-id="3486e-135">驗證摘要元件 (`<ValidationSummary>`) 摘要說明所有的驗證訊息，類似於[驗證摘要標記協助程式](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="3486e-135">The Validation Summary component (`<ValidationSummary>`) summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span></span>

<span data-ttu-id="3486e-136">驗證訊息元件 (`<ValidationMessage>`) 會顯示特定欄位中，也就是類似的驗證訊息[驗證訊息標籤協助程式](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="3486e-136">The Validation Message component (`<ValidationMessage>`) displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="3486e-137">指定的欄位具有之驗證`For`屬性及命名模型屬性的 lambda 運算式：</span><span class="sxs-lookup"><span data-stu-id="3486e-137">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

> [!NOTE]
> <span data-ttu-id="3486e-138">輸入的內建元件有我們預計會解決在未來版本的限制。</span><span class="sxs-lookup"><span data-stu-id="3486e-138">Built-in input components have limitations that we expect to resolve in future releases.</span></span> <span data-ttu-id="3486e-139">例如，您無法指定任意屬性上產生`<input>`標記。</span><span class="sxs-lookup"><span data-stu-id="3486e-139">For example, you can't specify arbitrary attributes on the generated `<input>` tags.</span></span> <span data-ttu-id="3486e-140">建置您自己的元件子類別，來處理無法使用的案例。</span><span class="sxs-lookup"><span data-stu-id="3486e-140">Build your own component subclasses to handle unavailable scenarios.</span></span>
