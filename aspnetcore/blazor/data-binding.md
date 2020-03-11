---
title: ASP.NET Core Blazor 資料系結
author: guardrex
description: 瞭解 Blazor 應用程式中元件和 DOM 元素的資料系結案例。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/24/2020
no-loc:
- Blazor
- SignalR
uid: blazor/data-binding
ms.openlocfilehash: 92377730b9d353a507ffd384710fb979affe7265
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661284"
---
# <a name="aspnet-core-opno-locblazor-data-binding"></a><span data-ttu-id="a31fc-103">ASP.NET Core Blazor 資料系結</span><span class="sxs-lookup"><span data-stu-id="a31fc-103">ASP.NET Core Blazor data binding</span></span>

<span data-ttu-id="a31fc-104">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="a31fc-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="a31fc-105">元件和 DOM 元素的資料系結都是使用[`@bind`](xref:mvc/views/razor#bind)屬性來完成。</span><span class="sxs-lookup"><span data-stu-id="a31fc-105">Data binding to both components and DOM elements is accomplished with the [`@bind`](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="a31fc-106">下列範例會將 `CurrentValue` 屬性系結至文字方塊的值：</span><span class="sxs-lookup"><span data-stu-id="a31fc-106">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="a31fc-107">當文字方塊失去焦點時，就會更新屬性的值。</span><span class="sxs-lookup"><span data-stu-id="a31fc-107">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="a31fc-108">只有在呈現元件時，才會更新 UI 中的文字方塊，而不會回應變更屬性的值。</span><span class="sxs-lookup"><span data-stu-id="a31fc-108">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="a31fc-109">由於元件會在事件處理常式程式碼執行之後自行呈現，因此在觸發事件處理常式之後，屬性更新*通常*會立即反映在 UI 中。</span><span class="sxs-lookup"><span data-stu-id="a31fc-109">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="a31fc-110">搭配 `CurrentValue` 屬性（`<input @bind="CurrentValue" />`）使用 `@bind` 基本上等同于下列內容：</span><span class="sxs-lookup"><span data-stu-id="a31fc-110">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="a31fc-111">當元件轉譯時，input 元素的 `value` 來自 `CurrentValue` 屬性。</span><span class="sxs-lookup"><span data-stu-id="a31fc-111">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="a31fc-112">當使用者在文字方塊中輸入並變更元素焦點時，`onchange` 事件會引發，而且 `CurrentValue` 屬性會設定為變更的值。</span><span class="sxs-lookup"><span data-stu-id="a31fc-112">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="a31fc-113">實際上，程式碼產生更為複雜，因為 `@bind` 會處理執行類型轉換的情況。</span><span class="sxs-lookup"><span data-stu-id="a31fc-113">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="a31fc-114">就原則而言，`@bind` 會將運算式的目前值與 `value` 屬性產生關聯，並使用已註冊的處理常式來處理變更。</span><span class="sxs-lookup"><span data-stu-id="a31fc-114">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="a31fc-115">藉由同時包含具有 `event` 參數的 `@bind:event` 屬性，在其他事件上系結屬性或欄位。</span><span class="sxs-lookup"><span data-stu-id="a31fc-115">Bind a property or field on other events by also including an `@bind:event` attribute with an `event` parameter.</span></span> <span data-ttu-id="a31fc-116">下列範例會系結 `oninput` 事件上的 `CurrentValue` 屬性：</span><span class="sxs-lookup"><span data-stu-id="a31fc-116">The following example binds the `CurrentValue` property on the `oninput` event:</span></span>

