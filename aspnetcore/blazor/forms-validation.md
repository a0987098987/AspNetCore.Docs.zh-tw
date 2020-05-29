---
<span data-ttu-id="fdd02-101">標題： ' ASP.NET Core Blazor 表單和驗證 ' 作者：描述： ' 瞭解如何在中使用表單和欄位驗證案例 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="fdd02-101">title: 'ASP.NET Core Blazor forms and validation' author: description: 'Learn how to use forms and field validation scenarios in Blazor.'</span></span>
<span data-ttu-id="fdd02-102">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="fdd02-102">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="fdd02-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-103">'Blazor'</span></span>
- <span data-ttu-id="fdd02-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="fdd02-104">'Identity'</span></span>
- <span data-ttu-id="fdd02-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="fdd02-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="fdd02-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-106">'Razor'</span></span>
- <span data-ttu-id="fdd02-107">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="fdd02-107">'SignalR' uid:</span></span> 

---
# <a name="aspnet-core-blazor-forms-and-validation"></a><span data-ttu-id="fdd02-108">ASP.NET Core Blazor 表單和驗證</span><span class="sxs-lookup"><span data-stu-id="fdd02-108">ASP.NET Core Blazor forms and validation</span></span>

<span data-ttu-id="fdd02-109">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fdd02-109">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="fdd02-110">Blazor使用[資料批註](xref:mvc/models/validation)可支援表單和驗證。</span><span class="sxs-lookup"><span data-stu-id="fdd02-110">Forms and validation are supported in Blazor using [data annotations](xref:mvc/models/validation).</span></span>

<span data-ttu-id="fdd02-111">下列型別會 `ExampleModel` 使用資料批註來定義驗證邏輯：</span><span class="sxs-lookup"><span data-stu-id="fdd02-111">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="fdd02-112">表單是使用元件定義的 <xref:Microsoft.AspNetCore.Components.Forms.EditForm> 。</span><span class="sxs-lookup"><span data-stu-id="fdd02-112">A form is defined using the <xref:Microsoft.AspNetCore.Components.Forms.EditForm> component.</span></span> <span data-ttu-id="fdd02-113">下列表單會示範典型的元素、元件和程式 Razor 代碼：</span><span class="sxs-lookup"><span data-stu-id="fdd02-113">The following form demonstrates typical elements, components, and Razor code:</span></span>

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

<span data-ttu-id="fdd02-114">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="fdd02-114">In the preceding example:</span></span>

* <span data-ttu-id="fdd02-115">表單會使用在型別中 `name` 定義的驗證來驗證欄位中的使用者輸入 `ExampleModel` 。</span><span class="sxs-lookup"><span data-stu-id="fdd02-115">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="fdd02-116">模型會建立在元件的區塊中 `@code` ，並保留在私用欄位（ `exampleModel` ）中。</span><span class="sxs-lookup"><span data-stu-id="fdd02-116">The model is created in the component's `@code` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="fdd02-117">欄位會指派給元素的 `Model` 屬性 `<EditForm>` 。</span><span class="sxs-lookup"><span data-stu-id="fdd02-117">The field is assigned to the `Model` attribute of the `<EditForm>` element.</span></span>
* <span data-ttu-id="fdd02-118"><xref:Microsoft.AspNetCore.Components.Forms.InputText>元件的系結 `@bind-Value` ：</span><span class="sxs-lookup"><span data-stu-id="fdd02-118">The <xref:Microsoft.AspNetCore.Components.Forms.InputText> component's `@bind-Value` binds:</span></span>
  * <span data-ttu-id="fdd02-119">模型屬性（ `exampleModel.Name` ）至 <xref:Microsoft.AspNetCore.Components.Forms.InputText> 元件的 `Value` 屬性。</span><span class="sxs-lookup"><span data-stu-id="fdd02-119">The model property (`exampleModel.Name`) to the <xref:Microsoft.AspNetCore.Components.Forms.InputText> component's `Value` property.</span></span> <span data-ttu-id="fdd02-120">如需屬性系結的詳細資訊，請參閱 <xref:blazor/data-binding#parent-to-child-binding-with-component-parameters> 。</span><span class="sxs-lookup"><span data-stu-id="fdd02-120">For more information on property binding, see <xref:blazor/data-binding#parent-to-child-binding-with-component-parameters>.</span></span>
  * <span data-ttu-id="fdd02-121">元件的屬性的變更事件委派 <xref:Microsoft.AspNetCore.Components.Forms.InputText> `ValueChanged` 。</span><span class="sxs-lookup"><span data-stu-id="fdd02-121">A change event delegate to the <xref:Microsoft.AspNetCore.Components.Forms.InputText> component's `ValueChanged` property.</span></span>
