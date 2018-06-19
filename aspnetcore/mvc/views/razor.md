---
title: ASP.NET Core 的 Razor 語法參考
author: rick-anderson
description: 了解將伺服器架構程式碼內嵌到網頁中的 Razor 標記語法。
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/razor
ms.openlocfilehash: 224c855b355b8ecde36377bba6966edec251af6a
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/10/2018
ms.locfileid: "33962488"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a>ASP.NET Core 的 Razor 語法參考

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Luke Latham](https://github.com/guardrex)、[Taylor Mullen](https://twitter.com/ntaylormullen) 與 [Dan Vicarel](https://github.com/Rabadash8820)

Razor 是將伺服器架構程式碼內嵌到網頁中的標記語法。 Razor 語法是由 Razor 標記、C# 和 HTML 所組成。 含有 Razor 的檔案通常具有 *.cshtml* 副檔名。

## <a name="rendering-html"></a>轉譯 HTML

Razor 語言預設為 HTML。 轉譯 Razor 標記中的 HTML 與轉譯 HTML 檔案中的 HTML 並無不同。 伺服器會依原狀轉譯 *.cshtml* Razor 檔案中的 HTML 標記。

## <a name="razor-syntax"></a>Razor 語法

Razor 支援 C#，並使用 `@` 符號從 HTML 轉換成 C#。 Razor 會評估 C# 運算式並轉譯成 HTML 輸出。

當 `@` 符號後面接著 [Razor 保留關鍵字](#razor-reserved-keywords)時，它會轉換成 Razor 特定標記； 否則會轉換成一般 C#。

若要將 Razor 標記中的 `@` 符號逸出，請使用第二個 `@` 符號：

```cshtml
<p>@@Username</p>
```

此程式碼在 HTML 中是使用單一 `@` 符號轉譯：

```html
<p>@Username</p>
```

HTML 屬性及含有電子郵件地址的內容不會將 `@` 符號視為轉換字元。 Razor 剖析不會處理下列範例中的電子郵件地址：

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>Razor 隱含運算式

Razor 隱含運算式會以 `@` 開頭，後面接著 C# 程式碼：

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

除了 C# `await` 關鍵字以外，隱含運算式不能包含空格。 如果 C# 陳述式具有明確的結束字元，則可以混合空格：

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

隱含運算式「不能」包含 C# 泛型，因為括弧 (`<>`) 內的字元會解譯為 HTML 標籤。 下列程式碼**無效**：

```cshtml
<p>@GenericMethod<int>()</p>
```

上述程式碼會產生類似下列其中一項的編譯器錯誤：

 * "Int" 項目未關閉。 所有項目都必須自行結束或具有相對應的結束標籤。
 *  無法將方法群組 'GenericMethod' 轉換成非委派類型 'type'。 您是否想要叫用方法？ 
 
泛型方法呼叫必須包裝在 [Razor 明確運算式](#explicit-razor-expressions)或 [Razor 程式碼區塊](#razor-code-blocks)中。

## <a name="explicit-razor-expressions"></a>Razor 明確運算式

Razor 明確運算式是由 `@` 符號與對稱的括弧所組成。 為了轉譯上週的時間，使用了下列 Razor 標記：

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

```
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

## <a name="razor-code-blocks"></a>Razor 程式碼區塊

Razor 程式碼區塊會以 `@` 開頭，並以 `{}` 括住。 不同於運算式，程式碼區塊內的 C# 程式碼不會轉譯。 一個檢視中的程式碼區塊和運算式會共用相同的範圍並依序定義：

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

### <a name="implicit-transitions"></a>隱含轉換

程式碼區塊中的預設語言是 C#，但 Razor Page 可以轉換回 HTML：

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a>明確分隔的轉換

若要定義程式碼區塊中應該轉譯 HTML 的子區段，請使用 Razor **\<text>** 標籤括住要轉譯的字元：

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

使用此方法可轉譯未以 HTML 標籤括住的 HTML。 若沒有 HTML 或 Razor 標籤，就會發生 Razor 執行階段錯誤。

**\<text>** 標籤可用來控制轉譯內容時的空白字元：

* **\<text>** 標籤之間的內容會轉譯。 
* **\<text>** 標籤前後不能有空白字元出現在 HTML 輸出中。

### <a name="explicit-line-transition-with-"></a>使用 @ 的明確行轉換：

若要將一整行的其餘部分轉譯為程式碼區塊內的 HTML，請使用 `@:` 語法：

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

若程式碼中沒有 `@:`，就會產生 Razor 執行階段錯誤。

警告：Razor 檔案中的額外 `@` 字元可能會導致稍後在區塊的陳述式中發生編譯器錯誤。 這些編譯器錯誤可能很難了解，因為實際錯誤發生在回報的錯誤之前。 將多個明確/隱含運算式合併成單一程式碼區塊之後，經常會出現此錯誤。

## <a name="control-structures"></a>控制結構

控制結構是程式碼區塊的延伸。 程式碼區塊的所有層面 (轉換成標記、內嵌 C#) 也適用於下列結構：

### <a name="conditionals-if-else-if-else-and-switch"></a>條件式 @if、else if、else 和 @switch

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

### <a name="looping-for-foreach-while-and-do-while"></a>迴圈 @for、@foreach、@while 和 @do while

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

### <a name="compound-using"></a>複合 @using

在 C# 中，`using` 陳述式可用來確保物件經過處置。 在 Razor 中，使用相同的機制來建立 HTML 協助程式，以包含其他內容。 在下列程式碼中，HTML 協助程式使用 `@using` 陳述式來轉譯表單標籤：


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

範圍層級動作可以透過[標籤協助程式](xref:mvc/views/tag-helpers/intro)來執行。

### <a name="try-catch-finally"></a>@try、catch、finally

例外狀況處理會類似於 C#：

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

Razor 可以使用 lock 陳述式來保護關鍵區段：

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

程式碼會轉譯下列 HTML：

```html
<!-- HTML comment -->
```

伺服器會先移除 Razor 註解，再轉譯網頁。 Razor 使用 `@*  *@` 來分隔註解。 下列程式碼會標記為註解，以確保伺服器不會轉譯任何標記：

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

Razor 指示詞是以隱含運算式表示，這些運算式具有保留關鍵字，後面接著 `@` 符號。 指示詞通常會變更檢視的剖析方式或啟用不同的功能。

了解 Razor 如何針對檢視產生程式碼，可讓您更容易了解指示詞的運作方式。

[!code-html[](razor/sample/Views/Home/Contact8.cshtml)]

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

本文稍後的[檢視針對檢視所產生的 Razor C# 類別](#viewing-the-razor-c-class-generated-for-a-view)一節將說明如何檢視此產生的類別。

<a name="using"></a>
### <a name="using"></a>@using

`@using` 指示詞會將 C# `using` 指示詞新增至產生的檢視：

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

`@model` 指示詞會指定傳遞至檢視的模型類型：

```cshtml
@model TypeNameOfModel
```

在使用個別使用者帳戶建立的 ASP.NET Core MVC 應用程式中，*Views/Account/Login.cshtml* 檢視包含下列模型宣告：

```cshtml
@model LoginViewModel
```

產生的類別繼承自 `RazorPage<dynamic>`：

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Razor 會公開 `Model` 屬性，以存取傳遞至檢視的模型：

```cshtml
<div>The Login Email: @Model.Email</div>
```

`@model` 指示詞會指定此屬性的類型。 該指示詞會將 `RazorPage<T>` 中的 `T` 指定為檢視從中衍生的產生類別。 若未指定 `@model` 指示詞，`Model` 屬性的類型為 `dynamic`。 模型的值會從控制器傳遞至檢視。 如需詳細資訊，請參閱[強型別模型和 &commat;model 關鍵字](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword)。

### <a name="inherits"></a>@inherits

`@inherits` 指示詞可讓您完全控制檢視所繼承的類別：

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

下列程式碼是自訂 Razor Page 類型：

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

`CustomText` 會顯示在檢視中：

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

程式碼會轉譯下列 HTML：

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 `@model` 和 `@inherits` 可用於相同的檢視。 `@inherits` 可能位於檢視匯入的 *_ViewImports.cshtml* 檔案中：

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

下列程式碼是強型別檢視的範例：

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

如果模型中傳遞了 "rick@contoso.com"，檢視會產生下列 HTML 標記：

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


`@inject` 指示詞可讓 Razor Page 從[服務容器](xref:fundamentals/dependency-injection)將服務插入至檢視。 如需詳細資訊，請參閱[在檢視中插入相依性](xref:mvc/views/dependency-injection)。

### <a name="functions"></a>@functions

`@functions` 指示詞可讓 Razor 頁面將 C# 程式碼區塊新增至檢視：

```cshtml
@functions { // C# Code }
```

例如: 

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

程式碼會產生下列 HTML 標記：

```html
<div>From method: Hello</div>
```

下列程式碼是產生的 Razor C# 類別：

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

`@section` 指示詞可搭配[配置](xref:mvc/views/layout)使用，讓檢視可以轉譯 HTML 頁面中不同部分的內容。 如需詳細資訊，請參閱[區段](xref:mvc/views/layout#layout-sections-label)。

## <a name="tag-helpers"></a>標籤協助程式

[標籤協助程式](xref:mvc/views/tag-helpers/intro)有三個相關的指示詞。

| 指示詞 | 功能 |
| --------- | -------- |
| [&commat;addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | 使標籤協助程式可供檢視。 |
| [&commat;removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | 移除先前從檢視新增的標籤協助程式。 |
| [&commat;tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | 指定標籤前置字元，以啟用標籤協助程式支援，並將標籤協助程式使用方式設為明確。 |

## <a name="razor-reserved-keywords"></a>Razor 保留關鍵字

### <a name="razor-keywords"></a>Razor 關鍵字

* page (需要 ASP.NET Core 2.0 和更新版本)
* namespace
* 函式
* 繼承
* model
* section
* helper (ASP.NET Core 目前不支援)

Razor 關鍵字會使用 `@(Razor Keyword)` (例如 `@(functions)`) 逸出。

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

C# Razor 關鍵字必須使用 `@(@C# Razor Keyword)` (例如 `@(@case)`) 雙重逸出。 第一個 `@` 會將 Razor 剖析器逸出。 第二個 `@` 會將 C# 剖析器逸出。

### <a name="reserved-keywords-not-used-by-razor"></a>Razor 未使用的保留關鍵字

* Class - 類別

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>檢視針對檢視所產生的 Razor C# 類別

請將下列類別新增至 ASP.NET Core MVC 專案：

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

以 `CustomTemplateEngine` 類別覆寫 MVC 所新增的 `RazorTemplateEngine`：

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

在 `CustomTemplateEngine` 的 `return csharpDocument` 陳述式上設定中斷點。 當程式在中斷點停止執行時，請檢視 `generatedCode` 的值。

![generatedCode 的文字視覺化檢視](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a>檢視查閱和區分大小寫

Razor 檢視引擎會針對檢視執行區分大小寫的查閱。 不過，實際查閱則取決於基礎檔案系統：

* 檔案式來源： 
  * 在具有不區分大小寫之檔案系統的作業系統上 (例如 Windows)，實體檔案提供者查閱不會區分大小寫。 例如，`return View("Test")` 針對 */Views/Home/Test.cshtml* 和 */Views/home/test.cshtml* (以及任何其他大小寫變體) 會有相符的結果。
  * 在區分大小寫的檔案系統上 (例如 Linux、OSX 及使用 `EmbeddedFileProvider`)，查閱會區分大小寫。 例如，`return View("Test")` 會明確符合 */Views/Home/Test.cshtml*。
* 先行編譯的檢視：在 ASP.NET Core 2.0 和更新版本中，在所有作業系統上查閱先行編譯的檢視不會區分大小寫。 此行為與 Windows 上之實體檔案提供者的行為相同。 如果兩個先行編譯的檢視只有大小寫不同，查閱的結果不會由此決定。

建議開發人員比對檔案和目錄的大小寫以及下列項目的大小寫：

    * 區域、控制器和動作名稱。 
    * Razor Pages。
    
比對大小寫可確保不論基礎檔案系統為何，部署作業都能夠找到其值。