```razor
<input @bind="CurrentValue" @bind:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="a31fc-117">不同于當元素失去焦點時引發的 `onchange`，`oninput` 當文字方塊的值變更時，就會引發。</span><span class="sxs-lookup"><span data-stu-id="a31fc-117">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="a31fc-118">使用 `@bind-{ATTRIBUTE}` 搭配 `@bind-{ATTRIBUTE}:event` 語法來系結 `value`以外的元素屬性。</span><span class="sxs-lookup"><span data-stu-id="a31fc-118">Use `@bind-{ATTRIBUTE}` with `@bind-{ATTRIBUTE}:event` syntax to bind element attributes other than `value`.</span></span> <span data-ttu-id="a31fc-119">在下列範例中，當 `_paragraphStyle` 值變更時，會更新段落的樣式：</span><span class="sxs-lookup"><span data-stu-id="a31fc-119">In the following example, the paragraph's style is updated when the `_paragraphStyle` value changes:</span></span>

```razor
@page "/binding-example"

<p>
    <input type="text" @bind="_paragraphStyle" />
</p>

<p @bind-style="_paragraphStyle" @bind-style:event="onchange">
    Blazorify the app!
</p>

@code {
    private string _paragraphStyle = "color:red";
}
```

## <a name="unparsable-values"></a><span data-ttu-id="a31fc-120">無法剖析的值</span><span class="sxs-lookup"><span data-stu-id="a31fc-120">Unparsable values</span></span>

<span data-ttu-id="a31fc-121">當使用者將無法剖析的值提供給資料系結元素時，會在觸發系結事件時，自動將無法剖析的值還原成先前的值。</span><span class="sxs-lookup"><span data-stu-id="a31fc-121">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="a31fc-122">請考慮下列案例：</span><span class="sxs-lookup"><span data-stu-id="a31fc-122">Consider the following scenario:</span></span>

* <span data-ttu-id="a31fc-123">`<input>` 元素會系結至 `int` 類型，其初始值為 `123`：</span><span class="sxs-lookup"><span data-stu-id="a31fc-123">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="a31fc-124">使用者會將元素的值更新為頁面中的 `123.45`，並變更元素的焦點。</span><span class="sxs-lookup"><span data-stu-id="a31fc-124">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="a31fc-125">在上述案例中，元素的值會還原為 `123`。</span><span class="sxs-lookup"><span data-stu-id="a31fc-125">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="a31fc-126">當 `123.45` 值拒絕 `123`的原始值時，使用者瞭解其值不被接受。</span><span class="sxs-lookup"><span data-stu-id="a31fc-126">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="a31fc-127">根據預設，系結會套用至元素的 `onchange` 事件（`@bind="{PROPERTY OR FIELD}"`）。</span><span class="sxs-lookup"><span data-stu-id="a31fc-127">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="a31fc-128">使用 `@bind="{PROPERTY OR FIELD}" @bind:event={EVENT}`，在不同的事件上觸發系結。</span><span class="sxs-lookup"><span data-stu-id="a31fc-128">Use `@bind="{PROPERTY OR FIELD}" @bind:event={EVENT}` to trigger binding on a different event.</span></span> <span data-ttu-id="a31fc-129">若為 `oninput` 事件（`@bind:event="oninput"`），回復會在引進無法剖析值的任何擊鍵之後發生。</span><span class="sxs-lookup"><span data-stu-id="a31fc-129">For the `oninput` event (`@bind:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="a31fc-130">以 `int`系結類型的 `oninput` 事件為目標時，使用者無法輸入 `.` 字元。</span><span class="sxs-lookup"><span data-stu-id="a31fc-130">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="a31fc-131">系統會立即移除 `.` 字元，讓使用者收到僅允許整數的立即回應。</span><span class="sxs-lookup"><span data-stu-id="a31fc-131">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="a31fc-132">在某些情況下，在 `oninput` 事件上還原值不是理想的情況，例如當使用者應允許清除無法剖析的 `<input>` 值時。</span><span class="sxs-lookup"><span data-stu-id="a31fc-132">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="a31fc-133">替代方案包括：</span><span class="sxs-lookup"><span data-stu-id="a31fc-133">Alternatives include:</span></span>