* <span data-ttu-id="fdd02-122"><xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator>元件會使用資料批註附加驗證支援。</span><span class="sxs-lookup"><span data-stu-id="fdd02-122">The <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> component attaches validation support using data annotations.</span></span>
* <span data-ttu-id="fdd02-123"><xref:Microsoft.AspNetCore.Components.Forms.ValidationSummary>元件會摘要驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="fdd02-123">The <xref:Microsoft.AspNetCore.Components.Forms.ValidationSummary> component summarizes validation messages.</span></span>
* <span data-ttu-id="fdd02-124">`HandleValidSubmit`當表單成功提交（通過驗證）時，就會觸發。</span><span class="sxs-lookup"><span data-stu-id="fdd02-124">`HandleValidSubmit` is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="fdd02-125">有一組內建的輸入元件可用來接收和驗證使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="fdd02-125">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="fdd02-126">當輸入變更時和送出表單時，會進行驗證。</span><span class="sxs-lookup"><span data-stu-id="fdd02-126">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="fdd02-127">下表顯示可用的輸入元件。</span><span class="sxs-lookup"><span data-stu-id="fdd02-127">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="fdd02-128">輸入元件</span><span class="sxs-lookup"><span data-stu-id="fdd02-128">Input component</span></span> | <span data-ttu-id="fdd02-129">轉譯為&hellip;</span><span class="sxs-lookup"><span data-stu-id="fdd02-129">Rendered as&hellip;</span></span> |
| ---
<span data-ttu-id="fdd02-130">標題： ' ASP.NET Core Blazor 表單和驗證 ' 作者：描述： ' 瞭解如何在中使用表單和欄位驗證案例 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="fdd02-130">title: 'ASP.NET Core Blazor forms and validation' author: description: 'Learn how to use forms and field validation scenarios in Blazor.'</span></span>
<span data-ttu-id="fdd02-131">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="fdd02-131">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="fdd02-132">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-132">'Blazor'</span></span>
- <span data-ttu-id="fdd02-133">'Identity'</span><span class="sxs-lookup"><span data-stu-id="fdd02-133">'Identity'</span></span>
- <span data-ttu-id="fdd02-134">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="fdd02-134">'Let's Encrypt'</span></span>
- <span data-ttu-id="fdd02-135">'Razor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-135">'Razor'</span></span>
- <span data-ttu-id="fdd02-136">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="fdd02-136">'SignalR' uid:</span></span> 

-
<span data-ttu-id="fdd02-137">標題： ' ASP.NET Core Blazor 表單和驗證 ' 作者：描述： ' 瞭解如何在中使用表單和欄位驗證案例 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="fdd02-137">title: 'ASP.NET Core Blazor forms and validation' author: description: 'Learn how to use forms and field validation scenarios in Blazor.'</span></span>
<span data-ttu-id="fdd02-138">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="fdd02-138">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="fdd02-139">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-139">'Blazor'</span></span>
- <span data-ttu-id="fdd02-140">'Identity'</span><span class="sxs-lookup"><span data-stu-id="fdd02-140">'Identity'</span></span>
- <span data-ttu-id="fdd02-141">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="fdd02-141">'Let's Encrypt'</span></span>
- <span data-ttu-id="fdd02-142">'Razor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-142">'Razor'</span></span>
- <span data-ttu-id="fdd02-143">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="fdd02-143">'SignalR' uid:</span></span> 

-
<span data-ttu-id="fdd02-144">標題： ' ASP.NET Core Blazor 表單和驗證 ' 作者：描述： ' 瞭解如何在中使用表單和欄位驗證案例 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="fdd02-144">title: 'ASP.NET Core Blazor forms and validation' author: description: 'Learn how to use forms and field validation scenarios in Blazor.'</span></span>
<span data-ttu-id="fdd02-145">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="fdd02-145">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="fdd02-146">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-146">'Blazor'</span></span>
- <span data-ttu-id="fdd02-147">'Identity'</span><span class="sxs-lookup"><span data-stu-id="fdd02-147">'Identity'</span></span>
- <span data-ttu-id="fdd02-148">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="fdd02-148">'Let's Encrypt'</span></span>
- <span data-ttu-id="fdd02-149">'Razor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-149">'Razor'</span></span>
- <span data-ttu-id="fdd02-150">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="fdd02-150">'SignalR' uid:</span></span> 

-
<span data-ttu-id="fdd02-151">標題： ' ASP.NET Core Blazor 表單和驗證 ' 作者：描述： ' 瞭解如何在中使用表單和欄位驗證案例 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="fdd02-151">title: 'ASP.NET Core Blazor forms and validation' author: description: 'Learn how to use forms and field validation scenarios in Blazor.'</span></span>
<span data-ttu-id="fdd02-152">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="fdd02-152">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="fdd02-153">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-153">'Blazor'</span></span>
- <span data-ttu-id="fdd02-154">'Identity'</span><span class="sxs-lookup"><span data-stu-id="fdd02-154">'Identity'</span></span>
- <span data-ttu-id="fdd02-155">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="fdd02-155">'Let's Encrypt'</span></span>
- <span data-ttu-id="fdd02-156">'Razor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-156">'Razor'</span></span>
- <span data-ttu-id="fdd02-157">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="fdd02-157">'SignalR' uid:</span></span> 

-
<span data-ttu-id="fdd02-158">標題： ' ASP.NET Core Blazor 表單和驗證 ' 作者：描述： ' 瞭解如何在中使用表單和欄位驗證案例 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="fdd02-158">title: 'ASP.NET Core Blazor forms and validation' author: description: 'Learn how to use forms and field validation scenarios in Blazor.'</span></span>
<span data-ttu-id="fdd02-159">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="fdd02-159">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="fdd02-160">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-160">'Blazor'</span></span>
- <span data-ttu-id="fdd02-161">'Identity'</span><span class="sxs-lookup"><span data-stu-id="fdd02-161">'Identity'</span></span>
- <span data-ttu-id="fdd02-162">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="fdd02-162">'Let's Encrypt'</span></span>
- <span data-ttu-id="fdd02-163">'Razor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-163">'Razor'</span></span>
- <span data-ttu-id="fdd02-164">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="fdd02-164">'SignalR' uid:</span></span> 

