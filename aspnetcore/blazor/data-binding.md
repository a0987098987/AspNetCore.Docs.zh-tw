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
# <a name="aspnet-core-opno-locblazor-data-binding"></a>ASP.NETBlazor核心 資料繫結

由[盧克·萊瑟姆](https://github.com/guardrex)和[丹尼爾·羅斯](https://github.com/danroth27)

Razor 元件透過名為欄位、屬性或 Razor[`@bind`](xref:mvc/views/razor#bind)表示式值的 HTML 元素屬性提供數據綁定功能。

以下範例將`CurrentValue`屬性結合文字框值:

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

當文字框失去焦點時,將更新屬性的值。

僅當呈現元件時,才在 UI 中更新文本框,而不是回應更改屬性的值。 由於元件在事件處理程式代碼執行後呈現自身,因此屬性更新*通常在*觸發事件處理程式後立即反映在UI中。

與`@bind``CurrentValue`屬性`<input @bind="CurrentValue" />`( ) 一起使用基本上等效於以下內容:

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

呈現元件時,`value`輸入元素的來自 屬性`CurrentValue`。 當使用者在文字框中鍵入並更改元素焦點時,`onchange`將觸發事件`CurrentValue`並將 屬性設置為更改的值。 實際上,代碼生成更為複雜,因為`@bind`處理執行類型轉換的情況。 原則上,`@bind`將表示式的目前值與屬性關聯`value`, 並使用已註冊的處理程式處理更改。

通過包含具有`@bind:event``event`參數的屬性,在其他事件上綁定屬性或欄位。 以下範例繫結`CurrentValue``oninput`事件上的屬性:

```razor
<input @bind="CurrentValue" @bind:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

與`onchange`在元素失去焦點時觸發`oninput`的 不同,當文本框的值發生更改時,將觸發。

使用`@bind-{ATTRIBUTE}``@bind-{ATTRIBUTE}:event`語法繫結元素屬性以外`value`的 。 在下面的範例中,當值更改時,`_paragraphStyle`段落的樣式將更新:

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

屬性繫結區分大小寫。 例如,`@bind`有效,`@Bind`並且無效。

## <a name="unparsable-values"></a>無法解析的值

當使用者向數據綁定元素提供不可解析的值時,在觸發綁定事件時,不可解析的值將自動還原到其上一個值。

請考慮下列案例：

* 元素`<input>`繫結到初始值`int`為`123`的型態:

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* 使用者將元素的值更新到`123.45`頁面中並更改元素焦點。

在前面的機制中,元素的值會回復到`123`。 當該值`123.45`被拒絕,以有利於的原始值`123`時 ,使用者會明白其值未被接受。

默認情況下,綁定應用於元素`onchange`的事件 (。`@bind="{PROPERTY OR FIELD}"` 用於`@bind="{PROPERTY OR FIELD}" @bind:event={EVENT}`觸發其他事件的綁定。 對於`oninput`事件`@bind:event="oninput"`( ), 還原發生在引入不可解析值的任何擊鍵之後。 使用`oninput``int`綁定類型定位事件時,將阻止用戶鍵`.`入字元。 字元`.`將立即刪除,因此使用者會立即收到僅允許整個數位的反饋。 在某些情況下,還原`oninput`事件上的值並不理想,例如何時應允許使用者清除`<input>`不可 解析的值。 替代專案包括:

* 不要使用該`oninput`事件。 使用預設`onchange`事件(僅`@bind="{PROPERTY OR FIELD}"`指定 ),其中無效值在元素失去焦點之前不會還原。
* 綁定到可無效類型(如`int?``string`或 ,並提供自定義邏輯來處理無效條目)。
* 使用[窗體驗證元件](xref:blazor/forms-validation),`InputNumber`如`InputDate`。 表單驗證元件具有管理無效輸入的內置支援。 表單驗證元件:
  * 允許使用者提供無效的輸入並在關聯的`EditContext`上接收驗證錯誤。
  * 在 UI 中顯示驗證錯誤,而不會干擾使用者輸入其他 Web 表單數據。

## <a name="format-strings"></a>格式化字串

資料繫結適用於<xref:System.DateTime>[`@bind:format`](xref:mvc/views/razor#bind)使用的格式字串。 其他格式運算式(如貨幣或數位格式)目前不可用。

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

在前面的代碼中,`<input>`元素的欄位型`type`態 (`text`) 預設為 。 `@bind:format`支援繫結以下 .NET 型態:

* <xref:System.DateTime?displayProperty=fullName>
* <xref:System.DateTime?displayProperty=fullName>?
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <xref:System.DateTimeOffset?displayProperty=fullName>?

該`@bind:format`屬性指定要應用`value``<input>`於 元素的日期格式。 該格式還用於在`onchange`事件發生時分析值。

不建議為`date`欄位類型指定格式,Blazor因為內置支援設定日期格式。 儘管有建議,但僅當提供`yyyy-MM-dd``date`欄位類型的格式時,才使用日期格式進行綁定才能正常工作:

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

## <a name="parent-to-child-binding-with-component-parameters"></a>使用元件參數的父子繫結

綁定可識別元件參數,其中`@bind-{PROPERTY}`可以將屬性值從父元件綁定到子元件。 從子級綁定到父級綁定在[帶有鏈綁定綁定的子到父綁定](#child-to-parent-binding-with-chained-bind)中涵蓋。

以下子元件 (`ChildComponent``Year`) 具有元件`YearChanged`參數 與回檔:

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

`EventCallback<T>`在中<xref:blazor/event-handling#eventcallback>解釋。

以下父元件使用:

* `ChildComponent`並將`ParentYear`參數從父參數綁定到子元件上的`Year`參數。
* 此`onclick`事件用於觸發`ChangeTheYear`方法 。 如需詳細資訊，請參閱 <xref:blazor/event-handling>。

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

載入`ParentComponent`產生以下標籤:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

如果透過選擇的`ParentYear``ParentComponent`按鈕更改屬性的值`Year``ChildComponent`, 則更新的屬性。 重新成`Year`成`ParentComponent`一個值 , 在 UI 中呈現的新值:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

參數`Year`是可綁定的,因為它具有`YearChanged``Year`與 參數類型匹配的配套事件。

依慣例,`<ChildComponent @bind-Year="ParentYear" />`基本上等同於寫作:

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

通常,屬性可以通過`@bind-{PROPRETY}:event`包含 屬性綁定到相應的事件處理程式。 例如,`MyProp`屬性可以使用以下兩個屬性繫`MyEventHandler`結到:

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="child-to-parent-binding-with-chained-bind"></a>具有鏈繫定的子到父綁定

常見方案是將數據綁定參數連結到元件輸出中的頁面元素。 此方案稱為*鏈綁定*,因為多個級別的綁定同時發生。

無法使用`@bind`頁面元素中的語法實現連結綁定。 事件處理程式和值必須單獨指定。 但是,父元件可以使用`@bind`語法與元件的參數。

以下`PasswordField`元件 (*密碼欄位. razor*):

* 將`<input>`元素的值設置到`Password`屬性。
* 使用`Password`[事件回調](xref:blazor/event-handling#eventcallback)將屬性的更改公開給父元件。
* 使用事件`onclick`用於觸`ToggleShowPassword`發 方法。 如需詳細資訊，請參閱 <xref:blazor/event-handling>。

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

此`PasswordField`元件用於另一個元件:

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<PasswordField @bind-Password="_password" />

@code {
    private string _password;
}
```

要在上述範例中對密碼執行檢查或捕捉錯誤,請執行以下操作:

* 為`Password`(`_password`在以下範例代碼中)建立後備欄位。
* 在`Password`設置器中執行檢查或陷阱錯誤。

如果密碼值中使用了空格,以下範例會立即向使用者提供回饋:

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

## <a name="radio-buttons"></a>選項按鈕

有關在表單選按鈕的資訊,請參閱<xref:blazor/forms-validation#work-with-radio-buttons>。
