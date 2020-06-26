---
title: ASP.NET Core Blazor 資料系結
author: guardrex
description: 瞭解應用程式中元件和 DOM 元素的資料系結功能 Blazor 。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/components/data-binding
ms.openlocfilehash: 35873e57171b4d86affcb475ad2d55aef443d3b5
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85399175"
---
# <a name="aspnet-core-blazor-data-binding"></a>ASP.NET Core Blazor 資料系結

By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)

Razor元件會透過 [`@bind`](xref:mvc/views/razor#bind) 以欄位、屬性或運算式值命名的 HTML 元素屬性，提供資料系結功能 Razor 。

下列範例會將屬性系結 `CurrentValue` 至文字方塊的值：

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

當文字方塊失去焦點時，就會更新屬性的值。

只有在呈現元件時，才會更新 UI 中的文字方塊，而不會回應變更屬性的值。 由於元件會在事件處理常式程式碼執行之後自行呈現，因此在觸發事件處理常式之後，屬性更新*通常*會立即反映在 UI 中。

使用 [`@bind`](xref:mvc/views/razor#bind) 與 `CurrentValue` 屬性（ `<input @bind="CurrentValue" />` ）基本上等同于下列內容：

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

當元件呈現時，輸入專案的會 `value` 來自 `CurrentValue` 屬性。 當使用者在文字方塊中輸入並變更元素焦點時， `onchange` 就會引發事件，並將 `CurrentValue` 屬性設定為已變更的值。 實際上，程式碼產生更為複雜，因為會 [`@bind`](xref:mvc/views/razor#bind) 處理執行類型轉換的情況。 在原則上，會將 [`@bind`](xref:mvc/views/razor#bind) 運算式的目前值與屬性產生關聯， `value` 並使用已註冊的處理常式來處理變更。

藉由同時包含具有參數的屬性，系結其他事件上的屬性或欄位 `@bind:event` `event` 。 下列範例會系結 `CurrentValue` 事件上的屬性 `oninput` ：

```razor
<input @bind="CurrentValue" @bind:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

不同于 `onchange` 當元素失去焦點時引發的，會 `oninput` 在文字方塊的值變更時引發。

使用 `@bind-{ATTRIBUTE}` with 語法來系結以外的 `@bind-{ATTRIBUTE}:event` 元素屬性 `value` 。 在下列範例中，當值變更時，會更新段落的樣式 `paragraphStyle` ：

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

屬性系結會區分大小寫：

* `@bind`有效。
* `@Bind`和 `@BIND` 無效。

## <a name="unparsable-values"></a>無法剖析的值

當使用者將無法剖析的值提供給資料系結元素時，會在觸發系結事件時，自動將無法剖析的值還原成先前的值。

請考慮下列案例：

* `<input>`元素會系結至 `int` 具有初始值的類型 `123` ：

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* 使用者會將專案的值更新為 `123.45` 頁面中的，並變更元素的焦點。

在上述案例中，元素的值會還原成 `123` 。 當以的 `123.45` 原始值拒絕值時 `123` ，使用者瞭解其值不被接受。

根據預設，系結會套用至元素的 `onchange` 事件（ `@bind="{PROPERTY OR FIELD}"` ）。 用 `@bind="{PROPERTY OR FIELD}" @bind:event={EVENT}` 來在不同的事件上觸發系結。 對於 `oninput` 事件（ `@bind:event="oninput"` ），回復會在引入無法剖析值的任何擊鍵之後發生。 以系結型別為目標的 `oninput` 事件時 `int` ，使用者無法輸入 `.` 字元。 `.`系統會立即移除字元，讓使用者收到只允許整數的立即回應。 在某些情況下，在事件上還原值 `oninput` 不是理想的情況，例如，當使用者應允許清除無法解析的 `<input>` 值時。 替代方案包括：

* 請勿使用 `oninput` 事件。 使用預設 `onchange` 事件（僅指定 `@bind="{PROPERTY OR FIELD}"` ），在元素失去焦點之前，不會還原不正確值。
* 系結至可為 null 的型別（例如 `int?` 或 `string` ），並提供自訂邏輯來處理不正確專案。
* 使用[表單驗證元件](xref:blazor/forms-validation)，例如 <xref:Microsoft.AspNetCore.Components.Forms.InputNumber%601> 或 <xref:Microsoft.AspNetCore.Components.Forms.InputDate%601> 。 表單驗證元件有內建支援，可管理不正確輸入。 表單驗證元件：
  * 允許使用者在相關聯的上提供不正確輸入和接收驗證錯誤 <xref:Microsoft.AspNetCore.Components.Forms.EditContext> 。
  * 在 UI 中顯示驗證錯誤，而不幹擾使用者輸入其他 webform 資料。

## <a name="format-strings"></a>格式字串

資料系結會 <xref:System.DateTime> 使用來處理格式字串 `@bind:format` 。 目前無法使用其他格式運算式，例如貨幣或數位格式。

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

在上述程式碼中， `<input>` 元素的欄位類型（ `type` ）預設為 `text` 。 `@bind:format`支援系結下列 .NET 類型：

* <xref:System.DateTime?displayProperty=fullName>
* <xref:System.DateTime?displayProperty=fullName>?
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <xref:System.DateTimeOffset?displayProperty=fullName>?

`@bind:format`屬性會指定要套用至元素之的日期格式 `value` `<input>` 。 當發生事件時，也會使用此格式來剖析值 `onchange` 。

`date`不建議指定欄位類型的格式，因為 Blazor 具有格式日期的內建支援。 在建議的情況下，只有在以欄位類型提供格式時，才使用「系結」的 `yyyy-MM-dd` 日期格式來正常運作 `date` ：

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

## <a name="parent-to-child-binding-with-component-parameters"></a>具有元件參數的父系對子系結

Binding 可辨識元件參數，其中 `@bind-{PROPERTY}` 可以將屬性值從父元件系結到子元件。 具有連鎖系結區段的[子系對父](#child-to-parent-binding-with-chained-bind)系結中涵蓋了從子系系結至父系的系結。

下列子元件（ `ChildComponent` ）具有 `Year` 元件參數和 `YearChanged` 回呼：

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

<xref:Microsoft.AspNetCore.Components.EventCallback%601>會在中說明 <xref:blazor/components/event-handling#eventcallback> 。

下列父元件會使用：

* `ChildComponent`和會將 `ParentYear` 參數從父系系結至 `Year` 子元件上的參數。
* `onclick`事件是用來觸發 `ChangeTheYear` 方法。 如需詳細資訊，請參閱 <xref:blazor/components/event-handling> 。

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

載入會 `ParentComponent` 產生下列標記：

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

如果藉 `ParentYear` 由選取中的按鈕來變更屬性的值，則 `ParentComponent` `Year` `ChildComponent` 會更新的屬性。 `Year`當為重新顯示時，的新值會在 UI 中呈現 `ParentComponent` ：

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

參數是可系結的， `Year` 因為它有 `YearChanged` 符合參數類型的伴隨事件 `Year` 。

依照慣例， `<ChildComponent @bind-Year="ParentYear" />` 基本上等同于撰寫：

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

一般而言，屬性可以藉由包含屬性，系結至對應的事件處理常式 `@bind-{PROPRETY}:event` 。 例如，您 `MyProp` 可以 `MyEventHandler` 使用下列兩個屬性，將屬性系結至：

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="child-to-parent-binding-with-chained-bind"></a>具有連鎖系結的子系對父系系結

常見的案例是將資料系結參數連結至元件輸出中的 page 元素。 此案例稱為*連鎖*系結，因為會同時發生多個層級的系結。

無法 [`@bind`](xref:mvc/views/razor#bind) 在頁面的元素中使用語法來執行連鎖系結。 事件處理常式和值必須分別指定。 不過，父系元件可以使用語法搭配 [`@bind`](xref:mvc/views/razor#bind) 元件的參數。

下列 `PasswordField` 元件（ `PasswordField.razor` ）：

* 將 `<input>` 元素的值設定為 `Password` 屬性。
* 將屬性的變更公開 `Password` 至具有的父元件 [`EventCallback`](xref:blazor/components/event-handling#eventcallback) 。
* 會使用 `onclick` 事件來觸發 `ToggleShowPassword` 方法。 如需詳細資訊，請參閱 <xref:blazor/components/event-handling> 。

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

`PasswordField`元件會在另一個元件中使用：

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

若要對上述範例中的密碼執行檢查或陷印錯誤：

* 建立的支援欄位 `Password` （ `password` 在下列範例程式碼中）。
* 執行 setter 中的檢查或陷阱錯誤 `Password` 。

如果密碼的值中使用了空格，下列範例會向使用者提供立即的意見反應：

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

## <a name="radio-buttons"></a>選項按鈕

如需系結至表單中選項按鈕的相關資訊，請參閱 <xref:blazor/forms-validation#work-with-radio-buttons> 。