<span data-ttu-id="fdd02-165">-------- |---標題： ' ASP.NET Core Blazor 表單和驗證 ' 作者：描述： ' 瞭解如何在中使用表單和欄位驗證案例 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="fdd02-165">-------- | --- title: 'ASP.NET Core Blazor forms and validation' author: description: 'Learn how to use forms and field validation scenarios in Blazor.'</span></span>
<span data-ttu-id="fdd02-166">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="fdd02-166">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="fdd02-167">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-167">'Blazor'</span></span>
- <span data-ttu-id="fdd02-168">'Identity'</span><span class="sxs-lookup"><span data-stu-id="fdd02-168">'Identity'</span></span>
- <span data-ttu-id="fdd02-169">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="fdd02-169">'Let's Encrypt'</span></span>
- <span data-ttu-id="fdd02-170">'Razor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-170">'Razor'</span></span>
- <span data-ttu-id="fdd02-171">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="fdd02-171">'SignalR' uid:</span></span> 

-
<span data-ttu-id="fdd02-172">標題： ' ASP.NET Core Blazor 表單和驗證 ' 作者：描述： ' 瞭解如何在中使用表單和欄位驗證案例 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="fdd02-172">title: 'ASP.NET Core Blazor forms and validation' author: description: 'Learn how to use forms and field validation scenarios in Blazor.'</span></span>
<span data-ttu-id="fdd02-173">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="fdd02-173">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="fdd02-174">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-174">'Blazor'</span></span>
- <span data-ttu-id="fdd02-175">'Identity'</span><span class="sxs-lookup"><span data-stu-id="fdd02-175">'Identity'</span></span>
- <span data-ttu-id="fdd02-176">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="fdd02-176">'Let's Encrypt'</span></span>
- <span data-ttu-id="fdd02-177">'Razor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-177">'Razor'</span></span>
- <span data-ttu-id="fdd02-178">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="fdd02-178">'SignalR' uid:</span></span> 

-
<span data-ttu-id="fdd02-179">標題： ' ASP.NET Core Blazor 表單和驗證 ' 作者：描述： ' 瞭解如何在中使用表單和欄位驗證案例 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="fdd02-179">title: 'ASP.NET Core Blazor forms and validation' author: description: 'Learn how to use forms and field validation scenarios in Blazor.'</span></span>
<span data-ttu-id="fdd02-180">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="fdd02-180">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="fdd02-181">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-181">'Blazor'</span></span>
- <span data-ttu-id="fdd02-182">'Identity'</span><span class="sxs-lookup"><span data-stu-id="fdd02-182">'Identity'</span></span>
- <span data-ttu-id="fdd02-183">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="fdd02-183">'Let's Encrypt'</span></span>
- <span data-ttu-id="fdd02-184">'Razor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-184">'Razor'</span></span>
- <span data-ttu-id="fdd02-185">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="fdd02-185">'SignalR' uid:</span></span> 

-
<span data-ttu-id="fdd02-186">標題： ' ASP.NET Core Blazor 表單和驗證 ' 作者：描述： ' 瞭解如何在中使用表單和欄位驗證案例 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="fdd02-186">title: 'ASP.NET Core Blazor forms and validation' author: description: 'Learn how to use forms and field validation scenarios in Blazor.'</span></span>
<span data-ttu-id="fdd02-187">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="fdd02-187">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="fdd02-188">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-188">'Blazor'</span></span>
- <span data-ttu-id="fdd02-189">'Identity'</span><span class="sxs-lookup"><span data-stu-id="fdd02-189">'Identity'</span></span>
- <span data-ttu-id="fdd02-190">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="fdd02-190">'Let's Encrypt'</span></span>
- <span data-ttu-id="fdd02-191">'Razor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-191">'Razor'</span></span>
- <span data-ttu-id="fdd02-192">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="fdd02-192">'SignalR' uid:</span></span> 

-
<span data-ttu-id="fdd02-193">標題： ' ASP.NET Core Blazor 表單和驗證 ' 作者：描述： ' 瞭解如何在中使用表單和欄位驗證案例 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="fdd02-193">title: 'ASP.NET Core Blazor forms and validation' author: description: 'Learn how to use forms and field validation scenarios in Blazor.'</span></span>
<span data-ttu-id="fdd02-194">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="fdd02-194">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="fdd02-195">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-195">'Blazor'</span></span>
- <span data-ttu-id="fdd02-196">'Identity'</span><span class="sxs-lookup"><span data-stu-id="fdd02-196">'Identity'</span></span>
- <span data-ttu-id="fdd02-197">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="fdd02-197">'Let's Encrypt'</span></span>
- <span data-ttu-id="fdd02-198">'Razor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-198">'Razor'</span></span>
- <span data-ttu-id="fdd02-199">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="fdd02-199">'SignalR' uid:</span></span> 

-
<span data-ttu-id="fdd02-200">標題： ' ASP.NET Core Blazor 表單和驗證 ' 作者：描述： ' 瞭解如何在中使用表單和欄位驗證案例 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="fdd02-200">title: 'ASP.NET Core Blazor forms and validation' author: description: 'Learn how to use forms and field validation scenarios in Blazor.'</span></span>
<span data-ttu-id="fdd02-201">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="fdd02-201">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="fdd02-202">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-202">'Blazor'</span></span>
- <span data-ttu-id="fdd02-203">'Identity'</span><span class="sxs-lookup"><span data-stu-id="fdd02-203">'Identity'</span></span>
- <span data-ttu-id="fdd02-204">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="fdd02-204">'Let's Encrypt'</span></span>
- <span data-ttu-id="fdd02-205">'Razor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-205">'Razor'</span></span>
- <span data-ttu-id="fdd02-206">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="fdd02-206">'SignalR' uid:</span></span> 