* <span data-ttu-id="a31fc-134">請勿使用 `oninput` 事件。</span><span class="sxs-lookup"><span data-stu-id="a31fc-134">Don't use the `oninput` event.</span></span> <span data-ttu-id="a31fc-135">使用預設的 `onchange` 事件（僅指定 `@bind="{PROPERTY OR FIELD}"`），在元素失去焦點之前，不會還原不正確值。</span><span class="sxs-lookup"><span data-stu-id="a31fc-135">Use the default `onchange` event (only specify `@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="a31fc-136">系結至可為 null 的型別（例如 `int?` 或 `string`），並提供自訂邏輯來處理不正確專案。</span><span class="sxs-lookup"><span data-stu-id="a31fc-136">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="a31fc-137">使用[表單驗證元件](xref:blazor/forms-validation)，例如 `InputNumber` 或 `InputDate`。</span><span class="sxs-lookup"><span data-stu-id="a31fc-137">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="a31fc-138">表單驗證元件有內建支援，可管理不正確輸入。</span><span class="sxs-lookup"><span data-stu-id="a31fc-138">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="a31fc-139">表單驗證元件：</span><span class="sxs-lookup"><span data-stu-id="a31fc-139">Form validation components:</span></span>
  * <span data-ttu-id="a31fc-140">允許使用者在相關聯的 `EditContext`上提供不正確輸入和接收驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="a31fc-140">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="a31fc-141">在 UI 中顯示驗證錯誤，而不幹擾使用者輸入其他 webform 資料。</span><span class="sxs-lookup"><span data-stu-id="a31fc-141">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

## <a name="format-strings"></a><span data-ttu-id="a31fc-142">格式字串</span><span class="sxs-lookup"><span data-stu-id="a31fc-142">Format strings</span></span>

