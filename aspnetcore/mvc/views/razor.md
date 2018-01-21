---
title: "適用於 ASP.NET Core razor 語法參考"
author: rick-anderson
description: "深入了解伺服器基礎的程式碼內嵌到網頁的 Razor 標記語法。"
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: d932e28246998c60e2b3f9c77a2521fe55991e85
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="razor-syntax-for-aspnet-core"></a>Razor 語法的 ASP.NET Core

由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Luke Latham](https://github.com/guardrex)， [Taylor Mullen](https://twitter.com/ntaylormullen)，和[Dan Vicarel](https://github.com/Rabadash8820)

Razor 是將伺服器端程式碼內嵌到網頁標記語法。 Razor 語法包含 Razor 標記、 C# 和 HTML。 通常包含 Razor 檔案有*.cshtml*檔案副檔名。

## <a name="rendering-html"></a>將 HTML 呈現

預設 Razor 語言為 HTML。 轉譯 HTML Razor 標記中的並無不同轉譯 HTML，從 HTML 檔案。 中的 HTML 標記*.cshtml* Razor 檔案會呈現未變更的伺服器。

## <a name="razor-syntax"></a>Razor 語法

Razor 支援 C#，並使用`@`轉換來自 HTML，以 C# 中的符號。 Razor 評估 C# 運算式，並將它們呈現的 HTML 輸出中。

當`@`符號後面[Razor 保留關鍵字](#razor-reserved-keywords)，它就會轉換成特定的 Razor 標記。 否則，會轉換成一般的 C#。

要逸出`@`Razor 標記中的符號，請使用第二個`@`符號：

```cshtml
<p>@@Username</p>
```

程式碼會以 HTML 轉譯單一`@`符號：

```html
<p>@Username</p>
```

HTML 屬性和內容包含電子郵件地址不會將`@`符號為轉換字元。 電子郵件地址，在下列範例中被不受影響的 Razor 剖析：

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>Razor 的隱含運算式

Razor 的隱含運算式開頭`@`後面接著 C# 程式碼：

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

除了 C#`await`關鍵字，隱含運算式不能包含空格。 如果 C# 陳述式已清除結束，混合空格：

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

隱含運算式**無法**包含 C# 泛型，括號內的字元 (`<>`) 會解譯為 HTML 標記。 下列程式碼是**不**有效：

```cshtml
<p>@GenericMethod<int>()</p>
```

上述程式碼會產生編譯器錯誤類似下列其中一項：

 * "Int"項目未結束。 所有元素都必須自行關閉或沒有對稱的結束標記。
 * 無法將方法群組 'GenericMethod' 為非委派類型 'object' 的轉換。 您是否想要叫用的方法？ ' 
 
泛型方法的呼叫必須包裝在[明確 Razor 運算式](#explicit-razor-expressions)或[Razor 程式碼區塊](#razor-code-blocks)。

## <a name="explicit-razor-expressions"></a>Razor 的明確運算式

Razor 的明確運算式組成`@`有對稱的括號的符號。 若要轉譯上週的時間，會使用下列 Razor 標記：

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

內的任何內容`@()`括號會評估並呈現至輸出。

上一節中所述的隱含運算式通常不能包含空格。 下列程式碼中，一週未減去目前的時間：

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

程式碼會轉譯下列 HTML:

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

明確運算式可以用來串連文字與運算式結果：

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

沒有明確的運算式，`<p>Age@joe.Age</p>`會被視為電子郵件地址和`<p>Age@joe.Age</p>`轉譯。 做為明確的運算式，寫入時`<p>Age33</p>`轉譯。


明確運算式可以用來呈現在泛型方法的輸出*.cshtml*檔案。 在隱含運算式中，括號內的字元 (`<>`) 會解譯為 HTML 標記。 下列標記是**不**有效 Razor:

```cshtml
<p>@GenericMethod<int>()</p>
```

上述程式碼會產生編譯器錯誤類似下列其中一項：

 * "Int"項目未結束。 所有元素都必須自行關閉或沒有對稱的結束標記。
 * 無法將方法群組 'GenericMethod' 為非委派類型 'object' 的轉換。 您是否想要叫用的方法？ ' 
 
 下列標記會顯示正確的方式寫入這段程式碼。 程式碼會寫入做為明確運算式：

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a>運算式的編碼方式

C# 運算式，評估為字串會以 HTML 編碼。 C# 運算式會評估得出`IHtmlContent`轉譯直接透過`IHtmlContent.WriteTo`。 C# 運算式未評估成`IHtmlContent`會轉換成字串`ToString`和編碼它們在呈現之前。

```cshtml
@("<span>Hello World</span>")
```

程式碼會轉譯下列 HTML:

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

HTML 會顯示為瀏覽器：

```
<span>Hello World</span>
```

`HtmlHelper.Raw`輸出不編碼，但轉譯為 HTML 標記。

> [!WARNING]
> 使用`HtmlHelper.Raw`unsanitized 使用者輸入會造成安全性風險。 使用者輸入可能包含惡意的 JavaScript 或其他入侵。 免於使用者輸入並不容易。 請避免使用`HtmlHelper.Raw`與使用者輸入。

```cshtml
@Html.Raw("<span>Hello World</span>")
```

程式碼會轉譯下列 HTML:

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Razor 程式碼區塊

Razor 程式碼區塊的開頭`@`和由`{}`。 不同於運算式中，不會呈現 C# 程式碼區塊內的程式碼。 程式碼區塊和檢視中的運算式共用相同範圍和順序中所定義：

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

程式碼會轉譯下列 HTML:

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a>隱含轉換

程式碼區塊中的預設語言是 C# 中，但 Razor 頁面可以轉換回 HTML:

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a>明確分隔的轉換

若要定義應該呈現的 HTML 程式碼區塊的子區段，使用 Razor 括住的字元轉譯**\<文字 >**標記：

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

使用此方式來呈現不以 HTML 標記括住的 HTML。 沒有 HTML 或 Razor 的標籤，就會發生 Razor 執行階段錯誤。

**\<文字 >**標記可用於轉譯內容時控制空白字元：

* 之間的內容**\<文字 >**標記的呈現。 
* 不能有空白之前或之後**\<文字 >** HTML 輸出中會顯示標籤。

### <a name="explicit-line-transition-with-"></a>使用明確列轉換 @:

若要轉譯為 HTML 的整行的其餘部分的程式碼區塊內，使用`@:`語法：

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

不含`@:`在程式碼會產生 Razor 的執行階段錯誤。

警告： 額外`@`Razor 檔案中的字元可能會造成在陳述式會導致編譯器錯誤稍後區塊。 這些編譯器錯誤很難了解，因為實際的錯誤發生之前報告的錯誤。 此錯誤之後結合成單一的程式碼區塊的多個隱含/明確運算式是常見的。

## <a name="control-structures"></a>控制結構

控制結構是一種擴充的程式碼區塊。 所有層面的程式碼區塊 （轉換到標記中，內嵌 C#） 也適都用於下列結構：

### <a name="conditionals-if-else-if-else-and-switch"></a>條件式@if，else 的 if、 和@switch

`@if`控制項時執行程式碼：

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

`else`和`else if`不需要`@`符號：

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

下列標記會顯示如何使用 switch 陳述式：

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

### <a name="looping-for-foreach-while-and-do-while"></a>迴圈@for， @foreach， @while，和@do時

樣板化 HTML 可以轉譯迴圈控制陳述式。 若要轉譯的人員清單：

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

支援下列的迴圈陳述式：

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

### <a name="compound-using"></a>複合@using

在 C# 中，`using`陳述式用來確保處置物件。 在 Razor，相同的機制用來建立包含其他內容的 HTML Helper。 下列程式碼中，HTML Helper 呈現表單標記與`@using`陳述式：


```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

範圍層級的動作可以使用執行[標記協助程式](xref:mvc/views/tag-helpers/intro)。

### <a name="try-catch-finally"></a>@trycatch、 finally

例外狀況處理會類似於 C#:

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

Razor 具有保護與 lock 陳述式的關鍵區段的功能：

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>註解

Razor 支援 C# 和 HTML 註解：

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

程式碼會轉譯下列 HTML:

```html
<!-- HTML comment -->
```

呈現網頁之前，伺服器會移除 razor 註解。 使用 razor`@*  *@`來分隔註解。 下列程式碼會標記為註解，讓伺服器不會呈現任何標記：

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

Razor 指示詞都由下列保留的關鍵字的隱含運算式`@`符號。 指示詞通常會變更的方式，檢視會剖析或啟用不同的功能。

了解 Razor 如何產生程式碼 檢視，可以更輕鬆地了解指示詞的運作方式。

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

程式碼會產生的類別如下所示：

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

在本文中 > 一節稍後[檢視產生檢視的 Razor C# 類別](#viewing-the-razor-c-class-generated-for-a-view)說明如何檢視這個產生的類別。

### <a name="using"></a>@using

`@using`指示詞加入 C#`using`指示詞加入產生的檢視：

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

`@model`指示詞會指定傳遞至檢視的模型型別：

```cshtml
@model TypeNameOfModel
```

在 ASP.NET Core MVC 應用程式中使用個別的使用者帳戶建立*Views/Account/Login.cshtml*檢視包含下列模型宣告：

```cshtml
@model LoginViewModel
```

產生的類別繼承自`RazorPage<dynamic>`:

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Razor 公開`Model`屬性，以存取模型傳遞至檢視：

```cshtml
<div>The Login Email: @Model.Email</div>
```

`@model`指示詞會指定此屬性的型別。 指示詞會指定`T`中`RazorPage<T>`，所產生類別，衍生自的檢視。 如果`@model`指示詞並不指定，`Model`屬性屬於型別`dynamic`。 模型值會從控制器到檢視。 如需詳細資訊，請參閱 [強類型模型和@model關鍵字。

### <a name="inherits"></a>@inherits

`@inherits`指示詞提供的檢視所繼承的類別的完整控制權：

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

下列程式碼是自訂的 Razor 頁面類型：

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

`CustomText`會顯示在檢視中：

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

程式碼會轉譯下列 HTML:

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 `@model`和`@inherits`可以使用相同的檢視中。 `@inherits`可以是*_ViewImports.cshtml*檢視匯入的檔案：

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

下列程式碼是強型別檢視的範例：

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

如果 「rick@contoso.com」 會在模型中，檢視就會產生下列的 HTML 標記：

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


`@inject`指示詞可讓 Razor 頁面，將服務從[服務容器](xref:fundamentals/dependency-injection)插入檢視。 如需詳細資訊，請參閱[檢視的相依性插入](xref:mvc/views/dependency-injection)。

### <a name="functions"></a>@functions

`@functions`指示詞可讓 Razor 頁面將函式層級內容加入至檢視：

```cshtml
@functions { // C# Code }
```

例如: 

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

程式碼會產生下列的 HTML 標記：

```html
<div>From method: Hello</div>
```

下列程式碼會產生的 Razor C# 類別：

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

`@section`指示詞用於搭配[配置](xref:mvc/views/layout)啟用檢視轉譯的 HTML 網頁的不同部分中的內容。 如需詳細資訊，請參閱[區段](xref:mvc/views/layout#layout-sections-label)。

## <a name="tag-helpers"></a>標記協助程式

有三個指示詞與[標記協助程式](xref:mvc/views/tag-helpers/intro)。

| 指示詞 | 功能 |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | 使標記協助程式可供檢視。 |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | 移除先前加入從檢視表的標記協助程式。 |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | 指定啟用標記協助程式支援，且能夠明確標記協助程式使用方式的標記首碼。 |

## <a name="razor-reserved-keywords"></a>Razor 保留關鍵字

### <a name="razor-keywords"></a>Razor 關鍵字

* 頁面 （需要 ASP.NET Core 2.0 和更新版本）
* 函式
* 繼承
* model
* section
* （目前不支援 ASP.NET Core） 的協助程式

Razor 關鍵字會以逸出`@(Razor Keyword)`(例如， `@(functions)`)。

### <a name="c-razor-keywords"></a>C# Razor 關鍵字

* 大小寫
* do
* default
* for
* foreach
* if
* else
* lock
* switch
* try
* catch
* finally
* 使用
* while

C# Razor 關鍵字必須是雙逸出與`@(@C# Razor Keyword)`(例如， `@(@case)`)。 第一個`@`逸出 Razor 剖析器。 第二個`@`逸出的 C# 剖析器。

### <a name="reserved-keywords-not-used-by-razor"></a>不使用 Razor 的保留的關鍵字

* namespace
* Class - 類別

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>檢視檢視產生的 Razor C# 類別

加入 ASP.NET Core MVC 專案中的下列類別：

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

覆寫`RazorTemplateEngine`加入與 MVC`CustomTemplateEngine`類別：

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

在 設定中斷點`return csharpDocument`陳述式的`CustomTemplateEngine`。 當在中斷點停止執行程式時，檢視 值`generatedCode`。

![GeneratedCode 文字視覺化檢視](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a>檢視對應及區分大小寫

Razor 檢視引擎會執行檢視的區分大小寫的查閱。 不過，實際查閱是由基礎檔案系統的方式決定：

* 根據的來源檔案： 
  * 在作業系統上使用不區分大小寫的檔案系統 (例如 Windows)，實體檔案提供者對應是不區分大小寫。 例如，`return View("Test")`符合的項目會導致*/Views/Home/Test.cshtml*， */Views/home/test.cshtml*，和任何其他大小寫變數。
  * 在區分大小寫的檔案系統上 (例如 Linux、 OSX，與`EmbeddedFileProvider`)，查詢不區分大小寫。 例如，`return View("Test")`特別相符項目*/Views/Home/Test.cshtml*。
* 先行編譯的檢視： 使用 ASP.NET Core 2.0 和更新版本，查閱先行編譯的檢視是不區分大小寫在所有作業系統上。 行為是在 Windows 上的實體檔案提供者的行為相同。 如果兩個先行編譯的檢視只有大小寫不同，查詢的結果會是不具決定性。

開發人員都要比對的大小寫的檔案和目錄名稱的大小寫：

    * 區域、 控制器和動作名稱。 
    * Razor 頁面。
    
相符的情況下，可確保部署尋找及其不論基礎檔案系統的檢視。