-
<span data-ttu-id="fdd02-207">標題： ' ASP.NET Core Blazor 表單和驗證 ' 作者：描述： ' 瞭解如何在中使用表單和欄位驗證案例 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="fdd02-207">title: 'ASP.NET Core Blazor forms and validation' author: description: 'Learn how to use forms and field validation scenarios in Blazor.'</span></span>
<span data-ttu-id="fdd02-208">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="fdd02-208">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="fdd02-209">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-209">'Blazor'</span></span>
- <span data-ttu-id="fdd02-210">'Identity'</span><span class="sxs-lookup"><span data-stu-id="fdd02-210">'Identity'</span></span>
- <span data-ttu-id="fdd02-211">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="fdd02-211">'Let's Encrypt'</span></span>
- <span data-ttu-id="fdd02-212">'Razor'</span><span class="sxs-lookup"><span data-stu-id="fdd02-212">'Razor'</span></span>
- <span data-ttu-id="fdd02-213">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="fdd02-213">'SignalR' uid:</span></span> 

<span data-ttu-id="fdd02-214">---------- | | <xref:Microsoft.AspNetCore.Components.Forms.InputText> | `<input>` | | <xref:Microsoft.AspNetCore.Components.Forms.InputTextArea> | `<textarea>` | | <xref:Microsoft.AspNetCore.Components.Forms.InputSelect%601> | `<select>` | | <xref:Microsoft.AspNetCore.Components.Forms.InputNumber%601> | `<input type="number">` | | <xref:Microsoft.AspNetCore.Components.Forms.InputCheckbox> | `<input type="checkbox">` | | <xref:Microsoft.AspNetCore.Components.Forms.InputDate%601> | `<input type="date">` |</span><span class="sxs-lookup"><span data-stu-id="fdd02-214">---------- | | <xref:Microsoft.AspNetCore.Components.Forms.InputText> | `<input>` | | <xref:Microsoft.AspNetCore.Components.Forms.InputTextArea> | `<textarea>` | | <xref:Microsoft.AspNetCore.Components.Forms.InputSelect%601> | `<select>` | | <xref:Microsoft.AspNetCore.Components.Forms.InputNumber%601> | `<input type="number">` | | <xref:Microsoft.AspNetCore.Components.Forms.InputCheckbox> | `<input type="checkbox">` | | <xref:Microsoft.AspNetCore.Components.Forms.InputDate%601> | `<input type="date">` |</span></span>

<span data-ttu-id="fdd02-215">所有的輸入元件（包括 <xref:Microsoft.AspNetCore.Components.Forms.EditForm> ）都支援任意屬性。</span><span class="sxs-lookup"><span data-stu-id="fdd02-215">All of the input components, including <xref:Microsoft.AspNetCore.Components.Forms.EditForm>, support arbitrary attributes.</span></span> <span data-ttu-id="fdd02-216">任何不符合元件參數的屬性都會加入至轉譯的 HTML 專案。</span><span class="sxs-lookup"><span data-stu-id="fdd02-216">Any attribute that doesn't match a component parameter is added to the rendered HTML element.</span></span>

<span data-ttu-id="fdd02-217">輸入元件會提供預設行為，以便在編輯和變更其 CSS 類別時進行驗證，以反映欄位狀態。</span><span class="sxs-lookup"><span data-stu-id="fdd02-217">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="fdd02-218">某些元件包含有用的剖析邏輯。</span><span class="sxs-lookup"><span data-stu-id="fdd02-218">Some components include useful parsing logic.</span></span> <span data-ttu-id="fdd02-219">例如，和會藉由將無法剖析 <xref:Microsoft.AspNetCore.Components.Forms.InputDate%601> <xref:Microsoft.AspNetCore.Components.Forms.InputNumber%601> 的值註冊為驗證錯誤來妥善處理。</span><span class="sxs-lookup"><span data-stu-id="fdd02-219">For example, <xref:Microsoft.AspNetCore.Components.Forms.InputDate%601> and <xref:Microsoft.AspNetCore.Components.Forms.InputNumber%601> handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="fdd02-220">可以接受 null 值的類型也支援目標欄位的 null 屬性（例如 `int?` ）。</span><span class="sxs-lookup"><span data-stu-id="fdd02-220">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="fdd02-221">下列 `Starship` 型別使用較早的屬性集和資料注釋，來定義驗證邏輯 `ExampleModel` ：</span><span class="sxs-lookup"><span data-stu-id="fdd02-221">The following `Starship` type defines validation logic using a larger set of properties and data annotations than the earlier `ExampleModel`:</span></span>

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

<span data-ttu-id="fdd02-222">在上述範例中， `Description` 是選擇性的，因為沒有任何資料批註存在。</span><span class="sxs-lookup"><span data-stu-id="fdd02-222">In the preceding example, `Description` is optional because no data annotations are present.</span></span>

<span data-ttu-id="fdd02-223">下列表單會使用模型中定義的驗證來驗證使用者輸入 `Starship` ：</span><span class="sxs-lookup"><span data-stu-id="fdd02-223">The following form validates user input using the validation defined in the `Starship` model:</span></span>

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