<span data-ttu-id="a31fc-143">資料系結可搭配使用[`@bind:format`](xref:mvc/views/razor#bind)<xref:System.DateTime> 格式字串。</span><span class="sxs-lookup"><span data-stu-id="a31fc-143">Data binding works with <xref:System.DateTime> format strings using [`@bind:format`](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="a31fc-144">目前無法使用其他格式運算式，例如貨幣或數位格式。</span><span class="sxs-lookup"><span data-stu-id="a31fc-144">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="a31fc-145">在上述程式碼中，`<input>` 元素的欄位類型（`type`）預設為 `text`。</span><span class="sxs-lookup"><span data-stu-id="a31fc-145">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="a31fc-146">支援系結下列 .NET 類型的 `@bind:format`：</span><span class="sxs-lookup"><span data-stu-id="a31fc-146">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="a31fc-147"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="a31fc-147"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="a31fc-148"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="a31fc-148"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="a31fc-149">`@bind:format` 屬性會指定要套用至 `<input>` 元素 `value` 的日期格式。</span><span class="sxs-lookup"><span data-stu-id="a31fc-149">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="a31fc-150">當發生 `onchange` 事件時，也會使用此格式來剖析值。</span><span class="sxs-lookup"><span data-stu-id="a31fc-150">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="a31fc-151">不建議指定 `date` 欄位類型的格式，因為 Blazor 具有格式日期的內建支援。</span><span class="sxs-lookup"><span data-stu-id="a31fc-151">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span> <span data-ttu-id="a31fc-152">在建議的情況下，只有在使用 `date` 欄位類型提供格式時，才使用 `yyyy-MM-dd` 日期格式來進行系結以正確運作：</span><span class="sxs-lookup"><span data-stu-id="a31fc-152">In spite of the recommendation, only use the `yyyy-MM-dd` date format for binding to work correctly if a format is supplied with the `date` field type:</span></span>

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

## <a name="parent-to-child-binding-with-component-parameters"></a><span data-ttu-id="a31fc-153">具有元件參數的父系對子系結</span><span class="sxs-lookup"><span data-stu-id="a31fc-153">Parent-to-child binding with component parameters</span></span>

<span data-ttu-id="a31fc-154">Binding 可辨識元件參數，其中 `@bind-{PROPERTY}` 可以將屬性值從父元件系結到子元件。</span><span class="sxs-lookup"><span data-stu-id="a31fc-154">Binding recognizes component parameters, where `@bind-{PROPERTY}` can bind a property value from a parent component down to a child component.</span></span> <span data-ttu-id="a31fc-155">具有連鎖系結區段的[子系對父](#child-to-parent-binding-with-chained-bind)系結中涵蓋了從子系系結至父系的系結。</span><span class="sxs-lookup"><span data-stu-id="a31fc-155">Binding from a child to a parent is covered in the [Child-to-parent binding with chained bind](#child-to-parent-binding-with-chained-bind) section.</span></span>

<span data-ttu-id="a31fc-156">下列子元件（`ChildComponent`）具有 `Year` 元件參數和 `YearChanged` 回呼：</span><span class="sxs-lookup"><span data-stu-id="a31fc-156">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

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

<span data-ttu-id="a31fc-157"><xref:blazor/event-handling#eventcallback>會說明 `EventCallback<T>`。</span><span class="sxs-lookup"><span data-stu-id="a31fc-157">`EventCallback<T>` is explained in <xref:blazor/event-handling#eventcallback>.</span></span>

<span data-ttu-id="a31fc-158">下列父元件會使用：</span><span class="sxs-lookup"><span data-stu-id="a31fc-158">The following parent component uses:</span></span>

* <span data-ttu-id="a31fc-159">`ChildComponent`，並將 `ParentYear` 參數從父系系結至子元件上的 `Year` 參數。</span><span class="sxs-lookup"><span data-stu-id="a31fc-159">`ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component.</span></span>
* <span data-ttu-id="a31fc-160">`onclick` 事件是用來觸發 `ChangeTheYear` 方法。</span><span class="sxs-lookup"><span data-stu-id="a31fc-160">The `onclick` event is used to trigger the `ChangeTheYear` method.</span></span> <span data-ttu-id="a31fc-161">如需詳細資訊，請參閱 <xref:blazor/event-handling>。</span><span class="sxs-lookup"><span data-stu-id="a31fc-161">For more information, see <xref:blazor/event-handling>.</span></span>

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

<span data-ttu-id="a31fc-162">載入 `ParentComponent` 會產生下列標記：</span><span class="sxs-lookup"><span data-stu-id="a31fc-162">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="a31fc-163">如果 `ParentYear` 屬性的值是藉由選取 `ParentComponent`中的按鈕來變更，則會更新 `ChildComponent` 的 `Year` 屬性。</span><span class="sxs-lookup"><span data-stu-id="a31fc-163">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="a31fc-164">當 `ParentComponent` 為重新顯示時，`Year` 的新值會在 UI 中呈現：</span><span class="sxs-lookup"><span data-stu-id="a31fc-164">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="a31fc-165">`Year` 參數是可系結的，因為它具有符合 `Year` 參數類型的伴隨 `YearChanged` 事件。</span><span class="sxs-lookup"><span data-stu-id="a31fc-165">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="a31fc-166">依照慣例，`<ChildComponent @bind-Year="ParentYear" />` 基本上等同于撰寫：</span><span class="sxs-lookup"><span data-stu-id="a31fc-166">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="a31fc-167">一般而言，屬性可以藉由包含 `@bind-{PROPRETY}:event` 屬性來系結至對應的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="a31fc-167">In general, a property can be bound to a corresponding event handler by including an `@bind-{PROPRETY}:event` attribute.</span></span> <span data-ttu-id="a31fc-168">例如，您可以使用下列兩個屬性，將屬性 `MyProp` 系結至 `MyEventHandler`：</span><span class="sxs-lookup"><span data-stu-id="a31fc-168">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="child-to-parent-binding-with-chained-bind"></a><span data-ttu-id="a31fc-169">具有連鎖系結的子系對父系系結</span><span class="sxs-lookup"><span data-stu-id="a31fc-169">Child-to-parent binding with chained bind</span></span>

<span data-ttu-id="a31fc-170">常見的案例是將資料系結參數連結至元件輸出中的 page 元素。</span><span class="sxs-lookup"><span data-stu-id="a31fc-170">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="a31fc-171">此案例稱為*連鎖*系結，因為會同時發生多個層級的系結。</span><span class="sxs-lookup"><span data-stu-id="a31fc-171">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="a31fc-172">在頁面的元素中，無法使用 `@bind` 語法來執行連結系結。</span><span class="sxs-lookup"><span data-stu-id="a31fc-172">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="a31fc-173">事件處理常式和值必須分別指定。</span><span class="sxs-lookup"><span data-stu-id="a31fc-173">The event handler and value must be specified separately.</span></span> <span data-ttu-id="a31fc-174">不過，父元件可以使用 `@bind` 語法搭配元件的參數。</span><span class="sxs-lookup"><span data-stu-id="a31fc-174">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="a31fc-175">下列 `PasswordField` 元件（*self.passwordfield.text*）：</span><span class="sxs-lookup"><span data-stu-id="a31fc-175">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="a31fc-176">將 `<input>` 元素的值設定為 `Password` 屬性。</span><span class="sxs-lookup"><span data-stu-id="a31fc-176">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="a31fc-177">將 `Password` 屬性的變更公開至具有[app eventcallback](xref:blazor/event-handling#eventcallback)的父元件。</span><span class="sxs-lookup"><span data-stu-id="a31fc-177">Exposes changes of the `Password` property to a parent component with an [EventCallback](xref:blazor/event-handling#eventcallback).</span></span>
* <span data-ttu-id="a31fc-178">會使用 `onclick` 事件來觸發 `ToggleShowPassword` 方法。</span><span class="sxs-lookup"><span data-stu-id="a31fc-178">Uses the `onclick` event is used to trigger the `ToggleShowPassword` method.</span></span> <span data-ttu-id="a31fc-179">如需詳細資訊，請參閱 <xref:blazor/event-handling>。</span><span class="sxs-lookup"><span data-stu-id="a31fc-179">For more information, see <xref:blazor/event-handling>.</span></span>

```razor
<h1>Child Component</h2>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool _showPassword;

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
        _showPassword = !_showPassword;
    }
}
```

<span data-ttu-id="a31fc-180">`PasswordField` 元件會在另一個元件中使用：</span><span class="sxs-lookup"><span data-stu-id="a31fc-180">The `PasswordField` component is used in another component:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<PasswordField @bind-Password="_password" />

@code {
    private string _password;
}
```

