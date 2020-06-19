---
title: ASP.NET Core 的 razor 語法參考
author: rick-anderson
description: 瞭解將 Razor 以伺服器為基礎的程式碼內嵌到網頁中的標記語法。
ms.author: riande
ms.date: 02/12/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/views/razor
ms.openlocfilehash: e85c9d384361f9169035e6a3ab8770e1a96b8650
ms.sourcegitcommit: 490434a700ba8c5ed24d849bd99d8489858538e3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85102727"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a>RazorASP.NET Core 的語法參考

由[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Taylor Mullen](https://twitter.com/ntaylormullen)和[Dan Vicarel](https://github.com/Rabadash8820)

Razor是將伺服器程式碼內嵌到網頁中的標記語法。 Razor語法包含 Razor 標記、c # 和 HTML。 通常包含 Razor 的檔案副檔名為 *.* #。 Razor也可以在[ Razor 元件](xref:blazor/components/index)檔案（*razor*）中找到。

## <a name="rendering-html"></a>轉譯 HTML

預設 Razor 語言為 HTML。 從標記呈現 HTML 與從 HTML 檔案轉譯 html Razor 並無不同。 伺服器會以不變的方式轉譯 *. cshtml*檔案中的 HTML 標籤 Razor 。

## <a name="razor-syntax"></a>Razor 語法

Razor支援 c #，並使用 `@` 符號從 HTML 轉換成 c #。 Razor評估 c # 運算式，並在 HTML 輸出中呈現它們。

當 `@` 符號後面接著[ Razor 保留關鍵字](#razor-reserved-keywords)時，它會轉換成 Razor 特定的標記。 否則會轉換成一般 C#。

若要 `@` 在標記中將符號換 Razor 用，請使用第二個 `@` 符號：

```cshtml
<p>@@Username</p>
```

此程式碼在 HTML 中是使用單一 `@` 符號轉譯：

```html
<p>@Username</p>
```

HTML 屬性及含有電子郵件地址的內容不會將 `@` 符號視為轉換字元。 下列範例中的電子郵件地址不會藉由剖析而改變 Razor ：

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>隱含 Razor 運算式

隱含 Razor 運算式的開頭為， `@` 後面接著 c # 程式碼：

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

除了 C# `await` 關鍵字以外，隱含運算式不能包含空格。 如果 C# 陳述式具有明確的結束字元，則可以混合空格：

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

隱含運算式「不能」**** 包含 C# 泛型，因為括弧 (`<>`) 內的字元會解譯為 HTML 標籤。 下列程式碼**無效**：

```cshtml
<p>@GenericMethod<int>()</p>
```

上述程式碼會產生類似下列其中一項的編譯器錯誤：

* "Int" 項目未關閉。 所有項目都必須自行結束或具有相對應的結束標籤。
* 無法將方法群組 'GenericMethod' 轉換成非委派類型 'type'。 您是否想要叫用方法？

泛型方法呼叫必須包裝在[明確 Razor 運算式](#explicit-razor-expressions)或程式[ Razor 代碼區塊](#razor-code-blocks)中。

## <a name="explicit-razor-expressions"></a>明確 Razor 運算式

明確 Razor 運算式是由 `@` 具有對稱括弧的符號所組成。 若要呈現上周的時間，則 Razor 會使用下列標記：

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

`@()` 括弧內的任何內容都會經過評估並轉譯成輸出。

上一節中所述的隱含運算式通常不能包含空格。 在下列程式碼中，不會從目前的時間減去一週：

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

程式碼會轉譯下列 HTML：

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

明確運算式可用來串連文字與運算式結果：

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

若沒有明確運算式，`<p>Age@joe.Age</p>` 會視為電子郵件地址，並轉譯 `<p>Age@joe.Age</p>`。 當撰寫為明確運算式時，會轉譯 `<p>Age33</p>`。

您可以使用明確運算式，透過 *.cshtml* 檔案中的泛型方法轉譯輸出。 下列標記會顯示如何修正稍早顯示的錯誤，此錯誤是由 C# 泛型的括弧所造成。 程式碼會撰寫為明確運算式：

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a>運算式編碼

評估為字串的 C# 運算式會以 HTML 編碼。 評估為 `IHtmlContent` 的 C# 運算式會透過 `IHtmlContent.WriteTo` 直接轉譯。 未評估為 `IHtmlContent` 的 C# 運算式會由 `ToString` 轉換成字串，並經過編碼再轉譯。

```cshtml
@("<span>Hello World</span>")
```

程式碼會轉譯下列 HTML：

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

此 HTML 在瀏覽器中會顯示為：

```html
<span>Hello World</span>
```

`HtmlHelper.Raw` 輸出不會經過編碼，而是轉譯為 HTML 標記。

> [!WARNING]
> 對未清理的使用者輸入使用 `HtmlHelper.Raw` 會造成安全性風險。 使用者輸入可能包含惡意的 JavaScript 或其他惡意探索。 清理使用者輸入並不容易。 請避免對使用者輸入使用 `HtmlHelper.Raw`。

```cshtml
@Html.Raw("<span>Hello World</span>")
```

程式碼會轉譯下列 HTML：

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Razor程式碼區塊

Razor程式碼區塊會以開頭 `@` ，並以括住 `{}` 。 不同於運算式，程式碼區塊內的 C# 程式碼不會轉譯。 一個檢視中的程式碼區塊和運算式會共用相同的範圍並依序定義：

```cshtml
@{
    var quote = "The future depends on what you do today. - Mahatma Gandhi";
}

<p>@quote</p>

@{
    quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";
}

<p>@quote</p>
```

程式碼會轉譯下列 HTML：

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

::: moniker range=">= aspnetcore-3.0"

在程式碼區塊中，使用標記宣告[區域函式](/dotnet/csharp/programming-guide/classes-and-structs/local-functions)作為樣板化方法：

```cshtml
@{
    void RenderName(string name)
    {
        <p>Name: <strong>@name</strong></p>
    }

    RenderName("Mahatma Gandhi");
    RenderName("Martin Luther King, Jr.");
}
```

程式碼會轉譯下列 HTML：

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="implicit-transitions"></a>隱含轉換

程式碼區塊中的預設語言是 c #，但 Razor 頁面可以轉換回 HTML：

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a>明確分隔的轉換

若要定義應該呈現 HTML 之程式碼區塊的子區段，請使用標記來括住要呈現的字元 Razor `<text>` ：

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

使用此方法可轉譯未以 HTML 標籤括住的 HTML。 如果沒有 HTML 或 Razor 標記， Razor 就會發生執行階段錯誤。

`<text>` 標籤可在轉譯內容時用來控制空白字元：

* 只會轉譯 `<text>` 標籤之間的內容。
* 在 HTML 輸出中，`<text>` 標籤前後都不能出現任何空白字元。

### <a name="explicit-line-transition"></a>明確行轉換

若要在程式碼區塊內將整行的其餘部分轉譯為 HTML，請使用 `@:` 語法：

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

如果 `@:` 程式碼中沒有，則 Razor 會產生執行階段錯誤。

檔案 `@` 中的額外字元 Razor 可能會導致稍後在區塊的語句中發生編譯器錯誤。 這些編譯器錯誤可能很難了解，因為實際錯誤發生在回報的錯誤之前。 將多個明確/隱含運算式合併成單一程式碼區塊之後，經常會出現此錯誤。

## <a name="control-structures"></a>控制結構

控制結構是程式碼區塊的延伸。 程式碼區塊的所有層面 (轉換成標記、內嵌 C#) 也適用於下列結構：

### <a name="conditionals-if-else-if-else-and-switch"></a>句式`@if, else if, else, and @switch`

`@if` 控制何時執行程式碼：

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

`else` 和 `else if` 不需要 `@` 符號：

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value is odd and small.</p>
}
```

下列標記示範如何使用 switch 陳述式：

```cshtml
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number wasn't 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a>遍歷`@for, @foreach, @while, and @do while`

樣板化 HTML 可以透過迴圈控制陳述式轉譯。 若要轉譯人員清單：

```cshtml
@{
    var people = new Person[]
    {
          new Person("Weston", 33),
          new Person("Johnathon", 41),
          ...
    };
}
```

支援的迴圈陳述式如下：

`@for`

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```cshtml
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```cshtml
@{ var i = 0; }
@while (i < people.Length)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
}
```

`@do while`

```cshtml
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a>複合 `@using`

在 C# 中，`using` 陳述式可用來確保物件經過處置。 在中 Razor ，您可以使用相同的機制來建立包含其他內容的 HTML 協助程式。 在下列程式碼中，HTML 協助程式會使用 `@using` 陳述式來轉譯 `<form>` 標籤：

```cshtml
@using (Html.BeginForm())
{
    <div>
        Email: <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

### `@try, catch, finally`

例外狀況處理會類似於 C#：

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### `@lock`

Razor具有使用 lock 語句來保護重要區段的功能：

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>註解

Razor支援 c # 和 HTML 批註：

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

程式碼會轉譯下列 HTML：

```html
<!-- HTML comment -->
```

Razor在轉譯網頁之前，伺服器會移除批註。 Razor用 `@*  *@` 來分隔批註。 下列程式碼會標記為註解，以確保伺服器不會轉譯任何標記：

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a>指示詞

Razor指示詞是以隱含運算式來表示，並在符號後面加上保留關鍵字 `@` 。 指示詞通常會變更檢視的剖析方式或啟用不同的功能。

瞭解如何 Razor 產生視圖的程式碼，可讓您更輕鬆地瞭解指示詞的工作方式。

[!code-cshtml[](razor/sample/Views/Home/Contact8.cshtml)]

程式碼會產生類似如下的類別：

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Getting old ain't for wimps! - Anonymous";

        WriteLiteral("/r/n<div>Quote of the Day: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

在本文稍後，[檢查 Razor 針對視圖產生的 c # 類別](#inspect-the-razor-c-class-generated-for-a-view)一節會說明如何查看這個產生的類別。

### `@attribute`

`@attribute` 指示詞會將指定屬性新增至所產生頁面或檢視的類別。 下列範例會新增 `[Authorize]` 屬性：

```cshtml
@attribute [Authorize]
```

::: moniker range=">= aspnetcore-3.0"

### `@code`

*此案例僅適用于 Razor 元件（razor）。*

`@code`區塊可讓[ Razor 元件](xref:blazor/components/index)將 c # 成員（欄位、屬性和方法）新增至元件：

```razor
@code {
    // C# members (fields, properties, and methods)
}
```

針對 Razor 元件， `@code` 是的別名， [`@functions`](#functions) 並建議使用 `@functions` 。 允許一個以上的 `@code` 區塊。

::: moniker-end

### `@functions`

`@functions` 指示詞能夠將 C# 成員 (欄位、屬性和方法) 新增至產生的類別：

```cshtml
@functions {
    // C# members (fields, properties, and methods)
}
```

::: moniker range=">= aspnetcore-3.0"

在[ Razor 元件](xref:blazor/components/index)中，請使用 `@code` Over `@functions` 來新增 c # 成員。

::: moniker-end

例如：

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

程式碼會產生下列 HTML 標記：

```html
<div>From method: Hello</div>
```

下列程式碼是產生的 Razor c # 類別：

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

::: moniker range=">= aspnetcore-3.0"

具有標記時，`@functions` 方法會作為樣板化方法：

```cshtml
@{
    RenderName("Mahatma Gandhi");
    RenderName("Martin Luther King, Jr.");
}

@functions {
    private void RenderName(string name)
    {
        <p>Name: <strong>@name</strong></p>
    }
}
```

程式碼會轉譯下列 HTML：

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

### `@implements`

`@implements` 指示詞會針對產生的類別實作介面。

下列範例會實作 <xref:System.IDisposable?displayProperty=fullName>，如此便能呼叫 <xref:System.IDisposable.Dispose*> 方法：

```cshtml
@implements IDisposable

<h1>Example</h1>

@functions {
    private bool _isDisposed;

    ...

    public void Dispose() => _isDisposed = true;
}
```

::: moniker-end

### `@inherits`

`@inherits` 指示詞可讓您完全控制檢視所繼承的類別：

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

下列程式碼是自訂 Razor 頁面類型：

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

`CustomText` 會顯示在檢視中：

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

程式碼會轉譯下列 HTML：

```html
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

 `@model` 和 `@inherits` 可用於相同的檢視。 `@inherits` 可能位於檢視匯入的 *_ViewImports.cshtml* 檔案中：

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

下列程式碼是強型別檢視的範例：

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

如果模型中傳遞了 "rick@contoso.com"，檢視會產生下列 HTML 標記：

```html
<div>The Login Email: rick@contoso.com</div>
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

### `@inject`

指示詞 `@inject` 可讓 Razor 頁面將服務從[服務容器](xref:fundamentals/dependency-injection)插入到視圖中。 如需詳細資訊，請參閱[在檢視中插入相依性](xref:mvc/views/dependency-injection)。

::: moniker range=">= aspnetcore-3.0"

### `@layout`

*此案例僅適用于 Razor 元件（razor）。*

指示詞會 `@layout` 指定元件的版面配置 Razor 。 版面配置元件可用來避免程式碼重複和不一致。 如需詳細資訊，請參閱 <xref:blazor/layouts> 。

::: moniker-end

### `@model`

*此案例僅適用于 MVC 視圖和 Razor 頁面（. cshtml）。*

`@model` 指示詞會指定傳遞至檢視或頁面的模型類型：

```cshtml
@model TypeNameOfModel
```

在以 Razor 個別使用者帳戶建立的 ASP.NET CORE MVC 或 Pages 應用程式中， *Views/Account/Login。 cshtml*包含下列模型宣告：

```cshtml
@model LoginViewModel
```

產生的類別繼承自 `RazorPage<dynamic>`：

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Razor公開 `Model` 屬性，以存取傳遞至視圖的模型：

```cshtml
<div>The Login Email: @Model.Email</div>
```

`@model` 指示詞會指定 `Model` 屬性的類型。 該指示詞會將 `RazorPage<T>` 中的 `T` 指定為檢視從中衍生的產生類別。 若未指定 `@model` 指示詞，`Model` 屬性的類型為 `dynamic`。 如需詳細資訊，請參閱強型別[模型和 @model 關鍵字](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword)。

### `@namespace`

`@namespace` 指示詞：

* 設定所產生 Razor 頁面、MVC 視圖或元件之類別的命名空間 Razor 。
* 從目錄樹狀結構中最接近的匯入檔案，設定頁面、views 或 components 類別的根衍生命名空間， *_ViewImports. cshtml* （views 或 pages）或 *_Imports razor* （ Razor 元件）。

```cshtml
@namespace Your.Namespace.Here
```

針對 Razor 下表所示的頁面範例：

* 每個頁面都會匯入 *Pages/_ViewImports.cshtml*。
* *Pages/_ViewImports.cshtml* 包含 `@namespace Hello.World`。
* 每個頁面都有 `Hello.World` 作為其命名空間的根目錄。

| 頁面                                        | 命名空間                             |
| ------------------------------------------- | ------------------------------------- |
| *Pages/Index. cshtml*                        | `Hello.World`                         |
| *Pages/MorePages/Page.cshtml*               | `Hello.World.MorePages`               |
| *Pages/MorePages/EvenMorePages/Page.cshtml* | `Hello.World.MorePages.EvenMorePages` |

前述關聯性適用于匯入用於 MVC 視圖和元件的檔案 Razor 。

當多個匯入檔案具有 `@namespace` 指示詞時，會使用目錄樹狀結構中最接近頁面、檢視或元件的檔案來設定根命名空間。

如果上述範例中的 *EvenMorePages* 資料夾具備含 `@namespace Another.Planet` 的匯入檔案 (或者 *Pages/MorePages/EvenMorePages/Page.cshtml* 檔案包含 `@namespace Another.Planet`)，則結果如下表所示。

| 頁面                                        | 命名空間               |
| ------------------------------------------- | ----------------------- |
| *Pages/Index. cshtml*                        | `Hello.World`           |
| *Pages/MorePages/Page.cshtml*               | `Hello.World.MorePages` |
| *Pages/MorePages/EvenMorePages/Page.cshtml* | `Another.Planet`        |

### `@page`

::: moniker range=">= aspnetcore-3.0"

`@page` 指示詞會根據其出現的檔案類型而有不同的效果。 指示詞：

* 在中， *cshtml*檔案表示檔案是 Razor 頁面。 如需詳細資訊，請參閱[自訂路由](xref:razor-pages/index#custom-routes)和 <xref:razor-pages/index> 。
* 指定 Razor 元件應直接處理要求。 如需詳細資訊，請參閱 <xref:blazor/fundamentals/routing> 。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

在 `@page` *cshtml*檔案第一行的指示詞表示檔案是 Razor 頁面。 如需詳細資訊，請參閱 <xref:razor-pages/index> 。

::: moniker-end

### `@section`

*此案例僅適用于 MVC 視圖和 Razor 頁面（. cshtml）。*

指示詞 `@section` 會與[MVC 和 Razor 頁面配置](xref:mvc/views/layout)搭配使用，讓視圖或頁面能夠在 HTML 網頁的不同部分呈現內容。 如需詳細資訊，請參閱 <xref:mvc/views/layout> 。

### `@using`

`@using` 指示詞會將 C# `using` 指示詞新增至產生的檢視：

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

::: moniker range=">= aspnetcore-3.0"

在[ Razor 元件](xref:blazor/components/index)中， `@using` 也會控制哪些元件在範圍內。

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="directive-attributes"></a>指示詞屬性

Razor指示詞屬性會以隱含運算式表示，並在符號後面加上保留關鍵字 `@` 。 指示詞屬性通常會變更剖析元素或啟用不同功能的方式。

### `@attributes`

*此案例僅適用于 Razor 元件（razor）。*

`@attributes` 允許元件轉譯非宣告的屬性。 如需詳細資訊，請參閱 <xref:blazor/components/index#attribute-splatting-and-arbitrary-parameters> 。

### `@bind`

*此案例僅適用于 Razor 元件（razor）。*

元件中的資料繫結會使用 `@bind` 屬性來完成。 如需詳細資訊，請參閱 <xref:blazor/components/data-binding> 。

### `@on{EVENT}`

*此案例僅適用于 Razor 元件（razor）。*

Razor提供元件的事件處理功能。 如需詳細資訊，請參閱 <xref:blazor/components/event-handling> 。

::: moniker-end

::: moniker range=">= aspnetcore-3.1"

### `@on{EVENT}:preventDefault`

*此案例僅適用于 Razor 元件（razor）。*

防止事件的預設動作。

### `@on{EVENT}:stopPropagation`

*此案例僅適用于 Razor 元件（razor）。*

停止事件的事件傳播。

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### `@key`

*此案例僅適用于 Razor 元件（razor）。*

`@key` 指示詞屬性導致元件差異比較演算法會根據索引鍵的值來保證元素或元件的保留。 如需詳細資訊，請參閱 <xref:blazor/components/index#use-key-to-control-the-preservation-of-elements-and-components> 。

### `@ref`

*此案例僅適用于 Razor 元件（razor）。*

元件參考 (`@ref`) 提供一種方式來參考元件執行個體，讓您可以對該執行個體發出命令。 如需詳細資訊，請參閱 <xref:blazor/components/index#capture-references-to-components> 。

### `@typeparam`

*此案例僅適用于 Razor 元件（razor）。*

指示詞會 `@typeparam` 為所產生的元件類別宣告泛型型別參數。 如需詳細資訊，請參閱 <xref:blazor/components/templated-components#generic-typed-components> 。

::: moniker-end

## <a name="templated-razor-delegates"></a>樣板化 Razor 委派

Razor範本可讓您定義具有下列格式的 UI 程式碼片段：

```cshtml
@<tag>...</tag>
```

下列範例說明如何將樣板化委派指定 Razor 為 <xref:System.Func%602> 。 該範例會指定 [dynamic 類型](/dotnet/csharp/programming-guide/types/using-type-dynamic)作為委派所封裝方法的參數。 並指定 [object 類型](/dotnet/csharp/language-reference/keywords/object)作為委派的傳回值。 此範本會搭配具有 `Name` 屬性之 `Pet` 的 <xref:System.Collections.Generic.List%601> 來使用。

```csharp
public class Pet
{
    public string Name { get; set; }
}
```

```cshtml
@{
    Func<dynamic, object> petTemplate = @<p>You have a pet named <strong>@item.Name</strong>.</p>;

    var pets = new List<Pet>
    {
        new Pet { Name = "Rin Tin Tin" },
        new Pet { Name = "Mr. Bigglesworth" },
        new Pet { Name = "K-9" }
    };
}
```

此範本使用 `foreach` 陳述式所提供的 `pets` 進行轉譯：

```cshtml
@foreach (var pet in pets)
{
    @petTemplate(pet)
}
```

轉譯輸出：

```html
<p>You have a pet named <strong>Rin Tin Tin</strong>.</p>
<p>You have a pet named <strong>Mr. Bigglesworth</strong>.</p>
<p>You have a pet named <strong>K-9</strong>.</p>
```

您也可以提供內嵌 Razor 範本做為方法的引數。 在下列範例中， `Repeat` 方法會接收 Razor 範本。 此方法使用範本來產生 HTML 內容，並重複出現清單所提供的項目：

```cshtml
@using Microsoft.AspNetCore.Html

@functions {
    public static IHtmlContent Repeat(IEnumerable<dynamic> items, int times,
        Func<dynamic, IHtmlContent> template)
    {
        var html = new HtmlContentBuilder();

        foreach (var item in items)
        {
            for (var i = 0; i < times; i++)
            {
                html.AppendHtml(template(item));
            }
        }

        return html;
    }
}
```

使用先前範例中的寵物清單，呼叫 `Repeat` 方法並指定：

* <xref:System.Collections.Generic.List%601> 的 `Pet`。
* 每個寵物的重複次數。
* 用於未排序清單中清單項目的內嵌範本。

```cshtml
<ul>
    @Repeat(pets, 3, @<li>@item.Name</li>)
</ul>
```

轉譯輸出：

```html
<ul>
    <li>Rin Tin Tin</li>
    <li>Rin Tin Tin</li>
    <li>Rin Tin Tin</li>
    <li>Mr. Bigglesworth</li>
    <li>Mr. Bigglesworth</li>
    <li>Mr. Bigglesworth</li>
    <li>K-9</li>
    <li>K-9</li>
    <li>K-9</li>
</ul>
```

## <a name="tag-helpers"></a>標籤協助程式

*此案例僅適用于 MVC 視圖和 Razor 頁面（. cshtml）。*

[標籤協助程式](xref:mvc/views/tag-helpers/intro)有三個相關的指示詞。

| 指示詞 | 函式 |
| --------- | -------- |
| [`@addTagHelper`](xref:mvc/views/tag-helpers/intro#add-helper-label) | 使標籤協助程式可供檢視。 |
| [`@removeTagHelper`](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | 移除先前從檢視新增的標籤協助程式。 |
| [`@tagHelperPrefix`](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | 指定標籤前置字元，以啟用標籤協助程式支援，並將標籤協助程式使用方式設為明確。 |

## <a name="razor-reserved-keywords"></a>Razor保留的關鍵字

### <a name="razor-keywords"></a>Razor字

* `page`（需要 ASP.NET Core 2.1 或更新版本）
* `namespace`
* `functions`
* `inherits`
* `model`
* `section`
* `helper`（目前不支援 ASP.NET Core）

Razor關鍵字會以進行轉義 `@(Razor Keyword)` （例如， `@(functions)` ）。

### <a name="c-razor-keywords"></a>C # Razor 關鍵字

* `case`
* `do`
* `default`
* `for`
* `foreach`
* `if`
* `else`
* `lock`
* `switch`
* `try`
* `catch`
* `finally`
* `using`
* `while`

C # Razor 關鍵字必須使用（例如）進行雙重轉義 `@(@C# Razor Keyword)` `@(@case)` 。 第一個會將剖析器進行 `@` 轉義 Razor 。 第二個 `@` 會將 C# 剖析器逸出。

### <a name="reserved-keywords-not-used-by-razor"></a>未使用的保留關鍵字Razor

* `class`

## <a name="inspect-the-razor-c-class-generated-for-a-view"></a>檢查 Razor 為視圖產生的 c # 類別

::: moniker range=">= aspnetcore-2.1"

在 .NET Core SDK 2.1 或更新版本中， [ Razor SDK](xref:razor-pages/sdk)會處理檔案的編譯 Razor 。 建立專案時，SDK 會 Razor 在專案根目錄中產生*obj/<build_configuration>/<target_framework_moniker Razor>/* 目錄。 目錄中的目錄結構會 *Razor* 反映專案的目錄結構。

在以 .NET Core 2.1 為目標的 ASP.NET Core 2.1 頁面專案中，請考慮下列目錄結構 Razor ：

```
 Areas/
   Admin/
     Pages/
       Index.cshtml
       Index.cshtml.cs
 Pages/
   Shared/
     _Layout.cshtml
   _ViewImports.cshtml
   _ViewStart.cshtml
   Index.cshtml
   Index.cshtml.cs
  ```

在 *Debug* 設定中建置專案會產生下列 *obj* 目錄：

```
 obj/
   Debug/
     netcoreapp2.1/
       Razor/
         Areas/
           Admin/
             Pages/
               Index.g.cshtml.cs
         Pages/
           Shared/
             _Layout.g.cshtml.cs
           _ViewImports.g.cshtml.cs
           _ViewStart.g.cshtml.cs
           Index.g.cshtml.cs
```

若要查看針對*Pages/Index*所產生的類別，請開啟*obj/Debug/netcoreapp 2.1/ Razor /Pages/Index.g.cshtml.cs*。

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

請將下列類別新增至 ASP.NET Core MVC 專案：

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

在 `Startup.ConfigureServices` 中，將 MVC 所新增的 `RazorTemplateEngine` 覆寫為 `CustomTemplateEngine` 類別：

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

在 `CustomTemplateEngine` 的 `return csharpDocument;` 陳述式上設定中斷點。 當程式在中斷點停止執行時，請檢視 `generatedCode` 的值。

![generatedCode 的文字視覺化檢視](razor/_static/tvr.png)

::: moniker-end

## <a name="view-lookups-and-case-sensitivity"></a>檢視查閱和區分大小寫

RazorView engine 會針對 views 執行區分大小寫的查閱。 不過，實際查閱則取決於基礎檔案系統：

* 檔案式來源：
  * 在具有不區分大小寫之檔案系統的作業系統上 (例如 Windows)，實體檔案提供者查閱不會區分大小寫。 例如，`return View("Test")` 針對 */Views/Home/Test.cshtml* 和 */Views/home/test.cshtml* (以及任何其他大小寫變體) 會有相符的結果。
  * 在區分大小寫的檔案系統上 (例如 Linux、OSX 及使用 `EmbeddedFileProvider`)，查閱會區分大小寫。 例如，`return View("Test")` 會明確符合 */Views/Home/Test.cshtml*。
* 先行編譯的檢視：在 ASP.NET Core 2.0 和更新版本中，在所有作業系統上查閱先行編譯的檢視不會區分大小寫。 此行為與 Windows 上之實體檔案提供者的行為相同。 如果兩個先行編譯的檢視只有大小寫不同，查閱的結果不會由此決定。

建議開發人員比對檔案和目錄的大小寫以及下列項目的大小寫：

* 區域、控制器和動作名稱。
* Razor頁面.

比對大小寫可確保不論基礎檔案系統為何，部署作業都能夠找到其值。

## <a name="additional-resources"></a>其他資源

[使用 ASP.NET Web 程式設計的 Razor 簡介語法](/aspnet/web-pages/overview/getting-started/introducing-razor-syntax-c)提供許多使用語法進行程式設計的範例 Razor 。