<span data-ttu-id="fdd02-224">會 <xref:Microsoft.AspNetCore.Components.Forms.EditForm> 建立 <xref:Microsoft.AspNetCore.Components.Forms.EditContext> 做為階層式[值](xref:blazor/components#cascading-values-and-parameters)，以追蹤編輯程式的相關中繼資料，包括已修改的欄位和目前的驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="fdd02-224">The <xref:Microsoft.AspNetCore.Components.Forms.EditForm> creates an <xref:Microsoft.AspNetCore.Components.Forms.EditContext> as a [cascading value](xref:blazor/components#cascading-values-and-parameters) that tracks metadata about the edit process, including which fields have been modified and the current validation messages.</span></span> <span data-ttu-id="fdd02-225"><xref:Microsoft.AspNetCore.Components.Forms.EditForm>也提供有效和無效提交的便利事件（ <xref:Microsoft.AspNetCore.Components.Forms.EditForm.OnValidSubmit> 、 <xref:Microsoft.AspNetCore.Components.Forms.EditForm.OnInvalidSubmit> ）。</span><span class="sxs-lookup"><span data-stu-id="fdd02-225">The <xref:Microsoft.AspNetCore.Components.Forms.EditForm> also provides convenient events for valid and invalid submits (<xref:Microsoft.AspNetCore.Components.Forms.EditForm.OnValidSubmit>, <xref:Microsoft.AspNetCore.Components.Forms.EditForm.OnInvalidSubmit>).</span></span> <span data-ttu-id="fdd02-226">或者，使用 <xref:Microsoft.AspNetCore.Components.Forms.EditForm.OnSubmit> 來觸發驗證，並使用自訂驗證程式代碼來檢查域值。</span><span class="sxs-lookup"><span data-stu-id="fdd02-226">Alternatively, use <xref:Microsoft.AspNetCore.Components.Forms.EditForm.OnSubmit> to trigger the validation and check field values with custom validation code.</span></span>

<span data-ttu-id="fdd02-227">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="fdd02-227">In the following example:</span></span>

* <span data-ttu-id="fdd02-228">`HandleSubmit`當選取 [**提交**] 按鈕時，就會執行方法。</span><span class="sxs-lookup"><span data-stu-id="fdd02-228">The `HandleSubmit` method runs when the **Submit** button is selected.</span></span>
* <span data-ttu-id="fdd02-229">表單會使用表單的進行驗證 <xref:Microsoft.AspNetCore.Components.Forms.EditContext> 。</span><span class="sxs-lookup"><span data-stu-id="fdd02-229">The form is validated using the form's <xref:Microsoft.AspNetCore.Components.Forms.EditContext>.</span></span>
* <span data-ttu-id="fdd02-230">藉由將傳遞 <xref:Microsoft.AspNetCore.Components.Forms.EditContext> 至在 `ServerValidate` 伺服器上呼叫 Web API 端點的方法（*未顯示*），進一步驗證表單。</span><span class="sxs-lookup"><span data-stu-id="fdd02-230">The form is further validated by passing the <xref:Microsoft.AspNetCore.Components.Forms.EditContext> to the `ServerValidate` method that calls a web API endpoint on the server (*not shown*).</span></span>
* <span data-ttu-id="fdd02-231">額外的程式碼會根據用戶端和伺服器端驗證的結果來執行（藉由檢查） `isValid` 。</span><span class="sxs-lookup"><span data-stu-id="fdd02-231">Additional code is run depending on the result of the client- and server-side validation by checking `isValid`.</span></span>

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

## <a name="inputtext-based-on-the-input-event"></a><span data-ttu-id="fdd02-232">根據輸入事件的 InputText</span><span class="sxs-lookup"><span data-stu-id="fdd02-232">InputText based on the input event</span></span>

<span data-ttu-id="fdd02-233">使用 <xref:Microsoft.AspNetCore.Components.Forms.InputText> 元件來建立使用事件的自訂群組件， `input` 而不是 `change` 事件。</span><span class="sxs-lookup"><span data-stu-id="fdd02-233">Use the <xref:Microsoft.AspNetCore.Components.Forms.InputText> component to create a custom component that uses the `input` event instead of the `change` event.</span></span>

<span data-ttu-id="fdd02-234">建立具有下列標記的元件，並使用如同使用的元件 <xref:Microsoft.AspNetCore.Components.Forms.InputText> ：</span><span class="sxs-lookup"><span data-stu-id="fdd02-234">Create a component with the following markup, and use the component just as <xref:Microsoft.AspNetCore.Components.Forms.InputText> is used:</span></span>

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="work-with-radio-buttons"></a><span data-ttu-id="fdd02-235">使用選項按鈕</span><span class="sxs-lookup"><span data-stu-id="fdd02-235">Work with radio buttons</span></span>

<span data-ttu-id="fdd02-236">使用表單中的選項按鈕時，資料系結的處理方式不同于其他元素，因為選項按鈕會評估為群組。</span><span class="sxs-lookup"><span data-stu-id="fdd02-236">When working with radio buttons in a form, data binding is handled differently than other elements because radio buttons are evaluated as a group.</span></span> <span data-ttu-id="fdd02-237">每個選項按鈕的值都是固定的，但是選項按鈕群組的值就是選取之選項按鈕的值。</span><span class="sxs-lookup"><span data-stu-id="fdd02-237">The value of each radio button is fixed, but the value of the radio button group is the value of the selected radio button.</span></span> <span data-ttu-id="fdd02-238">下列範例示範如何執行：</span><span class="sxs-lookup"><span data-stu-id="fdd02-238">The following example shows how to:</span></span>

* <span data-ttu-id="fdd02-239">處理選項按鈕群組的資料系結。</span><span class="sxs-lookup"><span data-stu-id="fdd02-239">Handle data binding for a radio button group.</span></span>
* <span data-ttu-id="fdd02-240">支援使用自訂群組件的驗證 `InputRadio` 。</span><span class="sxs-lookup"><span data-stu-id="fdd02-240">Support validation using a custom `InputRadio` component.</span></span>

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

<span data-ttu-id="fdd02-241">下列 <xref:Microsoft.AspNetCore.Components.Forms.EditForm> 使用上述 `InputRadio` 元件來取得並驗證使用者的評等：</span><span class="sxs-lookup"><span data-stu-id="fdd02-241">The following <xref:Microsoft.AspNetCore.Components.Forms.EditForm> uses the preceding `InputRadio` component to obtain and validate a rating from the user:</span></span>

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

## <a name="validation-support"></a><span data-ttu-id="fdd02-242">驗證支援</span><span class="sxs-lookup"><span data-stu-id="fdd02-242">Validation support</span></span>

<span data-ttu-id="fdd02-243"><xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator>元件會使用資料批註，將驗證支援附加至串聯的 <xref:Microsoft.AspNetCore.Components.Forms.EditContext> 。</span><span class="sxs-lookup"><span data-stu-id="fdd02-243">The <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> component attaches validation support using data annotations to the cascaded <xref:Microsoft.AspNetCore.Components.Forms.EditContext>.</span></span> <span data-ttu-id="fdd02-244">若要啟用使用資料批註進行驗證的支援，則需要這個明確的手勢。</span><span class="sxs-lookup"><span data-stu-id="fdd02-244">Enabling support for validation using data annotations requires this explicit gesture.</span></span> <span data-ttu-id="fdd02-245">若要使用不同于資料批註的驗證系統，請將取代為 <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> 自訂的執行。</span><span class="sxs-lookup"><span data-stu-id="fdd02-245">To use a different validation system than data annotations, replace the <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> with a custom implementation.</span></span> <span data-ttu-id="fdd02-246">ASP.NET Core 的執行可用於參考來源中的檢查： [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs) / [AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs)。</span><span class="sxs-lookup"><span data-stu-id="fdd02-246">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span></span>

Blazor<span data-ttu-id="fdd02-247">會執行兩種類型的驗證：</span><span class="sxs-lookup"><span data-stu-id="fdd02-247"> performs two types of validation:</span></span>

* <span data-ttu-id="fdd02-248">*欄位驗證*是在使用者跳到欄位外時執行。</span><span class="sxs-lookup"><span data-stu-id="fdd02-248">*Field validation* is performed when the user tabs out of a field.</span></span> <span data-ttu-id="fdd02-249">在欄位驗證期間， <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> 元件會將所有報告的驗證結果與欄位產生關聯。</span><span class="sxs-lookup"><span data-stu-id="fdd02-249">During field validation, the <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> component associates all reported validation results with the field.</span></span>
* <span data-ttu-id="fdd02-250">當使用者提交表單時，就會執行*模型驗證*。</span><span class="sxs-lookup"><span data-stu-id="fdd02-250">*Model validation* is performed when the user submits the form.</span></span> <span data-ttu-id="fdd02-251">在模型驗證期間， <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> 元件會嘗試根據驗證結果所報告的成員名稱來決定欄位。</span><span class="sxs-lookup"><span data-stu-id="fdd02-251">During model validation, the <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> component attempts to determine the field based on the member name that the validation result reports.</span></span> <span data-ttu-id="fdd02-252">未與個別成員相關聯的驗證結果會與模型相關聯，而不是欄位。</span><span class="sxs-lookup"><span data-stu-id="fdd02-252">Validation results that aren't associated with an individual member are associated with the model rather than a field.</span></span>

### <a name="validation-summary-and-validation-message-components"></a><span data-ttu-id="fdd02-253">驗證摘要和驗證訊息元件</span><span class="sxs-lookup"><span data-stu-id="fdd02-253">Validation Summary and Validation Message components</span></span>

<span data-ttu-id="fdd02-254">此 <xref:Microsoft.AspNetCore.Components.Forms.ValidationSummary> 元件會摘要所有驗證訊息，類似于[驗證摘要](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)標籤協助程式：</span><span class="sxs-lookup"><span data-stu-id="fdd02-254">The <xref:Microsoft.AspNetCore.Components.Forms.ValidationSummary> component summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):</span></span>