<span data-ttu-id="a31fc-181">若要對上述範例中的密碼執行檢查或陷印錯誤：</span><span class="sxs-lookup"><span data-stu-id="a31fc-181">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="a31fc-182">建立 `Password` 的支援欄位（在下列範例程式碼中`_password`）。</span><span class="sxs-lookup"><span data-stu-id="a31fc-182">Create a backing field for `Password` (`_password` in the following example code).</span></span>
* <span data-ttu-id="a31fc-183">執行 `Password` setter 中的檢查或陷阱錯誤。</span><span class="sxs-lookup"><span data-stu-id="a31fc-183">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="a31fc-184">如果密碼的值中使用了空格，下列範例會向使用者提供立即的意見反應：</span><span class="sxs-lookup"><span data-stu-id="a31fc-184">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@_validationMessage</span>

@code {
    private bool _showPassword;
    private string _password;
    private string _validationMessage;

    [Parameter]
    public string Password
    {
        get { return _password ?? string.Empty; }
        set
        {
            if (_password != value)
            {
                if (value.Contains(' '))
                {
                    _validationMessage = "Spaces not allowed!";
                }
                else
                {
                    _password = value;
                    _validationMessage = string.Empty;
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
        _showPassword = !_showPassword;
    }
}
```

## <a name="radio-buttons"></a><span data-ttu-id="a31fc-185">選項按鈕</span><span class="sxs-lookup"><span data-stu-id="a31fc-185">Radio buttons</span></span>

<span data-ttu-id="a31fc-186">如需系結至表單中選項按鈕的相關資訊，請參閱 <xref:blazor/forms-validation#work-with-radio-buttons>。</span><span class="sxs-lookup"><span data-stu-id="a31fc-186">For information on binding to radio buttons in a form, see <xref:blazor/forms-validation#work-with-radio-buttons>.</span></span>
