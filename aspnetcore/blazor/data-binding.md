---
title: ASP.NETBlazor核心 資料繫結
author: guardrex
description: 瞭解Blazor應用中元件和 DOM 元素的數據綁定功能。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/data-binding
ms.openlocfilehash: a7b3730dad48b5bbb6134dab181051da4e3651b4
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80320956"
---
# <a name="aspnet-core-opno-locblazor-data-binding"></a><span data-ttu-id="c98b0-103">ASP.NETBlazor核心 資料繫結</span><span class="sxs-lookup"><span data-stu-id="c98b0-103">ASP.NET Core Blazor data binding</span></span>

<span data-ttu-id="c98b0-104">由[盧克·萊瑟姆](https://github.com/guardrex)和[丹尼爾·羅斯](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="c98b0-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="c98b0-105">Razor 元件透過名為欄位、屬性或 Razor[`@bind`](xref:mvc/views/razor#bind)表示式值的 HTML 元素屬性提供數據綁定功能。</span><span class="sxs-lookup"><span data-stu-id="c98b0-105">Razor components provide data binding features via an HTML element attribute named [`@bind`](xref:mvc/views/razor#bind) with a field, property, or Razor expression value.</span></span>

<span data-ttu-id="c98b0-106">以下範例將`CurrentValue`屬性結合文字框值:</span><span class="sxs-lookup"><span data-stu-id="c98b0-106">The following example binds the `CurrentValue` property to the text box's value:</span></span>

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="c98b0-107">當文字框失去焦點時,將更新屬性的值。</span><span class="sxs-lookup"><span data-stu-id="c98b0-107">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="c98b0-108">僅當呈現元件時,才在 UI 中更新文本框,而不是回應更改屬性的值。</span><span class="sxs-lookup"><span data-stu-id="c98b0-108">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="c98b0-109">由於元件在事件處理程式代碼執行後呈現自身,因此屬性更新*通常在*觸發事件處理程式後立即反映在UI中。</span><span class="sxs-lookup"><span data-stu-id="c98b0-109">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="c98b0-110">與`@bind``CurrentValue`屬性`<input @bind="CurrentValue" />`( ) 一起使用基本上等效於以下內容:</span><span class="sxs-lookup"><span data-stu-id="c98b0-110">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="c98b0-111">呈現元件時,`value`輸入元素的來自 屬性`CurrentValue`。</span><span class="sxs-lookup"><span data-stu-id="c98b0-111">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="c98b0-112">當使用者在文字框中鍵入並更改元素焦點時,`onchange`將觸發事件`CurrentValue`並將 屬性設置為更改的值。</span><span class="sxs-lookup"><span data-stu-id="c98b0-112">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="c98b0-113">實際上,代碼生成更為複雜,因為`@bind`處理執行類型轉換的情況。</span><span class="sxs-lookup"><span data-stu-id="c98b0-113">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="c98b0-114">原則上,`@bind`將表示式的目前值與屬性關聯`value`, 並使用已註冊的處理程式處理更改。</span><span class="sxs-lookup"><span data-stu-id="c98b0-114">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="c98b0-115">通過包含具有`@bind:event``event`參數的屬性,在其他事件上綁定屬性或欄位。</span><span class="sxs-lookup"><span data-stu-id="c98b0-115">Bind a property or field on other events by also including an `@bind:event` attribute with an `event` parameter.</span></span> <span data-ttu-id="c98b0-116">以下範例繫結`CurrentValue``oninput`事件上的屬性:</span><span class="sxs-lookup"><span data-stu-id="c98b0-116">The following example binds the `CurrentValue` property on the `oninput` event:</span></span>

```razor
<input @bind="CurrentValue" @bind:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="c98b0-117">與`onchange`在元素失去焦點時觸發`oninput`的 不同,當文本框的值發生更改時,將觸發。</span><span class="sxs-lookup"><span data-stu-id="c98b0-117">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="c98b0-118">使用`@bind-{ATTRIBUTE}``@bind-{ATTRIBUTE}:event`語法繫結元素屬性以外`value`的 。</span><span class="sxs-lookup"><span data-stu-id="c98b0-118">Use `@bind-{ATTRIBUTE}` with `@bind-{ATTRIBUTE}:event` syntax to bind element attributes other than `value`.</span></span> <span data-ttu-id="c98b0-119">在下面的範例中,當值更改時,`_paragraphStyle`段落的樣式將更新:</span><span class="sxs-lookup"><span data-stu-id="c98b0-119">In the following example, the paragraph's style is updated when the `_paragraphStyle` value changes:</span></span>

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

<span data-ttu-id="c98b0-120">屬性繫結區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="c98b0-120">Attribute binding is case sensitive.</span></span> <span data-ttu-id="c98b0-121">例如,`@bind`有效,`@Bind`並且無效。</span><span class="sxs-lookup"><span data-stu-id="c98b0-121">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

## <a name="unparsable-values"></a><span data-ttu-id="c98b0-122">無法解析的值</span><span class="sxs-lookup"><span data-stu-id="c98b0-122">Unparsable values</span></span>

<span data-ttu-id="c98b0-123">當使用者向數據綁定元素提供不可解析的值時,在觸發綁定事件時,不可解析的值將自動還原到其上一個值。</span><span class="sxs-lookup"><span data-stu-id="c98b0-123">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="c98b0-124">請考慮下列案例：</span><span class="sxs-lookup"><span data-stu-id="c98b0-124">Consider the following scenario:</span></span>

* <span data-ttu-id="c98b0-125">元素`<input>`繫結到初始值`int`為`123`的型態:</span><span class="sxs-lookup"><span data-stu-id="c98b0-125">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="c98b0-126">使用者將元素的值更新到`123.45`頁面中並更改元素焦點。</span><span class="sxs-lookup"><span data-stu-id="c98b0-126">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="c98b0-127">在前面的機制中,元素的值會回復到`123`。</span><span class="sxs-lookup"><span data-stu-id="c98b0-127">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="c98b0-128">當該值`123.45`被拒絕,以有利於的原始值`123`時 ,使用者會明白其值未被接受。</span><span class="sxs-lookup"><span data-stu-id="c98b0-128">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="c98b0-129">默認情況下,綁定應用於元素`onchange`的事件 (。`@bind="{PROPERTY OR FIELD}"`</span><span class="sxs-lookup"><span data-stu-id="c98b0-129">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="c98b0-130">用於`@bind="{PROPERTY OR FIELD}" @bind:event={EVENT}`觸發其他事件的綁定。</span><span class="sxs-lookup"><span data-stu-id="c98b0-130">Use `@bind="{PROPERTY OR FIELD}" @bind:event={EVENT}` to trigger binding on a different event.</span></span> <span data-ttu-id="c98b0-131">對於`oninput`事件`@bind:event="oninput"`( ), 還原發生在引入不可解析值的任何擊鍵之後。</span><span class="sxs-lookup"><span data-stu-id="c98b0-131">For the `oninput` event (`@bind:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="c98b0-132">使用`oninput``int`綁定類型定位事件時,將阻止用戶鍵`.`入字元。</span><span class="sxs-lookup"><span data-stu-id="c98b0-132">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="c98b0-133">字元`.`將立即刪除,因此使用者會立即收到僅允許整個數位的反饋。</span><span class="sxs-lookup"><span data-stu-id="c98b0-133">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="c98b0-134">在某些情況下,還原`oninput`事件上的值並不理想,例如何時應允許使用者清除`<input>`不可 解析的值。</span><span class="sxs-lookup"><span data-stu-id="c98b0-134">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="c98b0-135">替代專案包括:</span><span class="sxs-lookup"><span data-stu-id="c98b0-135">Alternatives include:</span></span>

* <span data-ttu-id="c98b0-136">不要使用該`oninput`事件。</span><span class="sxs-lookup"><span data-stu-id="c98b0-136">Don't use the `oninput` event.</span></span> <span data-ttu-id="c98b0-137">使用預設`onchange`事件(僅`@bind="{PROPERTY OR FIELD}"`指定 ),其中無效值在元素失去焦點之前不會還原。</span><span class="sxs-lookup"><span data-stu-id="c98b0-137">Use the default `onchange` event (only specify `@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="c98b0-138">綁定到可無效類型(如`int?``string`或 ,並提供自定義邏輯來處理無效條目)。</span><span class="sxs-lookup"><span data-stu-id="c98b0-138">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="c98b0-139">使用[窗體驗證元件](xref:blazor/forms-validation),`InputNumber`如`InputDate`。</span><span class="sxs-lookup"><span data-stu-id="c98b0-139">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="c98b0-140">表單驗證元件具有管理無效輸入的內置支援。</span><span class="sxs-lookup"><span data-stu-id="c98b0-140">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="c98b0-141">表單驗證元件:</span><span class="sxs-lookup"><span data-stu-id="c98b0-141">Form validation components:</span></span>
  * <span data-ttu-id="c98b0-142">允許使用者提供無效的輸入並在關聯的`EditContext`上接收驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="c98b0-142">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="c98b0-143">在 UI 中顯示驗證錯誤,而不會干擾使用者輸入其他 Web 表單數據。</span><span class="sxs-lookup"><span data-stu-id="c98b0-143">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

## <a name="format-strings"></a><span data-ttu-id="c98b0-144">格式化字串</span><span class="sxs-lookup"><span data-stu-id="c98b0-144">Format strings</span></span>

<span data-ttu-id="c98b0-145">資料繫結適用於<xref:System.DateTime>[`@bind:format`](xref:mvc/views/razor#bind)使用的格式字串。</span><span class="sxs-lookup"><span data-stu-id="c98b0-145">Data binding works with <xref:System.DateTime> format strings using [`@bind:format`](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="c98b0-146">其他格式運算式(如貨幣或數位格式)目前不可用。</span><span class="sxs-lookup"><span data-stu-id="c98b0-146">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="c98b0-147">在前面的代碼中,`<input>`元素的欄位型`type`態 (`text`) 預設為 。</span><span class="sxs-lookup"><span data-stu-id="c98b0-147">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="c98b0-148">`@bind:format`支援繫結以下 .NET 型態:</span><span class="sxs-lookup"><span data-stu-id="c98b0-148">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="c98b0-149"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="c98b0-149"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="c98b0-150"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="c98b0-150"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="c98b0-151">該`@bind:format`屬性指定要應用`value``<input>`於 元素的日期格式。</span><span class="sxs-lookup"><span data-stu-id="c98b0-151">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="c98b0-152">該格式還用於在`onchange`事件發生時分析值。</span><span class="sxs-lookup"><span data-stu-id="c98b0-152">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="c98b0-153">不建議為`date`欄位類型指定格式,Blazor因為內置支援設定日期格式。</span><span class="sxs-lookup"><span data-stu-id="c98b0-153">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span> <span data-ttu-id="c98b0-154">儘管有建議,但僅當提供`yyyy-MM-dd``date`欄位類型的格式時,才使用日期格式進行綁定才能正常工作:</span><span class="sxs-lookup"><span data-stu-id="c98b0-154">In spite of the recommendation, only use the `yyyy-MM-dd` date format for binding to work correctly if a format is supplied with the `date` field type:</span></span>

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

## <a name="parent-to-child-binding-with-component-parameters"></a><span data-ttu-id="c98b0-155">使用元件參數的父子繫結</span><span class="sxs-lookup"><span data-stu-id="c98b0-155">Parent-to-child binding with component parameters</span></span>

<span data-ttu-id="c98b0-156">綁定可識別元件參數,其中`@bind-{PROPERTY}`可以將屬性值從父元件綁定到子元件。</span><span class="sxs-lookup"><span data-stu-id="c98b0-156">Binding recognizes component parameters, where `@bind-{PROPERTY}` can bind a property value from a parent component down to a child component.</span></span> <span data-ttu-id="c98b0-157">從子級綁定到父級綁定在[帶有鏈綁定綁定的子到父綁定](#child-to-parent-binding-with-chained-bind)中涵蓋。</span><span class="sxs-lookup"><span data-stu-id="c98b0-157">Binding from a child to a parent is covered in the [Child-to-parent binding with chained bind](#child-to-parent-binding-with-chained-bind) section.</span></span>

<span data-ttu-id="c98b0-158">以下子元件 (`ChildComponent``Year`) 具有元件`YearChanged`參數 與回檔:</span><span class="sxs-lookup"><span data-stu-id="c98b0-158">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

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

<span data-ttu-id="c98b0-159">`EventCallback<T>`在中<xref:blazor/event-handling#eventcallback>解釋。</span><span class="sxs-lookup"><span data-stu-id="c98b0-159">`EventCallback<T>` is explained in <xref:blazor/event-handling#eventcallback>.</span></span>

<span data-ttu-id="c98b0-160">以下父元件使用:</span><span class="sxs-lookup"><span data-stu-id="c98b0-160">The following parent component uses:</span></span>

* <span data-ttu-id="c98b0-161">`ChildComponent`並將`ParentYear`參數從父參數綁定到子元件上的`Year`參數。</span><span class="sxs-lookup"><span data-stu-id="c98b0-161">`ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component.</span></span>
* <span data-ttu-id="c98b0-162">此`onclick`事件用於觸發`ChangeTheYear`方法 。</span><span class="sxs-lookup"><span data-stu-id="c98b0-162">The `onclick` event is used to trigger the `ChangeTheYear` method.</span></span> <span data-ttu-id="c98b0-163">如需詳細資訊，請參閱 <xref:blazor/event-handling>。</span><span class="sxs-lookup"><span data-stu-id="c98b0-163">For more information, see <xref:blazor/event-handling>.</span></span>

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

<span data-ttu-id="c98b0-164">載入`ParentComponent`產生以下標籤:</span><span class="sxs-lookup"><span data-stu-id="c98b0-164">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="c98b0-165">如果透過選擇的`ParentYear``ParentComponent`按鈕更改屬性的值`Year``ChildComponent`, 則更新的屬性。</span><span class="sxs-lookup"><span data-stu-id="c98b0-165">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="c98b0-166">重新成`Year`成`ParentComponent`一個值 , 在 UI 中呈現的新值:</span><span class="sxs-lookup"><span data-stu-id="c98b0-166">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="c98b0-167">參數`Year`是可綁定的,因為它具有`YearChanged``Year`與 參數類型匹配的配套事件。</span><span class="sxs-lookup"><span data-stu-id="c98b0-167">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="c98b0-168">依慣例,`<ChildComponent @bind-Year="ParentYear" />`基本上等同於寫作:</span><span class="sxs-lookup"><span data-stu-id="c98b0-168">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="c98b0-169">通常,屬性可以通過`@bind-{PROPRETY}:event`包含 屬性綁定到相應的事件處理程式。</span><span class="sxs-lookup"><span data-stu-id="c98b0-169">In general, a property can be bound to a corresponding event handler by including an `@bind-{PROPRETY}:event` attribute.</span></span> <span data-ttu-id="c98b0-170">例如,`MyProp`屬性可以使用以下兩個屬性繫`MyEventHandler`結到:</span><span class="sxs-lookup"><span data-stu-id="c98b0-170">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="child-to-parent-binding-with-chained-bind"></a><span data-ttu-id="c98b0-171">具有鏈繫定的子到父綁定</span><span class="sxs-lookup"><span data-stu-id="c98b0-171">Child-to-parent binding with chained bind</span></span>

<span data-ttu-id="c98b0-172">常見方案是將數據綁定參數連結到元件輸出中的頁面元素。</span><span class="sxs-lookup"><span data-stu-id="c98b0-172">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="c98b0-173">此方案稱為*鏈綁定*,因為多個級別的綁定同時發生。</span><span class="sxs-lookup"><span data-stu-id="c98b0-173">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="c98b0-174">無法使用`@bind`頁面元素中的語法實現連結綁定。</span><span class="sxs-lookup"><span data-stu-id="c98b0-174">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="c98b0-175">事件處理程式和值必須單獨指定。</span><span class="sxs-lookup"><span data-stu-id="c98b0-175">The event handler and value must be specified separately.</span></span> <span data-ttu-id="c98b0-176">但是,父元件可以使用`@bind`語法與元件的參數。</span><span class="sxs-lookup"><span data-stu-id="c98b0-176">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="c98b0-177">以下`PasswordField`元件 (*密碼欄位. razor*):</span><span class="sxs-lookup"><span data-stu-id="c98b0-177">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="c98b0-178">將`<input>`元素的值設置到`Password`屬性。</span><span class="sxs-lookup"><span data-stu-id="c98b0-178">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="c98b0-179">使用`Password`[事件回調](xref:blazor/event-handling#eventcallback)將屬性的更改公開給父元件。</span><span class="sxs-lookup"><span data-stu-id="c98b0-179">Exposes changes of the `Password` property to a parent component with an [EventCallback](xref:blazor/event-handling#eventcallback).</span></span>
* <span data-ttu-id="c98b0-180">使用事件`onclick`用於觸`ToggleShowPassword`發 方法。</span><span class="sxs-lookup"><span data-stu-id="c98b0-180">Uses the `onclick` event is used to trigger the `ToggleShowPassword` method.</span></span> <span data-ttu-id="c98b0-181">如需詳細資訊，請參閱 <xref:blazor/event-handling>。</span><span class="sxs-lookup"><span data-stu-id="c98b0-181">For more information, see <xref:blazor/event-handling>.</span></span>

```razor
<h1>Child Component</h1>

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

<span data-ttu-id="c98b0-182">此`PasswordField`元件用於另一個元件:</span><span class="sxs-lookup"><span data-stu-id="c98b0-182">The `PasswordField` component is used in another component:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<PasswordField @bind-Password="_password" />

@code {
    private string _password;
}
```

<span data-ttu-id="c98b0-183">要在上述範例中對密碼執行檢查或捕捉錯誤,請執行以下操作:</span><span class="sxs-lookup"><span data-stu-id="c98b0-183">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="c98b0-184">為`Password`(`_password`在以下範例代碼中)建立後備欄位。</span><span class="sxs-lookup"><span data-stu-id="c98b0-184">Create a backing field for `Password` (`_password` in the following example code).</span></span>
* <span data-ttu-id="c98b0-185">在`Password`設置器中執行檢查或陷阱錯誤。</span><span class="sxs-lookup"><span data-stu-id="c98b0-185">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="c98b0-186">如果密碼值中使用了空格,以下範例會立即向使用者提供回饋:</span><span class="sxs-lookup"><span data-stu-id="c98b0-186">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```razor
<h1>Child Component</h1>

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

## <a name="radio-buttons"></a><span data-ttu-id="c98b0-187">選項按鈕</span><span class="sxs-lookup"><span data-stu-id="c98b0-187">Radio buttons</span></span>

<span data-ttu-id="c98b0-188">有關在表單選按鈕的資訊,請參閱<xref:blazor/forms-validation#work-with-radio-buttons>。</span><span class="sxs-lookup"><span data-stu-id="c98b0-188">For information on binding to radio buttons in a form, see <xref:blazor/forms-validation#work-with-radio-buttons>.</span></span>
