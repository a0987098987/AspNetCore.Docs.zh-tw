---
<span data-ttu-id="d7957-101">標題： ' ASP.NET Core Blazor 資料系結 ' 作者：描述： ' 瞭解應用程式中元件和 DOM 元素的資料系結功能 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="d7957-101">title: 'ASP.NET Core Blazor data binding' author: description: 'Learn about data binding features for components and DOM elements in Blazor apps.'</span></span>
<span data-ttu-id="d7957-102">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d7957-102">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d7957-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d7957-103">'Blazor'</span></span>
- <span data-ttu-id="d7957-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d7957-104">'Identity'</span></span>
- <span data-ttu-id="d7957-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d7957-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="d7957-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d7957-106">'Razor'</span></span>
- <span data-ttu-id="d7957-107">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d7957-107">'SignalR' uid:</span></span> 

---
# <a name="aspnet-core-blazor-data-binding"></a><span data-ttu-id="d7957-108">ASP.NET Core Blazor 資料系結</span><span class="sxs-lookup"><span data-stu-id="d7957-108">ASP.NET Core Blazor data binding</span></span>

<span data-ttu-id="d7957-109">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="d7957-109">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

Razor<span data-ttu-id="d7957-110">元件會透過 [`@bind`](xref:mvc/views/razor#bind) 以欄位、屬性或運算式值命名的 HTML 元素屬性，提供資料系結功能 Razor 。</span><span class="sxs-lookup"><span data-stu-id="d7957-110"> components provide data binding features via an HTML element attribute named [`@bind`](xref:mvc/views/razor#bind) with a field, property, or Razor expression value.</span></span>

<span data-ttu-id="d7957-111">下列範例會將屬性系結 `CurrentValue` 至文字方塊的值：</span><span class="sxs-lookup"><span data-stu-id="d7957-111">The following example binds the `CurrentValue` property to the text box's value:</span></span>

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="d7957-112">當文字方塊失去焦點時，就會更新屬性的值。</span><span class="sxs-lookup"><span data-stu-id="d7957-112">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="d7957-113">只有在呈現元件時，才會更新 UI 中的文字方塊，而不會回應變更屬性的值。</span><span class="sxs-lookup"><span data-stu-id="d7957-113">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="d7957-114">由於元件會在事件處理常式程式碼執行之後自行呈現，因此在觸發事件處理常式之後，屬性更新*通常*會立即反映在 UI 中。</span><span class="sxs-lookup"><span data-stu-id="d7957-114">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="d7957-115">使用 [`@bind`](xref:mvc/views/razor#bind) 與 `CurrentValue` 屬性（ `<input @bind="CurrentValue" />` ）基本上等同于下列內容：</span><span class="sxs-lookup"><span data-stu-id="d7957-115">Using [`@bind`](xref:mvc/views/razor#bind) with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="d7957-116">當元件呈現時，輸入專案的會 `value` 來自 `CurrentValue` 屬性。</span><span class="sxs-lookup"><span data-stu-id="d7957-116">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="d7957-117">當使用者在文字方塊中輸入並變更元素焦點時， `onchange` 就會引發事件，並將 `CurrentValue` 屬性設定為已變更的值。</span><span class="sxs-lookup"><span data-stu-id="d7957-117">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="d7957-118">實際上，程式碼產生更為複雜，因為會 [`@bind`](xref:mvc/views/razor#bind) 處理執行類型轉換的情況。</span><span class="sxs-lookup"><span data-stu-id="d7957-118">In reality, the code generation is more complex because [`@bind`](xref:mvc/views/razor#bind) handles cases where type conversions are performed.</span></span> <span data-ttu-id="d7957-119">在原則上，會將 [`@bind`](xref:mvc/views/razor#bind) 運算式的目前值與屬性產生關聯， `value` 並使用已註冊的處理常式來處理變更。</span><span class="sxs-lookup"><span data-stu-id="d7957-119">In principle, [`@bind`](xref:mvc/views/razor#bind) associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="d7957-120">藉由同時包含具有參數的屬性，系結其他事件上的屬性或欄位 `@bind:event` `event` 。</span><span class="sxs-lookup"><span data-stu-id="d7957-120">Bind a property or field on other events by also including an `@bind:event` attribute with an `event` parameter.</span></span> <span data-ttu-id="d7957-121">下列範例會系結 `CurrentValue` 事件上的屬性 `oninput` ：</span><span class="sxs-lookup"><span data-stu-id="d7957-121">The following example binds the `CurrentValue` property on the `oninput` event:</span></span>

```razor
<input @bind="CurrentValue" @bind:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="d7957-122">不同于 `onchange` 當元素失去焦點時引發的，會 `oninput` 在文字方塊的值變更時引發。</span><span class="sxs-lookup"><span data-stu-id="d7957-122">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="d7957-123">使用 `@bind-{ATTRIBUTE}` with 語法來系結以外的 `@bind-{ATTRIBUTE}:event` 元素屬性 `value` 。</span><span class="sxs-lookup"><span data-stu-id="d7957-123">Use `@bind-{ATTRIBUTE}` with `@bind-{ATTRIBUTE}:event` syntax to bind element attributes other than `value`.</span></span> <span data-ttu-id="d7957-124">在下列範例中，當值變更時，會更新段落的樣式 `paragraphStyle` ：</span><span class="sxs-lookup"><span data-stu-id="d7957-124">In the following example, the paragraph's style is updated when the `paragraphStyle` value changes:</span></span>

```razor
@page "/binding-example"

<p>
    <input type="text" @bind="paragraphStyle" />
</p>

<p @bind-style="paragraphStyle" @bind-style:event="onchange">
    Blazorify the app!
</p>

@code {
    private string paragraphStyle = "color:red";
}
```

<span data-ttu-id="d7957-125">屬性系結會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="d7957-125">Attribute binding is case sensitive.</span></span> <span data-ttu-id="d7957-126">例如， [`@bind`](xref:mvc/views/razor#bind) 是有效的，而且 [`@bind`](xref:mvc/views/razor#bind) 無效。</span><span class="sxs-lookup"><span data-stu-id="d7957-126">For example, [`@bind`](xref:mvc/views/razor#bind) is valid, and [`@bind`](xref:mvc/views/razor#bind) is invalid.</span></span>

## <a name="unparsable-values"></a><span data-ttu-id="d7957-127">無法剖析的值</span><span class="sxs-lookup"><span data-stu-id="d7957-127">Unparsable values</span></span>

<span data-ttu-id="d7957-128">當使用者將無法剖析的值提供給資料系結元素時，會在觸發系結事件時，自動將無法剖析的值還原成先前的值。</span><span class="sxs-lookup"><span data-stu-id="d7957-128">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="d7957-129">請考慮下列案例：</span><span class="sxs-lookup"><span data-stu-id="d7957-129">Consider the following scenario:</span></span>

* <span data-ttu-id="d7957-130">`<input>`元素會系結至 `int` 具有初始值的類型 `123` ：</span><span class="sxs-lookup"><span data-stu-id="d7957-130">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="d7957-131">使用者會將專案的值更新為 `123.45` 頁面中的，並變更元素的焦點。</span><span class="sxs-lookup"><span data-stu-id="d7957-131">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="d7957-132">在上述案例中，元素的值會還原成 `123` 。</span><span class="sxs-lookup"><span data-stu-id="d7957-132">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="d7957-133">當以的 `123.45` 原始值拒絕值時 `123` ，使用者瞭解其值不被接受。</span><span class="sxs-lookup"><span data-stu-id="d7957-133">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="d7957-134">根據預設，系結會套用至元素的 `onchange` 事件（ `@bind="{PROPERTY OR FIELD}"` ）。</span><span class="sxs-lookup"><span data-stu-id="d7957-134">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="d7957-135">用 `@bind="{PROPERTY OR FIELD}" @bind:event={EVENT}` 來在不同的事件上觸發系結。</span><span class="sxs-lookup"><span data-stu-id="d7957-135">Use `@bind="{PROPERTY OR FIELD}" @bind:event={EVENT}` to trigger binding on a different event.</span></span> <span data-ttu-id="d7957-136">對於 `oninput` 事件（ `@bind:event="oninput"` ），回復會在引入無法剖析值的任何擊鍵之後發生。</span><span class="sxs-lookup"><span data-stu-id="d7957-136">For the `oninput` event (`@bind:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="d7957-137">以系結型別為目標的 `oninput` 事件時 `int` ，使用者無法輸入 `.` 字元。</span><span class="sxs-lookup"><span data-stu-id="d7957-137">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="d7957-138">`.`系統會立即移除字元，讓使用者收到只允許整數的立即回應。</span><span class="sxs-lookup"><span data-stu-id="d7957-138">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="d7957-139">在某些情況下，在事件上還原值 `oninput` 不是理想的情況，例如，當使用者應允許清除無法解析的 `<input>` 值時。</span><span class="sxs-lookup"><span data-stu-id="d7957-139">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="d7957-140">替代方案包括：</span><span class="sxs-lookup"><span data-stu-id="d7957-140">Alternatives include:</span></span>

* <span data-ttu-id="d7957-141">請勿使用 `oninput` 事件。</span><span class="sxs-lookup"><span data-stu-id="d7957-141">Don't use the `oninput` event.</span></span> <span data-ttu-id="d7957-142">使用預設 `onchange` 事件（僅指定 `@bind="{PROPERTY OR FIELD}"` ），在元素失去焦點之前，不會還原不正確值。</span><span class="sxs-lookup"><span data-stu-id="d7957-142">Use the default `onchange` event (only specify `@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="d7957-143">系結至可為 null 的型別（例如 `int?` 或 `string` ），並提供自訂邏輯來處理不正確專案。</span><span class="sxs-lookup"><span data-stu-id="d7957-143">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="d7957-144">使用[表單驗證元件](xref:blazor/forms-validation)，例如 <xref:Microsoft.AspNetCore.Components.Forms.InputNumber%601> 或 <xref:Microsoft.AspNetCore.Components.Forms.InputDate%601> 。</span><span class="sxs-lookup"><span data-stu-id="d7957-144">Use a [form validation component](xref:blazor/forms-validation), such as <xref:Microsoft.AspNetCore.Components.Forms.InputNumber%601> or <xref:Microsoft.AspNetCore.Components.Forms.InputDate%601>.</span></span> <span data-ttu-id="d7957-145">表單驗證元件有內建支援，可管理不正確輸入。</span><span class="sxs-lookup"><span data-stu-id="d7957-145">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="d7957-146">表單驗證元件：</span><span class="sxs-lookup"><span data-stu-id="d7957-146">Form validation components:</span></span>
  * <span data-ttu-id="d7957-147">允許使用者在相關聯的上提供不正確輸入和接收驗證錯誤 <xref:Microsoft.AspNetCore.Components.Forms.EditContext> 。</span><span class="sxs-lookup"><span data-stu-id="d7957-147">Permit the user to provide invalid input and receive validation errors on the associated <xref:Microsoft.AspNetCore.Components.Forms.EditContext>.</span></span>
  * <span data-ttu-id="d7957-148">在 UI 中顯示驗證錯誤，而不幹擾使用者輸入其他 webform 資料。</span><span class="sxs-lookup"><span data-stu-id="d7957-148">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

## <a name="format-strings"></a><span data-ttu-id="d7957-149">格式字串</span><span class="sxs-lookup"><span data-stu-id="d7957-149">Format strings</span></span>

<span data-ttu-id="d7957-150">資料系結會 <xref:System.DateTime> 使用來處理格式字串 `@bind:format` 。</span><span class="sxs-lookup"><span data-stu-id="d7957-150">Data binding works with <xref:System.DateTime> format strings using `@bind:format`.</span></span> <span data-ttu-id="d7957-151">目前無法使用其他格式運算式，例如貨幣或數位格式。</span><span class="sxs-lookup"><span data-stu-id="d7957-151">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="d7957-152">在上述程式碼中， `<input>` 元素的欄位類型（ `type` ）預設為 `text` 。</span><span class="sxs-lookup"><span data-stu-id="d7957-152">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="d7957-153">`@bind:format`支援系結下列 .NET 類型：</span><span class="sxs-lookup"><span data-stu-id="d7957-153">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="d7957-154"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="d7957-154"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="d7957-155"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="d7957-155"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="d7957-156">`@bind:format`屬性會指定要套用至元素之的日期格式 `value` `<input>` 。</span><span class="sxs-lookup"><span data-stu-id="d7957-156">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="d7957-157">當發生事件時，也會使用此格式來剖析值 `onchange` 。</span><span class="sxs-lookup"><span data-stu-id="d7957-157">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="d7957-158">`date`不建議指定欄位類型的格式，因為 Blazor 具有格式日期的內建支援。</span><span class="sxs-lookup"><span data-stu-id="d7957-158">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span> <span data-ttu-id="d7957-159">在建議的情況下，只有在以欄位類型提供格式時，才使用「系結」的 `yyyy-MM-dd` 日期格式來正常運作 `date` ：</span><span class="sxs-lookup"><span data-stu-id="d7957-159">In spite of the recommendation, only use the `yyyy-MM-dd` date format for binding to work correctly if a format is supplied with the `date` field type:</span></span>

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

## <a name="parent-to-child-binding-with-component-parameters"></a><span data-ttu-id="d7957-160">具有元件參數的父系對子系結</span><span class="sxs-lookup"><span data-stu-id="d7957-160">Parent-to-child binding with component parameters</span></span>

<span data-ttu-id="d7957-161">Binding 可辨識元件參數，其中 `@bind-{PROPERTY}` 可以將屬性值從父元件系結到子元件。</span><span class="sxs-lookup"><span data-stu-id="d7957-161">Binding recognizes component parameters, where `@bind-{PROPERTY}` can bind a property value from a parent component down to a child component.</span></span> <span data-ttu-id="d7957-162">具有連鎖系結區段的[子系對父](#child-to-parent-binding-with-chained-bind)系結中涵蓋了從子系系結至父系的系結。</span><span class="sxs-lookup"><span data-stu-id="d7957-162">Binding from a child to a parent is covered in the [Child-to-parent binding with chained bind](#child-to-parent-binding-with-chained-bind) section.</span></span>

<span data-ttu-id="d7957-163">下列子元件（ `ChildComponent` ）具有 `Year` 元件參數和 `YearChanged` 回呼：</span><span class="sxs-lookup"><span data-stu-id="d7957-163">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```razor
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="d7957-164"><xref:Microsoft.AspNetCore.Components.EventCallback%601>會在中說明 <xref:blazor/event-handling#eventcallback> 。</span><span class="sxs-lookup"><span data-stu-id="d7957-164"><xref:Microsoft.AspNetCore.Components.EventCallback%601> is explained in <xref:blazor/event-handling#eventcallback>.</span></span>

<span data-ttu-id="d7957-165">下列父元件會使用：</span><span class="sxs-lookup"><span data-stu-id="d7957-165">The following parent component uses:</span></span>

* <span data-ttu-id="d7957-166">`ChildComponent`和會將 `ParentYear` 參數從父系系結至 `Year` 子元件上的參數。</span><span class="sxs-lookup"><span data-stu-id="d7957-166">`ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component.</span></span>
* <span data-ttu-id="d7957-167">`onclick`事件是用來觸發 `ChangeTheYear` 方法。</span><span class="sxs-lookup"><span data-stu-id="d7957-167">The `onclick` event is used to trigger the `ChangeTheYear` method.</span></span> <span data-ttu-id="d7957-168">如需詳細資訊，請參閱<xref:blazor/event-handling>。</span><span class="sxs-lookup"><span data-stu-id="d7957-168">For more information, see <xref:blazor/event-handling>.</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    public int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="d7957-169">載入會 `ParentComponent` 產生下列標記：</span><span class="sxs-lookup"><span data-stu-id="d7957-169">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="d7957-170">如果藉 `ParentYear` 由選取中的按鈕來變更屬性的值，則 `ParentComponent` `Year` `ChildComponent` 會更新的屬性。</span><span class="sxs-lookup"><span data-stu-id="d7957-170">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="d7957-171">`Year`當為重新顯示時，的新值會在 UI 中呈現 `ParentComponent` ：</span><span class="sxs-lookup"><span data-stu-id="d7957-171">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="d7957-172">參數是可系結的， `Year` 因為它有 `YearChanged` 符合參數類型的伴隨事件 `Year` 。</span><span class="sxs-lookup"><span data-stu-id="d7957-172">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="d7957-173">依照慣例， `<ChildComponent @bind-Year="ParentYear" />` 基本上等同于撰寫：</span><span class="sxs-lookup"><span data-stu-id="d7957-173">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="d7957-174">一般而言，屬性可以藉由包含屬性，系結至對應的事件處理常式 `@bind-{PROPRETY}:event` 。</span><span class="sxs-lookup"><span data-stu-id="d7957-174">In general, a property can be bound to a corresponding event handler by including an `@bind-{PROPRETY}:event` attribute.</span></span> <span data-ttu-id="d7957-175">例如，您 `MyProp` 可以 `MyEventHandler` 使用下列兩個屬性，將屬性系結至：</span><span class="sxs-lookup"><span data-stu-id="d7957-175">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="child-to-parent-binding-with-chained-bind"></a><span data-ttu-id="d7957-176">具有連鎖系結的子系對父系系結</span><span class="sxs-lookup"><span data-stu-id="d7957-176">Child-to-parent binding with chained bind</span></span>

<span data-ttu-id="d7957-177">常見的案例是將資料系結參數連結至元件輸出中的 page 元素。</span><span class="sxs-lookup"><span data-stu-id="d7957-177">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="d7957-178">此案例稱為*連鎖*系結，因為會同時發生多個層級的系結。</span><span class="sxs-lookup"><span data-stu-id="d7957-178">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="d7957-179">無法 [`@bind`](xref:mvc/views/razor#bind) 在頁面的元素中使用語法來執行連鎖系結。</span><span class="sxs-lookup"><span data-stu-id="d7957-179">A chained bind can't be implemented with [`@bind`](xref:mvc/views/razor#bind) syntax in the page's element.</span></span> <span data-ttu-id="d7957-180">事件處理常式和值必須分別指定。</span><span class="sxs-lookup"><span data-stu-id="d7957-180">The event handler and value must be specified separately.</span></span> <span data-ttu-id="d7957-181">不過，父系元件可以使用語法搭配 [`@bind`](xref:mvc/views/razor#bind) 元件的參數。</span><span class="sxs-lookup"><span data-stu-id="d7957-181">A parent component, however, can use [`@bind`](xref:mvc/views/razor#bind) syntax with the component's parameter.</span></span>

<span data-ttu-id="d7957-182">下列 `PasswordField` 元件（*self.passwordfield.text*）：</span><span class="sxs-lookup"><span data-stu-id="d7957-182">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="d7957-183">將 `<input>` 元素的值設定為 `Password` 屬性。</span><span class="sxs-lookup"><span data-stu-id="d7957-183">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="d7957-184">將屬性的變更公開 `Password` 至具有[app eventcallback](xref:blazor/event-handling#eventcallback)的父元件。</span><span class="sxs-lookup"><span data-stu-id="d7957-184">Exposes changes of the `Password` property to a parent component with an [EventCallback](xref:blazor/event-handling#eventcallback).</span></span>
* <span data-ttu-id="d7957-185">使用 `onclick` 事件來觸發 `ToggleShowPassword` 方法。</span><span class="sxs-lookup"><span data-stu-id="d7957-185">Uses the `onclick` event is used to trigger the `ToggleShowPassword` method.</span></span> <span data-ttu-id="d7957-186">如需詳細資訊，請參閱<xref:blazor/event-handling>。</span><span class="sxs-lookup"><span data-stu-id="d7957-186">For more information, see <xref:blazor/event-handling>.</span></span>

```razor
<h1>Child Component</h1>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool showPassword;

    [Parameter]
    public string Password { get; set; }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

<span data-ttu-id="d7957-187">`PasswordField`元件會在另一個元件中使用：</span><span class="sxs-lookup"><span data-stu-id="d7957-187">The `PasswordField` component is used in another component:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

<span data-ttu-id="d7957-188">若要對上述範例中的密碼執行檢查或陷印錯誤：</span><span class="sxs-lookup"><span data-stu-id="d7957-188">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="d7957-189">建立的支援欄位 `Password` （ `password` 在下列範例程式碼中）。</span><span class="sxs-lookup"><span data-stu-id="d7957-189">Create a backing field for `Password` (`password` in the following example code).</span></span>
* <span data-ttu-id="d7957-190">執行 setter 中的檢查或陷阱錯誤 `Password` 。</span><span class="sxs-lookup"><span data-stu-id="d7957-190">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="d7957-191">如果密碼的值中使用了空格，下列範例會向使用者提供立即的意見反應：</span><span class="sxs-lookup"><span data-stu-id="d7957-191">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```razor
<h1>Child Component</h1>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@validationMessage</span>

@code {
    private bool showPassword;
    private string password;
    private string validationMessage;

    [Parameter]
    public string Password
    {
        get { return password ?? string.Empty; }
        set
        {
            if (password != value)
            {
                if (value.Contains(' '))
                {
                    validationMessage = "Spaces not allowed!";
                }
                else
                {
                    password = value;
                    validationMessage = string.Empty;
                }
            }
        }
    }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

## <a name="radio-buttons"></a><span data-ttu-id="d7957-192">選項按鈕</span><span class="sxs-lookup"><span data-stu-id="d7957-192">Radio buttons</span></span>

<span data-ttu-id="d7957-193">如需系結至表單中選項按鈕的相關資訊，請參閱 <xref:blazor/forms-validation#work-with-radio-buttons> 。</span><span class="sxs-lookup"><span data-stu-id="d7957-193">For information on binding to radio buttons in a form, see <xref:blazor/forms-validation#work-with-radio-buttons>.</span></span>