```razor
<ValidationSummary />
```

<span data-ttu-id="fdd02-255">使用參數輸出特定模型的驗證訊息 `Model` ：</span><span class="sxs-lookup"><span data-stu-id="fdd02-255">Output validation messages for a specific model with the `Model` parameter:</span></span>
  
```razor
<ValidationSummary Model="@starship" />
```

<span data-ttu-id="fdd02-256"><xref:Microsoft.AspNetCore.Components.Forms.ValidationMessage%601>元件會顯示特定欄位的驗證訊息，類似于[驗證訊息](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="fdd02-256">The <xref:Microsoft.AspNetCore.Components.Forms.ValidationMessage%601> component displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="fdd02-257">使用屬性指定驗證欄位 `For` ，並以 lambda 運算式命名模型屬性：</span><span class="sxs-lookup"><span data-stu-id="fdd02-257">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```razor
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

<span data-ttu-id="fdd02-258"><xref:Microsoft.AspNetCore.Components.Forms.ValidationMessage%601>和 <xref:Microsoft.AspNetCore.Components.Forms.ValidationSummary> 元件支援任意屬性。</span><span class="sxs-lookup"><span data-stu-id="fdd02-258">The <xref:Microsoft.AspNetCore.Components.Forms.ValidationMessage%601> and <xref:Microsoft.AspNetCore.Components.Forms.ValidationSummary> components support arbitrary attributes.</span></span> <span data-ttu-id="fdd02-259">任何不符合元件參數的屬性都會加入至產生的 `<div>` 或 `<ul>` 元素。</span><span class="sxs-lookup"><span data-stu-id="fdd02-259">Any attribute that doesn't match a component parameter is added to the generated `<div>` or `<ul>` element.</span></span>

### <a name="custom-validation-attributes"></a><span data-ttu-id="fdd02-260">自訂驗證屬性</span><span class="sxs-lookup"><span data-stu-id="fdd02-260">Custom validation attributes</span></span>

<span data-ttu-id="fdd02-261">若要在使用[自訂驗證屬性](xref:mvc/models/validation#custom-attributes)時，確保驗證結果與欄位正確相關聯，請 <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> 在建立時傳遞驗證內容 <xref:System.ComponentModel.DataAnnotations.ValidationResult> ：</span><span class="sxs-lookup"><span data-stu-id="fdd02-261">To ensure that a validation result is correctly associated with a field when using a [custom validation attribute](xref:mvc/models/validation#custom-attributes), pass the validation context's <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> when creating the <xref:System.ComponentModel.DataAnnotations.ValidationResult>:</span></span>

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

### <a name="blazor-data-annotations-validation-package"></a>Blazor<span data-ttu-id="fdd02-262">資料批註驗證封裝</span><span class="sxs-lookup"><span data-stu-id="fdd02-262"> data annotations validation package</span></span>

<span data-ttu-id="fdd02-263">[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation)是使用元件來填滿驗證體驗缺口的套件 <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> 。</span><span class="sxs-lookup"><span data-stu-id="fdd02-263">The [Microsoft.AspNetCore.Components.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation) is a package that fills validation experience gaps using the <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> component.</span></span> <span data-ttu-id="fdd02-264">封裝目前為*實驗*性。</span><span class="sxs-lookup"><span data-stu-id="fdd02-264">The package is currently *experimental*.</span></span>

### <a name="compareproperty-attribute"></a><span data-ttu-id="fdd02-265">[CompareProperty] 屬性</span><span class="sxs-lookup"><span data-stu-id="fdd02-265">[CompareProperty] attribute</span></span>

<span data-ttu-id="fdd02-266"><xref:System.ComponentModel.DataAnnotations.CompareAttribute>無法與元件搭配運作， <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> 因為它不會將驗證結果與特定成員產生關聯。</span><span class="sxs-lookup"><span data-stu-id="fdd02-266">The <xref:System.ComponentModel.DataAnnotations.CompareAttribute> doesn't work well with the <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> component because it doesn't associate the validation result with a specific member.</span></span> <span data-ttu-id="fdd02-267">這可能會導致欄位層級驗證與整個模型在提交時進行驗證時的行為不一致。</span><span class="sxs-lookup"><span data-stu-id="fdd02-267">This can result in inconsistent behavior between field-level validation and when the entire model is validated on a submit.</span></span> <span data-ttu-id="fdd02-268">[DataAnnotations](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation) *實驗*性封裝引進了額外的驗證屬性，其 `ComparePropertyAttribute` 運作方式會因應這些限制。</span><span class="sxs-lookup"><span data-stu-id="fdd02-268">The [Microsoft.AspNetCore.Components.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation) *experimental* package introduces an additional validation attribute, `ComparePropertyAttribute`, that works around these limitations.</span></span> <span data-ttu-id="fdd02-269">在 Blazor 應用程式中， `[CompareProperty]` 是屬性的直接取代 [`[Compare]`](xref:System.ComponentModel.DataAnnotations.CompareAttribute) 。</span><span class="sxs-lookup"><span data-stu-id="fdd02-269">In a Blazor app, `[CompareProperty]` is a direct replacement for the [`[Compare]`](xref:System.ComponentModel.DataAnnotations.CompareAttribute) attribute.</span></span>

### <a name="nested-models-collection-types-and-complex-types"></a><span data-ttu-id="fdd02-270">嵌套模型、集合類型和複雜類型</span><span class="sxs-lookup"><span data-stu-id="fdd02-270">Nested models, collection types, and complex types</span></span>

Blazor<span data-ttu-id="fdd02-271">使用內建的資料批註，提供驗證表單輸入的支援 <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> 。</span><span class="sxs-lookup"><span data-stu-id="fdd02-271"> provides support for validating form input using data annotations with the built-in <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator>.</span></span> <span data-ttu-id="fdd02-272">不過，只會驗證系結 <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> 至不是集合或複雜型別屬性之表單之模型的最上層屬性。</span><span class="sxs-lookup"><span data-stu-id="fdd02-272">However, the <xref:Microsoft.AspNetCore.Components.Forms.DataAnnotationsValidator> only validates top-level properties of the model bound to the form that aren't collection- or complex-type properties.</span></span>

<span data-ttu-id="fdd02-273">若要驗證系結模型的整個物件圖形（包括集合型和複雜型別屬性），請使用實驗性的 DataAnnotations 所提供的。 `ObjectGraphDataAnnotationsValidator` *experimental* [驗證](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation)套件：</span><span class="sxs-lookup"><span data-stu-id="fdd02-273">To validate the bound model's entire object graph, including collection- and complex-type properties, use the `ObjectGraphDataAnnotationsValidator` provided by the *experimental* [Microsoft.AspNetCore.Components.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.DataAnnotations.Validation) package:</span></span>

```razor
<EditForm Model="@model" OnValidSubmit="HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

<span data-ttu-id="fdd02-274">使用標注模型屬性 `[ValidateComplexType]` 。</span><span class="sxs-lookup"><span data-stu-id="fdd02-274">Annotate model properties with `[ValidateComplexType]`.</span></span> <span data-ttu-id="fdd02-275">在下列模型類別中， `ShipDescription` 類別會包含其他資料批註，以在模型系結至表單時進行驗證：</span><span class="sxs-lookup"><span data-stu-id="fdd02-275">In the following model classes, the `ShipDescription` class contains additional data annotations to validate when the model is bound to the form:</span></span>

<span data-ttu-id="fdd02-276">*Starship.cs*：</span><span class="sxs-lookup"><span data-stu-id="fdd02-276">*Starship.cs*:</span></span>

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

<span data-ttu-id="fdd02-277">*ShipDescription.cs*：</span><span class="sxs-lookup"><span data-stu-id="fdd02-277">*ShipDescription.cs*:</span></span>

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

### <a name="enable-the-submit-button-based-on-form-validation"></a><span data-ttu-id="fdd02-278">根據表單驗證啟用 [提交] 按鈕</span><span class="sxs-lookup"><span data-stu-id="fdd02-278">Enable the submit button based on form validation</span></span>

<span data-ttu-id="fdd02-279">若要根據表單驗證啟用和停用 [提交] 按鈕：</span><span class="sxs-lookup"><span data-stu-id="fdd02-279">To enable and disable the submit button based on form validation:</span></span>

* <span data-ttu-id="fdd02-280">當元件初始化時，請使用表單的 <xref:Microsoft.AspNetCore.Components.Forms.EditContext> 來指派模型。</span><span class="sxs-lookup"><span data-stu-id="fdd02-280">Use the form's <xref:Microsoft.AspNetCore.Components.Forms.EditContext> to assign the model when the component is initialized.</span></span>
* <span data-ttu-id="fdd02-281">在內容的回呼中驗證表單 <xref:Microsoft.AspNetCore.Components.Forms.EditContext.OnFieldChanged> ，以啟用和停用 [提交] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="fdd02-281">Validate the form in the context's <xref:Microsoft.AspNetCore.Components.Forms.EditContext.OnFieldChanged> callback to enable and disable the submit button.</span></span>
* <span data-ttu-id="fdd02-282">解除掛接方法中的事件處理常式 `Dispose` 。</span><span class="sxs-lookup"><span data-stu-id="fdd02-282">Unhook the event handler in the `Dispose` method.</span></span> <span data-ttu-id="fdd02-283">如需詳細資訊，請參閱<xref:blazor/lifecycle#component-disposal-with-idisposable>。</span><span class="sxs-lookup"><span data-stu-id="fdd02-283">For more information, see <xref:blazor/lifecycle#component-disposal-with-idisposable>.</span></span>

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

<span data-ttu-id="fdd02-284">在上述範例中，將設定 `formInvalid` 為（ `false` 如果：</span><span class="sxs-lookup"><span data-stu-id="fdd02-284">In the preceding example, set `formInvalid` to `false` if:</span></span>

* <span data-ttu-id="fdd02-285">表單會預先載入有效的預設值。</span><span class="sxs-lookup"><span data-stu-id="fdd02-285">The form is preloaded with valid default values.</span></span>
* <span data-ttu-id="fdd02-286">當表單載入時，您會想要啟用 [提交] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="fdd02-286">You want the submit button enabled when the form loads.</span></span>

<span data-ttu-id="fdd02-287">上述方法的副作用在於，在 <xref:Microsoft.AspNetCore.Components.Forms.ValidationSummary> 使用者與任何一個欄位互動之後，元件會填入不正確欄位。</span><span class="sxs-lookup"><span data-stu-id="fdd02-287">A side effect of the preceding approach is that a <xref:Microsoft.AspNetCore.Components.Forms.ValidationSummary> component is populated with invalid fields after the user interacts with any one field.</span></span> <span data-ttu-id="fdd02-288">此案例可透過下列其中一種方式來解決：</span><span class="sxs-lookup"><span data-stu-id="fdd02-288">This scenario can be addressed in either of the following ways:</span></span>

* <span data-ttu-id="fdd02-289">請勿使用 <xref:Microsoft.AspNetCore.Components.Forms.ValidationSummary> 表單上的元件。</span><span class="sxs-lookup"><span data-stu-id="fdd02-289">Don't use a <xref:Microsoft.AspNetCore.Components.Forms.ValidationSummary> component on the form.</span></span>
* <span data-ttu-id="fdd02-290"><xref:Microsoft.AspNetCore.Components.Forms.ValidationSummary>選取 [提交] 按鈕時，讓元件顯示（例如，在方法中 `HandleValidSubmit` ）。</span><span class="sxs-lookup"><span data-stu-id="fdd02-290">Make the <xref:Microsoft.AspNetCore.Components.Forms.ValidationSummary> component visible when the submit button is selected (for example, in a `HandleValidSubmit` method).</span></span>

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

## <a name="troubleshoot"></a><span data-ttu-id="fdd02-291">疑難排解</span><span class="sxs-lookup"><span data-stu-id="fdd02-291">Troubleshoot</span></span>

> <span data-ttu-id="fdd02-292">InvalidOperationException： EditForm 需要模型參數或 EditCoNtext 參數，但不能同時使用兩者。</span><span class="sxs-lookup"><span data-stu-id="fdd02-292">InvalidOperationException: EditForm requires a Model parameter, or an EditContext parameter, but not both.</span></span>

<span data-ttu-id="fdd02-293">確認 <xref:Microsoft.AspNetCore.Components.Forms.EditForm> 具有 <xref:Microsoft.AspNetCore.Components.Forms.EditForm.Model> 或 <xref:Microsoft.AspNetCore.Components.Forms.EditContext> 。</span><span class="sxs-lookup"><span data-stu-id="fdd02-293">Confirm that the <xref:Microsoft.AspNetCore.Components.Forms.EditForm> has a <xref:Microsoft.AspNetCore.Components.Forms.EditForm.Model> or <xref:Microsoft.AspNetCore.Components.Forms.EditContext>.</span></span>

<span data-ttu-id="fdd02-294">將指派 <xref:Microsoft.AspNetCore.Components.Forms.EditForm.Model> 給表單時，請確認模型類型已具現化，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="fdd02-294">When assigning a <xref:Microsoft.AspNetCore.Components.Forms.EditForm.Model> to the form, confirm that the model type is instantiated, as the following example shows:</span></span>

```csharp
private ExampleModel exampleModel = new ExampleModel();
```
