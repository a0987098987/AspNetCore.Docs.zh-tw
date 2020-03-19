---
title: ASP.NET Core Blazor 資料系結
author: guardrex
description: 瞭解 Blazor 應用程式中元件和 DOM 元素的資料系結功能。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/data-binding
ms.openlocfilehash: 5b49d2598a451ee607e034913bd1aeaa03f941c6
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511193"
---
# <a name="aspnet-core-opno-locblazor-data-binding"></a>ASP.NET Core Blazor 資料系結

By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)

Razor 元件透過具有欄位、屬性或 Razor 運算式值的 HTML 專案屬性（名為[`@bind`](xref:mvc/views/razor#bind) ）提供資料系結功能。

下列範例會將 `CurrentValue` 屬性系結至文字方塊的值：

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

當文字方塊失去焦點時，就會更新屬性的值。

只有在呈現元件時，才會更新 UI 中的文字方塊，而不會回應變更屬性的值。 由於元件會在事件處理常式程式碼執行之後自行呈現，因此在觸發事件處理常式之後，屬性更新*通常*會立即反映在 UI 中。

搭配 `CurrentValue` 屬性（`<input @bind="CurrentValue" />`）使用 `@bind` 基本上等同于下列內容：

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

當元件轉譯時，input 元素的 `value` 來自 `CurrentValue` 屬性。 當使用者在文字方塊中輸入並變更元素焦點時，`onchange` 事件會引發，而且 `CurrentValue` 屬性會設定為變更的值。 實際上，程式碼產生更為複雜，因為 `@bind` 會處理執行類型轉換的情況。 就原則而言，`@bind` 會將運算式的目前值與 `value` 屬性產生關聯，並使用已註冊的處理常式來處理變更。

藉由同時包含具有 `event` 參數的 `@bind:event` 屬性，在其他事件上系結屬性或欄位。 下列範例會系結 `oninput` 事件上的 `CurrentValue` 屬性：

```razor
<input @bind="CurrentValue" @bind:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

不同于當元素失去焦點時引發的 `onchange`，`oninput` 當文字方塊的值變更時，就會引發。

使用 `@bind-{ATTRIBUTE}` 搭配 `@bind-{ATTRIBUTE}:event` 語法來系結 `value`以外的元素屬性。 在下列範例中，當 `_paragraphStyle` 值變更時，會更新段落的樣式：

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

屬性系結會區分大小寫。 例如，`@bind` 有效，而且 `@Bind` 無效。

## <a name="unparsable-values"></a>無法剖析的值

當使用者將無法剖析的值提供給資料系結元素時，會在觸發系結事件時，自動將無法剖析的值還原成先前的值。

請考慮下列案例：

* `<input>` 元素會系結至 `int` 類型，其初始值為 `123`：

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* 使用者會將元素的值更新為頁面中的 `123.45`，並變更元素的焦點。

在上述案例中，元素的值會還原為 `123`。 當 `123.45` 值拒絕 `123`的原始值時，使用者瞭解其值不被接受。

根據預設，系結會套用至元素的 `onchange` 事件（`@bind="{PROPERTY OR FIELD}"`）。 使用 `@bind="{PROPERTY OR FIELD}" @bind:event={EVENT}`，在不同的事件上觸發系結。 若為 `oninput` 事件（`@bind:event="oninput"`），回復會在引進無法剖析值的任何擊鍵之後發生。 以 `int`系結類型的 `oninput` 事件為目標時，使用者無法輸入 `.` 字元。 系統會立即移除 `.` 字元，讓使用者收到僅允許整數的立即回應。 在某些情況下，在 `oninput` 事件上還原值不是理想的情況，例如當使用者應允許清除無法剖析的 `<input>` 值時。 替代方案包括：

* 請勿使用 `oninput` 事件。 使用預設的 `onchange` 事件（僅指定 `@bind="{PROPERTY OR FIELD}"`），在元素失去焦點之前，不會還原不正確值。
* 系結至可為 null 的型別（例如 `int?` 或 `string`），並提供自訂邏輯來處理不正確專案。
* 使用[表單驗證元件](xref:blazor/forms-validation)，例如 `InputNumber` 或 `InputDate`。 表單驗證元件有內建支援，可管理不正確輸入。 表單驗證元件：
  * 允許使用者在相關聯的 `EditContext`上提供不正確輸入和接收驗證錯誤。
  * 在 UI 中顯示驗證錯誤，而不幹擾使用者輸入其他 webform 資料。

## <a name="format-strings"></a>格式字串

資料系結可搭配使用[`@bind:format`](xref:mvc/views/razor#bind)<xref:System.DateTime> 格式字串。 目前無法使用其他格式運算式，例如貨幣或數位格式。

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

在上述程式碼中，`<input>` 元素的欄位類型（`type`）預設為 `text`。 支援系結下列 .NET 類型的 `@bind:format`：

* <xref:System.DateTime?displayProperty=fullName>
* <xref:System.DateTime?displayProperty=fullName>?
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <xref:System.DateTimeOffset?displayProperty=fullName>?

`@bind:format` 屬性會指定要套用至 `<input>` 元素 `value` 的日期格式。 當發生 `onchange` 事件時，也會使用此格式來剖析值。

不建議指定 `date` 欄位類型的格式，因為 Blazor 具有格式日期的內建支援。 在建議的情況下，只有在使用 `date` 欄位類型提供格式時，才使用 `yyyy-MM-dd` 日期格式來進行系結以正確運作：

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

## <a name="parent-to-child-binding-with-component-parameters"></a>具有元件參數的父系對子系結

Binding 可辨識元件參數，其中 `@bind-{PROPERTY}` 可以將屬性值從父元件系結到子元件。 具有連鎖系結區段的[子系對父](#child-to-parent-binding-with-chained-bind)系結中涵蓋了從子系系結至父系的系結。

下列子元件（`ChildComponent`）具有 `Year` 元件參數和 `YearChanged` 回呼：

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

<xref:blazor/event-handling#eventcallback>會說明 `EventCallback<T>`。

下列父元件會使用：

* `ChildComponent`，並將 `ParentYear` 參數從父系系結至子元件上的 `Year` 參數。
* `onclick` 事件是用來觸發 `ChangeTheYear` 方法。 如需詳細資訊，請參閱 <xref:blazor/event-handling>。

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

載入 `ParentComponent` 會產生下列標記：

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

如果 `ParentYear` 屬性的值是藉由選取 `ParentComponent`中的按鈕來變更，則會更新 `ChildComponent` 的 `Year` 屬性。 當 `ParentComponent` 為重新顯示時，`Year` 的新值會在 UI 中呈現：

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

`Year` 參數是可系結的，因為它具有符合 `Year` 參數類型的伴隨 `YearChanged` 事件。

依照慣例，`<ChildComponent @bind-Year="ParentYear" />` 基本上等同于撰寫：

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

一般而言，屬性可以藉由包含 `@bind-{PROPRETY}:event` 屬性來系結至對應的事件處理常式。 例如，您可以使用下列兩個屬性，將屬性 `MyProp` 系結至 `MyEventHandler`：

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="child-to-parent-binding-with-chained-bind"></a>具有連鎖系結的子系對父系系結

常見的案例是將資料系結參數連結至元件輸出中的 page 元素。 此案例稱為*連鎖*系結，因為會同時發生多個層級的系結。

在頁面的元素中，無法使用 `@bind` 語法來執行連結系結。 事件處理常式和值必須分別指定。 不過，父元件可以使用 `@bind` 語法搭配元件的參數。

下列 `PasswordField` 元件（*self.passwordfield.text*）：

* 將 `<input>` 元素的值設定為 `Password` 屬性。
* 將 `Password` 屬性的變更公開至具有[app eventcallback](xref:blazor/event-handling#eventcallback)的父元件。
* 會使用 `onclick` 事件來觸發 `ToggleShowPassword` 方法。 如需詳細資訊，請參閱 <xref:blazor/event-handling>。

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

`PasswordField` 元件會在另一個元件中使用：

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<PasswordField @bind-Password="_password" />

@code {
    private string _password;
}
```

若要對上述範例中的密碼執行檢查或陷印錯誤：

* 建立 `Password` 的支援欄位（在下列範例程式碼中`_password`）。
* 執行 `Password` setter 中的檢查或陷阱錯誤。

如果密碼的值中使用了空格，下列範例會向使用者提供立即的意見反應：

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

## <a name="radio-buttons"></a>選項按鈕

如需系結至表單中選項按鈕的相關資訊，請參閱 <xref:blazor/forms-validation#work-with-radio-buttons>。
